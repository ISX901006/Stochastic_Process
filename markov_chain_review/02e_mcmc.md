# 02e Ch9：Markov Chain Monte Carlo（MCMC）與 Transition Matrix 估計

來源：[H10 全部]、[H8 p.19–20, 40–48]、[H9 p.10–11]

---

## 2e.1 為什麼需要 MCMC

### Step 1 直覺

前面的問題是「**給定** $K$，求它的 $\pi$」。MCMC 反過來：「**給定**想要的分布 $\pi$（例如 Bayesian posterior，只知道到一個常數），**設計**一個 $K$ 使 $\pi$ 是它的 invariant law」，然後跑一條長鏈，用 ergodic average 近似 $E_\pi f$。

H9 p.10–11 的動機：posterior 的正規化常數或邊際積分算不出來，於是「draw samples from $\pi(\theta\mid x)$ via MCMC，再取樣本中位數/平均」。

### 理論骨架（把前面章節串起來）

1. 設計 $K$ 使 detailed balance $\pi(x)K(x,y)=\pi(y)K(y,x)$ 成立 ⇒ $\pi K=\pi$（[02c §5](02c_structure_equilibrium_convergence.md)）。
2. 確保 irreducible + aperiodic ⇒ $\mathcal L(X_n)\to\pi$ [H10 p.15, 18]。
3. 用 ergodic theorem：$\frac1N\sum f(X_n)\to E_\pi f$（[02c §8](02c_structure_equilibrium_convergence.md)、[02d §10](02d_gaussian_ar1.md)）。
4. 樣本相依 ⇒ 變異數比 i.i.d. 大（AR(1) 例：$\sigma^2/(1-a)^2$ vs $v$），所以 proposal 的設計影響效率。

與 classical Monte Carlo 的對照 [H8 p.19]：classical MC 用 i.i.d. 樣本，$\mathrm{Var}(\hat h_m)=\mathrm{Var}(h(X))/m$；MCMC 樣本相依，只能用 ergodic theorem 而非 SLLN。

---

## 2e.2 Metropolis Algorithm（1953）[H10 p.4–9]

### Step 1 直覺

在山區（$\pi$ 的高度）隨機走路：每次往隨機方向試一步。若新位置比較高（$\pi(Y)\ge\pi(X_n)$）就一定去；若比較低，以比例 $\pi(Y)/\pi(X_n)$ 的機率去，否則留在原地。結果：停留在每個區域的時間比例 ∝ 該區高度。

「Accept good moves and **sometimes** bad moves」——有時接受壞移動才能探索整個空間 [H10 p.4]。

### Step 2 演算法 [H10 p.6]

1. Initialize $X_0$.
2. For $n=0,1,2,\dots$：
   1. Propose $Y\sim q(Y\mid X_n)$，**symmetric**：$q(y\mid x)=q(x\mid y)$（例如 $Y=X_n+\epsilon$，$\epsilon\sim N(0,\sigma^2)$）。
   2. $\alpha(X_n,Y)=\min\!\left(1,\dfrac{\pi(Y)}{\pi(X_n)}\right)$。
   3. $X_{n+1}=Y$ w.p. $\alpha$；否則 $X_{n+1}=X_n$。

| 項目 | 說明 |
|---|---|
| $q(Y\mid X_n)$ | Proposal：從現在位置提議下一個候選 |
| $\alpha$ | Acceptance probability |
| 為什麼只用比值 | 正規化常數在比值中消掉，所以 $\pi$ 只需知道到常數倍 |
| 正確性 | 對稱 $q$ ⇒ detailed balance ⇒ $\pi$ invariant（[03c §1](03c_derivations_mcmc.md)） |
| 拒絕時 | **留在原地也要記錄**一個樣本（$X_{n+1}=X_n$），這是 kernel 在 $x$ 有點質量的原因 |

### Step 5 數值

Target $\pi(x)\propto e^{-x^2/2}$，$X_n=0$，提議 $Y=1$：$\alpha=\min(1,e^{-1/2})\approx0.6065$。若 $U=0.4<0.6065$ ⇒ 接受，$X_{n+1}=1$；若 $U=0.8$ ⇒ 拒絕，$X_{n+1}=0$。反向從 1 提議 0：$\alpha=\min(1,e^{1/2})=1$，必接受。

**Discussion** [H10 p.9]：proposal 決定效率；高維或多峰時 mixing 慢。

---

## 2e.3 Metropolis–Hastings（1970）[H10 p.10–15]

### Step 1 直覺

如果 proposal 本身偏向某方向（例如總是往右偏 0.5），往右的提議「被給了太多機會」，必須在接受率中扣回來，這就是 Hastings correction $q(X_n\mid Y)/q(Y\mid X_n)$。

### Step 2 演算法 [H10 p.12]

$$
\alpha(X_n,Y)=\min\!\left(1,\frac{\pi(Y)\,q(X_n\mid Y)}{\pi(X_n)\,q(Y\mid X_n)}\right).
$$

- $q$ 對稱時退化為 Metropolis。
- Ensures detailed balance and convergence to $\pi$（若 chain irreducible + aperiodic [H10 p.15]）。

### Step 5 數值（asymmetric proposal $q(y\mid x)=N(y;x+0.5,1)$）[H10 p.13]

$X_n=0$、$Y=1$：$q(1\mid0)=\varphi(0.5)$、$q(0\mid1)=\varphi(0-1.5)=\varphi(1.5)$。

$$
\frac{\pi(1)q(0\mid1)}{\pi(0)q(1\mid0)}=e^{-1/2}\cdot\frac{e^{-1.5^2/2}}{e^{-0.5^2/2}}=e^{-0.5}e^{-1}=e^{-1.5}\approx0.2231 .
$$

### 反例

若用這個往右偏的 proposal 卻**忘了** Hastings correction（只用 $\pi(Y)/\pi(X_n)$），detailed balance 失敗，鏈的 invariant law 會偏向右邊，不再是 $N(0,1)$。

---

## 2e.4 Gibbs Sampling [H10 p.16–23]

### Step 1 直覺

多維分布不好整體抽，但「固定其他座標，只抽一個座標」很容易。輪流更新每個座標，就像一次只調整一個旋鈕。

### Step 2 演算法 [H10 p.18]

For $t=0,\dots,T-1$，for $i=1,\dots,d$：

$$
x_i^{(t+1)}\sim p\big(x_i\mid x_1^{(t+1)},\dots,x_{i-1}^{(t+1)},x_{i+1}^{(t)},\dots,x_d^{(t)}\big).
$$

注意已更新的座標用**新**值 $(t+1)$，尚未更新的用舊值 $(t)$。

- Special case of MH，acceptance probability 恆為 1（no rejection step）[H10 p.16, 22]；證明見 [03c §2](03c_derivations_mcmc.md)。
- Converges to the joint distribution if the chain is irreducible and aperiodic [H10 p.18]。
- 缺點：強相關或多峰時 mixing 慢 [H10 p.22–23]。

### Bivariate normal 例子 [H10 p.19, 24–28]

$(X,Y)\sim N\!\left(\begin{pmatrix}0\\0\end{pmatrix},\begin{pmatrix}1&0.8\\0.8&1\end{pmatrix}\right)$。一般條件分布：

$$
X\mid Y=y\sim N\!\left(\mu_X+\frac{\sigma_{XY}}{\sigma_Y^2}(y-\mu_Y),\ \sigma_X^2-\frac{\sigma_{XY}^2}{\sigma_Y^2}\right)\ \Rightarrow\ X\mid Y=y\sim N(0.8y,0.36),\quad Y\mid X=x\sim N(0.8x,0.36).
$$

（$1-0.8^2=0.36$。）程式中 `sigma = sqrt(1-rho**2)=0.6` 是**標準差** [H10 p.20]。推導見 [03c §3](03c_derivations_mcmc.md)。

**與 AR(1) 的連結**（本文件補充觀察）：只看 $X$ 座標，$X^{(t+1)}=0.8\,Y^{(t)}+0.6\,\epsilon$、$Y^{(t)}=0.8X^{(t)}+0.6\,\epsilon'$，合併得 $X^{(t+1)}=0.64X^{(t)}+\text{Gaussian noise}$，正是 AR(1)，$a=\rho^2=0.64$。相關越強（$\rho\to1$），$a\to1$，mixing 越慢——數學上解釋了「strong dependencies 使 Gibbs 變慢」。

---

## 2e.5 MALA [H10 p.29–37]

目標 $\pi(x)\propto e^{-U(x)}$。Proposal 用梯度把候選推向高機率區（離散化的 Langevin diffusion）：

$$
Y=X_n+\frac{\epsilon^2}{2}\nabla\log\pi(X_n)+\epsilon Z,\quad Z\sim N(0,I);\qquad q(Y\mid X_n)=N\!\left(Y;X_n+\tfrac{\epsilon^2}{2}\nabla\log\pi(X_n),\epsilon^2I\right).
$$

Proposal **不對稱**，所以用完整 MH ratio [H10 p.31]：

$$
\alpha=\min\!\left(1,\frac{\pi(Y)q(X_n\mid Y)}{\pi(X_n)q(Y\mid X_n)}\right).
$$

Standard normal：$\nabla\log\pi(x)=-x$，$Y=X_n-\frac{\epsilon^2}{2}X_n+\epsilon Z$ [H10 p.32]。

（H10 p.33 程式最後一行被截斷，應為除以 `epsilon**2`；見 [00 §1.1](00_course_map.md)。）

Discussion [H10 p.36]：梯度減少 random-walk 行為；步長 $\epsilon$ 影響接受率；需要 target 可微；是 HMC 的前身。

---

## 2e.6 Burn-in [H10 p.38–43]

### 直覺

從很不可能的地方（例如 $X_0=5$，目標 $N(0,1)$）出發，前面的樣本仍被起點影響，還沒「忘記」起點。丟掉前 $B$ 個樣本叫 burn-in。

### 數學表述 [H10 p.40]

小 $n$ 時 $E[f(X_n)]\ne\int f\pi$；$n\to\infty$ 時 $X_n\overset d\to\pi$。丟掉前 $B$ 個可降低估計偏差。

與 AR(1) 理論的連結：$E_x[X_n]=a^nx$，偏差以 $\lvert a\rvert^n$ 遞減；但 [AR1 p.46] 指出收斂**不是 uniform**——起點越遠，需要的 burn-in 越長，沒有一個對所有起點都夠用的固定 $B$。

### 實務 [H10 p.42]
- Trace plot 目視；rule of thumb 丟 10%–30%；多條不同起點的鏈驗證收斂。
- Burn-in 降低偏差但**不消除 autocorrelation**；必要時 thinning。

**常見誤解**：H10 p.38 說 burn-in 讓估計「unbiased」——嚴格而言只是**減少**初始偏差。

---

## 2e.7 Hamiltonian Monte Carlo [H10 p.44–58]

### 直覺

把 $U(x)=-\log\pi(x)$ 想成地形高度，放一顆有動量的球在上面滑。球沿等能量線長距離移動，提議點遠但仍在高機率區，所以接受率高 [H10 p.45, 48]。

### 定義

$$
K(p)=\tfrac12p^\top M^{-1}p,\qquad H(x,p)=U(x)+K(p),
$$

$$
\frac{dx}{dt}=\frac{\partial H}{\partial p}=M^{-1}p,\qquad\frac{dp}{dt}=-\frac{\partial H}{\partial x}=-\nabla U(x).
$$

（這裡的 $K(p)$ 是 kinetic energy，**不是** transition kernel。）

Leapfrog [H10 p.46]：

$$
p_{t+\epsilon/2}=p_t-\tfrac\epsilon2\nabla U(x_t),\quad x_{t+\epsilon}=x_t+\epsilon M^{-1}p_{t+\epsilon/2},\quad p_{t+\epsilon}=p_{t+\epsilon/2}-\tfrac\epsilon2\nabla U(x_{t+\epsilon}).
$$

演算法 [H10 p.47]：抽 $p\sim N(0,M)$；$L$ 步 leapfrog 得 $(x^*,p^*)$；

$$
\alpha=\min\!\big(1,\exp[-H(x^*,p^*)+H(x,p)]\big);
$$

接受則 $x_{n+1}=x^*$，否則 $x_{n+1}=x_n$。動量重抽保證 ergodicity；leapfrog 保持 reversibility 與 detailed balance。

### Metropolis vs HMC [H10 p.52]

| 面向 | Metropolis | HMC |
|---|---|---|
| Proposal | 隨機局部移動 | Hamiltonian 決定性軌跡 |
| 梯度 | 不用 | 用 $\nabla U$ |
| 步長 | 小（random walk） | 大 |
| 接受率 | 中到低 | 高（常見 60–90%） |
| 高維效率 | 迅速變差 | 平滑時良好 |
| 調參 | $\sigma^2$ | $\epsilon$、$L$、$M$ |

Banana posterior 例子 [H10 p.54]：$y=x_2+0.25x_1^2+\varepsilon$，$x_1\sim N(0,4)$，$y_{obs}=2$：

$$
p(x_1,x_2\mid y_{obs})\propto\exp\!\left[-\tfrac12\cdot\tfrac{x_1^2}{4}-\tfrac12(x_2+0.25x_1^2-2)^2\right].
$$

非線性強相關，random walk 沿彎曲山谷移動很慢 [H10 p.55]。

---

## 2e.8 NUTS [H10 p.59–66]

HMC 的「U-turn problem」：$L$ 太短 → 樣本高度相關；$L$ 太長 → 軌跡折返浪費計算；事先不知道正確 $L$ [H10 p.61]。NUTS（Hoffman & Gelman 2014）自動選軌跡長度、warm-up 調步長；停止條件 [H10 p.62]：

$$
(x^+-x^-)^\top p^-<0\quad\text{or}\quad(x^+-x^-)^\top p^+<0 .
$$

步驟 [H10 p.63]：隨機選方向 → 遞迴建 leapfrog 二元樹 → 偵測 U-turn 停止 → 用 slice sampling 從軌跡選樣本。Stan、PyMC、NumPyro、TFP 採用。

---

## 2e.9 RJMCMC [H10 p.67–73]

模型集合 $\lbrace M_k\rbrace$，參數 $\theta_k\in\mathbb R^{d_k}$（維度不同）。目標 $p(M_k,\theta_k\mid\text{data})\propto p(\text{data}\mid\theta_k,M_k)p(\theta_k\mid M_k)p(M_k)$。

- Within-model move：一般 MH。
- Between-model move：$(\theta_k,u)\to\theta_{k'}$（$u$ 為輔助變數，用來配對維度），

$$
\alpha=\min\!\left(1,\frac{p(\text{data}\mid\theta_{k'},M_{k'})p(\theta_{k'}\mid M_{k'})p(M_{k'})}{p(\text{data}\mid\theta_k,M_k)p(\theta_k\mid M_k)p(M_k)}\lvert\det J\rvert\right)
$$

（投影片寫 $\det J$；$J$ 是變換的 Jacobian。）例：未知元件數的 Gaussian mixture，split/merge 改變維度 [H10 p.71]。完整的一般式還包含 proposal 機率與 $u$ 的密度，教材簡化未列出。

---

## 2e.10 演算法總覽 [H10 p.2–3]

| 類別 | 演算法 | 核心想法 | 典型用途 |
|---|---|---|---|
| Classical | Metropolis | 依密度比接受/拒絕，對稱 proposal | 簡單目標 |
| | Metropolis–Hastings | 非對稱 proposal 修正 | 通用 |
| | Gibbs | 依序從條件分布抽 | Bayesian hierarchical |
| Gradient | MALA | 離散 Langevin | 連續高維 |
| | HMC | Hamiltonian 動力學 | 大規模 Bayesian |
| | NUTS | 自適應 HMC | 自動化 |
| Adaptive | Adaptive Metropolis、RWM with covariance adaptation | 依過去樣本調 proposal 共變異 | 高維 |
| Population | SMC、Particle MCMC、Parallel tempering、Population MCMC | 多粒子/多溫度鏈互動 | 多峰、state-space |

（Adaptive 與 population 類只在 H10 p.3 表格列出，未展開。）

---

## 2e.11 H8 Example 3.14：Transition Matrix 的 Bayesian 估計 [H8 p.40–48]

二狀態鏈 $T=\begin{pmatrix}p_1&1-p_1\\1-p_2&p_2\end{pmatrix}$，假設 $p_1+p_2<1$。觀察 $X_1,\dots,X_m$。

**轉移計數**：$m_{ij}=\sum_{t=1}^{m-1}\mathbf 1(X_t=i,X_{t+1}=j)$；$D=(m_{11},m_{12},m_{21},m_{22})$ 是 sufficient statistic。

**Likelihood**（Markov property 的直接應用）：

$$
L(p_1,p_2)\propto p_1^{m_{11}}(1-p_1)^{m_{12}}(1-p_2)^{m_{21}}p_2^{m_{22}} .
$$

**Prior** 均勻於三角形：$\pi(p_1,p_2)=2\,\mathbf 1\lbrace p_1+p_2<1\rbrace$。**Posterior**：

$$
\pi(p_1,p_2\mid D)\propto p_1^{m_{11}}(1-p_1)^{m_{12}}(1-p_2)^{m_{21}}p_2^{m_{22}}\mathbf 1\lbrace p_1+p_2<1\rbrace,
$$

即 $\mathrm{Be}(m_{11}+1,m_{12}+1)\times\mathrm{Be}(m_{22}+1,m_{21}+1)$ 限制在三角形上；截斷引入相依。

**抽樣策略比較**（資料 $(68,28,17,4)$，$\Pr(p_1+p_2<1\mid D)=0.21$）：
- 直接抽兩個 Beta 再拒絕：只有 21% 被接受，低效。
- $\pi_1$：Dirichlet$(m_{11}+1,m_{22}+1,m_{12}+m_{21}+1)$——比值 $\pi/\pi_1$ 無界，**變異數無限**，最差。
- $\pi_2$：Geweke 的 normal approximation（截斷常態，$\hat p_i=m_{ii}/(m_{ii}+m_{i(3-i)})$）——比值期望有限。
- $\pi_3$：保留 $p_1\sim\mathrm{Be}(m_{11}+1,m_{12}+1)$，$p_2\mid p_1$ 換成 $\frac{2}{(1-p_1)^2}p_2\mathbf 1\lbrace p_2<1-p_1\rbrace$；權重 $w\propto p_2^{m_{22}-1}(1-p_2)^{m_{21}}(1-p_1)^2$ 有界——**最佳**。

這是 importance sampling 題目，但 likelihood 的寫法是 Markov chain 的重點：**path probability = 初始機率 × 每一步轉移機率的乘積**，依計數分組即得上式。推導見 [03c §6](03c_derivations_mcmc.md)。

---

## 2e.12 Step 6 考試重點

**必背**：Metropolis 與 MH 的 $\alpha$；Gibbs 的更新順序（新值/舊值）；bivariate normal 條件分布；MALA proposal；$H(x,p)$、Hamilton 方程、leapfrog、HMC 的 $\alpha$；NUTS 停止條件；burn-in 的目的與限制。

**必須理解才能推導**：MH 為何滿足 detailed balance；為何只需 $\pi$ 的比值；Gibbs 接受率為 1；Example 3.14 likelihood 來自 Markov property。

**必會計算**：給定 $X_n,Y$ 算 $\alpha$；寫 bivariate normal 條件分布；寫二狀態鏈的 likelihood 與 MLE $\hat p_1=m_{11}/(m_{11}+m_{12})$。

**易混**：proposal $q$ vs 實際 transition kernel $K$（$K$ 包含拒絕時留在原地的機率）；HMC 的 $K(p)$ vs transition kernel；burn-in vs thinning。

**失分點**：非對稱 proposal 忘了 Hastings correction；拒絕時沒有重複記錄現在值；把 Gibbs 的條件分布寫成 $p(x_i\mid x_i)$；把變異數 0.36 當成標準差。
