---
date: "2026-11-15T19:11:57+08:00"
draft: false
title: "116交大資工丁組推甄紀錄"
description: "116 交大資工丁組推甄：備審、性向測驗、面試和備取紀錄"
cover:
  image: "https://blog-assets.hoshiyomi-dev.work/nycu-admi-post-cover.png"
  alt: "交大碩士班甄試入學網頁"
categories: ["Admission"]
tags: ["推甄", "nycu", "紀錄"]
series: ["研究所推甄"]
---

# 背景

清大數學系輔資工系，GPA班排42%，但因為丁組完全不看成績，所以基本沒差。

因為我本來完全沒有想要推甄，但我在今年`10/1`，也就是交大開始報名推甄的當天想說還是來推看看好了，打開交大推甄網頁發現今天就是報名開始日💀，我推薦信、備審、要推啥所、簡章全部都沒有看過，所以做的非常趕

## 競賽經歷

{{<collapse summary="Very cool">}}
<center>404 Not Found</center>
<hr><center>nginx</center>
{{</collapse>}}

## 專題

{{<collapse summary="Very cool">}}
<center>404 Not Found</center>
<hr><center>nginx</center>
{{</collapse>}}

至少這個部分不是學校的，完全是自己的side project，做的時候完全沒有想說要推甄啥的，好玩而已

## 檢定

{{<collapse summary="Very cool">}}
<center>404 Not Found</center>
<hr><center>nginx</center>
{{</collapse>}}

## 資工修課

- 計算機程式設計一、二（資工系）
- 資料結構
- 資料結構導論
- 計算機結構
- 邏輯設計（電機系）
- Python程式設計（資工系）

數學系線性代數跟離散數學必修

## 備審寫了啥

其實我全部都是用`Claude Code`生成的，只是說我有特別指定要使用我的 $\LaTeX$ 模版，條列式的重點而不是一大坨字，用`tikz`畫圖，而且改了挺多次（commit只有39次但也不是每次更改都會commit所以肯定更多）

![commit history](https://blog-assets.hoshiyomi-dev.work/nycu-admission-document-commit-history.png)

啊丁組戊組基本用同一個作品集，只改一點點，在丁組可以拿89，戊組拿79，說明肯定有在看

聯招就別提了我沒有競賽沒有證書啥的才是大問題

### 自傳

有提到的點：

- `Ubuntu Server` Home lab管理2年經驗，有`Netbird`、 `NextCloud aio`、 `Local LLM`架構、`nginx as reverse proxy`、以及初步`Observability`架設（在推甄邊生備審邊做的）
- 開發[客服機器人](https://github.com/xDDoubleTea/DragonBot)
- 開發[LeetCodeBot](https://github.com/xDDoubleTea/LeetCodeBot)
- NTHUMods[開源專案貢獻](https://github.com/nthumodifications/courseweb/pulls?q=is:pr+author:xDDoubleTea+is:merged)
- 奇怪的東西[kcalc](https://github.com/xDDoubleTea/kcalc)
- 正在修計算機系統管理，教`FreeBSD`以及如何維護伺服器運作，架網頁跟服務

### 讀書計畫

基本上就是畫大餅，沒啥好說

### 作品集

就是把在自傳有提到的講細節一點，比較著重於：發現問題，如何搜尋解決方案並解決問題，tradeoffs跟一些debug紀錄

## 面試流程

### 第一天

1. 早上8:50開始性向測驗到12點，考古題基本沒用，其實也不算分，你要真的有經驗才能寫得出東西來，基本上不可能在一兩個月就準備所有的東西
2. 午餐交大麥當勞（頂尖新竹美食）
3. 下午學長面試關，一人10分鐘，我排第二個，據說之前是兩關，現在剩一關

### 第二天

排第18個，每個人五分鐘，中間的是管系計中的謝秉均教授，其他兩位不認識

[Ping-Chun Hsieh (謝秉均)](https://pinghsieh.github.io/)

> 不知道5分鐘要面啥鬼東西就是了

## 性向測驗是啥

今年第一年改成線上，問我怎麼知道的？

1. 今年性向測驗教室改了，而且是改成電腦教室，之前的（113~115）查了一下不是電腦教室
2. 線上系統只有一年的考卷（也許會刪掉前面的但看看明年吧，但第一點應該就足夠說明了）

是中文，字很多，給情境，要你盡量從你的經驗出發去回答

> 我是土生土長的台灣人沒錯，可惜我中文現在不好，我基本上所有軟體工程跟`Linux`都是看原文，例如`arch wiki`，官方文件直接學的，鳥哥啥的沒在看但知道

> 我跟`LLM`對話也都是直接英文，習慣了，為什麼我要用中文打這篇其實我也不知道，大概是我覺得可能真的有人想看，怕有人跟我抱怨看不懂吧

涵蓋範圍有簡單的git操作、`agentic LLM`也就是`Claude Code`或`codex`如何結合原有`workflow`、前後端開發、`CI/CD`、資料庫、`k8s`、`Linux`相關、網路相關、`mail`相關，還有問你會怎麼架一個`LLM inference engine`，因為他們最近要搞這個

我記得有題是要我們介紹資安事件，10月跟11月特別多資安事件，例如`php2shell`，`eMudhra CA key compromise`剛好發生，啊我就隨便寫一些東西

總共46題，我只寫了14題，我也忘了有啥了，但反正考古題應該是沒有用，而且也不算分。
本來就不可能寫完，寫完才有鬼！！！你時間差不多三小時，一題就只有4分鐘，你4分鐘可以看完平均每題>1000字的題目，且能夠打幾百字（畢竟要描述你的經驗），那也就是說你中打少說200WPM，且不用思考，切輸入法不用時間，可以完美的寫好markdown，那我就服了好吧

聽說之前會看你性向測驗寫了啥來問你東西，但我感覺我好像跟沒交卷一樣，不知道他們為什麼都問我備審裡面的東西，我在猜可能是因為我是第二個所以他們來不及看我都寫了啥然後能問啥問題

## 面試了問啥

### 第一天的面試

自我介紹炸掉了，我完全無法組織語言能力，想講的太多，結果爆炸，大概就是這邊被扣分吧

問的問題都是從我備審裡面挑的，這個時候我的語言能力變正常了

- 為什麼`vaultwarden`是開在公網的？那不是密碼管理器嗎？
  - 回答：我是用`cloudflare tunnel`來公開的，因為`vaultwarden`需要`https`才能運作，而由於我懶惰就直接用最快的方式：用一個`cloudflare tunnel`。之後會遷回`NetBird`裡面。我有把註冊介面關掉，`master password`也是用`bitwarden`的生成器生的，應該是夠安全
  - 我沒有講到我`Netbird`的密碼也在裡面，但其實我`zen`的密碼管理器也有，所以應該沒差- 我後來才發現我根本不是用`tunnel`，是直接一個`DNS record`指到我伺服器公網`IP`，但有開`proxy`就是。
  - 寫這篇的時候，`vaultwarden`已經在`Netbird`後面，而且全部都有`certbot`弄好`cloudflare`的憑證，都有`https`了
- 如何備份？
- 做LDAP了嗎？（備審有寫我想試試LDAP）不過後來我發現LDAP有夠難搞所以我搞了自架SSO服務，串我的`gitea`，`nextcloud`跟`netbird`
- 你`Uptime Kuma`是不是不能監控歷史資源數據？你會怎麼解決這問題？
- 什麼事情是你最印象深刻的
- `docker compose`有管理上的困難嗎？如果需要高可用跟好維護會用什麼？如何解決目前的單點故障問題？（Homelab 就一台機器跑所有東西）
- 你是數學系吧？你有修資工的課嗎？是輔系資工還是只有修課？

懶得寫我回啥，基本就是照實回答，不會就說不會，不會的話還能學，小問題，不能隨便亂編，這我想有常識的都知道

### 第二天的面試

教授很Chill，就先讓我自我介紹，然後我自我介紹有提到我有架homelab，所以他就先從那問起

這天自我介紹有練過所以沒炸

基本上教授就問我為什麼要搞Homelab，主要原因是架Minecraft伺服器，還有架我的Discord機器人，然後我提了我上面架了啥其他的東西

我有提為何我不用雲服務，其實是因為隱私而且讓我這樣更好設定東西，而且短短一年有幾十次outage，所以才搞Homelab

> 這我在面試的時候沒有提，之前找的雲服務是[Sparked Host - Game Servers For 30+ Games](https://sparkedhost.com/)，我只拿來架Discord機器人，但那短短一年一堆outage所以我就放棄了，直接買一台實體主機當Homelab自己管

教授發現我提的東西好像都是現成的，所以問我有沒有自己寫的，我說有，其實機器人就是我寫的，教授以為是實體的機器人，我說是類似聊天機器人的東西

我應該多提我也有寫一個webapp來管理我架的服務，主要是[Navidrome](https://www.navidrome.org/)這個沒有網頁版上傳音樂或是加入音樂的功能，所以我就做了一個webapp，能夠搜尋YouTube，下載音樂並存到他的路徑底下，會根據誰登入來該存誰的Library

教授還問我跟熟不熟計中在幹嘛，我說我只知道計中是在管伺服器跟維護服務，我不熟的是伺服器間如何進行溝通，所以我說我會模擬多節點的架構，可能加個樹梅派之類的，把一些服務分散掉不要讓他SPOF

## 結果

備取4

![成績單](https://blog-assets.hoshiyomi-dev.work/nycu-admi-post-result.png)

2026-11-15，未開始備取作業

## 檢討

我做備審的時候完全沒有先看招生說明就做了，畢竟我報名開始當天才決定要推甄，到報名結束之後才發現有招生說明這東西💀，而且看起來應該要從他們有啥服務開始下手才對

面試之前其實應該就已經定好誰應該會正取，只是要確定備審寫的都是真的，所以這對我兩個成績都很傷，尤其我第一天又表現不好，結果就只有備取，但至少我備審寫的都是真的才敢備取我

還有個可能扣分原因是我其實還沒修作業系統跟演算法，資工最重要的兩個我沒修還能備取其實也是滿瞎的，我是有畫大餅說下學期會修啦，還有我做了[kcalc](https://github.com/xDDoubleTea/kcalc)，而且日常就用`Linux`，所以其實作業系統的實作方面還有範例我**可能**挺熟悉的，才敢讓我備取

如果備上了大概就跟中樂透一樣吧，沒備上就是延畢一學期，反正可以二月入學，這段期間參加比賽檢定證照啥的，寫這個部落格，公開[LeetCodeBot](https://github.com/xDDoubleTea/LeetCodeBot)，做好流量分析還有`Observability`，然後明年一定要好好的看他們的招生說明會，這樣應該就會進了啦我猜吧🤓

## 之前備取結果表格參考

> 113 報名人數資訊從此篇[Dcard](https://www.dcard.tw/f/graduate_school/p/253576729)得到，其餘113以後（含113）之數字皆為交大官網可得知

| 學年度 | 報名人數 | 複試人數 | 名額                | 備取人數 | 備取至                            |
| ------ | -------- | -------- | ------------------- | -------- | --------------------------------- |
| 116    | 77       | 22       | 直接錄取 2 + 正取 5 | 9        | 尚未開始                          |
| 115    | 76       | 25       | 直接錄取 1 + 正取 6 | 9        | 0                                 |
| 114    | 75       | 31       | 直接錄取 1 + 正取 6 | 10       | 4（備取 3 預先放棄，實際叫 3 人） |
| 113    | 100      | 31       | 直接錄取 2 + 正取 4 | 14       | 0                                 |
| 112    | ?        | ?        | ?                   | ?        | ?                                 |
| 111    | ?        | ?        | ?                   | ?        | 1                                 |
| 110    | ?        | ?        | ?                   | ?        | 至少 2                            |
| 109    | ?        | ?        | ?                   | ?        | ?                                 |
| 108    | ?        | ?        | ?                   | ?        | 至少 1                            |

112 之前的資料已經不在現今的官網上了，最終備取人數是直接爬`wayback machine`的資料，含`cs.nycu.edu.tw`以及`cs.nctu.edu.tw`的網域資訊

111是某篇[Dcard](https://www.dcard.tw/f/graduate_school/p/240486891)說的，看起來可信

## 如何學習跟丁組有關的知識以拿到面試機會呢

> 我進面試了，你問我怎麼進的？我他馬的怎麼會知道？

其實最重要的是興趣，你沒有興趣你根本就不應該來報丁組

參考 [110 陽明交大資工所丁組（實務）推甄心得 | Lin's Blog](https://coder.tw/posts/nycu-cs-master-recommendation-interview/) 所說：

> 我覺得丁組可以準備的範圍，只有把你平常的東西、作品整理成履歷，但如果你平常就沒有相關經歷，是沒辦法經過刻意準備而變成丁組要的人，不可能因為要投丁組才特意去準備 Side Project（嚴格來說那也不算 Side Project 了），那樣的東西就算準備的很漂亮讓面試官錄取你，最後也只是兩敗俱傷，我們找了一個不適合的人，而你也會獲得很痛苦的兩年碩士生活甚至無法畢業。

> 題外話，我架好這個站才知道他也用Hugo+PaperMod的，我不是抄的

假設你有興趣，幾個YouTube頻道推薦，我常看的除了數學以外的頻道：

- [Kevin Fang - YouTube](https://www.youtube.com/@kevinfaang)
- [ThePrimeagen - YouTube](https://www.youtube.com/@ThePrimeagen)
- [Tsoding - YouTube](https://www.youtube.com/@Tsoding)
- [bigboxSWE - YouTube](https://www.youtube.com/@bigboxSWE)

什麼你問我為什麼FireShip不在上面？他現在只會Larp，不想看了

### Kevin Fang

影片基本上就是在介紹丁組做的事情（Software Reliability Engineering SRE）為什麼很重要，因為如果沒有丁組，世界可能真的會毀滅以至於我們全都得回去農耕

含金量很高，知識量很高，梗很多，建議看好幾遍複習整個incident流程，以及怎麼從發現問題到解決問題的流程，關鍵專有名詞聽多了其實就會了

基本上你就Pattern matching到你會為止，指標是可以看懂所有梗為止，把自己當成LLM就對了，你搞不好就有機會進面試了💀

### ThePrimeagen

科技圈新聞來源，雖然是第二手消息，但至少解釋得很好，對於不想看文檔，在吃飯的時候可以看，也會出一些知識型影片

老兄轉用Arch跟hyprland很扯

### Tsoding

頂尖`C/C++`的程式設計解說，沒有什麼`Rust`,`Golang`,`PHP`,`webdev`那些brainrot

老兄用`emacs`，我用`neovim`，但我還是看得很爽

但有些真的在Larp的就會掀起戰爭，建議可以看留言區增加無用小知識

### bigboxSWE

基本上是在教你如何成為軟體工程師，也是如果你可以看懂所有梗，基本上就穩進面試了吧

他們影片都塞很多梗，看得懂的話真的很好笑🤓

---

那你問我常看的數學頻道呢

畢竟我是數學系我還是會看數學的

數學還是有幫助的，至少邏輯會比較好，可以發現問題，做實驗，想解法來解決問題

- [3Blue1Brown - YouTube](https://www.youtube.com/@3blue1brown)
- **早期的**[Veritasium - YouTube](https://www.youtube.com/@veritasium)

你說漫士沉思錄為什麼不在上面？中文太難了，而且他不知道為啥很強調自己是清華博士，沒很想看

## 小註解

1. 有拿到一個丁組修業安排，第一天性向測驗完發給我們的

![修業安排](https://blog-assets.hoshiyomi-dev.work/nycu-ding-handout.png)

2. 基本上全用`Claude Code`來準備面試，想面試可能的問題。沒有什麼模擬面試那些無聊的東西，那太麻煩了

3. 面試有穿衣服就好，不用穿西裝打領帶穿皮鞋

## 一些連結

- [國立陽明交通大學資工系資訊中心](https://it.cs.nycu.edu.tw/)
- [碩士班甄試入學 — 國立陽明交通大學 招生資訊網](https://exam.nycu.edu.tw/ma-md.html)
- [News - Department of Computer Science, National Yang Ming Chiao Tung University](https://www.cs.nycu.edu.tw/announcements)
- [國立陽明交通大學 116 學年度 資訊科學與工程研究所丁組 招生線上說明會 - 國立陽明交通大學資訊工程學系](https://www.cs.nycu.edu.tw/announcements/detail/14308)
- [115 丁組招生說明會(Google Slides)](https://docs.google.com/presentation/d/1iFwVIT8W_iST3Q-a_KJtUUoK-Dpj6Z5Hfqqo-ecdfOk/edit?usp=sharing)
- [國立陽明交通大學 116 學年度 資訊科學與工程研究所 丁組招生線上說明會 - YouTube](https://www.youtube.com/live/mOFw2zx0QTU)

### 其他可以參考的歷年資料

- [交大資工備取速度 - 研究所 forum | Dcard](https://www.dcard.tw/f/graduate_school/p/260344129)

- [2019 交大資工丁組推甄心得](https://blog.yiyu0x.org/2019/11/22/)
- [2020推甄交通大學資訊工程所丁戊組 – 吾輩乃程序猿](https://lv5.in/2020%E6%8E%A8%E7%94%84%E4%BA%A4%E9%80%9A%E5%A4%A7%E5%AD%B8%E8%B3%87%E8%A8%8A%E5%B7%A5%E7%A8%8B%E6%89%80%E4%B8%81%E6%88%8A%E7%B5%84/)

- [HackMD 交大丁組面試心得](https://hackmd.io/@txLtb1_dT1eziDq4utYbqA/Byj371TEs)
- [coder.tw 110 丁組心得](https://coder.tw/posts/nycu-cs-master-recommendation-interview/)
- [\[心得\] 交大資工丁組 - 看板 graduate - 批踢踢實業坊](https://www.ptt.cc/bbs/graduate/M.1384014636.A.61F.html)
- [系計中 114 招生說明會](https://csdrive.cs.nycu.edu.tw/release/80553d9e-a9b3-4281-ac5e-607ee041e430)

## 特別感謝

- [Claude](https://claude.com)
- 張君天教授為我寫推薦信
- 江金城教授為我寫推薦信
