---
layout: assembly
title: アセンブラ
---
# アセンブラ

## アセンブラ言語とは

機械語を人間にわかりやすい単語で表現したもの。といっても基本的に一対一の対訳なので、CPUの考え方が必要となる。プログラムは上から一行ずつ実行されると考えればよい。

* [アセンブラ言語とは](about-asm.html)
  アセンブラ言語でプログラムを書くにあたって知っておいたほうが楽なこと
  
* [マイコンプログラミングエッセンス](essence) 諸注意 ファイル分割 etc...
* [アセンブラ言語とC言語](asm-c)
  C言語での考え方や書き方は、アセンブラ言語になおすとどうなるか
* [トラブルシューティング](troubleshoot)
  謎の不具合に対処する
* [逆引きアセンブラ](ref-asm)
  こういうことを実現したい。どうすれば？


## 付録
* [Makefile](makefile.html) アセンブルから転送まで
* [文字変換] カタカナ -> アセンブラ埋め込み文字列


## 前提
* C言語の必要最低限の読み書きができること
* 特に何も記載しない場合、環境は H8/300H 3052F ＵＳＢ開発セット，アセンブラは h8300-hms-as を用いるものとする
* 語句の表記は『[H8アセンブラ入門](https://books.google.co.jp/books?id=9dd1xBNFRpAC)』にならう
* コードの表記は `h8300-hms-as` が受け付ける形式にならう
