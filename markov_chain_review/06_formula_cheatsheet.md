# 06 公式總表

記號：row-stochastic $K$（列 = 現在，欄 = 下一步）；分布為 row vector；函數為 column vector。推導連結見 [03](03_formula_derivations.md)。

## A. Kernel 與 Markov property

| 名稱 | 公式 | 符號 | 條件 | 用途 | 常見錯誤 |
|---|---|---|---|---|---|
| Transition kernel | $K(x,A)=P(X_{n+1}\in A\mid X_n=x)$ | $x$ 現在狀態；$A$ 目標集合 | (1) $A\mapsto K(x,A)$ 機率測度 (2) $x\mapsto K(x,A)$ 可測 | 定義模型 [MC p.4] | 以為要 $\lvert a\rvert<1$ |
| 有限版 | $K(i,A)=\sum_{j\in A}K_{ij}$ | $K_{ij}=K(i,\lbrace j\rbrace)$ | 列和 1、非負 | 集合機率 [MC p.13] | 加成一欄 |
| Density 版 | $K(x,A)=\int_Ak(x,y)\,dy$ | $k$ transition density | $k\ge0$、$\int k\,dy=1$ | 連續狀態 [MC p.37] | $K(x,\lbrace y\rbrace)=k(x,y)$（錯，應為 0） |
| Markov property | $P(X_{n+1}\in A\mid\mathcal F_n)=P(X_{n+1}\in A\mid X_n)$ a.s. | $\mathcal F_n=\sigma(X_0,\dots,X_n)$ | | 判斷是否 Markov [MC p.8] | 說成「未來與過去獨立」 |
| Homogeneity | $P(X_{n+1}\in A\mid\mathcal F_n)=K(X_n,A)$，同一 $K$ 所有 $n$ | | | [MC p.9] | 依賴 $x$ 誤判為 inhomogeneous |
| Path probability | $P(X_0=i_0,\dots,X_n=i_n)=\nu_{i_0}\prod_{k=1}^nK_{i_{k-1}i_k}$ | | Markov + homogeneous | likelihood、stationarity 證明 | 忘了 $\nu_{i_0}$ |

## B. 多步、初始分布、期望

| 名稱 | 公式 | 條件 | 用途 | 常見錯誤 |
|---|---|---|---|---|
| n-step kernel | $K^r(x,A)=P_x(X_r\in A)$，$K^0(x,A)=\mathbf 1_A(x)$ | homogeneous | [MC p.20] | |
| 矩陣乘法 | $(K^2)_{ij}=\sum_zK_{iz}K_{zj}$ | | [MC p.21] | $(K^2)_{ij}=K_{ij}^2$ |
| Chapman–Kolmogorov | $K^{r+s}(x,A)=\int_EK^s(z,A)K^r(x,dz)$；$(K^{r+s})_{ij}=\sum_z(K^r)_{iz}(K^s)_{zj}$ | homogeneous Markov | 拆步數 [MC p.46] | 忘了對 $z$ 加總 |
| Density C–K | $k_2(x,y)=\int k(x,z)k(z,y)\,dz$ | | [MC p.46] | |
| 分布更新 | $(\nu K)_j=\sum_i\nu_iK_{ij}$；$(\nu K)(A)=\int K(x,A)\nu(dx)$ | | [MC p.22, 27] | $K\nu$ |
| 時間 $n$ 分布 | $\mathcal L(X_n)=\nu K^n$ | | [MC p.22] | |
| 條件期望算子 | $(Kf)(i)=\sum_jf(j)K_{ij}=E[f(X_{n+1})\mid X_n=i]$ | $f$ 有界或可積 | [MC p.23, 48] | 函數放左邊 |
| 算子性質 | $K1=1$；$f\ge0\Rightarrow Kf\ge0$；$\lVert Kf\rVert_\infty\le\lVert f\rVert_\infty$ | | DCT 控制 [MC p.72] | |

## C. Bernoulli–Laplace urn（$m$ 紅 $m$ 藍）

| 名稱 | 公式 | 來源 |
|---|---|---|
| 上升 | $K_{i,i+1}=\frac{(m-i)^2}{m^2}$ | MC p.17 |
| 下降 | $K_{i,i-1}=\frac{i^2}{m^2}$ | MC p.17 |
| 不變 | $K_{ii}=\frac{2i(m-i)}{m^2}$ | MC p.17 |
| $m=2$ 矩陣 | $K=\begin{pmatrix}0&1&0\\1/4&1/2&1/4\\0&1&0\end{pmatrix}$；$K^2=\begin{pmatrix}1/4&1/2&1/4\\1/8&3/4&1/8\\1/4&1/2&1/4\end{pmatrix}$ | MC p.12, 21 |
| Invariant | $\pi_i=\binom mi^2/\binom{2m}m$；$m=2$：$(1/6,2/3,1/6)$ | MC p.32 |
| 平均回返 | $E_i\tau_i^+=1/\pi_i$；$m=2$：$(6,3/2,6)$ | MC p.68 |
| Restoring mean | $E[X_{n+1}-\frac m2\mid X_n=i]=(1-\frac2m)(i-\frac m2)$ | MC p.69 |

## D. 結構：irreducibility、period、recurrence

| 名稱 | 公式 | 條件/備註 | 來源 |
|---|---|---|---|
| Accessible | $K^n(i,\lbrace j\rbrace)>0$ for some $n\ge0$ | 看多步 | MC p.24 |
| Irreducible | 每對狀態 communicate | | MC p.24 |
| ψ-irreducible | $\psi(A)>0\Rightarrow K^n(x,A)>0$ some $n\ge1$，$\forall x$ | AR(1) 用 $\lambda$ | MC p.54 |
| Period | $d(i)=\gcd\lbrace n\ge1:K^n(i,\lbrace i\rbrace)>0\rbrace$ | irreducible ⇒ 全部相同 | MC p.25 |
| Lazy chain | $K_L=\frac12\mathbf 1_A(x)+\frac12K(x,A)$ | 去週期、保 $\pi$ | MC p.26 |
| Return time | $\tau_i^+=\inf\lbrace n\ge1:X_n=i\rbrace$ | | MC p.68 |
| Recurrent / transient | $P_i(\tau_i^+<\infty)=1$ / $<1$ | | MC p.68 |
| Positive / null | $E_i\tau_i^+<\infty$ / $=\infty$（recurrent 前提） | finite irreducible ⇒ positive | MC p.68 |
| Harris recurrent | $P_x(\tau_A^+<\infty)=1$，$\forall x$，$\forall A:\lambda(A)>0$ | positive Harris = 加 invariant prob. | AR1 p.26 |

## E. 平衡

| 名稱 | 公式 | 條件 | 來源 | 常見錯誤 |
|---|---|---|---|---|
| Invariant | $\pi K=\pi$；$\pi_j=\sum_i\pi_iK_{ij}$，$\pi_i\ge0$，$\sum\pi_i=1$ | | MC p.27 | 漏正規化；用 $K\pi^\top$ |
| Strict stationarity | $(X_h,\dots,X_{h+r})\overset d=(X_0,\dots,X_r)$ | | MC p.29 | 只檢查邊際 |
| Stationary ⇔ | strictly stationary $\iff\nu K=\nu$ | homogeneous Markov | MC p.29 | |
| Reversible | $(X_0,\dots,X_r)\overset d=(X_r,\dots,X_0)$ under $P_\pi$ | | MC p.30 | |
| Detailed balance | $\pi_iK_{ij}=\pi_jK_{ji}$；$\int_AK(x,B)\pi(dx)=\int_BK(y,A)\pi(dy)$；$\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$ | | MC p.30, 53 | 檢查 $K_{ij}=K_{ji}$ |
| DB ⇒ invariant | $\sum_i\pi_iK_{ij}=\pi_j\sum_iK_{ji}=\pi_j$ | 充分不必要 | MC p.31 | 反向推論 |

## F. 收斂與遍歷

| 名稱 | 公式 | 條件 | 來源 |
|---|---|---|---|
| 有限收斂定理 | $K^n(i,\lbrace j\rbrace)\to\pi_j$ | finite + irreducible + aperiodic（充分） | MC p.33 |
| Weak convergence | $\mu_n\Rightarrow\mu\iff\int fd\mu_n\to\int fd\mu$，$f$ 有界連續 | | MC p.56 |
| Total variation | $\lVert\mu-\nu\rVert_{TV}=\sup_A\lvert\mu(A)-\nu(A)\rvert=\frac12\int\lvert p-q\rvert$ | | AR1 p.30 |
| Geometric ergodic | $\lVert K^n(x,\cdot)-\pi\rVert_{TV}\le M(x)\rho^n$ | AR(1)：$\rho=\lvert a\rvert$ | AR1 p.30 |
| Not uniform | $\sup_x\lVert K^n(x,\cdot)-\pi\rVert_{TV}=1$ | AR(1) $0<\lvert a\rvert<1$ | AR1 p.46 |
| Ergodic average（有限） | $\frac1N\sum_{n<N}\mathbf 1_{\lbrace j\rbrace}(X_n)\to\pi_j$ a.s. | finite irreducible | MC p.73 |
| Ergodic theorem（AR(1)） | $\frac1N\sum_{n<N}f(X_n)\to\int fd\pi$ a.s. | $\int\lvert f\rvert d\pi<\infty$，$\lvert a\rvert<1$ | AR1 p.32 |

## G. Gaussian AR(1)：$X_{n+1}=aX_n+\sigma Z_{n+1}$

| 名稱 | 公式 | 條件 | 來源 |
|---|---|---|---|
| 一步 kernel | $K(x,\cdot)=N(ax,\sigma^2)$；$k(x,y)=\frac1{\sigma\sqrt{2\pi}}e^{-(y-ax)^2/(2\sigma^2)}$ | 任何 $a$，$\sigma>0$ | MC p.37 |
| 區間 | $K(x,(b,c])=\Phi(\frac{c-ax}\sigma)-\Phi(\frac{b-ax}\sigma)$ | | MC p.40 |
| $Kf$ | $Kf(x)=E[f(ax+\sigma Z)]$；$Ky=ax$；$Ky^2=a^2x^2+\sigma^2$ | | AR1 p.10 |
| n-step | $X_n=a^nx+\sigma\sum_{j=1}^na^{n-j}Z_j$；$K^n(x,\cdot)=N(a^nx,\sigma^2\sum_{k=0}^{n-1}a^{2k})$ | | AR1 p.12 |
| n-step（穩定） | $N\big(a^nx,\frac{\sigma^2(1-a^{2n})}{1-a^2}\big)$ | $\lvert a\rvert<1$ | MC p.56 |
| Invariant | $\pi=N\big(0,\frac{\sigma^2}{1-a^2}\big)$；$v=a^2v+\sigma^2$ | $\lvert a\rvert<1$ | MC p.50 |
| 非零平均 | $X_{n+1}=\mu+a(X_n-\mu)+\sigma Z$ ⇒ $\pi=N(\mu,\frac{\sigma^2}{1-a^2})$ | | MC p.71 |
| 隨機初始 | $X_0\sim N(m_0,s_0^2)$ ⇒ $X_1\sim N(am_0,a^2s_0^2+\sigma^2)$ | $X_0\perp Z_1$ | MC p.49 |
| 相關 | $\mathrm{Cov}_\pi(X_0,X_h)=va^h$；$\mathrm{Corr}_\pi=a^h$ | stationary | AR1 p.20 |
| Reversibility | $(1-a^2)x^2+(y-ax)^2=x^2+y^2-2axy$ | $\lvert a\rvert<1$，含 $a<0$ | AR1 p.22 |
| Coupling | $X_n-Y_n=a^n(X_0-Y_0)$ | 同一雜訊 | AR1 p.29 |
| 樣本平均恆等式 | $\bar X_N=\frac\sigma{1-a}\frac1N\sum_{j=1}^NZ_j-\frac{X_N-X_0}{(1-a)N}$ | $a\ne1$ | AR1 p.33 |
| 平均的變異數 | $\mathrm{Var}_\pi(\bar X_N)=\frac vN[1+2\sum_{h=1}^{N-1}(1-\frac hN)a^h]$ | stationary | AR1 p.34 |
| 漸近變異數 | $N\mathrm{Var}_\pi(\bar X_N)\to v\frac{1+a}{1-a}=\frac{\sigma^2}{(1-a)^2}$；CLT $\sqrt N\bar X_N\Rightarrow N(0,\frac{\sigma^2}{(1-a)^2})$ | | AR1 p.34, 50 |
| Random walk | $a=1$：$K^n(x,\cdot)=N(x,n\sigma^2)$ | 無平衡 | AR1 p.36 |
| 特徵函數 | $\chi(t)=\chi(at)e^{-\sigma^2t^2/2}$ | invariant 存在時 | AR1 p.47 |
| OU 取樣 | $a=e^{-\kappa\Delta}$，$\sigma^2=\frac{\eta^2}{2\kappa}(1-e^{-2\kappa\Delta})$ | | MC p.63 |

## H. MCMC

| 名稱 | 公式 | 條件 | 來源 |
|---|---|---|---|
| Metropolis | $\alpha=\min\big(1,\frac{\pi(Y)}{\pi(X_n)}\big)$ | $q$ 對稱 | H10 p.6 |
| MH | $\alpha=\min\big(1,\frac{\pi(Y)q(X_n\mid Y)}{\pi(X_n)q(Y\mid X_n)}\big)$ | 一般 $q$ | H10 p.12 |
| MH kernel | $K(x,dy)=q(y\mid x)\alpha(x,y)dy+r(x)\delta_x(dy)$ | 補充 | 03c §1 |
| Gibbs | $x_i^{(t+1)}\sim p(x_i\mid x_{1:i-1}^{(t+1)},x_{i+1:d}^{(t)})$ | 條件分布可抽 | H10 p.18 |
| Bivariate cond. | $X\mid Y=y\sim N\big(\mu_X+\frac{\sigma_{XY}}{\sigma_Y^2}(y-\mu_Y),\sigma_X^2-\frac{\sigma_{XY}^2}{\sigma_Y^2}\big)$ | | H10 p.27 |
| MALA proposal | $Y=X_n+\frac{\epsilon^2}2\nabla\log\pi(X_n)+\epsilon Z$ | 可微 | H10 p.30 |
| MALA（std normal） | $\alpha=\min\big(1,e^{\epsilon^2(x^2-y^2)/8}\big)$ | 補充 | 03c §4 |
| Hamiltonian | $H(x,p)=U(x)+\frac12p^\top M^{-1}p$，$U=-\log\pi$ | | H10 p.45 |
| Hamilton eq. | $\dot x=M^{-1}p$，$\dot p=-\nabla U(x)$ | | H10 p.46 |
| Leapfrog | $p_{t+\epsilon/2}=p_t-\frac\epsilon2\nabla U(x_t)$；$x_{t+\epsilon}=x_t+\epsilon M^{-1}p_{t+\epsilon/2}$；$p_{t+\epsilon}=p_{t+\epsilon/2}-\frac\epsilon2\nabla U(x_{t+\epsilon})$ | | H10 p.46 |
| HMC accept | $\alpha=\min\big(1,e^{-H(x^*,p^*)+H(x,p)}\big)$ | | H10 p.47 |
| NUTS stop | $(x^+-x^-)^\top p^-<0$ 或 $(x^+-x^-)^\top p^+<0$ | | H10 p.62 |
| RJMCMC | $\alpha=\min\big(1,\frac{p(\text{data}\mid\theta_{k'})p(\theta_{k'}\mid M_{k'})p(M_{k'})}{p(\text{data}\mid\theta_k)p(\theta_k\mid M_k)p(M_k)}\lvert\det J\rvert\big)$ | 投影片簡化式 | H10 p.69 |
| Burn-in | $E[f(X_n)]\ne\int f\pi$（小 $n$）；$X_n\overset d\to\pi$ | | H10 p.40 |

## I. 二狀態鏈估計（H8 Example 3.14）

| 名稱 | 公式 |
|---|---|
| 轉移計數 | $m_{ij}=\sum_{t=1}^{m-1}\mathbf 1(X_t=i,X_{t+1}=j)$ |
| Likelihood | $L\propto p_1^{m_{11}}(1-p_1)^{m_{12}}(1-p_2)^{m_{21}}p_2^{m_{22}}$ |
| MLE | $\hat p_1=\frac{m_{11}}{m_{11}+m_{12}}$，$\hat p_2=\frac{m_{22}}{m_{21}+m_{22}}$ |
| Posterior | $\propto L\cdot\mathbf 1\lbrace p_1+p_2<1\rbrace$ = $\mathrm{Be}(m_{11}+1,m_{12}+1)\times\mathrm{Be}(m_{22}+1,m_{21}+1)$ 截斷 |

---

## 相似公式比較

| 公式對 | 差別 |
|---|---|
| $\pi K=\pi$ vs $K\mathbf 1=\mathbf 1$ | 前者 left eigenvector（分布，要解）；後者 right eigenvector（列和為 1，自動成立） |
| $\pi_iK_{ij}=\pi_jK_{ji}$ vs $\sum_i\pi_iK_{ij}=\pi_j$ | 逐對平衡（強） vs 總量平衡（弱） |
| $K^n(x,\cdot)\to\pi$ vs $\frac1N\sum f(X_n)\to\pi(f)$ | 分布 vs 時間平均；後者在 periodic 時仍成立 |
| Metropolis vs MH ratio | 有無 $q(X_n\mid Y)/q(Y\mid X_n)$ |
| $\mathrm{Var}(\bar X)=v/N$ vs $\sigma^2/((1-a)^2N)$ | i.i.d. vs 相依（AR(1)） |
| $N(ax,\sigma^2)$ vs $N(a^nx,\sigma^2\sum a^{2k})$ | 一步 vs $n$ 步 |
| $N(0,\sigma^2)$（$a=0$）vs $N(0,\sigma^2/(1-a^2))$ | 特例 vs 一般 invariant |

## 數學符號規範

| 符號對 | 區分 |
|---|---|
| $P(X=x)$ vs $P(X=x\mid Y=y)$ | 邊際 vs 條件 |
| $K_{ij}$ vs $(K^n)_{ij}$ | 一步 vs $n$ 步；後者不是 $K_{ij}^n$ |
| $K$ vs $K^n$ | 規則 vs $n$ 次合成 |
| $\nu$ vs $\pi$ | 任意初始 vs invariant |
| $E$（state space）vs $x$（state） | 集合 vs 元素 |
| Stationary distribution vs limiting distribution | 方程解 vs 極限 |
| $E_x[\cdot]$ vs $P_x(\cdot)$ | 期望 vs 機率（$P_x(A)=E_x\mathbf 1_A$） |
| Row vector $\nu$ vs column vector $f$ | 放 $K$ 左邊 vs 右邊 |
| $K(x,A)$（kernel）vs $K(p)$（HMC 動能） | 同字母不同物件 |
| $\sigma$ vs $\sigma^2$ | 標準差（標準化用） vs 變異數 |
