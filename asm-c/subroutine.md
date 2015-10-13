---
layout: assembly
---
# 手続き
C言語で"関数"、アセンブラでは"サブルーチン"と呼ばれているものに関する解説を行います

## C言語での手続き
引数をもち、ローカル変数は自動的に動的記憶域期間をもち、ブロックの最後もしくは `return` 文によって呼び出し元に帰り、場合によっては戻り値を返す。

## アセンブラでの手続き
引数はレジスタのどれを使うかの取り決めによって受け取り、ローカル変数はないのでレジスタを使い[^1]、`rts` 命令によって呼び出し元に帰り、戻り値もないのでレジスタの値で戻します。



## サンプルコード

```C
int gcd(int m, int n) {
  int tmp;
  if (n == 0) {
    return m;
  }
  tmp = n;
  n = m % n;
  m = tmp;
  return gcd(m, n);
}
```
  
```ASM
;;; ****************
;;; gcd 最大公約数
;;;   引数 er0, e1
;;;   戻値 r1
;;;   (tmp r1)
;;; ****************
    .global gcd
gcd:
    mov.w   e1,e1
    bne     gcd_rec
    mov.w   r0,r1
    rts
gcd_rec:
    mov.w   e1,r1
    divxu.w er0,e1  ; e0 = er0 % e1, r0 = er0 / e1;
    mov.w   e0,e1   ; n = m % n;
    mov.w   r1,r0   ; m = tmp;
    extu.l  er0
    jsr     @gcd    ; 引数にあたる値を決めたレジスタに入れてから呼出し
    rts
```

[^1]: variable.html#ローカル変数