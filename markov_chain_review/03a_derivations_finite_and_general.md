# 03a 推導講義（一）：一般理論與有限狀態鏈

每個推導的格式：**目標 → 已知 → 策略 → 逐步推導（每行附說明）→ 關鍵觀念 → 考試精簡版**。

全檔假設 $(X_n)$ 是 time-homogeneous Markov chain，kernel $K$，初始分布 $\nu$（除非特別說明）。

---

## §1 Path probability（路徑機率乘法公式）

**1. 目標**：$P(X_0=i_0,X_1=i_1,\dots,X_n=i_n)=\nu_{i_0}K_{i_0i_1}K_{i_1i_2}\cdots K_{i_{n-1}i_n}$。

**2. 已知**：條件機率乘法規則；Markov property；homogeneity；$\nu=\mathcal L(X_0)$。

**3. 策略**：把聯合機率拆成「先發生的 × 已知先前後再發生的」，每一層用 Markov property 砍掉多餘的過去。

**4. 逐步推導**

$$
P(X_0=i_0,\dots,X_n=i_n)=P(X_0=i_0,\dots,X_{n-1}=i_{n-1})\cdot P(X_n=i_n\mid X_0=i_0,\dots,X_{n-1}=i_{n-1})
$$

說明：條件機率定義 $P(A\cap B)=P(A)P(B\mid A)$，$A$ = 前 $n$ 步、$B$ = 第 $n$ 步。

$$
=P(X_0=i_0,\dots,X_{n-1}=i_{n-1})\cdot P(X_n=i_n\mid X_{n-1}=i_{n-1})
$$

說明：Markov property——給定整段歷史等同只給定現在 $X_{n-1}$。

$$
=P(X_0=i_0,\dots,X_{n-1}=i_{n-1})\cdot K_{i_{n-1}i_n}
$$

說明：homogeneity——任何時間從 $i_{n-1}$ 走一步到 $i_n$ 的機率都是 $K_{i_{n-1}i_n}$。

對前段重複同一步驟 $n$ 次，最後剩 $P(X_0=i_0)=\nu_{i_0}$。

**5. 關鍵**：第二行是 Markov property 唯一被用到的地方；若過程非 Markov，就得保留整段條件，乘法公式失效。若非 homogeneous，第三行變成 $(K_{n-1})_{i_{n-1}i_n}$。

**6. 考試版**：By the multiplication rule, the Markov property and homogeneity, $P(X_0=i_0,\dots,X_n=i_n)=P(X_0=i_0,\dots,X_{n-1}=i_{n-1})K_{i_{n-1}i_n}=\cdots=\nu_{i_0}\prod_{k=1}^nK_{i_{k-1}i_k}$.

**數值檢查**：urn $m=2$，路徑 $0,1,0,1,2,1$ 從 $X_0=0$：$1\cdot K_{01}K_{10}K_{01}K_{12}K_{21}=1\cdot\frac14\cdot1\cdot\frac14\cdot1=\frac1{16}$。

---

## §2 Chapman–Kolmogorov Equation

**1. 目標**：$(K^{r+s})_{ij}=\sum_{z\in E}(K^r)_{iz}(K^s)_{zj}$；一般形式 $K^{r+s}(x,A)=\int_EK^s(z,A)\,K^r(x,dz)$ [MC p.46]。

**2. 已知**：$K^r(x,A)=P_x(X_r\in A)$ [MC p.20]；全機率公式；§1；Markov property。

**3. 策略**：第 $r$ 步時鏈必在某個 $z$，按 $z$ 分類（全機率），每一類的機率拆成「先到 $z$」×「從 $z$ 再走 $s$ 步」。

**4. 逐步推導**

$$
(K^{r+s})_{ij}=P_i(X_{r+s}=j)
$$

說明：$n$-step 機率的定義。

$$
=\sum_{z\in E}P_i(X_r=z,\ X_{r+s}=j)
$$

說明：事件 $\lbrace X_r=z\rbrace$，$z\in E$，互斥且聯集為全空間（全機率公式／可數可加）。

$$
=\sum_{z\in E}P_i(X_r=z)\,P_i(X_{r+s}=j\mid X_r=z)
$$

說明：乘法規則（只對 $P_i(X_r=z)>0$ 的項；為 0 的項本來就是 0）。

$$
=\sum_{z\in E}P_i(X_r=z)\,P_z(X_s=j)
$$

說明：**這一步是關鍵**。由 §1，從時間 $r$ 起的路徑機率只依賴 $X_r=z$（Markov property，可對多步重複使用）且規則與時間無關（homogeneity），所以「時間 $r$ 在 $z$ 再走 $s$ 步」等同「時間 0 在 $z$ 走 $s$ 步」。展開寫：$P_i(X_{r+s}=j\mid X_r=z)=\sum_{z_{r+1},\dots,z_{r+s-1}}K_{zz_{r+1}}\cdots K_{z_{r+s-1}j}$，與 $i$ 和 $r$ 都無關。

$$
=\sum_{z\in E}(K^r)_{iz}(K^s)_{zj}.
$$

說明：定義代回。這就是矩陣乘法 $(K^rK^s)_{ij}$，所以 $K^{r+s}=K^rK^s$，特別地 $K^n$ 就是矩陣的 $n$ 次方。

一般 state space：把 $\sum_z$ 換成對 $K^r(x,dz)$ 積分；有 density 時 $k_2(x,y)=\int k(x,z)k(z,y)\,dz$ [MC p.46]。

**5. 關鍵**：最容易卡住的是第四行——為什麼可以「重新從 0 開始計時」。需要 Markov（忘記 $i$）+ homogeneous（忘記 $r$）兩者。$K^0=I$（$\mathbf 1_A(x)$）讓 $r=0$ 或 $s=0$ 時也成立。

**6. 考試版**：Conditioning on $X_r$, $P_i(X_{r+s}=j)=\sum_zP_i(X_r=z)P(X_{r+s}=j\mid X_r=z)=\sum_z(K^r)_{iz}(K^s)_{zj}$, where the Markov property and homogeneity give $P(X_{r+s}=j\mid X_r=z,X_0=i)=P_z(X_s=j)$.

**數值**：$(K^2)_{11}=\frac34$（[02b §3](02b_finite_chain_bernoulli_laplace.md)）；$K^2(0,\lbrace0,2\rbrace)=K_{01}K_{10}+K_{01}K_{12}=\frac12$ [MC p.20]。

---

## §3 State distribution $\mathcal L(X_n)=\nu K^n$

**1. 目標**：$P(X_n=j)=(\nu K^n)_j$；一般 $(\nu K)(A)=\int K(x,A)\nu(dx)$。

**2. 已知**：全機率公式；$P(X_n=j\mid X_0=i)=(K^n)_{ij}$。

**4. 逐步推導**

$$
P(X_n=j)=\sum_iP(X_0=i)\,P(X_n=j\mid X_0=i)=\sum_i\nu_i(K^n)_{ij}=(\nu K^n)_j .
$$

說明：第一個等號按起點分類（全機率）；第二個是定義；第三個是 row vector 乘矩陣的定義。

遞迴形式：$\mathcal L(X_{n+1})=\mathcal L(X_n)K$（按 $X_n$ 分類）。

**5. 關鍵**：$\nu$ 必須是 row vector 放左邊，因為加總的 index $i$ 是 $K$ 的**列** index。

**6. 考試版**：$P(X_n=j)=\sum_iP(X_0=i)P(X_n=j\mid X_0=i)=\sum_i\nu_i(K^n)_{ij}$.

---

## §4 Bernoulli–Laplace 的轉移機率 [MC p.16–17]

**1. 目標**：$K_{i,i+1}=\frac{(m-i)^2}{m^2}$、$K_{i,i-1}=\frac{i^2}{m^2}$、$K_{ii}=\frac{2i(m-i)}{m^2}$，其他為 0，且列和為 1。

**2. 已知**：$X_n=i$ ⇒ 左 urn $i$ 紅 $m-i$ 藍、右 urn $m-i$ 紅 $i$ 藍；兩 urn 的選擇獨立且均勻。

**3. 策略**：列出四種顏色組合，算每種的機率，看左邊紅球數如何變化。

**4. 逐步推導**

| 左取 | 右取 | 機率 | 左紅球數變化 |
|---|---|---|---|
| 紅 | 紅 | $\frac im\cdot\frac{m-i}m$ | 不變（紅換紅） |
| 紅 | 藍 | $\frac im\cdot\frac im$ | $-1$（送出紅、收到藍） |
| 藍 | 紅 | $\frac{m-i}m\cdot\frac{m-i}m$ | $+1$ |
| 藍 | 藍 | $\frac{m-i}m\cdot\frac im$ | 不變 |

說明：左取紅的機率 = 左邊紅球比例 $i/m$；右取紅 = 右邊紅球比例 $(m-i)/m$；獨立所以相乘。

合併同一新狀態的列：$K_{ii}=\frac{i(m-i)}{m^2}+\frac{(m-i)i}{m^2}=\frac{2i(m-i)}{m^2}$。

列和：$\frac{(m-i)^2+i^2+2i(m-i)}{m^2}=\frac{\big((m-i)+i\big)^2}{m^2}=1$。

說明：四種組合窮盡所有可能，所以和必為 1；代數上就是 $(a+b)^2=a^2+2ab+b^2$。

**5. 關鍵**：Markov property 成立是因為 $i$ 決定了兩個 urn 的完整組成 [MC p.16]。

**6. 考試版**：Given $X_n=i$, the left urn has $i$ red, $m-i$ blue and the right urn $m-i$ red, $i$ blue. The count increases iff left-blue and right-red: $(m-i)^2/m^2$; decreases iff left-red and right-blue: $i^2/m^2$; otherwise unchanged: $2i(m-i)/m^2$. These sum to $m^2/m^2=1$.

---

## §5 Detailed balance ⇒ invariance [MC p.31]

**1. 目標**：若 $\pi_iK_{ij}=\pi_jK_{ji}$ 對所有 $i,j$，且 $\pi$ 是機率分布，則 $\pi K=\pi$。一般版：$\int_AK(x,B)\pi(dx)=\int_BK(y,A)\pi(dy)$ ⇒ $\pi K=\pi$。

**2. 已知**：$\sum_iK_{ji}=1$（列和為 1）。

**4. 逐步推導（有限）**

$$
(\pi K)_j=\sum_i\pi_iK_{ij}
$$

說明：$\pi K$ 第 $j$ 分量的定義。

$$
=\sum_i\pi_jK_{ji}
$$

說明：逐項用 detailed balance，把「從 $i$ 流入 $j$」換成「從 $j$ 流出到 $i$」。

$$
=\pi_j\sum_iK_{ji}=\pi_j .
$$

說明：$\pi_j$ 與 $i$ 無關可提出；第 $j$ 列和為 1。

**一般版**：取 $B=E$：$\int_AK(x,E)\pi(dx)=\int_EK(y,A)\pi(dy)$，左邊 $=\int_A1\,\pi(dx)=\pi(A)$，右邊 $=(\pi K)(A)$。

**5. 關鍵**：detailed balance 是**充分不必要**條件。反例見 [02_concept_review](02_concept_review.md) 第 7 例。

**6. 考試版**：$\sum_i\pi_iK_{ij}=\sum_i\pi_jK_{ji}=\pi_j$.

---

## §6 Bernoulli–Laplace 的超幾何不變分布 [MC p.32, 67]

**1. 目標**：$\pi_i=\binom mi^2/\binom{2m}m$ 對 $K$ 滿足 detailed balance，因此 invariant。

**2. 已知**：§4、§5；$\sum_i\binom mi\binom m{m-i}=\binom{2m}m$（Vandermonde），所以 $\pi$ 是機率分布。

**3. 策略**：Birth–death 型（只能 ±1 或不動）的鏈，只需檢查相鄰狀態 $i,i+1$。

**4. 逐步推導**

$$
\frac{\pi_{i+1}}{\pi_i}=\frac{\binom m{i+1}^2}{\binom mi^2}=\left(\frac{m-i}{i+1}\right)^2
$$

說明：$\binom m{i+1}/\binom mi=\frac{m!}{(i+1)!(m-i-1)!}\cdot\frac{i!(m-i)!}{m!}=\frac{m-i}{i+1}$。

$$
\frac{K_{i,i+1}}{K_{i+1,i}}=\frac{(m-i)^2/m^2}{(i+1)^2/m^2}=\left(\frac{m-i}{i+1}\right)^2
$$

說明：§4 的公式，$K_{i+1,i}$ 用 $i+1$ 代入 $K_{k,k-1}=k^2/m^2$。

兩式相等 ⇒ $\pi_{i+1}K_{i+1,i}=\pi_iK_{i,i+1}$。

$\lvert i-j\rvert\ge2$：$K_{ij}=K_{ji}=0$，兩邊都是 0。$i=j$：兩邊相同。所以 detailed balance 對所有 $(i,j)$ 成立，由 §5 得 $\pi K=\pi$。

**5. 關鍵**：組合直覺——均勻分配編號球時，左邊恰 $i$ 紅的配置數是 $\binom mi\binom m{m-i}$。

**6. 考試版**：For adjacent states, $\pi_{i+1}/\pi_i=((m-i)/(i+1))^2=K_{i,i+1}/K_{i+1,i}$, so $\pi_iK_{i,i+1}=\pi_{i+1}K_{i+1,i}$; non-adjacent off-diagonal flows are zero. Detailed balance holds, hence $\pi K=\pi$.

**數值**：$m=4$：$\pi=(1,16,36,16,1)/70$；已用程式驗證 $\pi K=\pi$。

---

## §7 Strict stationarity ⇔ $\nu K=\nu$ [MC p.29, AR1 p.18]

**1. 目標**：homogeneous Markov chain，$X_0\sim\nu$。$(X_h,\dots,X_{h+r})\overset d=(X_0,\dots,X_r)\ \forall r,h$ ⇔ $\nu K=\nu$。

**2. 已知**：§1（有限狀態的聯合分布公式）；§3。

**4. 逐步推導**

(⇒ 必要) 取 $r=0,h=1$：$X_1\overset dX_0$，即 $\nu K=\mathcal L(X_1)=\mathcal L(X_0)=\nu$。

(⇐ 充分) 設 $\nu K=\nu$。

$$
\mathcal L(X_h)=\nu K^h=(\nu K)K^{h-1}=\nu K^{h-1}=\cdots=\nu
$$

說明：§3 加歸納。

$$
P(X_h=i_0,\dots,X_{h+r}=i_r)=P(X_h=i_0)\prod_{k=1}^rK_{i_{k-1}i_k}=\nu_{i_0}\prod_{k=1}^rK_{i_{k-1}i_k}=P(X_0=i_0,\dots,X_r=i_r)
$$

說明：第一個等號是 §1 從時間 $h$ 開始套用（Markov + homogeneous）；第二個用 $\mathcal L(X_h)=\nu$；第三個是 §1 從時間 0。

**5. 關鍵**：充分性同時用到 homogeneity（每段都用同一 $K$）與 $\nu$ invariant（每段起點分布相同）。少一個都不成立。

**6. 考試版**：Necessity: $\mathcal L(X_1)=\nu K$ must equal $\nu$. Sufficiency: $\nu K=\nu$ gives $\mathcal L(X_h)=\nu$ for all $h$, and the block $(X_h,\dots,X_{h+r})$ has law $\nu(dx_0)K(x_0,dx_1)\cdots K(x_{r-1},dx_r)$, identical to that of $(X_0,\dots,X_r)$.

---

## §8 Lazy chain 保留 invariance 並去除週期 [MC p.26]

**1. 目標**：$K_L=\frac12I+\frac12K$。(a) $\pi K=\pi\Rightarrow\pi K_L=\pi$；(b) $K$ irreducible ⇒ $K_L$ irreducible 且 aperiodic。

**4. 推導**

(a) $\pi K_L=\frac12\pi I+\frac12\pi K=\frac12\pi+\frac12\pi=\pi$。

(b) $(K_L)_{ii}\ge\frac12>0$ ⇒ $1\in\lbrace n:K_L^n(i,\lbrace i\rbrace)>0\rbrace$ ⇒ $d(i)=1$。Irreducible：$K_L^n\ge2^{-n}K^n$ 逐元素（展開 $(\frac12I+\frac12K)^n$ 中含 $\frac1{2^n}K^n$ 一項且其餘項非負），所以 $K^n_{ij}>0\Rightarrow(K_L^n)_{ij}>0$。

**6. 考試版**：$\pi K_L=\tfrac12\pi+\tfrac12\pi K=\pi$; self-loops $(K_L)_{ii}\ge1/2$ give period 1, and $K_L^n\ge2^{-n}K^n$ preserves accessibility.

---

## §9 $E_i\tau_i^+=1/\pi_i$ 的數值驗證（首步分析，補充技巧）

教材 [MC p.68] 只陳述結果、未證明。考試若要求驗證，可用首步分析：令 $h_k=E_k[\text{hitting time of }i]$，$h_i=0$，對 $k\ne i$：$h_k=1+\sum_lK_{kl}h_l$；然後 $E_i\tau_i^+=1+\sum_lK_{il}h_l$。

$m=2$、$i=0$：$h_2=1+h_1$、$h_1=1+\frac12h_1+\frac14h_2$ ⇒ $h_1=5$ ⇒ $E_0\tau_0^+=1+h_1=6=1/\pi_0$ ✓。$i=1$：$h_0=h_2=1$ ⇒ $E_1\tau_1^+=1+\frac14+\frac14=\frac32=1/\pi_1$ ✓。

說明：「走一步（+1），再依落點的期望剩餘時間加權平均」——這是全期望公式加 Markov property。

---

## §10 $Kf$ 的算子性質 [MC p.72]

$$
\lvert Kf(x)\rvert=\left\lvert\int f(y)K(x,dy)\right\rvert\le\int\lvert f(y)\rvert K(x,dy)\le\lVert f\rVert_\infty\int K(x,dy)=\lVert f\rVert_\infty .
$$

說明：三角不等式（積分版）；$\lvert f\rvert\le\lVert f\rVert_\infty$；$K(x,\cdot)$ 是機率測度總質量 1。這個 bound 在 AR(1) uniqueness 證明中被用作 dominated convergence 的控制函數 [AR1 p.28]。

線性 $K(cf+dg)=cKf+dKg$：積分線性。$K1=K(x,E)=1$。$f\ge0\Rightarrow Kf\ge0$：非負函數對機率測度積分非負。
