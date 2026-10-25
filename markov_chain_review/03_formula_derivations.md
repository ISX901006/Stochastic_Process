# 03 公式推導索引

每個推導都有「詳細理解版」與「考試精簡版（英文）」。標示「補充推導」者為教材只陳述結論、本目錄補上證明。

| # | 推導 | 檔案 | 來源 | 考試重要度 |
|---|---|---|---|---|
| 1 | Path probability 乘法公式 | [03a §1](03a_derivations_finite_and_general.md) | MC p.8–9（隱含） | ★★★ |
| 2 | Chapman–Kolmogorov | [03a §2](03a_derivations_finite_and_general.md) | MC p.20–21, 46 | ★★★ |
| 3 | $\mathcal L(X_n)=\nu K^n$ | [03a §3](03a_derivations_finite_and_general.md) | MC p.22 | ★★★ |
| 4 | Bernoulli–Laplace 轉移機率 | [03a §4](03a_derivations_finite_and_general.md) | MC p.16–17 | ★★★ |
| 5 | Detailed balance ⇒ invariance | [03a §5](03a_derivations_finite_and_general.md) | MC p.31 | ★★★ |
| 6 | 超幾何不變分布（general $m$） | [03a §6](03a_derivations_finite_and_general.md) | MC p.32, 67 | ★★ |
| 7 | Strict stationarity ⇔ $\nu K=\nu$ | [03a §7](03a_derivations_finite_and_general.md) | MC p.29, AR1 p.18 | ★★ |
| 8 | Lazy chain | [03a §8](03a_derivations_finite_and_general.md) | MC p.26 | ★ |
| 9 | $E_i\tau_i^+=1/\pi_i$ 的數值驗證（首步分析，補充） | [03a §9](03a_derivations_finite_and_general.md) | MC p.68 | ★ |
| 10 | $Kf$ 算子性質 | [03a §10](03a_derivations_finite_and_general.md) | MC p.72 | ★ |
| 11 | AR(1) n-step law | [03b §1](03b_derivations_ar1.md) | MC p.45, 47；AR1 p.12 | ★★★ |
| 12 | C–K density 版一致性 | [03b §2](03b_derivations_ar1.md) | MC p.46 | ★ |
| 13 | AR(1) invariant law | [03b §3](03b_derivations_ar1.md) | MC p.50；AR1 p.17 | ★★★ |
| 14 | $\mathrm{Corr}_\pi(X_0,X_h)=a^h$ | [03b §4](03b_derivations_ar1.md) | MC p.52；AR1 p.20 | ★★★ |
| 15 | AR(1) reversibility | [03b §5](03b_derivations_ar1.md) | MC p.53；AR1 p.22 | ★★★ |
| 16 | Uniqueness via dominated convergence | [03b §6](03b_derivations_ar1.md) | AR1 p.28 | ★★ |
| 17 | Weak convergence 與 coupling | [03b §7](03b_derivations_ar1.md) | AR1 p.27, 29 | ★★ |
| 18 | $\bar X_N\to0$（初等證明） | [03b §8](03b_derivations_ar1.md) | AR1 p.33 | ★★ |
| 19 | $\mathrm{Var}_\pi(\bar X_N)$ 與極限 | [03b §9](03b_derivations_ar1.md) | AR1 p.34 | ★★ |
| 20 | $\lvert a\rvert\ge1$ 無 invariant probability | [03b §10](03b_derivations_ar1.md) | AR1 p.47 | ★ |
| 21 | MH detailed balance（補充） | [03c §1](03c_derivations_mcmc.md) | H10 p.6, 12 | ★★★ |
| 22 | Gibbs 保留 $\pi$、$\alpha=1$（補充） | [03c §2](03c_derivations_mcmc.md) | H10 p.16 | ★★ |
| 23 | Bivariate normal 條件分布 | [03c §3](03c_derivations_mcmc.md) | H10 p.24–28 | ★★ |
| 24 | MALA 接受率（standard normal，補充） | [03c §4](03c_derivations_mcmc.md) | H10 p.31–32 | ★ |
| 25 | HMC 接受率（補充思路） | [03c §5](03c_derivations_mcmc.md) | H10 p.47 | ★ |
| 26 | 二狀態鏈 likelihood 與 MLE | [03c §6](03c_derivations_mcmc.md) | H8 p.40–47 | ★★ |

**重要度判斷依據**：★★★ = 在 MC 與 AR1 兩份投影片都出現、並有練習題或 exit question；★★ = 出現在主體投影片；★ = 附錄（optional material）或教材未證明。教材沒有歷屆考題，無法斷定實際出題頻率。

## 推導之間的依賴

```
條件機率乘法 + Markov + homogeneity
  → #1 path probability
      → #2 Chapman–Kolmogorov → #3 νK^n
      → #7 stationarity ⇔ νK=ν
      → #26 likelihood of a chain
  → #4 urn probabilities → #6 hypergeometric π（用 #5）
#5 detailed balance ⇒ invariance
  → #6、#15、#21、#22（所有 MCMC 正確性）
#11 n-step law → #13 invariant → #14 correlation → #19 Var(mean)
#11 → #17 convergence → #16 uniqueness
```
