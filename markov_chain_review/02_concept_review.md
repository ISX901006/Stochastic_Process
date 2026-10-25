# 02 概念總複習：章節索引與易混淆概念比較

## 章節索引（依課程順序）

| 章 | 檔案 | 主題 | 主要來源 |
|---|---|---|---|
| Ch1–2 | [02a](02a_kernel_markov_property.md) | Stochastic process、state space、transition kernel 兩條件、Markov property、time homogeneity、非 Markov 反例 | MC p.4–9, 16；AR1 p.3–5, 9 |
| Ch3–4 | [02b](02b_finite_chain_bernoulli_laplace.md) | Transition matrix、Bernoulli–Laplace、$K(i,A)$、路徑生成、$K^n$、Chapman–Kolmogorov、$\nu K^n$、$Kf$ | MC p.10–23 |
| Ch6–8（有限） | [02c](02c_structure_equilibrium_convergence.md) | Irreducibility、period、lazy chain、invariant、stationarity、detailed balance、recurrence、$E_i\tau_i^+=1/\pi_i$、收斂定理、ergodic average | MC p.24–33, 67–68, 73 |
| Ch5–8（連續） | [02d](02d_gaussian_ar1.md) | AR(1) kernel、n-step、invariant、stationarity 與相關、reversibility、λ-irreducibility、Harris、weak/TV 收斂、coupling、ergodic theorem、$\mathrm{Var}(\bar X_N)$、$\lvert a\rvert\ge1$ | MC p.35–63；AR1 全部 |
| Ch9 | [02e](02e_mcmc.md) | Metropolis、MH、Gibbs、MALA、burn-in、HMC、NUTS、RJMCMC、Example 3.14 | H10；H8 p.40–48 |

每一章的結構：直覺 → 正式定義與符號拆解 → 公式表 → 數值例子與反例 → 考試重點。

---

## 易混淆概念比較表

| 概念 A | 概念 B | 核心差異 | 如何辨認 | 常見錯誤 |
|---|---|---|---|---|
| Random variable $X_n$ | Stochastic process $(X_n)_{n\ge0}$ | 一個時間點 vs 整串 | 有沒有時間 index、問的是邊際還是聯合 | 把「過程的分布」說成單一分布 |
| $\Omega$ | $E$ | 隨機性來源 vs 觀察值空間 | $\omega\in\Omega$；$X_n(\omega)\in E$ | 把 state space 當 sample space |
| State $x$ | State space $E$ | 一個值 vs 所有可能值 | | |
| $K(x,A)$ | $k(x,y)$ | 機率（集合） vs density（點的高度） | 有沒有積分 | 以為 $k>1$ 不合法；以為 $K(x,\lbrace y\rbrace)=k(x,y)$ |
| Transition probability $K_{ij}$ | State probability $P(X_n=j)$ | 條件（已知現在 $i$） vs 邊際 | 條件裡有沒有指定現在狀態 | 用一列當作 $\mathcal L(X_1)$ |
| $K_{ij}$ | $(K^n)_{ij}$ | 一步 vs $n$ 步 | 題目步數 | $(K^2)_{ij}=K_{ij}^2$ |
| $K$ | $K^n$ | 規則 vs 規則重複 $n$ 次 | | |
| $\nu K$ | $Kf$ | 分布更新（向量在左） vs 條件期望（函數在右） | 輸入是分布還是函數 | 寫成 $K\nu$ |
| Markov property | Independence | 條件獨立於過去（給定現在） vs 完全獨立 | AR(1) $\mathrm{Corr}(X_0,X_2)=a^2\ne0$ 但為 Markov | 「Markov 表示未來與過去無關」 |
| Time homogeneous | Stationary | kernel 不隨時間變 vs 聯合分布不隨時間平移變 | 後者還需 $\nu=\pi$ | 「homogeneous 所以 stationary」 |
| Dependence on $x$ | Dependence on $n$ | 隨現在狀態變（允許） vs 隨時間變（破壞 homogeneity） | 規則裡有沒有 $n$ | $K(x,(0,\infty))=\Phi(x/2)$ 被誤判 inhomogeneous |
| Initial distribution $\nu$ | Stationary distribution $\pi$ | 任意給定 vs 滿足 $\pi K=\pi$ | | 以為初始分布會一直保持 |
| Invariant law $\pi$ | Stationary process | kernel 與分布的方程 vs 過程性質 | $\pi K=\pi$ vs $(X_h,\dots)\overset d=(X_0,\dots)$ | |
| Stationary distribution | Limiting distribution | $\pi K=\pi$（靜止點） vs $\lim\nu K^n$（吸引子） | 檢查 aperiodic、irreducible | $m=1$：有 $\pi$ 無極限 |
| Stationary | Independent | 平移不變 vs 無相依 | stationary AR(1) 相鄰相關 $a$ | 「從 $\pi$ 出發所以樣本獨立」 |
| Reversible | Symmetric $K$ | $\pi_iK_{ij}=\pi_jK_{ji}$ vs $K_{ij}=K_{ji}$ | 有沒有 $\pi$ 權重 | urn $K_{01}=1\ne K_{10}=\frac14$ 但 reversible |
| Detailed balance | Global balance $\pi K=\pi$ | 每對狀態流量相等 vs 每個狀態總流入=$\pi_j$ | 前者 ⇒ 後者，反之不成立 | 用 $\pi K=\pi$ 推 reversible |
| Irreducible | 一步可達 | 存在某 $n$ vs $n=1$ | 看 $K^n$ 不是只看 $K$ | $K_{02}=0$ 誤判不可達 |
| Accessible $i\to j$ | Communicate $i\leftrightarrow j$ | 單向 vs 雙向 | | |
| Periodic | 負相關/振盪平均 | 以機率 1 循環 vs 平均交替 | 有沒有 cyclic sets | AR(1) $a<0$ 被誤判 period 2 |
| Recurrent | Positive recurrent | 回返機率 1 vs 再加平均回返時間有限 | $E_i\tau_i^+<\infty$ | |
| Recurrent | Transient | 回返機率 $=1$ vs $<1$ | | |
| Irreducibility（正機率造訪） | Harris recurrence（機率 1 造訪） | $K^n(x,A)>0$ vs $P_x(\tau_A^+<\infty)=1$ | | |
| 分布收斂 $K^n(x,\cdot)\to\pi$ | Ergodic average $\frac1N\sum f(X_n)\to\pi(f)$ | 很多複本在時間 $n$ vs 一條路徑沿時間 | | 以為 periodic 的時間平均也不收斂 |
| Weak convergence | Total variation | 有界連續函數期望收斂 vs 所有事件機率一致收斂 | | |
| Geometric ergodicity | Uniform ergodicity | 界 $M(x)\rho^n$ 依起點 vs $M\rho^n$ 不依起點 | AR(1) 不是 uniform | 以為固定 burn-in 適用所有起點 |
| 機率 | 期望 | $P(A)=E[\mathbf 1_A]$ 是特例 | $Kf$ with $f=\mathbf 1_A$ | |
| Row vector | Column vector | 分布 vs 函數 | 乘法位置 | |
| Proposal $q$ | Transition kernel $K$（MCMC） | 候選分布 vs 含接受/拒絕的實際轉移 | 拒絕時 $K$ 在 $x$ 有點質量 | 把 $q$ 當成鏈的 kernel |
| Metropolis | Metropolis–Hastings | 對稱 $q$ vs 一般 $q$（含 $q$ 比值） | proposal 是否對稱 | 非對稱還用 Metropolis ratio |
| Burn-in | Thinning | 去除初始偏差 vs 降低 autocorrelation | | 以為 burn-in 消除相關 |
| Kernel $K(x,A)$ | HMC kinetic energy $K(p)$ | 同字母不同意義 | 看參數 | |

## 容易誤判的例子（逐組說明）

1. **$k(x,y)>0$ 但 $K(x,\lbrace y\rbrace)=0$**：AR(1) $x=2$，$k(2,1)=\varphi(0)\approx0.399>0$，但點的面積為 0 [AR1 p.8]。
2. **條件列 vs 邊際分布**：$m=2$ 的列 1 是 $(1/4,1/2,1/4)$；但若 $X_0\sim(1/2,1/2,0)$，$\mathcal L(X_1)=(1/8,3/4,1/8)$ [MC p.22]。
3. **Homogeneous 但非 stationary**：urn 從 $X_0=0$；AR(1) 從 $X_0=0$（平均一直 0 但變異數 $1,5/4,\dots$ 在變）[AR1 p.41]。
4. **Stationary 但相依**：AR(1) $X_0\sim N(0,4/3)$，$\mathrm{Corr}(X_0,X_1)=1/2$ [AR1 p.20]。
5. **有 stationary 無 limit**：$m=1$，$K=\begin{pmatrix}0&1\\1&0\end{pmatrix}$ [MC p.26]。
6. **Reversible 但 $K$ 不對稱**：urn $m=2$ [MC p.31]；AR(1) $k(0,2)\ne k(2,0)$ [AR1 p.41]。
7. **Invariant 但不 reversible**（補充）：三狀態確定循環 $0\to1\to2\to0$，uniform 為 invariant，但 $\pi_0K_{01}=\frac13\ne\pi_1K_{10}=0$。
8. **Irreducible 但非一步可達**：urn $K_{02}=0$，$(K^2)_{02}=1/4$。
9. **平均先達平衡**：urn 從 0 出發，$n\ge1$ 平均已是 1，分布到極限才是 $\pi$ [MC p.33]。
10. **振盪相關不是 period**：AR(1) $a=-1/2$，$\mathrm{Corr}(X_0,X_1)=-1/2$，仍 aperiodic [AR1 p.38]。
11. **合法 kernel 不一定有平衡**：AR(1) $a=1$，$K^n(x,\cdot)=N(x,n\sigma^2)$ [MC p.58]。
