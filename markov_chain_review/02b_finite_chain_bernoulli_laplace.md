# 02b Ch3–4：Transition Matrix、Bernoulli–Laplace Urn、n-Step、Initial Distribution、$Kf$

來源：[MC p.10–23]、[AR1 p.2]、[H8 p.40]

---

## 2b.1 Transition Probability 與 Transition Matrix

### Step 1 直覺

一個系統有「正常（0）/故障（1）」兩個狀態。工程師觀察到：今天正常，明天仍正常的機率 0.9；今天故障，明天修好的機率 0.6。把這些「從哪裡到哪裡」的機率排成表格，就是 transition matrix。

**為什麼要用條件機率**：0.9 是「已知今天正常」才成立的數字，它不是「明天正常」的無條件機率。

### Step 2 正式定義

有限或可數 state space 時，one-step transition probability：

$$
K_{ij}=K(i,\lbrace j\rbrace)=P(X_{n+1}=j\mid X_n=i).
$$

把它排成矩陣 $K=(K_{ij})$，**列 index = 現在狀態，欄 index = 下一狀態** [MC p.12]。

| 名詞 | 定義 | 每「列/欄」和 |
|---|---|---|
| **Row-stochastic**（本課程） | $K_{ij}=P(\text{next}=j\mid\text{now}=i)$ | **每列**和為 1 |
| Column-stochastic（部分教科書） | $P_{ij}=P(\text{next}=i\mid\text{now}=j)$ | 每欄和為 1；分布用 column vector，$p_{n+1}=Pp_n$ |

合法矩陣的條件：(1) $K_{ij}\ge 0$；(2) $\sum_j K_{ij}=1$ 對每個 $i$。這就是 kernel 條件 1 在有限空間的樣子；條件 2 自動成立 [MC p.6]。

**Interpretation** [MC p.12]：Each row is the entire next-state probability law for one current state.

### Step 3 公式拆解

| 項目 | 說明 |
|---|---|
| 公式 | $K(i,A)=\sum_{j\in A}K_{ij}$ [MC p.13] |
| 符號 | $i$：現在狀態；$A$：下一步狀態集合 |
| 用途 | 問「下一步落在某一群狀態」的機率 |
| 條件 | 有限或可數 state space |
| 直覺 | 把那一列中屬於 $A$ 的格子加起來 |
| 常見誤解 | 加成一欄（那是不同起點到同一終點，沒有機率意義） |
| 辨識 | 「至少」、「是極端值」、「屬於某集合」 |

### Step 5 例題：從文字建立矩陣（正常/故障系統）

1. 狀態：0 = 正常，1 = 故障；順序 (0,1)。
2. 列 0：$K_{00}=0.9$，$K_{01}=1-0.9=0.1$。
3. 列 1：$K_{10}=0.6$，$K_{11}=0.4$。

$$
K=\begin{pmatrix}0.9&0.1\\0.6&0.4\end{pmatrix}.
$$

4. 檢查：每格在 $[0,1]$；列和 $1,1$ ✓。
5. 讀取：「今天故障，明天故障」$=K_{11}=0.4$。

**反例**：若誤把「明天修好的機率 0.6」放在 $K_{01}$，列 0 的和變成 1.5，立刻能發現錯誤。**每次建完矩陣都要檢查列和。**

H8 Example 3.14 [H8 p.40] 的二狀態矩陣（狀態記為 1,2）：

$$
T=\begin{pmatrix}p_1&1-p_1\\1-p_2&p_2\end{pmatrix},\quad p_1=P(X_{t+1}=1\mid X_t=1),\ p_2=P(X_{t+1}=2\mid X_t=2),
$$

同樣是 row-stochastic。

---

## 2b.2 Bernoulli–Laplace Urn（modified）

### 實驗設定 [MC p.10]

$m$ 顆紅球、$m$ 顆藍球，分放兩個 urn，各 $m$ 顆。每一步：
1. 從兩個 urn 各隨機（均勻、獨立）取一顆；
2. 交換這兩顆；
3. 重複，每次都用新的隨機選擇。

狀態 $X_n=$ 第 $n$ 次交換後**左 urn 的紅球數**，$E=\lbrace 0,1,\dots,m\rbrace$。狀態是「計數」，不記錄個別球的編號。

### $m=2$、現在在狀態 1 的完整實驗 [MC p.11]

兩個 urn 都是 1 紅 1 藍，四種顏色組合等機率：

| 左取 | 右取 | 新左紅球數 | 機率 |
|---|---|---|---|
| 紅 | 紅 | 1 | 1/4 |
| 紅 | 藍 | 0 | 1/4 |
| 藍 | 紅 | 2 | 1/4 |
| 藍 | 藍 | 1 | 1/4 |

所以 $K(1,\lbrace 0\rbrace)=1/4$、$K(1,\lbrace 1\rbrace)=1/2$、$K(1,\lbrace 2\rbrace)=1/4$。

**Discuss：為什麼兩種不同的交換得到同一狀態？** 紅換紅、藍換藍都不改變左邊的紅球**數**；狀態只記錄計數，所以兩個不同的實驗結果被合併為同一個狀態，機率相加。

狀態 0：左全藍、右全紅，任何交換都是「左藍換右紅」→ 狀態 1。狀態 2 同理 → 狀態 1。

$$
K=\begin{pmatrix}0&1&0\\ \tfrac14&\tfrac12&\tfrac14\\ 0&1&0\end{pmatrix},\qquad\text{列/欄順序 }0,1,2.
$$

### 用 $K(i,A)$ 回答集合問題 [MC p.13]

現在在 1，列為 $(1/4,1/2,1/4)$：

| $A$ | 意義 | $K(1,A)$ |
|---|---|---|
| $\lbrace 2\rbrace$ | 下一個計數是 2 | 1/4 |
| $\lbrace 0,2\rbrace$ | 下一個是極端狀態 | 1/2 |
| $\lbrace 1,2\rbrace$ | 下一個至少 1 | 3/4 |
| $E$ | 下一個是合法狀態 | 1 |

Discuss：$K(0,\lbrace 0,2\rbrace)=0$、$K(2,\lbrace 0,2\rbrace)=0$（只能去 1）。

### 一般 $m$ 的轉移機率 [MC p.16–17]

若 $X_n=i$：左 urn $i$ 紅 $m-i$ 藍；右 urn $m-i$ 紅 $i$ 藍。兩次選擇獨立：

$$
K_{i,i+1}=\underbrace{\frac{m-i}{m}}_{\text{左取藍}}\cdot\underbrace{\frac{m-i}{m}}_{\text{右取紅}}=\frac{(m-i)^2}{m^2},\qquad
K_{i,i-1}=\frac{i}{m}\cdot\frac{i}{m}=\frac{i^2}{m^2},
$$

$$
K_{ii}=P(\text{紅紅})+P(\text{藍藍})=\frac{i}{m}\frac{m-i}{m}+\frac{m-i}{m}\frac{i}{m}=\frac{2i(m-i)}{m^2}.
$$

其他 $K_{ij}=0$（一次交換紅球數最多變 1）。列和分子：$(m-i)^2+i^2+2i(m-i)=m^2$ ✓。**Interpretation：物理實驗同時提供了機率值與正規化。**

### 用 Uniform 生成樣本路徑 [MC p.15]

取獨立 $U_n\sim\mathrm{Unif}[0,1)$。在 0 或 2 就跳到 1；在 1 時把列機率當作區間長度：

$$
X_{n+1}=\begin{cases}0,&0\le U_{n+1}<1/4\\1,&1/4\le U_{n+1}<3/4\\2,&3/4\le U_{n+1}<1\end{cases}
$$

從 0 出發，$U$ 依序 $0.83,0.10,0.46,0.91,0.23$：$X_1=1$（在 0 必跳 1，$U_1$ 沒用到）；$U_2=0.10$→0；$X_3=1$；$U_4=0.91$→2；$X_5=1$。路徑 $0,1,0,1,2,1$。這是 H7 的 discrete inverse transform（Example 2.10）用在每一列上。**One kernel generates many possible paths.**

---

## 2b.3 n-Step Transition Probabilities 與 Chapman–Kolmogorov

### Step 1 直覺

從 urn 狀態 0 出發，**一步**到 $\lbrace 0,2\rbrace$ 不可能（只能去 1），但**兩步**可以：先到 1，再到 0 或 2。所以 $K(0,\lbrace 0,2\rbrace)=0$ 但 $K^2(0,\lbrace 0,2\rbrace)=1/2>0$ [MC p.20]。

多步機率 = 「列出所有可能的中途站，每條路線機率相乘，再全部相加」。

### Step 2 定義 [MC p.20]

$$
K^r(x,A)=P_x(X_r\in A),\qquad K^0(x,A)=\mathbf 1_A(x),
$$

$P_x$ 是從 $X_0=x$ 出發的機率。有限狀態：

$$
(K^2)_{ij}=\sum_{z\in E}K_{iz}K_{zj}\qquad[\text{MC p.21}],
$$

$$
\text{Chapman–Kolmogorov: }K^{r+s}(x,A)=\int_E K^s(z,A)\,K^r(x,dz)\quad[\text{MC p.46}],\qquad (K^{r+s})_{ij}=\sum_z (K^r)_{iz}(K^s)_{zj}.
$$

### Step 3 公式拆解

| 項目 | 說明 |
|---|---|
| 名稱 | Chapman–Kolmogorov equation / kernel composition |
| 符號 | $r$：前段步數；$s$：後段步數；$z$：第 $r$ 步時的中途狀態 |
| 每一項 | $(K^r)_{iz}$ = 先走 $r$ 步從 $i$ 到 $z$；$(K^s)_{zj}$ = 再走 $s$ 步從 $z$ 到 $j$ |
| 為什麼加總 | 第 $r$ 步「一定」在某個 $z$，這些事件互斥且窮盡 → 全機率公式 |
| 為什麼相乘 | Markov property + homogeneity：到了 $z$ 之後怎麼走與之前無關，且規則與時間無關 |
| 條件 | Time-homogeneous Markov chain（inhomogeneous 時要用 $K_nK_{n+1}\cdots$） |
| 誤解 | $(K^2)_{ij}\ne(K_{ij})^2$（Exit question 3 [MC p.64]） |
| 辨識 | 「after $n$ steps」、「two-step」、「in $n$ swaps」 |

完整推導見 [03a §2](03a_derivations_finite_and_general.md)。

### Step 5 數值：計算 $K^2$（$m=2$）

第 1 列 × 第 1 欄（從 1 到 1）的三條路線 [MC p.21]：

$$
(K^2)_{11}=\underbrace{K_{10}K_{01}}_{1\to0\to1}+\underbrace{K_{11}K_{11}}_{1\to1\to1}+\underbrace{K_{12}K_{21}}_{1\to2\to1}=\tfrac14(1)+\tfrac12\cdot\tfrac12+\tfrac14(1)=\tfrac34.
$$

全部：

$$
K^2=\begin{pmatrix}1/4&1/2&1/4\\1/8&3/4&1/8\\1/4&1/2&1/4\end{pmatrix}.
$$

驗證：每列和為 1 ✓（$K^n$ 仍是 stochastic matrix）。

再如 $(K^2)_{10}=K_{10}K_{00}+K_{11}K_{10}+K_{12}K_{20}=0+\frac12\cdot\frac14+0=\frac18$。

$K^3$（給 2c 的收斂表用）：

$$
K^3=K^2K=\begin{pmatrix}1/8&3/4&1/8\\3/16&5/8&3/16\\1/8&3/4&1/8\end{pmatrix}.
$$

**如何用 $K^n$**：
- 從**特定狀態** $i$ 出發，$n$ 步後在 $j$：讀 $(K^n)_{ij}$（第 $i$ 列）。
- 從**初始分布** $\nu$ 出發：算 $\nu K^n$（見 2b.4）。

---

## 2b.4 Initial Distribution 與 State Distribution

### Step 1 直覺

Kernel 是「更新規則」，但它沒告訴你**系統從哪裡開始**。如果起點不確定（例如一半機率在 0、一半在 1），下一步分布就是「各起點的列，按起點機率加權平均」[MC p.22]。

### Step 2 定義

$$
\nu=\mathcal L(X_0)\ (\text{row vector}),\qquad (\nu K)_j=\sum_i\nu_iK_{ij},\qquad \mathcal L(X_n)=\nu K^n.
$$

一般空間：$(\nu K)(A)=\int_E K(x,A)\,\nu(dx)$ [MC p.27, 49]。

| | 初始確定 $X_0=i$ | 初始不確定 $X_0\sim\nu$ |
|---|---|---|
| $\nu$ | $e_i=(0,\dots,1,\dots,0)$（Dirac $\delta_i$） | 一般機率向量 |
| $\mathcal L(X_n)$ | $K^n$ 的第 $i$ 列 | $\nu K^n$ = 各列的加權平均 |

### Step 5 數值 [MC p.22]

$\nu=(1/2,1/2,0)$：

$$
\nu K=\tfrac12(0,1,0)+\tfrac12\left(\tfrac14,\tfrac12,\tfrac14\right)=\left(\tfrac18,\tfrac34,\tfrac18\right).
$$

檢查：和為 1 ✓。意義：$P(X_1=1)=3/4$。

**Interpretation：A conditional row and an unconditional next-state law answer different questions.** 列 $(1/4,1/2,1/4)$ 回答「已知在 1」；$\nu K$ 回答「不知道在哪，只知道起點分布」。

**方向錯誤示範**：若算成 $K\nu^\top$，得到 $(1/2,3/8,1/2)^\top$，和為 $11/8\ne 1$，不是機率分布——這個數字其實是 $(Kf)$ with $f=\nu$，意義完全不同。

---

## 2b.5 $Kf$：用同一列算條件期望

### Step 1 直覺

如果每個狀態有「得分」$f(j)$（例如紅球數、紅球數平方），$Kf(i)$ 就是「現在在 $i$，下一步得分的平均」。

### Step 2 定義 [MC p.23]

$$
(Kf)(i)=\sum_j f(j)K_{ij}=E[f(X_{n+1})\mid X_n=i];\qquad (Kf)(x)=\int f(y)\,k(x,y)\,dy\ \text{（連續）}[\text{MC p.48}].
$$

特例：$f=\mathbf 1_A$ ⇒ $K\mathbf 1_A=K(\cdot,A)$；$f\equiv 1$ ⇒ $K1=1$。

### Step 5 數值（$m=2$，現在在 1）[MC p.23]

- $K\mathbf 1_{\lbrace 2\rbrace}(1)=1/4$
- $f(j)=j$：$Kf(1)=0\cdot\frac14+1\cdot\frac12+2\cdot\frac14=1$
- $g(j)=j^2$：$Kg(1)=0+\frac12+4\cdot\frac14=\frac32$
- $\mathrm{Var}(X_{n+1}\mid X_n=1)=\frac32-1^2=\frac12$

### Kf 與 νK 的比較

| | $\nu K$ | $Kf$ |
|---|---|---|
| 輸入 | 分布（row vector） | 函數（column vector） |
| 位置 | 向量在 $K$ 左邊 | 函數在 $K$ 右邊 |
| 輸出 | 下一步的分布 | 每個現在狀態對應的條件期望 |
| 意義 | 「平均」轉移法則 | 「平均」函數值 [AR1 p.10] |
| 關係 | $E_\nu[f(X_1)]=\nu(Kf)=(\nu K)f$ | |

算子性質 [MC p.72]：線性 $K(cf+dg)=cKf+dKg$；$K1=1$；$f\ge0\Rightarrow Kf\ge0$；$\lVert Kf\rVert_\infty\le\lVert f\rVert_\infty$（因為 $Kf$ 是 $f$ 的加權平均）。

---

## 2b.6 Exercise 1（$m=4$、現在在 1）[MC p.18–19]

完整解見 [04 §1](04_solved_examples.md)。答案：左 urn 1 紅 3 藍、右 urn 3 紅 1 藍；$K(1,\lbrace0\rbrace)=1/16$、$K(1,\lbrace1\rbrace)=6/16$、$K(1,\lbrace2\rbrace)=9/16$；$K(1,\lbrace2,3,4\rbrace)=9/16$；期望下一計數 $3/2$。

**Restoring mean** [MC p.69]：$E[X_{n+1}-X_n\mid X_n=i]=\frac{(m-i)^2-i^2}{m^2}=1-\frac{2i}{m}$，所以 $E[X_{n+1}-\tfrac m2\mid X_n=i]=\left(1-\tfrac2m\right)(i-\tfrac m2)$。低於 $m/2$ 時期望上升、高於時下降（mean reversion，與 AR(1) 同型）。檢查：$m=4,i=1$：期望增量 $1-\frac{2}{4}=\frac12$，期望下一計數 $=1+\frac12=\frac32$；或用第二式 $\frac m2+(1-\frac2m)(i-\frac m2)=2+\frac12(-1)=\frac32$ ✓。$m=2$ 時 $1-\frac2m=0$，所以不論 $i$，下一步平均都是 1 [MC p.69]。

---

## 2b.7 Step 6 考試重點

**必背**：$K_{ij}$ 定義、row-stochastic 條件、$(K^2)_{ij}=\sum_zK_{iz}K_{zj}$、$\mathcal L(X_n)=\nu K^n$、$Kf(i)=\sum_jf(j)K_{ij}$、Bernoulli–Laplace 三個轉移公式。

**必須理解才能推導**：C–K 為什麼「乘再加」；為什麼 $\nu$ 放左邊；為什麼兩種交換合併為同一狀態。

**必會計算**：由實驗建矩陣、$K(i,A)$、$K^2$ 某一格、$\nu K$、條件期望與條件變異數。

**失分點**：列/欄搞反；$K^2$ 只算某格平方；忘了檢查列和；$\nu K$ 寫成 $K\nu$；把 $K^n$ 的列和 $\nu K^n$ 混淆。

**Exit question 3** [MC p.64]：$K^2$ 不是把矩陣元素或 density 值平方，而是對中間狀態加總（或積分）路線機率的乘積。
