---
date: "2026-11-16T17:06:10+08:00"
draft: false
title: "The F Word Theorem"
description: "如何用數學符號搞出一個很奇怪的定理"
categories: ["Math"]
tags: ["高等微積分", "拓樸學", "advanced-calculus", "topology", "math"]
series: ["奇怪的數學"]
---

# 連續函數的定理？

## 連續性的定義（簡單版本）

在大一微積分中，我們是如何定義一個實數函數是連續的？

### 一維的情況

設$f:[a,b]\to\mathbb{R}$是一個函數，且$x_0\in[a,b]$，其中$a<b$。
我們說$f$在$x_0$處連續，意思就是當$x$靠近$x_0$，$f(x)$的極限值會恰好等於函數值$f(x_0)$

$$
\lim_{x\to x_0}f(x)=f(x_0)
$$

或者是以$\varepsilon-\delta$的語言寫出來的話

$$
\forall\varepsilon>0,\exists\delta>0,\text{s.t. } \forall x,0<|x-x_0|<\delta\implies|f(x)-f(x_0)|<\varepsilon
$$

### 高維度的情況

設$f:\mathbb{R}^n\to\mathbb{R}$是一個函數，且$\mathbf{x}_0\in\mathbb{R}^n$，其中$n$是正整數。
我們說$f$在$\mathbf{x}_0$處連續，意思一樣是當$\mathbf{x}\in\mathbb{R}^n$靠近$\mathbf{x}_0$時，$f(\mathbf{x})$的極限值會恰好等於函數值$f(\mathbf{x}_0)$

以$\varepsilon-\delta$的語言寫出來的話

$$
\forall\varepsilon>0,\exists\delta>0,\text{s.t. } 0<|\mathbf{x}-\mathbf{x}_0|<\delta\implies|f(\mathbf{x})-f(\mathbf{x}_0)|<\varepsilon
$$

其中，若$\mathbf{x}=(x_1,x_2,\dots,x_n)\in\mathbb{R}^n$，那麼$|\mathbf{x}|$的定義為

$$
|\mathbf{x}|=\left(\sum_{k=1}^nx_k^2\right)^{1/2}
$$

以及若$\mathbf{y}=(y_1,\dots,y_n)\in\mathbb{R}^n$，那麼$\mathbf{x}-\mathbf{y}$就定義為$(x_1-y_1,\dots,x_n-y_n)$。

> 當然了，對應域是$\mathbb{R}^m$的情況也行

### 抽象化？

我們需要描述$x$靠近$x_0$跟$f(x)$靠近$f(x_0)$的意思，於是，我們需要有一個『距離』。
這邊有發現到一件事情：就是我們從一維到高維度，好像只有改變了距離的定義，極限的定義完全就是照抄過去

一維預設的距離是：$|x|$，我們想問的是有沒有辦法把這個距離抽象化：也就是說，有沒有辦法用一個抽象的距離函數來定義極限？

為何需要這麼做？因為有的時候，兩點間的距離不一定只能是歐幾理得距離，像是在實際情況中，有人會使用[Taxicab geometry - Wikipedia](https://en.wikipedia.org/wiki/Taxicab_geometry)，這個就不是歐幾理得距離，但我們一樣能夠定義什麼叫做極限，就是透過將距離的概念抽象化，將其性質整理出來，並且定義極限。

我們可以再將極限的定義優化一些，改成集合論的語言來描述：

$$
\forall\varepsilon>0,\exists\delta>0,\text{s.t. }\mathbf{x}\in B_\delta(\mathbf{x}_0)-\{\mathbf{x}_0\}\implies f(\mathbf{x})\in B_\varepsilon(f(\mathbf{x}_0))
$$

或者，更簡潔

$$
\forall\varepsilon>0,\exists\delta>0,\text{s.t. }f(B_\delta(\mathbf{x}_0)-\{\mathbf{x}_0\})\subseteq B_\varepsilon(f(\mathbf{x}_0))
$$

其中，給定$r>0$以及$\mathbf{c}\in\mathbb{R}^n$，我們定義

$$
B_r(\mathbf{c})=\{\mathbf{x}\in\mathbb{R}^n:|\mathbf{x}-\mathbf{c}|<r\}
$$

藉由集合論語言，我們基本上已經把距離的概念漸漸淡化，考慮連續性的時候，不需要特別的每次都把距離標示出來

抽象化最重要的步驟，就是把距離的性質提取出來，以下給出距離需要有的三個性質：

1. 半正定性，且自己與自己的距離是$0$，且只有自己與自己的距離是$0$
2. $x$看$y$的距離應該等於$y$看$x$的距離
3. 三角不等式

有了這些性質，我們就可以來定義什麼叫做距離空間：

## 距離空間

設$X$是一個非空集合，且$d:X\times X\to \mathbb{R}$是一個函數，滿足對任意$x,y,z\in X$：

1. $d(x,y)\ge 0$, $d(x,y)=0$ 當且僅當 $x=y$
2. $d(x,y)=d(y,x)$
3. $d(x,z)+d(z,y)\le d(x,y)$

則我們說$d$是$X$上的一個*距離(metric)*，且將序對$(X,d)$稱為*距離空間(metric space)*，通常寫為$X$。

### 範例

好懶喔可以跳過嗎

## 連續性（距離空間版本）

設$X$跟$Y$都是距離空間，且$f:X\to Y$是一個函數，以$d_X,d_Y$分別表示$X$與$Y$上的距離

我們說$f$在$x_0\in X$連續，意思就是

$$
\forall\varepsilon>0,\exists\delta>0,\text{s.t. }\forall x,0<d_X(x,x_0)<\delta\implies d_Y(f(x),f(x_0))<\varepsilon
$$

若$E\subset X$，則我們說$f$在$E$上連續如果對任意$x_0\in E$，$f$都在$x_0$處連續。

## 一些由距離空間生成的拓樸的名詞定義

設$X$是一個距離空間。給定$r>0$以及$x\in X$，我們說關於$x$的一個半徑為$r$的球就是：

$$
B_r(x):=\{y\in X:d(x,y)<r\}
$$

- 我們說集合$U\subset X$是開集合，如果對任意的$x\in U$，我們都可以找到一個半徑$r$，使得以$x$為球心，半徑$r$的球，完全落在$U$中，也就是說：

$$
\forall x\in U,\exists r>0,\text{s.t. }B_r(x)\subset U
$$

- 我們說集合$F\subset X$是閉集合，如果$X-F$是開集合。
- 給定集合$A\subset X$，我們說集合族$\{U_i\}_{i\in I}$是$A$的一個開覆蓋的意思就是$U_i$都是開集，並且所有$U_i$的聯集包含$A$
- 我們說集合$K$是緊緻的，如果對於$K$的任意一個開覆蓋$\{U_i\}_{i\in I}$，都能找到有限個$U_{i_1},U_{i_2},\dots,U_{i_k},i_j\in I$，使得$\{U_{i_j}\}_{j\in\{1,2,\dots,k\}}$還是$K$的開覆蓋。
- 我們說集合$A$是不連通的，如果可以找到兩個非空開集合$U$和$V$，滿足
  - $U\cup V=A$
  - $U\cap V=\varnothing$
- 我們說集合$C$是連通的，如果$C$不是不連通的

### 一些定理

設$X$是距離空間

1. 任意給定$x\in X$以及$r>0$，$B_r(x)$都是開集合
2. 開集合的任意聯集都是開集合
3. 開集合的有限交集都是開集合
4. 緊緻集的閉子集都是緊緻集

## F word定理

設$X$跟$Y$都是距離空間，且$f:X\to Y$是一個連續函數，那麼以下敘述成立：

1. 對於任意$Y$中的閉集合$F$，$f^{-1}(F)$是$X$中的閉集合
2. 對於任意$Y$中的開集合$U$，$f^{-1}(U)$是$X$中的開集合
3. 對於任意$X$中的連通集$C$，$f(C)$是$Y$中的連通集
4. 對於任意$X$中的緊緻集$K$，$f(K)$是$Y$中的緊緻集

> 符號特別排列成這樣，所以稱為F word定理

> 1. 2. 跟連續性等價。3. 4. 需要加條件才能等價

### 證明

證明連續性跟2.是等價的之後，再利用3來證明其他的

{{<collapse summary="連續性推得2">}}

假設$f$是連續的，且設$U\subset Y$是開集合。

- 若$x_0\in f^{-1}(U)$，那麼根據定義，$f(x_0)\in U$，也就存在$r>0$，使得$B_r(f(x_0))\subset U$。
- 由$f$在$f^{-1}(U)$上連續，存在$\delta_r>0$，使得只要$x\in B_{\delta_r}(x_0)$，皆有$f(x)\in B_r(f(x_0))\subset U$，也就是說，$f(B_{\delta_r}(x_0))\subset U$。
- 對任意函數$g:A\to B$，若$E\subset A$，則$g^{-1}(g(E))\supset E$
- 對任意函數$g:A\to B$，若$E_1\subset E_2\subset B$，則$g^{-1}(E_1)\subset g^{-1}(E_2)\subset A$

因此，

$$
f^{-1}(U)\supset f^{-1}(f(B_{\delta_r}(x_0)))\supset B_{\delta_r}(x_0)
$$

也就是說我們真的找到一個半徑$\delta_r$，使得若以$x_0$為球心，$\delta_r$為半徑，都有$B_{\delta_r}(x_0)$包含在$f^{-1}(U)$裡面，根據定義$f^{-1}(U)$就是開集合。

{{</collapse>}}

{{<collapse summary="2推得連續性">}}
假設任意$Y$中開集合$U$，我們都能推得$f^{-1}(U)$是$X$中的開集合，並且設$x_0\in X$。此時給定$\varepsilon>0$，我們就知道

- $U:=B_\varepsilon(f(x_0))$是開集合
- 根據假設$f^{-1}(U)$就是開集合

可以發現，$x_0\in f^{-1}(U)$， 因為根據定義$f(x_0)\in U$，於是，存在$\delta>0$，使得$B_\delta(x_0)\subset f^{-1}(U)$。

由定理：對任意函數$g:A\to B$，只要$E\subset B$，則$g(g^{-1}(E))\subset E$，可知$f(B_\delta(x_0))\subset f(f^{-1}(U))\subset U=B_\varepsilon(f(x_0))$
意思就是只要$x\in B_\delta(x_0)$，則$f(x)\in B_\varepsilon(f(x_0))$。因為$x_0$是任取的，所以根據定義，$f$在$X$上連續。

{{</collapse>}}

---

{{<collapse summary="2推得1">}}

假設任意$Y$中開集合$U$，我們都能推得$f^{-1}(U)$是$X$中的開集合。因為$Y-U$是閉集合，且

$$
f^{-1}(Y-U)=f^{-1}(Y)-f^{-1}(U)
$$

因此$f^{-1}(Y-U)$是閉集合。

{{</collapse>}}

而反過來的證明，只需要將開集合換成閉集合，閉集合換成開集合。

---

{{<collapse summary="2推得3">}}

假設任意$Y$中開集合$U$，我們都能推得$f^{-1}(U)$是$X$中的開集合。假設$C\subset X$是連通集合，顯然的，$C=\varnothing$則$f(C)=\varnothing$是連通集合。如果$C\neq\varnothing$，並且$f(C)$不是連通集合，那麼就存在兩個非空開集合$U,V$，使得$U\cup V=f(C)$以及$U\cap V=\varnothing$。

由於$U$和$V$都是$f(C)$的非空子集合，$f^{-1}(U)$以及$f^{-1}(V)$皆非空，此時，令

$$
U':=f^{-1}(U)\cap C,V':=f^{-1}(V)\cap C
$$

若$U'$是空集合，也就是$f^{-1}(U)\cap C=\varnothing$，那麼任取$x\in f^{-1}(U)\neq\varnothing$，$f(x)\in U\subset f(C)$，因此$x\in C$，矛盾。

$$
U'\cup V'=(f^{-1}(U)\cup f^{-1}(V))\cap C
$$

且

$$
f^{-1}(U)\cup f^{-1}(V)=f^{-1}(U\cup V)=f^{-1}(f(C))\supset C
$$

可得$U'\cup V'=C$。

又

$$
U'\cap V'=C\cap f^{-1}(U)\cap f^{-1}(V)=C\cap f^{-1}(U\cap V)=C\cap f^{-1}(\varnothing)=\varnothing
$$

所以$C$是不連通的，矛盾。因此$f(C)$必須是連通的。

{{</collapse>}}

---
{{<collapse summary="2推得4">}}

假設任意$Y$中開集合$U$，我們都能推得$f^{-1}(U)$是$X$中的開集合。設$K$是$X$中的一個緊緻集合，並且$\{U_i\}_{i\in I}$是$f(K)$的一個開覆蓋。因為

$$
K\subset f^{-1}(f(K))\subset f^{-1}\left(\bigcup_{i\in I}(U_i)\right)=\bigcup_{i\in I}f^{-1}(U_i)
$$

並且$U_i$都是開集合，所以$f^{-1}(U_i)$也都是開集合，也就是說$\{f^{-1}(U_i)\}_{i\in I}$是$K$的一個開覆蓋，由於$K$是緊緻集，所以存在$i_1,\dots,i_k\in I$，使得

$$
K\subset\bigcup_{j=1}^{k}f^{-1}(U_{i_j})
$$

所以，我們有

$$
f(K)\subset f\left(\bigcup_{j=1}^kf^{-1}(U_{i_j})\right)=\bigcup_{j=1}^kf(f^{-1}(U_{i_j}))\subset\bigcup_{j=1}^kU_{i_j}
$$

也就是說，$U_{i_j},j=1,\dots,k$就會是一組$f(K)$的有限開覆蓋，所以$f(K)$是緊緻的。

{{</collapse>}}
