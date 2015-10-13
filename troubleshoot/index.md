---
layout: assembly
---
# トラブルシューティング

## 一般的なデバッグ
1. 文字出力を利用してどのあたりで想定しない動作をしているか特定
  (どのレジスタがおかしいか、どの行からが動いていないか、等)
2. 周辺のレジスタやジャンプ命令を見なおしてみる
  (書き間違いはないか、思い違いはないか)
3. こんがらがったら紙に書く
  (フローチャート、Cのコード、レジスタ対応図、等)

## 挙動編
* サブルーチンが戻ってこない / 変なところに飛ぶ
  + `rts` の書き忘れはないですか、`rts` 命令のある行に*本当に*たどり着いていますか
  + push-pop の対応は正しいですか、er7(e7,r7,r7h,r7l)の値を変えていませんか
* プログラムが進まない
  + 分岐命令は正しいですか(無限ループしていませんか)
  + 
* 

## エラーメッセージ編
<!-- (実際のところ、きちんと命令の説明を読めば回避できるのでここはおまけです) -->
> unknown opcode

"不明な命令": たいてい命令のタイプミスです

> invalid operands

"無効なオペランド":

* `cmp` 命令
  + 第二オペランドに即値は指定できません(例: `cmp.b r0l,#10`)。逆にしてください

> Opcode *** with these operand types not available

"オペランドの形式が不正"

* `mov` 命令
  + 即値->レジスタ間接 での移動(例: `mov.b #10,@er0`)はありません。いったんレジスタを経由してください



