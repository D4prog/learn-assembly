---
layout: assembly
---
# 変数
C言語においての変数はメモリ上に予約された領域と、そこに付けられた名前で扱われます。（※[メモリ](memory.html)も参照のこと）
アセンブラで変数を考えるときは、C言語でそれがグローバル変数にあたるかローカル変数にあたるかを考える必要があります。

## グローバル変数
考え方： C言語と同じように、メモリ上に領域を予約します。

```C
int cursor_x;
```

これをアセンブラで表現すると、以下のようになります(BSSセクションに書くこと。)

```ASM
    .section bss
    .grobal cursor_x        ;; 全ファイルで cursor_x というラベルを使えるようにする
cursor_x:   .long   0       ;; ここで指定した初期値は無意味。
```

## ローカル変数(簡単)
C言語でのローカル変数の実現方式にあたるものは用意されていないので、[レジスタ](../essence/register)を使います。

```C
void sample_function(void) {
  int x;

}
```

サブルーチンの中でレジスタを使うとき、そのレジスタの元の値は失われるので、push-pop命令を用いて退避します。

```ASM
sample_subroutine:
    push.l  er0             ;; er0の元の値をスタックに退避

    ;; ここで er0 を変数 x として用いる。

    pop.l   er0             ;; er0の元の値をスタックから復元
    rts
```

## ローカル変数(複雑)
自分で適当なレジスタをスタックポインタとして用いて、スタックマシンとしてローカル変数を作ります。

```C
int i = 1;
char c = '\n';
```

```ASM
    push.w  r6
    mov.w   r7,r6
    subs    #2,r7
    subs    #2,r7
    mov.w   #1,r2
    mov.w   r2,@(-4,r6)
    mov.b   #10,r2l
    mov.b   r2l,@(-1,r6)
    mov.w   r6,r7
    pop.w   r6
    rts
```

(h8300-hms-gcc に -S オプションを渡してコードを吐かせてみたもの。)