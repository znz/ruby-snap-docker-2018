# 最近のrubyインストール方法

author
:   Kazuhiro NISHIYAMA

content-source
:   第84回 Ruby関西 勉強会

date
:   2018/12/01

institution
:   株式会社Ruby開発

allotted-time
:   20m

theme
:   lightning-simple

# 自己紹介

- 西山 和広
- Ruby のコミッター
- twitter, github など: @znz
- 株式会社Ruby開発 www.ruby-dev.jp

# agenda

- 通常インストール
- snap
- docker

# 通常インストール

- 最新版は公式サイトのダウンロード <http://www.ruby-lang.org/ja/downloads/> から
  - Windows なら RubyInstaller
  - Ruby 開発者には rbenv + ruby-build が人気で rvm は評判が悪い
- Linux などなら OS 標準のパッケージでインストールも良い
  - yum や apt など

# snap とは?

- canonical が開発している新しいパッケージシステム
- Ubuntu 16.04 以降には標準で入っている
- その他の対応環境は <https://docs.snapcraft.io/installing-snapd/6735> 参照
- <http://www.ruby-lang.org/ja/news/2018/11/08/snap/>

## memo

- https://www.notion.so/The-official-ruby-snap-is-available-48a5dddd4a004373aa041989d4ed8b7d

# snap でのインストール

- `sudo snap install ruby --classic`
  - 2018/11 現在 channel を指定しない場合は 2.5.3 がインストールされる
- 2.4 を利用したい場合

```
sudo snap install ruby --classic --channel=2.4/stable
```

# 切り替え

2.3 に切り替えるには以下のコマンドを実行:

```
sudo snap switch ruby --channel=2.3/stable
sudo snap refresh
```

# snap の制限事項

- RubyGems は `$HOME/.gem` にインストールされるように `GEM_HOME` と `GEM_PATH` が設定されている
- `bundle exec` なしで `rails` コマンドなどを実行したい場合 `.bashrc` などに以下が必要

```
eval `ruby.env`
```

# snap での gem の注意事項

- `$HOME/.gem` が複数バージョンで共有される
- 切り替え時にC拡張は `gem pristine --extensions` で再コンパイルが必要
  - nokogiri など

# フィードバック先

- <https://github.com/ruby/snap.ruby>
  - 不具合報告やフィードバックなどはこちらへ

# docker とは?

- Linux のコンテナ環境
- 簡単にいうと、外側の環境にあまり影響を与えずに、独立した環境の中でプログラムを動かせるもの

# docker ruby イメージ

- <https://hub.docker.com/_/ruby/>
  - docker pull ruby のもの
  - docker オフィシャル
  - production 環境向き
  - ruby 本体の開発者は関わっていない
    (Linux ディストリビューションのパッケージと同じ)

# rubylang/ruby イメージ

- <https://hub.docker.com/r/rubylang/ruby/>
  - docker pull rubylang/ruby
  - ruby-lang.org オフィシャル
  - 2018/11現在 実験的 (EXPERIMENTAL) 扱い
  - trunk のナイトリービルドがある
  - 開発中のバージョンを一番手軽に試せる環境になるかも

# rubylang/all-ruby イメージ

- <https://hub.docker.com/r/rubylang/all-ruby/>
  - docker pull rubylang/all-ruby
  - 大きい (現在 10.2GB) ので注意
  - リリースされたすべての ruby での動作を確認できるイメージ
  - バグ報告をするときやドキュメントを書く時などに便利

# 実行例

```
$ docker run -it --rm rubylang/all-ruby ./all-ruby -e 'print("hello\n")'
ruby-0.49             hello
...
ruby-2.6.0-preview2   hello
```

# まとめ

- 通常インストールは公式サイト参照
- ディストリビューションのパッケージのインストールもあり
- snap パッケージが最近増えた
- 用途によっては docker イメージも便利
