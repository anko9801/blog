```
pub enum List {
    Empty,
    Elem(i32, List),
}
```

単方向だと要素を追加するときに前のリストを変更する必要が無いから永続化可能