---
title: "Go"
---

# Go GC Pacer Redesign
###### tags:`weekly tiken`

## メモ
GoのGCに詳しくない人向け: https://deepu.tech/memory-management-in-golang/
- これまで
    - heap sizeがしきい値（`freshly allocated / remaining memory > GOGC/100`）を超える(or 一定時間経過←怪しい、忘れた)と次のサイクルを実行
    - https://docs.google.com/document/d/1wmjrocXIWTr1JxU-3EQBI6BK6KgtiFArkG47XK73xIQ/edit

> The GOGC variable sets the initial garbage collection target percentage. A collection is triggered when the ratio of freshly allocated data to live data remaining after the previous collection reaches this percentage. The default is GOGC=100. Setting GOGC=off disables the garbage collector entirely. The runtime/debug package's SetGCPercent function allows changing this percentage at run time. See https://golang.org/pkg/runtime/debug/#SetGCPercent.

- これから
    - 色々加味する
> 1. Including all non-heap sources of GC work (stacks, globals) in pacing decisions.
> → will resolve long-standing issues with small heap sizes, allowing the Go garbage collector to scale down and act more predictably in general.
> 2. Reframing the pacing problem as a search problem, "solved" by a proportional-integral controller,
> → will eliminate offset error present in the current design, will allow turning off mark-assist almost entirely outside of exceptional cases, improving allocation latency, and will enable clearer designs for setting memory limits on Go applications.
> 3. Extending the hard heap goal to the worst-case heap goal of the next GC,
> → will enable smooth and consistent response to large changes in the live heap size with large GOGC values.

GCの処理自体はRCと比べて重いので、適宜タイミングを見計らって実行される。
- ヒープサイズが大きくなったとき
- システムが暇なとき
- プログラムから明示的に呼び出されたとき

モチベーション: https://github.com/golang/go/issues/42430

Javaとかはどうなってんの？
例: https://wiki.openjdk.java.net/display/shenandoah/Main#Main-ImplementationOverview
基本はheap size見てるけどHeuristicsのとこにモードとその説明がある。

GC Paceがなぜ大事か
> These two goals are tightly related. If a garbage collection cycle starts too late, for instance, it may consume more CPU to avoid missing its target. If a cycle begins too early, it may end too early, resulting in GC cycles happening more often than expected.

==CPU Utilization: 30% (GC 25%, assist 5%)==
> the steady-state is implicitly defined as: constant allocation rate, constant heap size, and constant heap composition (hence, constant mark rate). The pacer expects the application to settle on some average global behavior across GC cycles.

but still robust for transcient state, and this is acomplished by finding new steady-state. The GC makes allocating goroutines donate their time to assist the garbage collector, proportionally to the amount of memory that they allocate. 

what is propotional controller?

what is assist system?
> The GC assist system operates by dynamically computing an assist ratio. The assist ratio is the slope of a curve in the space of allocation time and GC work time, a curve that the application is required to stay under. This assist ratio is then used as a conversion factor between the amount a goroutine has allocated, and how much GC assist work it should do. Meanwhile, GC workers generate assist credit from the work that they do and place it in a global pool that allocating goroutines may steal from to avoid having to assist.

meta-issues: https://github.com/golang/go/issues/42430

## TODO
- 以前のペーサーのコードリーディング
    - https://cs.opensource.google/go/go/+/master:src/runtime/mgcpacer.go
- assist systemの実装（Gorutineのglobal pool?）
- 現在のペーサーがどのように加味するようになったか実装を確認

## refs
- https://github.com/golang/go/issues/44167
- https://github.com/golang/proposal/blob/master/design/44167-gc-pacer-redesign.md
- https://github.com/golang/go/commit/a108b280bc724779ebaa6656d35f0fb307fb2a9b
- https://github.com/golang/go/issues/14951
- https://eng.uber.com/how-we-saved-70k-cores-across-30-mission-critical-services/
- https://concertio.com/blog/optimizing-the-go-garbage-collector-and-concurrency/