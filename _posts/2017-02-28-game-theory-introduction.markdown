---
layout: "post"
title: "[Game Theory] Introduction"
date: "2017-02-28 11:03"
tag: [gametheory]
---

## 動機

這學期修了一門課叫「網路經濟學」(Economics of networks)，課程內容是把圖論(Graph theory)應用在賽局理論(Game theory)，去model很多經濟學的問題。(詳細課程內容可見我之後的post！)  
根據課程網，這門課的prerequisites是圖論和賽局理論，但我只有在經原中短短接觸過賽局理論，沒有修過任何賽局專門的課程。因此，趁開學還有空時，我打算先自行學習。  
剛好[Coursera](https://www.coursera.org/learn/game-theory-1/home/)上有這麼一門賽局理論的線上課程，是stanford開的。根據一些影片裡的蛛絲馬跡，似乎是2013年錄製的影片，還不會到很out-of-date，就打算學學看了。  
這個系列的post應該會記錄一些我自己的學習筆記！

## Definition of games

一個game有三個要素：*Players*, *Actions*, *Payoffs*。

> 在課程裡, player有時被稱為agent。

#### Players: self-interested agents

這裡的self-interested的意思不是只關心自己，而對周遭漠不關心的人。而是指**每個agent都有自己的喜惡偏好**，因此會對現實世界的狀況有不同的description。

#### Payoffs: Utility function

定義為 "quantifies degree of preferences across alternatives"。在這樣的定義下，根據[Decision theoretic](https://zh.wikipedia.org/wiki/%E5%86%B3%E7%AD%96%E8%AE%BA) rationality，一個理性的agent會試著最大化他的utility。

### Two standard representation of games

我們通常有兩種game的表示方法：

1. Normal form (a.k.a matrix form, strategic form)
    * 這個方法會將payoff視為action的函數
    * 所有player會*同時(simlutaneously)*做出action  
2. Extensive form
    * 這種表示法通常會表示成*樹狀結構*
    * 包含時間(timing of moves)
    * 會記錄每個player在作出決定時所知道的資訊

### Mathematical representation

我們可以將一個finite, n-player normal form game表示成$$\langle N,A,u \rangle$$  
* Players: $$N={1,...,n}$$
* Action set: $$A_i$$ 代表 player i 的action
    * action profile: $$a=(a_1,...,a_n)$$，其中 $$a_i$$ 代表player i的action
    > 我覺得這裡的notation非常confusing，不知道為什麼要分大小寫
* Utility function: $$ u_i :A \mapsto R$$
    * utility profile: $$u=(u_1,...,u_n)$$，其中 $$u_i$$ 代表player i的utility

### Example of games

1. Prisoner's dilemma  
    課程影片用了一個很有趣的例子：*TCP Backoff*！正好是上學期計網的內容XD  這個game的中文敘述如下：

    > **TCP Backoff**
    >
    > TCP protocol是一種網路上傳遞資料的協定。假設我們要將一個檔案從A傳到B，TCP會將檔案切成很多封包，然後一個一個傳。B收到封包後，會回傳確認訊息(ACK)。傳遞的路上會經過很多node，每個node都有可能隨機丟掉封包(ex.buffer overflow)，造成ACK沒辦法順利回傳到A。若A在傳出一個封包後，沒有在時間內收到ACK，則傳遞速率會降低，也就是back off。但每個人可以選擇遵守(coorperate)或違反(defect)TCP。假設現在網路上有兩個user：
    > * 若兩個人都遵守TCP，則每個人會有1 ms的delay。
    > * 若僅一個人遵守TCP，則他將會有0 ms的delay，而另一個人將有4 ms的delay。
    > * 若兩人都不遵守TCP，則每個人會有3 ms的delay。

    我們可以將這個game用normal form表示：  

    |   | C     | D     |
    |---|:------:|:-------:|
    | C | -1,-1 | -4,0  |
    | D | 0,-4  | -3,-3 |

    事實上，我們可以一般化成這個形式：

    |   | C     | D     |
    |---|:------:|:-------:|
    | C | a,a | b,c  |
    | D | c,b  | d,d |

    對於任何 $$ c>a>b>d $$
2. Games of pure competition
    - 剛好2個player
    - 對於任何action profile $$ a \in A , u_1(a)+u_2(a)=c$$
        若 $$ c = 0 $$，則稱為 *零合遊戲(zero-sum game)*

    以下是一個例子：
    > **Penny matching**
    >
    > 兩個人各丟一枚硬幣。其中一人希望兩枚硬幣正反相同，另一人則希望相反。

    同樣用normal form表示：

    |   | Heads | Tails |
    |---|:------:|:-------:|
    | Heads | 1,-1 | -1,1  |
    | Tails | -1,1  | 1,-1 |

3. Games of pure coordination
    - 這跟剛剛相反：每個player都想要相同的結果！換句話說，合作(coordinate)才會讓大家受益。
    - 對於任何action profile $$ a \in A , \forall i,j, u_i(a)=u_j(a)$$

    以下是一個例子：
    > **Which side of the road to pick?**
    >
    > 兩個人在路上面對面相遇。若兩人都走向相對的左/右，則不會撞到。反之則撞。

    同樣用normal form表示：

    |   | Left | Right |
    |---|:------:|:-------:|
    | Left | 1,1 | 0,0  |
    | right | 0,0  | 1,1 |

4. General games
    > 我也不知道為什麼要叫general games，一點都不general呀。

    這裡的例子如下：
    > **Battle of Sexes**
    >
    > 一對男女朋友想要去看電影，男生想看A電影，女生想看B電影。
    > * 如果他們都選擇去看同一部(這裡我解釋成"表態")，那他們就會一起去，但比較不想去看那部電影的人的payoff比較低。
    > * 如果他們想選擇去看不同部，那他們就會吵架，並不會去看電影。

    同樣用normal form表示：

    |   | A | B |
    |---|:------:|:-------:|
    | A | 2,1 | 0,0  |
    | B | 0,0  | 1,2 |

## Nash Equilibrium

Nash equilibrium (NE) 有他的數學定義，但我覺得了解他的intuitive definition比較重要。課程中給了幾個定義，我節錄了我認為比較重要的一個：

> *Nobody has incentive to deviate from their action if an equilibrium profile is played.*

意思是說，就算現在有兩個action profile，不管他們有什麼性質(ex.更靠近提高payoff的condition)，只要對於某player而言，沒有incentive，i.e.payoff沒有更高，則他就沒有理由從一個action換到另一個action。這也是為什麼*NE通常會有很多個*。

### Best Response

- 對於某player i，令 $$ a_{-i}= \langle a_1,...,a_{i-1}, a_{i+1},...,a_n \rangle $$ 為除了他以外的player的profile，我們可以將某個action profile表示成$$ a=(a_{-1}, a_i) $$
- __*Definition:*__

$$ {a_i}^{*} \in BR(a_{-i}) \Leftrightarrow \forall a_i \in A_i, u_i({a_i}^{*}, a_{-i}) \geq u_i(a_i, a_{-i}) $$

若將$$a_{-i}$$視為一個條件，則我們定義Best Response為：給定這個條件，採取某action是*最好*的。這個「最好」的定義就是一開始提到的：沒有incentive去採取其他行動。

- __*Definition:*__

$$a=(a_1,...,a_n)$$ is a ("pure strategy") __nash equilibrium__ $$\Leftrightarrow \forall i , a_i \in  BR(a_{-i}). $$

這句話的意思是說，如果每個人都採取了他的Best Response，則這樣的action profile就形成了Nash equilibrium。也就是說，「是否為NE」可說是一個action profile的性質。

### Domination

這裡我們得先定義什麼是strategy。但因為目前都只是pure strategy(固定的選某action)，所以我們定義strategy為「採取某action」。

- $$ s_i, s_i'$$ are two strategies for player i, and $$S_{-i}$$ is the set of all possible strategy profiles fot others.
- __*Definition:*__ $$s_i$$ strictly dominates $$S_i'$$ if

$$ \forall s_{-i} \in S_{-i}, u_i(s_i, s_{-i})>u_i(s_i', s_{-i}).$$

- __*Definition:*__ Similarly, $$s_i$$ very weakly dominates $$S_i'$$ if

$$ \forall s_{-i} \in S_{-i}, u_i(s_i, s_{-i}) \geq u_i(s_i', s_{-i}).$$

- 為什麼要叫very weakly呢？因為允許payoff一樣。如果payoff一樣的話，還能叫dominate嗎？
- 若某strategy dominate其他所有strategies，則稱它*dominant*.
- 若一個strategy profile裡的所有strategy都是dominant strategy，則它一定是NE.
- 雖然目前為止討論的都是pure strategy，看不出來差別，這裡可以說是給了前面的討論一個延伸：「是否為NE」可說是一個strategy profile的性質。。
- 雖然前面有說NE可能有很多個，但若一個strategy profile裡都是strictly dominant strategy，則該NE必為唯一。應該是滿顯而易見的！

### Pareto-optimality

前面的討論都針對特定player，但Pareto-optimality著重在整體：怎樣的outcome是對整體都好的？

- __*Definition:*__ 對於兩個 outcome o, o'，若o pareto-dominates o'，則o一定要至少對*所有*agent都不比o'差，且對*某些*agent而言，他們一定strictly較偏好o。
    - 舉個例子：若o = (-1, 1), o' = (0, 1)，則對後面的player而言，他的payoff不變，但對前者的player而言是比較好的，因此o pareto-dominates o'。
    - 也就是說，一定要有某些agent的payoff變好。若o = (-1, 1), o' = (-1, 1)，則o並不pareto-dominate o'。
- __*Definition:*__ 某outcome是一個pareto-optimal，若沒有其他outcome可以pareto-dominate它。
    - 好難翻譯的一個定義Orz，因為這又是一個負面定義。
    - 可以有很多pareto-optimal。
    - 因為定義裡有「strictly」，所以至少會有一個pareto-optimal。
    - 我覺得Pareto-optimal通常都不好找...
