---
layout: assembly
---

# 制御構造
[コンディションコードレジスタ(CCR)](../essence/ccr.html)、
および[よく使う分岐命令](../essence/ccr.html#よく使う分岐命令)も参照のこと

* [do-while を用いたループ](#do-while-を用いたループ)
* [if](#if)
* [if-else](#if-else)
* [while](#while)
* [for](#for)

## do-while を用いたループ
```C
int i = 10;
do {
    ...
} while (--i != 0);
```
i -> r0l として考えます。

```ASM
    mov.b   #10,r0l
loop:
    ...
    dec.b   r0l
    bne     loop
```
(そもそもなぜ最初に do-while を持ち出すかといえば、これがいちばんアセンブラで書いたときに覚えやすく短いコードになるからです。)

解説： `dec.b r0l` でループ変数の減算と[CCR](../essence/ccr.html)のセットを行い、ゼロでないときにラベル `loop` に戻っています。
ここで注意するべき点は、`dec.b r0l` と `bne loop` の間に他の命令を入れないことです。(正確には、CCRのゼロフラグを変化させない命令を入れるかCCRを保存しておいて戻すなら可)

考え方は、

1. ループ開始地点にラベルを付ける
2. ループの終わりで終了判定をして、終了していないときはラベルまで戻す

## if
```C
if (a == b) {
  ...
}
```

a -> e0, b -> e1 とすると、

```ASM
    cmp.w   e0,e1
    bne     if_end
    ...
if_end:
```

解説： e0 と e1 の比較命令によって[CCR](../essence/ccr.html)のゼロフラグがセットされるので、等しくないとき `bne if_end` でラベル `if_end` まで飛ばしてやります。

1. **条件を満たしたとき実行させる** のではなく、**条件を満たさなかったときスキップする** と考える
2. スキップする先にラベルを付ける
3. 分岐命令でラベルまで飛ばす
  
## if-else
```C
if (a == b) {
  ...(1)
} else {
  ...(2)
}
```

```ASM
    cmp.w   e0,e1
    bne     if_else
    ...(1)
    bra     if_end
if_else:
    ...(2)
if_end:
```

解説： (1) の命令列の最後に、ifの終了まで無条件分岐させる命令を入れることで、else ブロック(2)が実行されないようにします。
(`bra if_end` は `jmp #if_end` でも可)

1. 条件が満たされたとき、**elseブロックをスキップする**。

## while

```C
while (a == b) {
  ...
}
```

```ASM

while_start:
    cmp.w   e0,e1       ; e0 と e1 を比較して
    bne     while_end   ; 等しくないとき while_end へ飛ぶ
    ...
    bra while_start     ; 無条件で while_start へ飛ぶ
while_end:

```


## for

```C
for (i = 0; i < 10; ++i) {
  ...
}
```

```ASM
    sub.b   r0l,r0l
for_start:
    cmp.b   #10,r0l     ; A = r0h, B = 10
    bge     for_end     ; IF A >= B THEN GOTO for_end
    ...
    inc.b   r0l
    bra     for_start
for_end:
```
