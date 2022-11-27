---
title: "命題論理"
---

## 推論規則

```=
MP: Modus Ponens
P -> Q
P
------
Q

MT: Modus Tollens
P -> Q
~Q
------
~P

DN: Double Negation
P
------
~~P

~~P
------
P

R: Repetition
P
------
P

Adj: Adjunction
P
Q
------
P ∧ Q

S: Simplification
P ∧ Q
------
P

P ∧ Q
------
Q

Add: Addition
P
------
P ∨ Q

Q
------
P ∨ Q

MTP: Modus Tollendo Ponens
P ∨ Q
~P
------
Q

P ∨ Q
~Q
------
P

CB: Conditional-Biconditional
P -> Q
Q -> P
-------
P <-> Q

BC: Binconditional-Conditional
P <-> Q
-------
P -> Q

P <-> Q
-------
Q -> P
```

## 証明

`P -> Q, ~P -> Q ∴ Q`
```=
Show Q
     ~Q        AssID
     P -> Q    Pr
     ~P -> Q   Pr
     ~P        2,3MT
     ~~P       2,4MT
5,6ID
```

`P -> (R -> ~Q), Q ∴ P -> ~R`
```=
Show P -> ~R
     P              AssCD
     P -> (R -> ~Q) Pr
     Q              Pr
     R -> ~Q        2,3MP
     ~~Q            4DN
     ~R             5,6MT
7CD
```

`~P ∧ ~Q ∴ ~(P ∨ Q)`
```=
Show ~(P ∨ Q)
     ~~(P ∨ Q) AssID
     P ∨ Q     2DN
     ~P ∧ ~Q   Pr
     ~P        4S
     Q         3,5MTP
     ~Q        4S
6,7ID
```

`P ∨ Q -> R ∴ P -> R ∨ Q`
```=
Show P -> R ∨ Q
     P          AssCD
     P ∨ Q -> R Pr
     P ∨ Q      2Add
     R          3,4MP
     R ∨ Q      5Add
6CD
```

`P -> Q, ~Q ∴ ~P`
```=
Show ~P
     ~~P    AssID
     P      2DN
     P -> Q Pr
     ~Q     Pr
     Q      3,4MP
5,6ID
```

`P, ~P ∴ Q`
```=
Show Q
     P     Pr
     P ∨ Q 2Add
     ~P    Pr
     Q     3,4MTP
5DD
```

`(P -> ~Q) -> ~T, ~Q ∴ ~T`
```=
Show ~T
     ~~T             AssID
     (P -> ~Q) -> ~T Pr
     ~Q              Pr
     ~(P -> ~Q)      2,3MTP
     Show P -> ~Q
          P          AssCD
          ~Q         4R
     8CD
5,6ID
```

`W, (P -> W) -> (R -> T) ∴ ~T -> (Q -> ~R)`
```=
Show ~T -> (Q -> ~R)
     ~T                   AssCD
     W                    Pr
     (P -> W) -> (R -> T) Pr
     Show P -> W
          P               AssCD
          W               3R
     7CD
     R -> T               4,5MP
     ~R                   2,9MTP
     Show Q -> ~R
          Q               AssCD
          ~R              10R
     13CD
11CD
```


ここから自明なDNは省略する.

`~(P -> Q) -> ~(R -> S), S -> ~Q ∴ P -> ~S`
```=
Show P -> ~S
     P                      AssCD
     ~(P -> Q) -> ~(R -> S) Pr
     S -> ~Q                Pr
     Show ~S
          S                 AssID
          ~Q                4,6MP
          Show ~(P -> Q)
               P -> Q       AssID
               Q            2,9MP
               ~Q           7R
          10,11ID
          ~(R -> S)         3,8MP
          Show R -> S
               R            AssCD
               S            6R
          16CD
     13,14ID
5CD
```

`(P -> Q) -> R, ~R ∴ ~Q`
```=
Show ~Q
     Q             AssID
     (P -> Q) -> R Pr
     ~R            Pr
     Show P -> Q
          P        AssCD
          Q        2R
     7CD
     R             3,5MP
4,9ID
```

`(R -> S) -> P, ~S -> Q ∴ ~P -> Q`
```=
Show ~P -> Q
     ~P            AssCD
     (R -> S) -> P Pr
     ~S -> Q       Pr
     Show Q
          ~Q       AssID
          S        4,6MT
          Show R -> S
               R   AssCD
               S   7R
          10CD
          P        3,8MP
          ~P       2R
     12,13ID
5CD
```

`(Q -> ~~S) -> (~R -> ~S) ∴ S -> R`
```=
Show S -> R
     S                        AssCD
     (Q -> ~~S) -> (~R -> ~S) Pr
     ~~S                      2DN
     Show Q -> ~~S
          Q                   AssCD
          ~~S                 4R
     7CD
     ~R -> ~S                 3,5MP
     R                        4,9MT
10CD
```

`(P -> Q) -> (T -> R), U -> ~R, ~(S -> P) ∴ U -> ~T`
```=
Show U -> ~T
     U                      AssCD
     (P -> Q) -> (T -> R)   Pr
     U -> ~R                Pr
     ~(S -> P)              Pr
     ~R                     2,4MP
     Show ~T
          T                 AssID
          Show ~(T -> R)
               T -> R       AssID
               R            8,10MP
               ~R           6R
          11,12ID
          ~(P -> Q)         3,9MT
          Show P -> Q
               P            AssCD
               Show S -> P
                    S       AssCD
                    P       16R
               19CD
               (S -> P) ∨ Q 17Add
               Q            5,21MTP
          22CD
     14,15ID
7CD
```

`(~Q -> S) -> P, ~R -> Q, R -> S ∴ P`
```=
Show P
     ~P             AssID
     (~Q -> S) -> P Pr
     ~R -> Q        Pr
     R -> S         Pr
     ~(~Q -> S)     2,3MT
     Show ~Q -> S
          ~Q        AssCD
          R         4,8MT
          S         5,9MP
     10CD
6,7ID
```

`P -> (Q -> R), P -> (~Q -> R), ~P -> (Q -> R), ~P -> (~Q -> R) ∴ R`
```=
Show R
     ~R              AssID
     P -> (Q -> R)   Pr
     P -> (~Q -> R)  Pr
     ~P -> (Q -> R)  Pr
     ~P -> (~Q -> R) Pr
     Show P
          ~P         AssID
          Q -> R     5,8MP
          ~Q -> R    6,8MP
          ~Q         2,9MT
          ~~Q        2,10MT
     11,12ID
     Q -> R          3,7MP
     ~Q -> R         4,7MP
     ~Q              2,14MT
     ~~Q             2,15MT
16,17ID
```

`(P -> Q) -> ~(Q -> P) ∴ P -> ~Q`
```=
Show P -> ~Q
     P                     AssCD
     (P -> Q) -> ~(Q -> P) Pr
     Show ~Q
          Q                AssID
          Show P -> Q
               P           AssCD
               Q           5R
          8CD
          ~(Q -> P)        3,6MP
          Show Q -> P
               Q           AssCD
               P           2R
          13CD
     10,11ID
4CD
```

`(P -> Q) -> (R -> S), (Q -> P) -> (R -> S) ∴ ~S -> ~R`
```=
Show ~S -> ~R
     ~S                          AssCD
     (P -> Q) -> (R -> S)        Pr
     (Q -> P) -> (R -> S)        Pr
     Show ~R
          R                      AssID
          Show P -> Q
               P                 AssCD
               Show Q
                    ~Q           AssID
                    Show Q -> P
                         Q       AssCD
                         P       8R
                    13CD
                    R -> S       4,11MP
                    ~R           2,15MT
                    R            6R
               16,17ID
          9CD
          R -> S                 3,7MP
          ~R                     2,20MT
     6,21ID
5CD
```

対偶律
`∴ (P -> Q) <-> (~Q -> ~P)`
```=
Show (P -> Q) <-> (~Q -> ~P)
     Show (P -> Q) -> (~Q -> ~P)
          P -> Q             AssCD
          Show ~Q -> ~P
               ~Q            AssCD
               ~P            3,5MT
          6CD
     4CD
     Show (~Q -> ~P) -> (P -> Q)
          ~Q -> ~P           AssCD
          Show P -> Q
               P             AssCD
               ~~P           12DN
               ~~Q           10,13MT
               Q             14DN
          15CD
     11CD
     (P -> Q) <-> (~Q -> ~P) 2,9CB
18DD
```

排中律
`∴ P ∨ ~P`
```=
Show P ∨ ~P
     ~(P ∨ ~P)      AssID
     Show P
          ~P        AssID
          P ∨ ~P    4Add
          ~(P ∨ ~P) 2R
     5,6ID
     P ∨ ~P         3Add
2,8ID
```

CDJ
`∴ P ∨ Q <-> (~P -> Q)`
```=
Show P ∨ Q <-> (~P -> Q)
     Show P ∨ Q -> (~P -> Q)
          P ∨ Q         AssCD
          Show ~P -> Q
               ~P       AssCD
               Q        3,5MTP
          6CD
     4CD
     Show (~P -> Q) -> P ∨ Q
          ~P -> Q       AssCD
          Show P ∨ Q
               ~(P ∨ Q) AssID
               ~P ∧ ~Q  12DM
               ~P       13S
               Q        10,14MP
               ~Q       13S
          15,16ID
     11CD
     P ∨ Q <-> (~P -> Q) 2,9CB
19DD
```

ド・モルガン則
`∴ ~(P ∧ Q) <-> ~P ∨ ~Q`
```=
Show ~(P ∧ Q) <-> ~P ∨ ~Q
     Show ~(P ∧ Q) -> ~P ∨ ~Q
          ~(P ∧ Q)                AssCD
          Show ~P ∨ ~Q
               ~(~P ∨ ~Q)         AssID
               Show ~P
                    ~~P           AssID
                    P             7DN
                    Show ~Q
                         ~~Q      AssID
                         Q        10DN
                         P ∧ Q    8,11Adj
                         ~(P ∧ Q) 3R
                    12,13ID
                    ~P ∨ ~Q       9Add    
                    ~(~P ∨ ~Q)    5R
               ~P ∨ ~Q            6Add
          4CD
     Show ~P ∨ ~Q -> ~(P ∧ Q)
          ~P ∨ ~Q                 AssCD
          Show ~(P ∧ Q)
               ~~(P ∧ Q)          AssID
               P ∧ Q              22DN
               P                  23S
               ~~P                24DN
               ~Q                 20,25MTP
               Q                  23S
          26,27ID
     21CD
     ~(P ∧ Q) <-> ~P ∨ ~Q         2,19CB
30DD
```

ド・モルガン則
`∴ ~(P ∨ Q) <-> ~P ∧ ~Q`
```=
Show ~(P ∨ Q) <-> ~P ∧ ~Q
     Show ~(P ∨ Q) -> ~P ∧ ~Q
          ~(P ∨ Q)        AssCD
          Show ~P
               P          AssID
               P ∨ Q      5Add
               ~(P ∨ Q)   3R
          6,7ID
          Show ~Q
               Q          AssID
               P ∨ Q      10Add
               ~(P ∨ Q)   3R
          11,12ID
          ~P ∧ ~Q         4,9Adj
     14CD
     Show ~P ∧ ~Q -> ~(P ∨ Q)
          ~P ∧ ~Q         AssCD
          Show ~(P ∨ Q)
               ~~(P ∨ Q)  AssID
               P ∨ Q      19DN
               ~P         17S
               Q          20,21MTP
               ~Q         17S
          22,23ID
     18CD
     ~(P ∨ Q) <-> ~P ∧ ~Q 2,16CB
26DD
```

定理
`∴ Q -> (P -> Q)`
```=
Show Q -> (P -> Q)
     Q      AssCD
     Show P -> Q
          P AssCD
          Q 2R
     5CD
3CD
```

```
DM: De Morgan
~(P ∨ Q)
--------
~P ∧ ~Q

~(P ∧ Q)
--------
~P ∨ ~Q

CDJ
~P -> Q
--------
P ∨ Q
```

`∴ P ∨ (Q ∧ R) -> (P ∨ Q) ∧ (P ∨ R)`
```=
Show P ∨ (Q ∧ R) -> (P ∨ Q) ∧ (P ∨ R)
     P ∨ (Q ∧ R)        AssCD
     Show P ∨ Q
          ~(P ∨ Q)      AssID
          ~P ∧ ~Q       4DM
          ~P            5S
          Q ∧ R         2,6MTP
          Q             7S
          ~Q            5S
     8,9ID
     Show P ∨ R
          ~(P ∨ R)      AssID
          ~P ∧ ~R       12DM
          ~P            13S
          Q ∧ R         2,14MTP
          R             15S
          ~R            13S
     16,17ID
     (P ∨ Q) ∧ (P ∨ R)  3,11Adj
19CD
```

`∴ (P ∨ Q) ∧ (P ∨ R) -> P ∨ (Q ∧ R)`
```=
Show (P ∨ Q) ∧ (P ∨ R) -> P ∨ (Q ∧ R)
     (P ∨ Q) ∧ (P ∨ R)    AssCD
     Show P ∨ (Q ∧ R)
          ~(P ∨ (Q ∧ R))  AssID
          ~P ∧ ~(Q ∧ R)   4DM
          ~P              5S
          P ∨ Q           2S
          P ∨ R           2S
          Q               6,7MTP
          R               6,8MTP
          Q ∧ R           9,10Adj
          ~(Q ∧ R)        5S
     11,12ID
3CD
```

`∴ P ∧ (Q ∨ R) -> (P ∧ Q) ∨ (P ∧ R)`
```=
Show P ∧ (Q ∨ R) -> (P ∧ Q) ∨ (P ∧ R)
     P ∧ (Q ∨ R)               AssCD
     Show (P ∧ Q) ∨ (P ∧ R)
          ~((P ∧ Q) ∨ (P ∧ R)) AssID
          ~(P ∧ Q) ∧ ~(P ∧ R)  4DM
          ~(P ∧ Q)             5S
          ~P ∨ ~Q              6DM
          P                    2S
          ~Q                   7,8MTP
          Q ∨ R                2S
          R                    9,10MTP
          ~(P ∧ R)             5S
          ~P ∨ ~R              12DM
          ~R                   8,13MTP
     11,13ID
3CD
```

`∴ (P ∧ Q) ∨ (P ∧ R) -> P ∧ (Q ∨ R)`
```=
Show (P ∧ Q) ∨ (P ∧ R) -> P ∧ (Q ∨ R)
     (P ∧ Q) ∨ (P ∧ R) AssCD
     Show P
          ~P           AssID
          ~P ∨ ~R      4Add
          ~(P ∧ R)     5DM
          P ∧ Q        2,6MTP
          P            7S
     4,8ID
     Show Q ∨ R
          ~(Q ∨ R)     AssID
          ~Q ∧ ~R      11DM
          ~R           12S
          ~P ∨ ~R      13Add
          ~(P ∧ R)     14DM
          P ∧ Q        2,15MTP
          Q            16S
          ~Q           12S
     17,18ID
     P ∧ (Q ∨ R)       3,10Adj
20CD
```

`∴ P ∧ (Q ∨ R) -> (P ∧ Q) ∨ (P ∧ R)`
```=
Show P ∧ (Q ∨ R) -> (P ∧ Q) ∨ (P ∧ R)
     P ∧ (Q ∨ R)               AssCD
     Show (P ∧ Q) ∨ (P ∧ R)
          ~((P ∧ Q) ∨ (P ∧ R)) AssID
          ~(P ∧ Q) ∧ ~(P ∧ R)  4DM
          ~(P ∧ Q)             5S
          ~(P ∧ R)             5S
          ~P ∨ ~Q              6DM
          ~P ∨ ~R              7DM
          P                    2S
          ~Q                   8,10MTP
          ~R                   9,10MTP
          ~Q ∧ ~R              11,12Adj
          ~(Q ∨ R)             13DM
          Q ∨ R                2S
     14,15ID
3CD
```

`P ∧ Q -> R ∨ S ∴ (~P ∨ R) ∨ (Q -> S)`
```=
Show (~P ∨ R) ∨ (Q -> S)
     ~((~P ∨ R) ∨ (Q -> S)) AssID
     ~(~P ∨ R) ∧ ~(Q -> S)  2DM
     P ∧ Q -> R ∨ S         Pr
     ~(~P ∨ R)              3S
     P ∧ ~R                 5DM
     ~(Q -> S)              3S
     Show Q ∧ ~S
          ~(Q ∧ ~S)         AssID
          ~Q ∨ S            9DM
          Q -> S            10CDJ
          ~(Q -> S)         7R
     11,12ID
     P                      6S
     Q                      8S
     P ∧ Q                  14,15Adj
     R ∨ S                  4,16MP
     ~R                     6S
     S                      17,18MTP
     ~S                     8S
19,20ID
```

`(P ∧ Q) ∨ (P ∧ ~R), R ∨ ~P ∴ Q`
```=
Show Q
     ~Q                 AssID
     (P ∧ Q) ∨ (P ∧ ~R) Pr
     R ∨ ~P             Pr
     ~P ∨ ~Q            2Add
     ~(P ∧ Q)           5DM
     P ∧ ~R             3,6MTP
     ~(P ∧ ~R)          4DM
7,8ID
```

`P ∨ Q, P -> S, Q <-> T ∴ S ∨ T`
```=
Show S ∨ T
     ~(S ∨ T)  AssID
     ~S ∧ ~T   2DM
     P ∨ Q     Pr
     P -> S    Pr
     Q <-> T   Pr
     Q -> T    6BC
     ~T        3S
     ~Q        7,8MT
     P         4,9MTP
     S         5,10MP
     ~S        3S
11,12ID
```

`~(P ∧ R) -> (S -> R) ∴ R ∨ ~S`
```=
Show R ∨ ~S
     ~(R ∨ ~S)            AssID
     ~R ∧ S               2DM
     ~(P ∧ R) -> (S -> R) Pr
     ~R                   3S
     ~P ∨ ~R              5Add
     ~(P ∧ R)             6DM
     S -> R               7MP
     S                    3S
     R                    8,9MP
5,10ID
```

`(Q ∧ ~T) ∨ ~(T -> Q) ∴ (P -> Q) ∨ (T ∨ R)`
```=
Show (P -> Q) ∨ (T ∨ R)
     ~((P -> Q) ∨ (T ∨ R)) AssID
     ~(P -> Q) ∧ ~(T ∨ R)  2DM
     (Q ∧ ~T) ∨ ~(T -> Q)  Pr
     ~(T ∨ R)              4S
     ~T ∧ ~R               5DM
     ~T                    6S
     ~T ∨ Q                7Add
     T -> Q                8CDJ
     Q ∧ ~T                3,9MTP
     Q                     10S
     ~P ∨ Q                11Add
     P -> Q                12CDJ
     ~(P -> Q)             4S
13,14ID
```

`(P <-> Q ∧ S) ∨ (P <-> R ∧ T) ∴ P -> Q ∨ R`
```=
Show P -> Q ∨ R
     P                             AssCD
     (P <-> Q ∧ S) ∨ (P <-> R ∧ T) Pr
     Show Q ∨ R
          ~(Q ∨ R)                 AssID
          ~Q ∧ ~R                  5DM
          ~Q                       6S
          ~R                       6S
          ~Q ∨ ~S                  7Add
          ~R ∨ ~T                  8Add
          ~(Q ∧ S)                 9DM
          ~(R ∧ T)                 10DM
          P ∧ ~(Q ∧ S)             2,11Adj
          P ∧ ~(R ∧ T)             2,12Adj
          ~(~P ∨ (Q ∧ S))          13DM
          ~(~P ∨ (R ∧ T))          14DM
          Show ~(P <-> (Q ∧ S))
               P <-> (Q ∧ S)       AssID
               P -> (Q ∧ S)        18BC
               ~P ∨ (Q ∧ S)        19CDJ
               ~(~P ∨ (Q ∧ S))     15R
          20,21ID
          Show ~(P <-> (R ∧ T))
               P <-> (R ∧ T)       AssID
               P -> (R ∧ T)        24BC
               ~P ∨ (R ∧ T)        25CDJ
               ~(~P ∨ (R ∧ T))     16R
          26,27ID
          P <-> R ∧ T              3,17MTP
     23,29ID
4CD
```

`~(P <-> Q), ~(P <-> R) ∴ Q <-> R`
```=
Show Q <-> R
     ~(P <-> Q)                          Pr
     ~(P <-> R)                          Pr
     Show Q -> R
          Q                              AssCD
          Show R
               ~R                        AssID
               Show P -> Q
                    P                    AssCD
                    Q                    5R
               10CD
               Show Q -> P
                    Q                    AssCD
                    Show P
                         ~P              AssID
                         Show P <-> R
                              Show P -> R
                                   P     AssCD
                                   P ∨ R 18Add
                                   R     15,19MTP
                              20CD
                              Show R -> P
                                   R     AssCD
                                   P ∨ R 23Add
                                   P     7,24MTP
                              25CD
                              P <-> R    17,22CB
                         27DD
                         ~(P <-> R)      3R
                    16,29ID
               14CD
               P <-> Q                   8,12CB
               ~(P <-> Q)                2R
          32,33ID
     6CD
     Show R -> Q
          R                              AssCD
          Show Q
               ~Q                        AssID
               Show P -> R
                    P                    AssCD
                    R                    37R
               42CD
               Show R -> P
                    R                    AssCD
                    Show P
                         ~P              AssID
                         Show P <-> Q
                              Show P -> Q
                                   P     AssCD
                                   P ∨ Q 50Add
                                   Q     47,51MTP
                              52CD
                              Show Q -> P
                                   Q     AssCD
                                   P ∨ Q 55Add
                                   P     39,56MTP
                              57CD
                              P <-> Q    49,54CB
                         59DD
                         ~(P <-> Q)      2R
                    48,61ID
               46CD
               P <-> R                   40,44CB
               ~(P <-> R)                3R
          64,65ID
     38CD
     Q <-> R                             4,36CB
68DD
```

