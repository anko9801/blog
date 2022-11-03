---
title: "DRM"
---

# DRMの仕組みと実装を理解する

## メモ

DRM(Digital Rights Management)とは
> Digital rights management (DRM) is the management of legal access to digital content. Various tools or technological protection measures (TPM)[1] such as access control technologies can restrict the use of proprietary hardware and copyrighted works.[2] DRM technologies govern the use, modification, and distribution of copyrighted works (such as software and multimedia content), as well as systems that enforce these policies within devices.[3]

1. マネタイズのために各種assetへのアクセスをコントロールしたい
2. 物理assetは物理的束縛があるからコントロールしやすい
3. degital assetはどうしようね
4. **DRM**（に関する各種技術スタック）
    - https://en.wikipedia.org/wiki/Digital_rights_management
    - DRMはencryption/decryption keysをsecureにstore/deliverする技術（？）
        - DRM protectedな書籍をキャプチャできないのとかはどう解釈できるのか？
        - deliverされたkeyにconsumerが直接アクセスできない（はず）

> 【マネタイズ】
> For example, you could set rules to,
> - block people from certain countries,
> - allow access to the content for a certain period,
> - prevent a user from casting the movie onto a screen,
> - block free users from accessing premium content,
> - block playback on specific devices,
> - etc.

【コア】
encryptしたい人(creator side)とdecryptしたい人(consumer)がいる
第三者(store)がkeyを安全かつ適切なルールに基づいて配布する
![](https://md.trap.jp/uploads/upload_5feb762e9db1d37f5cb1159f2012a9e5.png)
【ここまでで実現できていること】
- a high-security level through daily renewal and code rotation
- authentication and rights management (read, write permissions)
    - 読み書きの権限を分ける
    - 複数人が読むことも可能
    - コンテンツへの段階的なアクセス
    - 期限付きの鍵
        - encryptする鍵をどんどん更新していけば最新のコンテンツが見れなくなるみたいなのは理解できる
        - 画面の直撮りとかは考えないにしてもencrypted+keyを得た時点でdecryptedが得られそうだけどどうやって防いでるんだろう（単純に手間になるように実装されてるからだれもやってないだけ？）
        - 専用クライアントみたいなのがあるわけだけどそのへんの実装に踏み込まないと分からなさそう？
    - etc
- a well-defined pricing model
【更に実現したいこと】
- コピー防止
- キャプチャ防止

---

- building blocks
    - EME(Encrypted Media Extensions)
        - https://www.w3.org/2017/07/EME-backgrounder.html
        - OS / ブラウザレベルでCDMとのインターフェースになっている
        - 復号化されたデータが扱われることはない (CDMのみが扱う)
        - https://bitmovin.com/digital-rights-management-everything-to-know/
    - CDM(Content Decryption Module)
    - AES(Advanced Encryption Standard)
        - みんな知ってるアレ
    - 動画固有
        - ABR(Adaptive Bitrate Streaming)
            - 各クライアントの帯域幅に応じて適切な複数bitrateで配信したい
            - https://ottverse.com/what-is-abr-video-streaming/
        - CMAF(Common Media Format)
            - > which said that videos can be stored in the fragmented mp4 container format (fmp4). With support from both MPEG-DASH and HLS, you can now create only one set of videos, store it in fmp4 format, and use a common set of files for both protocols.
            - https://mpeg.chiariglione.org/standards/mpeg-a/common-media-application-format
    - CENC(Common Encryption)
        - storeが違う暗号化方式を採用してるとcreatorがダルいので共通化しようと言うモチベ
        - > specifying that videos can be encrypted using either cenc (AES-128 CTR) or cbcs (AES-128 CBC). CTR stands for Counter; and CBC stands for Cipher Block Chaining.
    - Keys & Key Servers.
        - keyとencrypted contentはKeyIDで紐づく
        - KeyIDはmanifestにのってる

![](https://md.trap.jp/uploads/upload_09363d2df17afc60ec315fde1b1a9fb5.png)

現実のカスポイント
> Well, CENC might sound like a magic wand for DRM-unification, but it is not.
> 
> There are three primary DRM technologies in the market – Apple FairPlay, Google Widevine, and Microsoft PlayReady.
> 
> Apple FairPlay supports only AES-CBC cbcs mode.
HLS supports only AES-CBC cbcs mode (irrespective of CMAF)
Widevine and PlayReady support both AES-128 CTR cenc or AES-128 CBC cbcs modes.
MPEG-DASH with CMAF supports both AES-128 CTR cenc or AES-128 CBC cbcs modes.
MPEG-DASH without CMAF supports only AES-128 CTR cenc mode.
As you can see, the CMAF and CENC specs have lead to confusion and fragmentation in the streaming space.
> 
> A possible convergence point is the universal use of CMAF and AES-CBC cbcs mode, but, how will these impact legacy devices that support only CTR or only MPEG-TS?
> 
> That’s a discussion for another time.


---
分かりな疑問
> We’ve described a simple scheme, but there are many problems (technical and commercial) with our scheme. Here are some problems right off the bat.
> 
> We’ve described a prototypical “player” that sends a request for the decryption keys to the DRM License Server. But,
How does the license server know if the player is trustworthy?
And, what if the decryption software in the player exposes the key and the decrypted content?
Also, if you are a video player developer, do you have to develop decryption modules for every DRM technology? And, do you have to update it each time they make a change to their interfaces?
Furthermore, the sequence of events at the player (client-side) looks something like this –
> 
> 1. obtain the movie & its manifest from the CDN
> 1. extract the KeyID from the manifest
> 1. create the license request
> 1. send the license request to the license server
> 1. wait, listen, and receive the response from the license server.
> 1. use the decryption key from the server to decrypt the content
> 1. decode the decrypted content
> 1. display the decoded movie
> 
> A single program or entity should NOT do all of the above.

以下の2つに分ければいいらしい

- Player
    - 1, 2, 4, 5, (8 display これ安全じゃなさそう)
- CDM
    - 3(create Licence request), (understand the key), 6(decrypt), 7(decode)

### CDM(これ大事)
> Every DRM provider provides its own
> 1. mechanism to create a license request (using the KeyID, device identifier, signing the request, etc.)
> 1. mechanism to understand the license response received from the DRM License Server (the response is encrypted too) and extract the decryption key.
> 1. rules around storing the license locally on the client, license renewal, expiry, etc.

==browserにbuild-inでclosed-source==

### EME
![](https://md.trap.jp/uploads/upload_d6d10fe763ae69624a5057f9b1f37e00.png)

---

Q. 手元にある鍵はずっと使えそうだけど...？（「期限が切れたら自分のものでなくなる」というのをいかに実現しているか？）
A. バージョンバインディングの仕組み
バージョンを使って鍵の有効性を決めたい。普通の方法では古いバージョンのシステムの脆弱性だったりTEEの脆弱性があるとそれを使って有効にさせることができてしまう。そこでプロセスが起動する前にデバイスブートローダーがバージョン情報をTEE上で実行されているハードウェア格納型暗号サービスに渡し、鍵を生成する。アップグレード後は無効化するようにする。androidの記事より
キーストア=DRM Serverである？
他人が攻撃するときは完全に防げるけど自分のはブートローダーのレイヤーで攻撃すればいい話では（「正しく」実装されていることをなんらかの方法で証明できれば嬉しそう。しらんけど）
（TODO: 可能ということは分かったが、鍵の有効無効という概念があんまり理解できておらず）

Q. そもそもバージョンって何？なにのバージョン？
今回だとデバイスとOSのバージョンらしい??→OS等をアップデートしなければコンテンツを見続けられる...？（謎そう)

Q.有効なタイミングでdecryptedを保存するとかを防ぐのは別の仕組み？
A. :hi_UD::naruhodo:
encrypted+key(ネットワークを見ればいいからここは取れる。はず)->(ここにユーザーが介入できないようにする仕組みが必要)->view

---
:eyes:
![](https://www.w3.org/TR/encrypted-media/stack_overview.svg)
https://www.w3.org/TR/encrypted-media/

### どうでもいい話
streaming protocols
- https://linuxhit.com/rtmp-vs-hls-vs-dash-streaming-protocols/
- https://youtube-eng.googleblog.com/2018/04/making-high-quality-video-efficient.html
- https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming
    - https://ottverse.com/what-is-abr-video-streaming/
    - https://gist.github.com/voluntas/dd3af733825c7ae64505a1fd1bd0d684

## TODO
暗号化や署名周りの具体的な実装（理論）
DRM Provider各社のエコシステム

## refs
- https://source.android.com/security/keystore
- https://developers.google.com/widevine/drm/overview
- https://ottverse.com/eme-cenc-cdm-aes-keys-drm-digital-rights-management/
