# 02a Ch1–2：Stochastic Process、Transition Kernel、Markov Property、Time Homogeneity

來源：[MC p.4–9, 16, 36, 42]、[AR1 p.3–5, 9]

---

## 2a.1 Stochastic Process 與 State Space

### Step 1 直覺

天氣預報員每天記錄一次「晴/雨」。單獨某一天的天氣是一個 random variable；把每天的紀錄排成一串 $X_0,X_1,X_2,\dots$，就是一個 stochastic process。它回答的問題是：**一個隨機系統如何隨時間演變？**

### Step 2 正式定義

Stochastic process 是一族以時間為索引的 random variables $(X_n)_{n\ge 0}$，每個 $X_n:(\Omega,\mathcal F)\to(E,\mathcal E)$ 可測 [MC p.7]。

| 名詞 | 意義 | 本課程例子 |
|---|---|---|
| Time index $n$ | 第幾次觀察 | 第 $n$ 次交換球、第 $n$ 期 |
| State | $X_n$ 的某個可能值 | 左邊 urn 有 1 顆紅球；$x=0.6$ |
| State space $E$ | 所有可能狀態的集合 | $\lbrace 0,1,\dots,m\rbrace$；$\mathbb R$ |
| Sample path | 固定 $\omega$ 後的整串值 $n\mapsto X_n(\omega)$ | $0,1,0,1,2,1$ [MC p.15] |
| History $\mathcal F_n$ | $\sigma(X_0,\dots,X_n)$，到時間 $n$ 為止「已知的資訊」 | |

| 類型 | Discrete-state | Continuous-state |
|---|---|---|
| **Discrete-time** | Bernoulli–Laplace urn（本課程） | Gaussian AR(1)（本課程） |
| Continuous-time | CTMC（**教材未涵蓋**） | Ornstein–Uhlenbeck（MC p.63 僅作連結） |

**考試範圍**：本課程只考 **discrete-time**；state space 有限（urn）或 $\mathbb R$（AR(1)）兩種。

### Step 5 完整例子：同一個過程如何演變

AR(1) $X_{n+1}=X_n/2+Z_{n+1}$，$X_0=2$，innovation 依序為 $-0.4,0.7,-1.2$ [MC p.36]：

| $n$ | $X_n$ | 新雜訊 $Z_{n+1}$ | $X_{n+1}=X_n/2+Z_{n+1}$ |
|---|---|---|---|
| 0 | 2 | $-0.4$ | $1-0.4=0.6$ |
| 1 | 0.6 | $0.7$ | $0.3+0.7=1.0$ |
| 2 | 1.0 | $-1.2$ | $0.5-1.2=-0.7$ |

觀察：每一步只用「現在的值」和「一個全新的雜訊」。這就是 Markov property 的來源。

**常見誤解**：random variable 是「一個數」，stochastic process 是「一整串」；問「$X$ 的分布」時要先問「哪個時間的 $X_n$」，或「整串的聯合分布」。

---

## 2a.2 Transition Kernel $K(x,A)$

### Step 1 直覺

想像一台「預測機」：你輸入「現在的狀態 $x$」和「一個你關心的集合 $A$」，它回答「下一步落在 $A$ 裡的機率」。

- urn 例子：「現在左 urn 有 1 顆紅球，下一步紅球數是 0 或 2 的機率？」答案 $1/2$ [MC p.13]。
- AR(1) 例子：「現在 $x=2$，下一步落在 $[0,2]$ 的機率？」答案 $\approx 0.6827$ [MC p.39]。

**比喻對應**：預測機 = $K$；輸入 1 = 現在狀態 $x$；輸入 2 = 目標集合 $A$；輸出 = 機率。固定 $x$ 時，預測機對所有 $A$ 給出的答案合起來就是「下一步的完整機率分布」。

### Step 2 正式定義 [MC p.4, AR1 p.5]

> A transition kernel on $(E,\mathcal E)$ is a map $K:E\times\mathcal E\to[0,1]$ with two properties:
> 1. For each fixed $x\in E$, $A\mapsto K(x,A)$ is a probability measure.
> 2. For each fixed $A\in\mathcal E$, $x\mapsto K(x,A)$ is $\mathcal E$-measurable.

拆解：

| 部分 | 意思 |
|---|---|
| $E\times\mathcal E$ | 第一個輸入是一個**狀態**（點），第二個輸入是一個**集合**（事件） |
| $\to[0,1]$ | 輸出是機率 |
| 條件 1 | 固定現在狀態，對所有集合的回答必須是一個合法機率分布：$K(x,E)=1$、$K(x,\varnothing)=0$、$K(x,A^c)=1-K(x,A)$、互斥可數可加 |
| 條件 2 | 固定集合，把它當作「現在狀態的函數」必須可測（才能對隨機的 $X_n$ 代入並求期望） |

兩個方向的閱讀 [MC p.5]：

- **固定 $x$、變動 $A$**：描述「一個」下一步分布。
- **固定 $A$、變動 $x$**：看同一事件的機率如何隨現在狀態改變。例如 AR(1) $K(x,(0,\infty))=\Phi(x/2)$ [MC p.42]。

### Step 3 公式拆解表

| 項目 | 說明 |
|---|---|
| 公式名稱 | Transition kernel |
| 公式 | $K(x,A)=P(X_{n+1}\in A\mid X_n=x)$ |
| 符號 | $x$：現在狀態（一個點）；$A$：下一步可能落入的狀態集合 |
| 有限狀態 | $K(i,A)=\sum_{j\in A}K_{ij}$，$K_{ij}=K(i,\lbrace j\rbrace)$ |
| 有 density | $K(x,A)=\int_A k(x,y)\,dy$ |
| 使用條件 | 需驗證兩個 kernel 條件 |
| 直覺 | 每個現在狀態配一整個下一步分布 |
| 常見誤解 | 把 $K(x,\cdot)$ 當成一個數；以為 $k(x,y)$ 是機率（它是 density 高度） |
| 考試辨識 | 題目問「從某狀態出發，下一步落在某集合」、「驗證是否為 kernel」 |

### Step 5 數值驗證

urn $m=2$ 的列 $(1/4,1/2,1/4)$（現在在 1）：

- 條件 1：$1/4+1/2+1/4=1$，非負；$K(1,\lbrace 0\rbrace\cup\lbrace 2\rbrace)=1/2=K(1,\lbrace 0\rbrace)+K(1,\lbrace 2\rbrace)$ ✓ [MC p.14]
- 條件 2：固定 $A=\lbrace 0,2\rbrace$，$i\mapsto K(i,A)$ 為 $0\mapsto 0,\ 1\mapsto 1/2,\ 2\mapsto 0$，有限空間自動可測 ✓

AR(1)：$k(x,y)\ge 0$、$\int_{\mathbb R}k(x,y)\,dy=1$（Normal density 積分為 1）；$x\mapsto\Phi\!\left(\frac{c-ax}{\sigma}\right)-\Phi\!\left(\frac{b-ax}{\sigma}\right)$ 是連續函數，所以可測 [MC p.41, AR1 p.7]。**驗證 kernel 不需要 $\lvert a\rvert<1$** [AR1 p.7]。

**反例（不是 kernel）**：$K=\begin{pmatrix}0.5&0.6\\0.3&0.7\end{pmatrix}$，第一列和為 1.1，違反條件 1。

---

## 2a.3 Markov Property

### Step 1 直覺

玩大富翁時，下一回合你會走到哪裡，只取決於「你現在站在哪一格」和「這次擲出的骰子」，而跟你過去怎麼走到這格無關。

要注意：**「無關」是「在已知現在的條件下」才成立**。若不知道現在位置，過去的位置當然對預測未來很有幫助（昨天在第 3 格，今天大概在 5–15 格）。

### Step 2 正式定義

老師的版本 [MC p.8, AR1 p.4]：

$$
P(X_{n+1}\in A\mid\mathcal F_n)=P(X_{n+1}\in A\mid X_n)\quad\text{a.s.},\qquad A\in\mathcal E,
$$

並可用 kernel 寫成 $P(X_{n+1}\in A\mid\mathcal F_n)=K_n(X_n,A)$ a.s.

有限狀態的常見寫法（與老師版本等價）：

$$
P(X_{n+1}=j\mid X_n=i,X_{n-1}=i_{n-1},\dots,X_0=i_0)=P(X_{n+1}=j\mid X_n=i).
$$

逐一解釋：

| 部分 | 意思 |
|---|---|
| $X_n$ | 時間 $n$ 的狀態（隨機） |
| $i,j,i_0,\dots$ | 具體的狀態值（固定數字） |
| $P(\cdot\mid\cdot)$ | 條件機率：直線右邊是「已知條件」 |
| 左邊 | 已知**完整歷史**，下一步是 $j$ 的機率 |
| 右邊 | 只知道**現在**，下一步是 $j$ 的機率 |
| 「=」的意義 | 多知道過去，不會改變預測：$\mathcal F_n$ 比 $\sigma(X_n)$ 多出的資訊對 $X_{n+1}$ 無用 |
| $\mathcal F_n$ | $\sigma(X_0,\dots,X_n)$，整段歷史 |
| a.s. | 除了機率 0 的歷史外成立 |

老師額外提醒三件事 [MC p.8]：
- Given the present, earlier observations add no information about the next state.
- The kernel $K_n$ may still depend on time $n$（Markov 不等於 homogeneous）。
- 初始分布 $\nu=\mathcal L(X_0)$ 必須另外給定。

### 為什麼 urn 與 AR(1) 有 Markov property

- **urn** [MC p.16]：若 $X_n=i$，則左 urn 必為 $i$ 紅 $m-i$ 藍、右 urn 為 $m-i$ 紅 $i$ 藍。下一次抽球只依這個組成，過去交換的細節不提供額外資訊。**「選對狀態變數」是 Markov property 成立的關鍵**。
- **AR(1)** [MC p.36, AR1 p.4]：$X_{n+1}=aX_n+\sigma Z_{n+1}$，$Z_{n+1}$ 與歷史獨立，所以

$$
P(X_{n+1}\in A\mid\mathcal F_n)=\int_{\mathbb R}\mathbf 1_A(aX_n+\sigma z)\,\varphi(z)\,dz,
$$

右邊只是 $X_n$ 的函數。**新鮮的獨立雜訊解釋了 Markov property**。

### Step 5 數值例子（符合）

urn $m=2$，$X_0=0$。則 $X_1=1$ 必然。

$$
P(X_2=2\mid X_1=1,X_0=0)=\frac{P(X_0=0,X_1=1,X_2=2)}{P(X_0=0,X_1=1)}=\frac{1\cdot 1\cdot\frac14}{1\cdot 1}=\frac14=K_{12}.
$$

若改成 $X_0=2$ 出發，$P(X_2=2\mid X_1=1,X_0=2)$ 同樣 $=1/4$。過去是 0 還是 2 不影響。

### 反例（不符合 Markov property）

urn 裝 2 紅 2 藍，**不放回**逐顆抽，令 $Y_n=$ 第 $n$ 顆的顏色（呼應 MC p.2–3 的超幾何抽樣）。

- $P(Y_3=R\mid Y_2=R,Y_1=R)=0$（紅球用完）。
- $P(Y_3=R\mid Y_2=R)=\dfrac{P(Y_2=R,Y_3=R)}{P(Y_2=R)}=\dfrac{1/6}{1/2}=\dfrac13$（由對稱性，$P(Y_2=R,Y_3=R)=P(Y_1=R,Y_2=R)=\frac12\cdot\frac13$）。

$0\ne 1/3$，所以 $(Y_n)$ **不是** Markov chain。但若把狀態改成「目前已抽出的紅球數與總數」$(R_n,n)$，就變成 Markov——這正是 MC p.16 的訊息：狀態要包含「足夠的資訊」。

### Markov property 的常見誤解

| 誤解 | 正確 |
|---|---|
| Markov = 未來與過去獨立 | 只有**在給定現在**時條件獨立；不給定現在時通常相依（AR(1) 中 $\mathrm{Corr}(X_0,X_2)=a^2\ne0$） |
| Markov = 每個狀態機率相同 | 完全無關；轉移機率可以任意 |
| Markov = 序列 i.i.d. | i.i.d. 是 Markov 的特例（$K(x,\cdot)$ 不依賴 $x$，如 AR(1) 的 $a=0$ [MC p.58]） |
| 有 Markov property 就是 stationary | 不對，見 2c |

---

## 2a.4 Time Homogeneity

### Step 1 直覺

同一台預測機**每天都用同一套規則**。週一和週五站在同一格、擲同一顆骰子，下一步的分布一樣。

### Step 2 正式定義 [MC p.9]

> A Markov chain is time homogeneous if one kernel $K$ can be used at every time:
> $P(X_{n+1}\in A\mid\mathcal F_n)=K(X_n,A)$ a.s., $n\ge 0$, $A\in\mathcal E$.

等價：$K_n=K$ 對所有 $n$。

| 例子 | 是否 homogeneous |
|---|---|
| urn：球數與交換規則固定 | 是 |
| AR(1)：$a,\sigma$ 與雜訊分布固定 | 是 |
| $X_{n+1}=a_nX_n+\sigma Z_{n+1}$（係數隨時間變） | 一般**不是**，kernel 為 $K_n(x,\cdot)=N(a_nx,\sigma^2)$ |

### 最容易考的觀念 [MC p.9, 42; AR1 p.9, 41]

> Homogeneity means no change with **time**. The distribution $K(x,\cdot)$ may still change with the **current state** $x$.

數值：AR(1) $a=1/2$，$K(x,(0,\infty))=\Phi(x/2)$：

| $x$ | $-2$ | 0 | 2 | 4 |
|---|---|---|---|---|
| $K(x,(0,\infty))$ | 0.1587 | 0.5000 | 0.8413 | 0.9772 |

隨 $x$ 改變，但「同一個 $x$」不論在第幾期都用同一公式，所以仍是 homogeneous。AR1 Exercise 2 第 1 題「因為 $K(x,(0,\infty))$ 隨 $x$ 變，所以 inhomogeneous」是**錯**的 [AR1 p.41]。

另一個陷阱 [AR1 p.14]：Exercise 1 的答案「依賴 $X_0=2$」並不違反 homogeneity——任何時間從 2 出發，兩步後分布都相同。

---

## 2a.5 Step 6 考試重點

**必背定義**：kernel 的兩個條件；Markov property（$\mathcal F_n$ 版與離散版）；time homogeneity。

**必須理解**：
- 為什麼 urn 計數與 AR(1) 有 Markov property（狀態足夠、雜訊獨立）。
- 「依賴 $x$」vs「依賴 $n$」。
- Markov chain 的完整描述 = kernel $K$ + 初始分布 $\nu$。

**必會計算**：驗證一列是否為機率分布；用條件機率定義驗證/否證 Markov property。

**常見失分**：寫 Markov property 時漏了條件中的「現在」；把 $\Omega$ 與 $E$ 混為一談；說 $k(x,y)>1$ 不合法。

**題型辨識**：「Is this process a Markov chain?」→ 寫出條件機率比較兩邊；「Is it time homogeneous?」→ 看規則中是否有 $n$。

**Exit question 對照** [MC p.64]：(1) $\Omega$ 與 $E$ 的差異、$K(x,A)$ 的兩個輸入 → 本檔 2a.1–2a.2；(2) kernel 的兩個條件 → 2a.2。
