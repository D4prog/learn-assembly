---
layout: assembly
---
<style>.reg rect,.reg line,.reg path{ fill: rgba(1,1,1,0.1); stroke: black; }
  rect:hover{fill: rgba(255,255,0,0.2); }</style>
# レジスタ
ここでは、H8/300H シリーズのレジスタの構成と使用法について説明する

## レジスタのビット長と使用ビットに対する表記
32ビットの汎用レジスタ8本のどのレジスタの、どのビットをどう指定するか

| ビット長 | コード中での表記 |使用ビット|
|---------:|:------:|:--------:|
| 32ビット | `er`*n*|32ビット全体|
| 16ビット | `e`*n* |32ビット中の上位16ビット|
| 16ビット | `r`*n* |32ビット中の下位16ビット|
|  8ビット | `r`*n*`h`|下位16ビット中の上位8ビット|
|  8ビット | `r`*n*`l`|下位16ビット中の下位8ビット|

*n* は 0 から 7 までのレジスタの番号で、`er7` は push-pop 命令のためのスタックポインタとして使用するため、プログラム中では使用しない。

レジスタの上位16ビットを8ビットずつ分けてアクセスする表記(例:`e0l`)は存在しない

<svg x="0px" y="0px" width="650px" height="351px" viewBox="0 0 500 270" class="reg">
  <g transform="scale(1,0.6) rotate(30,0,0) translate(50,0)">
    <rect class="er l" x="5" y="5" width="480px" height="40px" title="er0: 32bit"/>
    <text x="245" y="30" text-anchor="middle">er0</text>
    <path d="M 5 5 Q 245 -60 485 5 Q 245 -60 5 5" stroke-dasharray="8"/>
    <text x="245" y="-35" font-size="14" text-anchor="middle">32bit</text>
  </g>
  <g transform="translate(0,30) scale(1,0.6) rotate(30,0,0) translate(50,0)">
    <rect class="e w"  x="5" y="5" width="240px" height="40px" title="e0: 16bit(H)"/>
    <rect class="r w"  x="245" y="5" width="240px" height="40px" title="r0: 16bit(L)"/>
    <text x="125" y="30" text-anchor="middle">e0</text>
    <text x="365" y="30" text-anchor="middle">r0</text>
  </g>
  <g transform="translate(0,60) scale(1,0.6) rotate(30,0,0) translate(50,0)">
    <rect class="rh b" x="245" y="5" width="120px" height="40px" title="r0h: 8bit(H)"/>
    <rect class="rl b" x="365" y="5" width="120px" height="40px" title="r0l: 8bit(L)"/>
    <text x="305" y="30" text-anchor="middle">r0h</text>
    <text x="425" y="30" text-anchor="middle">r0l</text>
  </g>
  <g stroke-dasharray="2">
    <line x1="25" y1="40" x2="25" y2="70"/>
    <line x1="460" y1="165" x2="460" y2="225"/>
    <line x1="233" y1="141" x2="233" y2="170"/>
  </g>
</svg>

上図は汎用レジスタ1本の、重なっている様子を表現したもの。これが8本ある。


## 注意
`e1` や `er2` 等、表記は多いが、実際のレジスタは8本しかなく、例えば `er0` と `r0h` は同じレジスタの異なる大きさの範囲を示しているにすぎない。よって、`e0` と `r0` の組み合わせや `r0l` と `r0h` と `e0` 等は同時に使用出来るが、例えば `e0` に値が入っているときに `er0` に値を代入したとき、もとの `e0` の値は失われる。
