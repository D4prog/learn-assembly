---
layout: assembly
---
# 配列
C言語においての配列と同じように、メモリ上に連続した領域を予約し、先頭アドレスとそこからの差で表現します。

rept-endr擬似命令を使うことで同じ内容の行を大量に書かずにすみます。

ローカル変数での配列は考えません。

```C
#define ARRAY_SIZE 16
int global_array[ARRAY_SIZE];
char buffer[41];

void init(void) {
    buffer[40] = '\0';
}
```

```
    .equ    ARRAY_SIZE, 16

    .section    .text
init:
    mov.b   #7, r0l
    mov.b   r0l,    #buffer+40
    rts
    
    .section    .bss
global_array:
    .rept   ARRAY_SIZE
        .long   0
    .endr
buffer:
    .rept   41
        .byte   0
    .endr
```