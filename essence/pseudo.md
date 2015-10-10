---
layout: assembly
---
# 擬似命令
ニーモニックに対応しない、アセンブラに対する命令。

## 一覧

* [`.section`](#section)
* [`.global`](#global)
* [`.equ`](#equ)
* [`.align`](#align)
* [`.end`](#end)
* [`.rept` - `.endr`](#rept-endr)

## 解説

### section
[セクション](./section.html)の開始宣言。次のセクションの開始、もしくは終端までをセクションとする。

```ASM
    .section    .text
```
### global
ラベル名をグローバルに扱えるようにする。つまり他のファイルからサブルーチン等を参照するために必要。

```ASM
    .global     update
update:
    ...
    rts
```

### equ
`#define`。以上。

```ASM
    .equ    OBJ_MAX,40
```
### align
アラインメント（データのメモリへの格納に際する揃え位置）を調整する
### end
ソースプログラムの終了
### rept-endr
繰り返す。使用できる用例に関しては実験中。
[配列](../ref-asm/array.html)