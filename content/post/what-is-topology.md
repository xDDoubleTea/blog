---
date: "2026-11-26T16:19:38+08:00"
draft: false
title: "拓樸是什麼？（1）"
description: "介紹基本的拓樸學"
# cover:
#   image: ""
#   alt: ""
categories: ["Math"]
tags: ["math", "topology"]
series: ["Topology"]
---

# 什麼是拓樸？

科普文章或影片中，常常介紹拓樸的概念是討論形狀跟形狀之間的關係，還有給一些例子，例如說甜甜圈與馬克杯在拓樸學上是等價的，理由是能夠透過「不把物體拆開的形變」來把甜甜圈變為馬克杯。可是什麼叫做「不把物體拆開的形變」？為什麼這樣就是一種等價？

我並沒有要否定這些科普文章或影片，畢竟他們並沒有錯，但是他們隱藏了太多數學，以至於這些科普看起來講了什麼但又好像沒講。

在這篇文章中，我想介紹拓樸的數學定義是什麼，還有一些簡單的範例。

# 拓樸

給定非空集合$X$，我們說$\mathcal{T}$是$X$上的一個*拓樸*，如果$\mathcal{T}$中的成員都是$X$中的子集合，並且滿足：

1. $\varnothing,X\in\mathcal{T}$
2. 任意多個$U_i\in\mathcal{T},i\in I$，他們的聯集$\bigcup_{i\in I}U_i$一樣落在$\mathcal{T}$中，其中$I$是任意的指標集合
3. 有限多個$U_1,U_2,\dots,U_n\in\mathcal{T}$，他們的交集$\bigcap_{i=1}^n$一樣落在$\mathcal{T}$中

我們說集合$U$是$X$上的*開集合*，如果$U\in\mathcal{T}$。我們稱$(X,\mathcal{T})$是*拓樸空間*，並通常只記為$X$。

我們說$U$不是開集合，如果$U\not\in\mathcal{T}$。我們說$F$是閉集合，如果$X-F$是開集合；我們說$F$不是閉集合，如果$X-F$不是開集合。

**定理**：$U$不是開的等價於存在$x\in U$，使得每個包含$x$之開集合$V$，都有$U$以外的點，或者，等價地說，$(X-U)\cap V\neq\varnothing$。

{{<collapse summary="證明">}}
我們可以證明以下敘述：對所有的$x\in U$，都存在包含$x$之開集合$V$，使得$(X-U)\cap V=\varnothing$，等價於$U$就是開集合。把兩邊都做否定，就會得到我們的定理。

如果$U$是開集合，那麼，對於任意$x\in U$以及任意包含$x$之開集合$V'$，我們都可以取$V:=V'\cap U$，這樣一來，$V$就會完全包含在$U$中，且還是包含$x$。

反過來說，若對於所有的$x\in U$，都能夠找到包含$x$之開集合$V_x$，使得$(X-U)\cap V=\varnothing$，那麼，$\bigcup_{x\in X}V_x$就會等於$U$，且由於開集合的任意聯集都還是開集合，$U$自己就是一個開集合。
{{</collapse>}}

## 範例

給定距離空間$X$，設定$\mathcal{T}$是$X$上的所有在距離空間定義的「開集合」的集合。那麼，$\mathcal{T}$是一個拓樸空間。

1. 根據定義$\varnothing$就是一個開集合，因為你找不到元素$x\in\varnothing$違反$\exists r>0$使得$B_r(x)\not\subset X$。$X$也會是一個開集合，因為$B_r(x)$之定義只有收集$X$中所有滿足距離和$x$小於$r$的元素，所以任給$x\in X$，隨便取一個$r>0$，$B_r(x)\subset X$。
2. 假設$\{U_i\}_{i\in I}$是一些開集合，並且假設$x\in\bigcup_{i\in I}U_i$，那麼，存在某些$i\in I$，使得$x\in U_i$。由於$U_i$也是開集合，所以，存在$r>0$使得$B_r(x)\subset U_i$。因為$U_i$本來就是$\bigcup_{i\in I}U_i$的子集合，所以$B_r(x)\subset\bigcup_{i\in I}U_i$。
3. 給定有限多個開集合$U_1,U_2\dots,U_n\subset X$，並且假設$x\in\bigcap_{i=1}^n U_i$，那麼，對所有$1,2,\dots,n$，$x\in U_i$，也就存在$n$個正數$r_1,r_2,\dots,r_n$，使得

$$
B_{r_i}(x)\subset U_i,i=1,2,\dots,n
$$

所以，兩邊取交集得到

$$
x\in\bigcap_{i=1}^nB_{r_i}(x)\subset\bigcap_{i=1}^nU_i
$$

但是，$\bigcap_{i=1}^nB_{r_i}(x)$實際上是一堆同心球，所以，他們的交集實際上就完全等於半徑最小的那顆球，也就是說，$\bigcap_{i=1}^nB_{r_i}(x)=B_{r_k}(x)$，其中$k$是使得$r_k$是在$r_1,r_2,\dots,r_n$中最小的指標。$k$的存在性是被自然數的良序原理所保證。

> 為何需要有限多個？因為這樣才可以使用自然數的良序原理。

那如果無限多個開集合，他們的交集還會是開集合嗎？

$X=\mathbb{R}$，且因為我們剛剛證明了距離就可以推得拓樸，所以，$\mathbb{R}$上的任意開區間都是一個開集合。

如果取$U_n:=(-\frac{1}{n},\frac{1}{n}),n\in\mathbb{N}$，那麼，他們所有的交集就只是$U:=\{0\}$，至於為什麼$U$不是開集合，我們只能先用距離空間對於開集合的定義：

$$
\forall x\in U,\exists r>0,\text{s.t. } B_r(x)=(x-r,x+r)\subset U
$$

唯一需要驗的$x$就是$0$，任意$(-r,r)$顯然都不可能完全包含在$U$中，因為$0<\frac{r}{2}\in(-r,r)$。因此$U$不是開集合。

可是，我們用的是距離空間的定義，我們有沒有辦法只用拓樸空間的定義，也就是說，如果我們想要證明$U$不是開集合，我們要證明任意包含$0$的開集合$V$，都不完全包含在$U$裡面。

## 拓樸的基底

要是給定一個$\mathcal{T}\subset\mathcal{P}(X)$，但我們還不知道他是不是拓樸，這時候我們得要檢驗$\mathcal{T}$的成員是不是都滿足三條定義，但這樣我們基本上就必須要檢驗每個集合。

剛剛的範例，我們想要要說明的是$\{0\}$不是開集合，也就是在拓樸必須證明任意包含$0$的開集合$V$，都不完全包含在$U$裡面。我們就得要驗所有的開集合，可是，我們一開始其實只定義了某些集合，也就是開球是開集合，從而得到一些額外的開集合，例如在$\mathbb{R}$上配上絕對值所形成的拓樸，我們可以知道，$(0,1)\cup(2,3)$也應該要是一個開集合，但這是因為我們一開始定義的「所有開區間都是開集合」而產生的開集合，所以，如果只檢驗開區間，在邏輯上來說是不能在拓樸上證明$\{0\}$不是開集合的。但是為何在距離空間上，我們只需要檢驗開球，就能夠判斷某個集合是不是開集合呢？

我們一開始給的開集合只有開區間，但我們利用這些開區間生出了一個拓樸空間，所以，這些開區間有一種「基底」的想法在裡面。準確的說，只需要定義某些集合是開集合，也許就能夠把整個拓樸空間生成出來。

_定義_

設$X$為非空集合，我們說$\mathcal{B}$是$X$的*拓樸基底*，如果$\mathcal{B}$中的成員是由$X$中的開集合所組成，且

1. $\bigcup_{B\in\mathcal{B}}B=X$
2. 給定兩開集$B_1,B_2\in\mathcal{B}$，對任意$x\in B_1\cap B_2$，都存在$B_3\in\mathcal{B}$，使得$x\in B_3\subset B_1\cap B_2$。

**定理**

設$X$是一個非空集合，並且$\mathcal{B}$是$X$的拓樸基底。考慮所有的集合$U\subset X$使得

$$
\forall x\in U,\exists B\in\mathcal{B},\text{s.t. }x\in B\subset U
$$

那麼，所有的$U$收集起來的集合$\mathcal{T}$就會是$X$上的一個拓樸。我們稱$\mathcal{T}$為由$\mathcal{B}$所生出來的拓樸。

也就是說：

$$
\mathcal{T}=\left\{U\subset X\ \Bigg{|}\ \forall x\in U,\exists B\in\mathcal{B},\text{s.t. }x\in B\subset U\right\}
$$

> 注意到這條定理跟距離空間所要求的開集合條件很像，都是需要有一個開球，或者在這裡是基底的成員，使得基底還在原本給的開集合內。

{{<collapse summary="證明">}}

沒有辦法找到$x\in\varnothing$使得條件不成立，所以，$\varnothing\in\mathcal{T}$。

因為所有的基底元素聯集就是$X$，自然的，$X\in\mathcal{T}$。

---

接下來我們要證明任意滿足條件的集合的任意聯集還是會滿足條件：

給定一些滿足條件的$U_i,i\in I$，並且令$U:=\bigcup_{i\in I}U_i$，給定$x\in U$，那麼，就存在某些$i_j\in I$使得$x\in U_i$，其中$j\in J$。
此時，就會對應的存在$B_{i_j}\subset U_{i_j}$使得$x\in B_{i_j}$。所以，$\bigcup_{j\in J}B_{i_j}\subset \bigcup_{j\in J}U_{i_j}\subset U$，因此$U$也滿足條件，得到$U\in\mathcal{T}$。

---

接下來我們要證明滿足條件的有限多個集合之交集還是滿足條件：

首先，若我們可以證明給定兩個集合的情況是對的，那麼可以根據數學歸納法，當$n$需要推論$n+1$的時候，將前$n$個組一起和第$n+1$個當成兩個集合，再用兩個集合的情況，就可以證明完畢了。

給定$U_1,U_2\in\mathcal{T}$，設$x\in U_1\cap U_2$，那麼，就存在$B_1,B_2$，使得$x\in B_1\subset U_1$以及$x\in B_2\subset U_2$。
別忘記$\mathcal{B}$本身是基底，所以，存在$B_3$使得$x\in B_3\subset B_1\cap B_2\subset U_1\cap U_2$。因此，$U_1\cap U_2\in\mathcal{T}$。

假設對於某個$n\in\mathbb{N}$，$U_1,U_2,\dots,U_n\in\mathcal{T}$，我們都有$\bigcap_{i=1}^{n}U_i\in\mathcal{T}$，則對於$n+1$的情況：

令$V_1:=\bigcap_{i=1}^nU_i,V_2=U_{n+1}$，並使用$n=2$的證明，那就得到$V_1\cap V_2=\bigcap_{i=1}^{n+1}U_i\in\mathcal{T}$，所以根據數學歸納法，我們就可以知道對於有限個集合$U_1,U_2,\dots,U_n$而言，他們的交集還是會滿足條件。

{{</collapse>}}

---

如果我們設$X=\mathbb{R}$，以及規定以下的$(a,b)$是指開區間，還有$\mathcal{B}$定義為：

$$
\mathcal{B}:=\{(a,b):a,b\in\mathbb{R}\}
$$

那麼，$\mathcal{B}$就會是$X$的拓樸基底。可以發現，我們目前完全捨棄了距離的概念，實際上只需要實數的排序而已。

{{<collapse summary="證明$\mathcal{B}$是$X$的基底">}}

顯然地：

$$
\left(\bigcup_{a<0}(a,1)\right)\cup\left(\bigcup_{b>0}(-1,b)\right)=\mathbb{R}
$$

設$B_1,B_2\in\mathcal{B}$，那麼$B_1=(a_1,b_1),B_2=(a_2,b_2)$，其中$a_1,a_2,b_1,b_2\in\mathbb{R}$。若$B_1\cap B_2$是空集合，那就不會有元素$x$違反第二條，所以此情況證明完畢。

若$B_1\cap B_2\neq\varnothing$，不失一般性的假設$a_1<a_2$，那麼必然表示$a_2<b_1$，然後就會有兩種情況：

1. $b_1<b_2$
2. $b_1\ge b_2$

第一種情況能夠推論$a_1<a_2<b_1<b_2$，並且$B_1\cap B_2=(a_2,b_1)$，所以，對任意$x\in B_1\cap B_2$，直接取$B_3:=(a_2,b_1)$，那麼，$B_3\in\mathcal{B}$且$x\in B_3\subset B_1\cap B_2$。

第二種情況能夠推論$a_1<a_2<b_2\le b_1$，並且$B_1\cap B_2=(a_2,b_2)$，所以，對任意$x\in B_1\cap B_2$，取$B_3=B_2=(a_2,b_2)$，那麼，$B_3\in\mathcal{B}$且$x\in B_3\subset B_1\cap B_2$。

因此，$\mathcal{B}$就會是$X$的基底。

{{</collapse>}}

既然$\mathcal{B}$是$X$的基底，以它生出來的拓樸和我們利用絕對值在實數上所定義的距離空間的拓樸是一樣的嗎？也就是說，任意以$\mathcal{B}$所生成出來的$\mathcal{T}$裡面的開集合，和距離空間所描述的所有開集合，是一樣的嗎？答案是肯定的。$\mathcal{T}$在此情況下，就是

$$
\mathcal{T}=\left\{U\subset\mathbb{R}\ \Bigg{|}\ \forall x\in U,\exists (a,b)\in\mathbb{B},\text{s.t. }x\in (a,b)\subset U\right\}
$$

跟由距離空間所定義的開集合所生出的拓樸$\mathcal{T}'$：$U$是$\mathbb{R}$的開集合，如果對所有$x\in U$，都存在$r>0$使得$(x-r,x+r)\subset U$會是一樣的：

$$
\mathcal{T}'=\left\{U\subset\mathbb{R}\ \Bigg{|}\ \forall x\in U,\exists r>0,\text{s.t. }x\in(x-r,x+r)\subset U\right\}
$$

所以，我們需要證明$\mathcal{T}=\mathcal{T}'$。

{{<collapse summary="證明">}}
設$U\in\mathcal{T}$，此時若給定$x\in U$，那麼，就存在$a,b\in\mathbb{R}$，使得$x\in(a,b)\subset U$。我們可以取

$$
r=\min(\frac{b-x}{2},\frac{x-a}{2})
$$

那麼，$x\in(x-r,x+r)\subset (a,b)\subset U$，所以，$U\in\mathcal{T}'$。

設$U\in\mathcal{T}'$，給定$x\in U$，那麼存在$r>0$，使得$x\in(x-r,x+r)\subset U$。所以，直接取$a=x-r,b=x+r$就結束了，得到$U\in\mathcal{T}$。

{{</collapse>}}

這就是為什麼我們在距離空間上定義開集合的時候，我們只在意開球，因為幾乎一模一樣的證明能夠被套用：在距離空間中，若以所有的開球作為基底，那麼，開集合$U$的定義等價於$\forall x\in U,\exists r>0,\text{s.t. }B_r(x)\subset U$。
