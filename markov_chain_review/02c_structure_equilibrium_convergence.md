# 02c Ch6–8（有限狀態）：Irreducibility、Periodicity、Invariant、Stationarity、Reversibility、Recurrence、收斂

來源：[MC p.24–33, 67–68, 73]；連續狀態版本見 [02d](02d_gaussian_ar1.md)。

貫穿全章的例子：Bernoulli–Laplace $m=2$，

$$
K=\begin{pmatrix}0&1&0\\ \tfrac14&\tfrac12&\tfrac14\\ 0&1&0\end{pmatrix}.
$$

老師把問題分三個層次 [MC p.60]：(1) 公式是否定義 kernel；(2) 用 kernel 能算什麼；(3) 鏈**隨時間**做什麼——accessible？periodic？invariant law？是否收斂？本章是第 (3) 層。

---

## 2c.1 Accessibility、Communication、Irreducibility

### Step 1 直覺

把狀態畫成城市，$K_{ij}>0$ 畫一條單行道 $i\to j$。「$j$ 可從 $i$ 到達」= 存在一條（可能很長的）路。「irreducible」= 任兩座城市都能互相到達。

**為什麼要看多步**：$m=2$ 中 $K_{02}=0$（0 不能一步到 2），但 $0\to1\to2$ 兩步可以。只看 $K$ 的單格會誤判。

### Step 2 定義 [MC p.24]

- $j$ is **accessible** from $i$（$i\to j$）：$K^n(i,\lbrace j\rbrace)>0$ for some $n\ge 0$。
- $i$ and $j$ **communicate**（$i\leftrightarrow j$）：$i\to j$ 且 $j\to i$。
- Chain is **irreducible**：every pair of states communicates。

$n=0$ 允許，所以每個狀態都與自己 communicate（$K^0(i,\lbrace i\rbrace)=1$）。

**Interpretation** [MC p.24]：Irreducibility concerns possible paths. It does not say that every transition is possible in one step.

### 為什麼 Bernoulli–Laplace 是 irreducible

$K_{i,i+1}=\frac{(m-i)^2}{m^2}>0$（$i<m$），$K_{i,i-1}=\frac{i^2}{m^2}>0$（$i>0$）。任兩狀態 $i<j$：$i\to i+1\to\cdots\to j$ 每步機率為正，乘積為正；反方向同理。

### 從矩陣判斷的 SOP

1. 畫圖：每個 $K_{ij}>0$（$i\ne j$）畫箭頭 $i\to j$；$K_{ii}>0$ 畫自環。
2. 對每個 $i$ 找出所有能沿箭頭到達的狀態（可達集合）。
3. $i\leftrightarrow j$ ⇔ 互在對方的可達集合中；communicating classes 就是 $\leftrightarrow$ 的等價類。
4. 只有一個 class ⇒ irreducible。

（以下第 5–6 步為**補充背景，不一定屬於考試範圍**；教材未定義 closed class / absorbing state）

5. Closed class：從 class 內無法走到 class 外（class 內各列在 class 外的元素全為 0）。
6. Absorbing state：$K_{ii}=1$。

### Step 5 例子（reducible，補充）

$$
Q=\begin{pmatrix}0.5&0.5&0&0\\0.3&0.7&0&0\\0.2&0&0.3&0.5\\0&0&0&1\end{pmatrix}\ (\text{狀態 }1,2,3,4).
$$

箭頭：$1\leftrightarrow2$；$3\to1$、$3\to4$、$3$ 自環；$4$ 自環。Classes：$\lbrace1,2\rbrace$（closed）、$\lbrace3\rbrace$（不 closed，會離開）、$\lbrace4\rbrace$（absorbing）。不是 irreducible：1 無法到 3。

---

## 2c.2 Periodicity

### Step 1 直覺

$m=1$ 的 urn：每次交換一定是紅換藍，狀態 0、1 交替 $0,1,0,1,\dots$ 回到 0 只可能在偶數步，像鐘擺一樣有節奏。這種「只能在特定倍數步數回家」叫 periodic。

### Step 2 定義 [MC p.25]

$$
d(i)=\gcd\lbrace n\ge 1:K^n(i,\lbrace i\rbrace)>0\rbrace .
$$

$d(i)=1$ 稱 aperiodic。**All states in an irreducible chain have the same period**（教材僅陳述，需背）。

### 判斷 SOP

1. 找出所有 $n$ 使「存在長度 $n$ 的迴路回到 $i$」。
2. 取 gcd。實務上找兩個互質的回返長度即可證 aperiodic；有自環（$K_{ii}>0$）則 $1$ 在集合中，立即 $d(i)=1$。
3. Irreducible 時只需算一個狀態。

### Step 5 例子

**Aperiodic**（$m=2$）[MC p.25]：狀態 0 沒有自環，但 $0\to1\to0$（長度 2）與 $0\to1\to1\to0$（長度 3）機率皆正，$\gcd(2,3)=1$。且狀態 1 有自環 $K_{11}=1/2$。對 $m\ge2$，內部狀態 $K_{ii}=\frac{2i(m-i)}{m^2}>0$，所以整條鏈 aperiodic。**A self-loop at every state is unnecessary.**

**Periodic**（$m=1$）[MC p.26]：

$$
K=\begin{pmatrix}0&1\\1&0\end{pmatrix},\quad K^n(0,\lbrace0\rbrace)=\begin{cases}1&n\text{ 偶}\\0&n\text{ 奇}\end{cases},\quad d(0)=\gcd\lbrace2,4,6,\dots\rbrace=2.
$$

### 對長期行為的影響

$m=1$ 從 $X_0=0$：$\mathcal L(X_n)$ 在 $(1,0)$ 與 $(0,1)$ 間交替，**永不收斂**；但 $(1/2,1/2)$ 是 invariant law。所以 periodicity 使「有 stationary distribution」卻「沒有 limiting distribution」。

### Lazy chain [MC p.26]

$$
K_L(x,A)=\tfrac12\mathbf 1_A(x)+\tfrac12K(x,A),\qquad K_L=\tfrac12 I+\tfrac12K\ (\text{有限}).
$$

以機率 1/2 停在原地、否則照原規則走。
- 每個狀態有自環 ⇒ aperiodic（irreducible 有限鏈）。
- 保留 invariant law：$\pi K_L=\tfrac12\pi+\tfrac12\pi K=\pi$。

$m=1$：$K_L=\begin{pmatrix}1/2&1/2\\1/2&1/2\end{pmatrix}$，一步後就是 $(1/2,1/2)$。

---

## 2c.3 Invariant（Stationary）Distribution

### Step 1 直覺

想像 urn 有很多份平行複本（例如 6000 份）。若 1000 份在 0、4000 份在 1、1000 份在 2，每份各自交換一次後：
- 在 0 的 1000 份全部變成 1；在 2 的 1000 份全部變成 1；
- 在 1 的 4000 份：1000 份到 0、2000 份留 1、1000 份到 2。

結果：0 有 1000、1 有 $1000+2000+1000=4000$、2 有 1000——**比例完全不變**。個別複本一直在動，但「人口比例」不動。這個比例 $(1/6,2/3,1/6)$ 就是 invariant distribution。

**比喻對應**：複本的比例 = 分布 $\pi$；每份交換一次 = 乘 $K$；比例不變 = $\pi K=\pi$。

### Step 2 定義 [MC p.27]

$$
(\nu K)(A)=\int_E K(x,A)\,\nu(dx);\qquad \pi\text{ invariant}\iff \pi K=\pi,\ \text{i.e. }\int_E K(x,A)\,\pi(dx)=\pi(A)\ \forall A.
$$

有限狀態：

$$
\pi_j=\sum_i\pi_iK_{ij},\qquad \pi_i\ge0,\qquad\sum_i\pi_i=1.
$$

拆解：
- $\pi$ 是 **row vector 機率分布**：非負（機率不能負）、和為 1（必須是分布，否則 $\pi=0$ 也滿足 $\pi K=\pi$）。
- $\pi K=\pi$：「以 $\pi$ 加權平均各列，得到 $\pi$ 自己」。也就是 $\pi$ 是 $K$ 的 eigenvalue 1 的 **left** eigenvector。
- Also called **stationary distribution**。

### 求解 SOP（$m=2$）[MC p.28]

1. 寫 $\pi K=\pi$，展開成每一欄的方程：
   - 欄 0：$\pi_0=\pi_0\cdot0+\pi_1\cdot\tfrac14+\pi_2\cdot0=\tfrac14\pi_1$
   - 欄 1：$\pi_1=\pi_0+\tfrac12\pi_1+\pi_2$
   - 欄 2：$\pi_2=\tfrac14\pi_1$
2. 方程有冗餘：三式相加兩邊都是 $\pi_0+\pi_1+\pi_2$（因為 $K$ 列和為 1），所以只有兩條獨立，**必須**加上正規化。
3. $\pi_0+\pi_1+\pi_2=\tfrac14\pi_1+\pi_1+\tfrac14\pi_1=\tfrac32\pi_1=1\Rightarrow\pi_1=\tfrac23$。
4. $\pi=(1/6,2/3,1/6)$。
5. 檢查未用的方程（欄 1）：$\tfrac16+\tfrac12\cdot\tfrac23+\tfrac16=\tfrac23$ ✓；非負 ✓；和為 1 ✓。

**為什麼不能只靠消去法判斷唯一性**：$\pi K=\pi$ 永遠有非零解（$K$ 有 eigenvalue 1），冗餘程度取決於鏈的結構。若鏈 reducible（例如有兩個 closed classes），方程會有**兩個以上**獨立解，正規化後仍不唯一。唯一性要靠定理（有限 + irreducible ⇒ 唯一 [MC p.68]），不能靠「我解出一個」。

### 一般 $m$：超幾何分布 [MC p.32]

$$
\pi_i=\frac{\binom mi\binom m{m-i}}{\binom{2m}m}=\frac{\binom mi^2}{\binom{2m}m},\qquad i=0,\dots,m.
$$

直覺：把 $2m$ 顆球編號，均勻選 $m$ 顆放左邊，共 $\binom{2m}m$ 種；左邊恰 $i$ 紅 = 選 $i$ 紅與 $m-i$ 藍。$m=2$：計數 $1,4,1$，分母 6 ⇒ $(1/6,4/6,1/6)$ ✓。**Uniform labelled configurations produce a nonuniform distribution of counts.** 這就是 Hypergeometric$(2m,m,m)$——投影片開頭介紹超幾何分布 [MC p.2–3] 的原因。

---

## 2c.4 Strict Stationarity

### Step 1 直覺

Invariant 是 kernel 與分布的關係（數學物件）；stationarity 是**過程**的性質：「不論你從第幾期開始看，看到的統計行為都一樣」。

### Step 2 定義 [MC p.29, AR1 p.18]

$$
(X_h,\dots,X_{h+r})\overset d=(X_0,\dots,X_r)\qquad\forall\,r,h\ge0 .
$$

注意是**所有有限區塊的聯合分布**，不只是邊際分布。

定理：time-homogeneous Markov chain 初始分布為 $\nu$ 時，

$$
\text{strictly stationary}\iff\nu K=\nu .
$$

- 必要：$\mathcal L(X_1)=\mathcal L(X_0)$ ⇒ $\nu K=\nu$。
- 充分：每個區塊都從分布 $\nu$ 起步、使用同一個 $K$ [AR1 p.18]。

完整證明見 [03a §7](03a_derivations_finite_and_general.md)。

### Step 5 例子 [MC p.29]

- $X_0\sim(1/6,2/3,1/6)$：stationary chain。
- $X_0=0$：$X_1=1$ 必然，$\mathcal L(X_0)\ne\mathcal L(X_1)$；chain is **homogeneous but not stationary**。

**Exit question 5**：Why can a homogeneous chain fail to be stationary? → Homogeneity 是 kernel 不隨時間變；stationarity 還要求初始分布是 invariant。

---

## 2c.5 Reversibility 與 Detailed Balance

### Step 1 直覺

在平衡狀態下拍一段影片，再倒著播放。如果你**無法從統計上分辨**哪個是正向播放，鏈就是 reversible。等價地：任意兩個狀態之間，平衡時「每單位時間 $i\to j$ 的機率流量」等於「$j\to i$ 的流量」，就像兩城市間每天往返的人數相同。

### Step 2 定義 [MC p.30]

從 invariant $\pi$ 出發的 homogeneous chain 是 reversible 若

$$
(X_0,\dots,X_r)\overset d=(X_r,\dots,X_0)\ \text{under }P_\pi,\quad r\ge1 .
$$

對 Markov chain 等價於 **detailed balance**：

$$
\int_A K(x,B)\,\pi(dx)=\int_B K(y,A)\,\pi(dy)\quad\forall A,B;\qquad\text{有限：}\ \pi_iK_{ij}=\pi_jK_{ji}\ \forall i,j .
$$

稱 $K$ is reversible with respect to $\pi$。

### Detailed balance ⇒ invariance [MC p.31]

$$
\sum_i\pi_iK_{ij}=\sum_i\pi_jK_{ji}=\pi_j\sum_iK_{ji}=\pi_j .
$$

所以 detailed balance 是**檢查 invariance 的第二種方法**，而且常常比解 $\pi K=\pi$ 簡單（只需看相鄰狀態）。反過來不成立：invariant 不一定 reversible（補充：例如三狀態單向循環 $0\to1\to2\to0$ 機率 1，均勻分布 invariant，但 $\pi_0K_{01}=1/3\ne\pi_1K_{10}=0$）。

### Step 5 數值（$m=2$）

邊 $0$–$1$：$\pi_0K_{01}=\frac16\times1=\frac16$；$\pi_1K_{10}=\frac23\times\frac14=\frac16$ ✓。邊 $1$–$2$ 同理。邊 $0$–$2$：兩邊皆 0 ✓。

**Interpretation**：The transition probabilities themselves need not be symmetric（$K_{01}=1\ne K_{10}=1/4$）。Detailed balance 是**加權**對稱，不是 $K$ 對稱。

一般 $m$ [MC p.67]：

$$
\frac{\pi_{i+1}}{\pi_i}=\left(\frac{m-i}{i+1}\right)^2=\frac{K_{i,i+1}}{K_{i+1,i}},
$$

所以 $\pi_iK_{i,i+1}=\pi_{i+1}K_{i+1,i}$；非相鄰的非對角元素兩方向流量都是 0。推導見 [03a §6](03a_derivations_finite_and_general.md)。

**用途**：MCMC 的所有演算法都是「設計 $K$ 使其對目標 $\pi$ 滿足 detailed balance」[H10 p.6, 12]。

---

## 2c.6 Recurrence、Transience、Mean Return Time

### Step 1 直覺

從家出發隨機散步：
- 「一定會回家」（機率 1）= recurrent；
- 「有機會永遠回不了家」= transient；
- 一定回家且**平均**花有限時間 = positive recurrent；一定回家但平均時間無限 = null recurrent。

### Step 2 定義 [MC p.68]（附錄，optional material）

$P_i,E_i$：從 $X_0=i$ 出發。首次回返時間

$$
\tau_i^+=\inf\lbrace n\ge1:X_n=i\rbrace,\qquad\inf\varnothing=\infty .
$$

| 名稱 | 條件 |
|---|---|
| Recurrent | $P_i(\tau_i^+<\infty)=1$ |
| Positive recurrent | recurrent 且 $E_i\tau_i^+<\infty$ |
| Null recurrent | recurrent 但 $E_i\tau_i^+=\infty$ |
| Transient | $P_i(\tau_i^+<\infty)<1$ |

注意 $n\ge1$：$\tau_i^+$ 至少為 1，「時間 0 在 $i$」不算回返。

**定理** [MC p.68]：Every finite irreducible chain is positive recurrent, with a unique invariant law, and

$$
E_i\tau_i^+=\frac{1}{\pi_i}.
$$

直覺：長期而言 $\pi_i$ 比例的時間在 $i$，所以平均每 $1/\pi_i$ 步回來一次。

**不能推廣到所有鏈**：
- 有限但 reducible：可能有 transient 狀態（如 2c.1 例子的狀態 3）、多個 invariant law。
- 無限狀態：可能 null recurrent 或 transient（補充：simple symmetric random walk on $\mathbb Z$ 是 null recurrent）；AR(1) 在 $\lvert a\rvert\ge1$ 時沒有 invariant probability [AR1 p.36, 47]。

### Step 5 數值（$m=2$）

$1/\pi=(6,3/2,6)$ [MC p.68]。用首步分析（**補充技巧**）驗證：

- $E_1\tau_1^+$：以 1/2 留在 1（1 步回來）；以 1/2 去 0 或 2，下一步必回 1（2 步）。$E_1\tau_1^+=\frac12\cdot1+\frac12\cdot2=\frac32$ ✓。
- $E_0\tau_0^+$：設 $h_i=E_i[\text{到達 0 的時間}]$，$h_0=0$。
  $h_2=1+h_1$；$h_1=1+\tfrac14h_0+\tfrac12h_1+\tfrac14h_2=1+\tfrac12h_1+\tfrac14(1+h_1)$ ⇒ $\tfrac14h_1=\tfrac54$ ⇒ $h_1=5$。
  $E_0\tau_0^+=1+h_1=6$ ✓。

---

## 2c.7 Limiting Distribution 與收斂定理

### Step 1 直覺

Stationary：「若現在已經是 $\pi$，下一步還是 $\pi$」（一個靜止點）。Limiting：「不論從哪裡出發，跑很久之後分布都趨近 $\pi$」（會被吸過去）。靜止點存在不代表會被吸過去——$m=1$ 的鐘擺就是反例。

### 三個概念的比較

| 概念 | 定義 | 依賴初始分布？ |
|---|---|---|
| State distribution at time $n$ | $\mathcal L(X_n)=\nu K^n$ | 是 |
| Stationary（invariant） | $\pi K=\pi$ | 否（kernel 的性質） |
| Limiting | $\lim_n\nu K^n$（或 $\lim_nK^n(i,\lbrace j\rbrace)$），且與起點無關 | 定義上要求與起點無關 |

### 收斂定理 [MC p.33]

> A finite irreducible aperiodic chain converges to its unique invariant law: $K^n(i,\lbrace j\rbrace)\to\pi_j$ for every $i,j$.

假設的角色：
- **Finite + irreducible** ⇒ positive recurrent、$\pi$ 存在且唯一 [MC p.68]（irreducible 排除「多個互不相通的區塊各有自己的平衡」）。
- **Aperiodic** ⇒ 排除鐘擺式振盪（$m=1$）。
- 這是**充分條件**，不是必要條件：例如 $K$ 每列都等於 $\pi$（i.i.d. 抽樣）一步就收斂；AR(1) 是無限狀態也收斂。不可寫成「收斂 ⇒ 有限」。

Bernoulli–Laplace $m\ge2$：irreducible + aperiodic ⇒ $K^n(i,\lbrace j\rbrace)\to\pi_j$。

### Step 5 數值：從 $X_0=0$ 出發 [MC p.33]

| $n$ | $\mathcal L(X_n)$ | 平均 |
|---|---|---|
| 0 | $(1,0,0)$ | 0 |
| 1 | $(0,1,0)$ | 1 |
| 2 | $(1/4,1/2,1/4)$ | 1 |
| 3 | $(1/8,3/4,1/8)$ | 1 |
| 4 | $(3/16,5/8,3/16)$ | 1 |
| Limit | $(1/6,2/3,1/6)$ | 1 |

（$n=4$ 為本文件補算：$(1/8,3/4,1/8)K=(3/16,5/8,3/16)$。）

**Interpretation：The mean can reach its equilibrium value before the whole distribution does.** 從 $n=1$ 起平均已是 1，但分布在 $n=3$ 仍不是 $\pi$。所以「平均等於平衡平均」不能證明已經平衡。中間狀態機率 $1,1/2,3/4,5/8,\dots$ 繞著 $2/3$ 上下振盪並收斂。

### 反例：stationary 存在但無 limit

$m=1$：$\pi=(1/2,1/2)$ invariant；但從 0 出發 $\mathcal L(X_n)$ 交替，無極限 [MC p.26]。

---

## 2c.8 Ergodic Time Averages [MC p.73]

$$
\frac1N\sum_{n=0}^{N-1}\mathbf 1_{\lbrace j\rbrace}(X_n)\to\pi_j\quad\text{almost surely}
$$

（finite irreducible chain）。白話：**單一條**路徑上，待在 $j$ 的時間比例趨近 $\pi_j$。

與 $K^n\to\pi$ 的差別：
- $K^n(i,\cdot)\to\pi$ 是「分布」（很多平行複本在時間 $n$ 的比例）。
- Ergodic average 是「一條路徑沿時間的平均」。

注意：ergodic average 只需 irreducible（$m=1$ 的鐘擺，時間比例仍趨近 1/2），而分布收斂還需要 aperiodic。**Averages can settle even while individual states continue to fluctuate.** 這是 MCMC 用「一條長鏈的平均」估計 $E_\pi f$ 的理論根據。

---

## 2c.9 補充背景：Absorbing Markov Chain（不屬於考試範圍）

教材**沒有**教 canonical form 與 fundamental matrix。若遇到，記號為

$$
K=\begin{pmatrix}Q&R\\0&I\end{pmatrix},\quad N=(I-Q)^{-1},\quad B=NR,\quad t=N\mathbf 1,
$$

$Q$：transient→transient；$R$：transient→absorbing；$N_{ij}$：從 transient $i$ 出發造訪 transient $j$ 的期望次數；$B_{ik}$：被 $k$ 吸收的機率；$t_i$：期望吸收時間。$N=\sum_{n\ge0}Q^n$ 收斂因為 transient 狀態最終會離開。考試若沒出現此題型，不必準備。

---

## 2c.10 Step 6 考試重點

**必背**：accessible / communicate / irreducible；$d(i)$ 的 gcd 定義與「irreducible 同 period」；lazy chain 公式；$\pi K=\pi$ 三條件；strict stationarity 定義與 $\iff\nu K=\nu$；detailed balance；$\tau_i^+$ 與四種 recurrence 定義；$E_i\tau_i^+=1/\pi_i$；收斂定理的三個假設。

**必須理解才能推導**：detailed balance ⇒ invariance（三行證明）；為什麼要正規化；為什麼 periodic 不收斂；為什麼平均先收斂。

**必會計算**：解 $\pi$；檢查 detailed balance；算 period；由 $\pi$ 寫平均回返時間。

**易混**：invariant vs stationary process vs limiting；reversible vs symmetric matrix；irreducible vs 每步都可到。

**失分點**：漏正規化；用 $K\pi^\top=\pi^\top$（right eigenvector，得到的是常數向量 $\mathbf 1$）；把收斂定理的充分條件說成必要；以為 $\pi$ 存在就收斂。

**Exit question 6** [MC p.64]：Stationarity 檢查「時間平移」後聯合分布不變；Reversibility 檢查「時間反轉」後聯合分布不變。
