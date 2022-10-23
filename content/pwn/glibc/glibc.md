
### glibc ビルド方法

1. https://www.gnu.org/software/libc/ のリリースページから ftp に飛んでダウンロードする.
2. ビルド作業用のディレクトリを作ってそこに入る
3. `../glibc-hoge/congifure --prefix=/path/to/インストールしたい場所`
4. `make -j{N}`
6. `make install`

### glibc

自前ビルドしてるとデバッグ情報も付いてくる.
[patchelf](https://github.com/NixOS/patchelf) では手で叩かないといけない.
[pwninit](https://github.com/io12/pwninit) なら glibc を同じ階層に入れておけば `pwninit` で勝手にやってくれる.
`ldd` コマンドで確認できる.

パッチ自動化できそう.