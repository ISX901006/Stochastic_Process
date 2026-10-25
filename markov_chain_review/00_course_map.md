# 00 課程地圖：MH6504 Markov Chains 複習系統

本目錄依照課堂實際教材整理 Markov Chain 相關考試內容。所有內容都標註來源，格式為 `[檔案代號 p.頁碼]`，頁碼即 PDF 頁碼（投影片編號）。

## 1. 教材盤點

| 代號 | 檔案名稱 | 頁數 | 主題 | 讀取狀態 | 與 Markov Chain 考試的關係 |
|---|---|---|---|---|---|
| MC | `Markov_Chains.pdf`（標題：Understanding transition kernels） | 73 | Transition kernel、Bernoulli–Laplace urn、Gaussian AR(1)、n-step、invariant、stationarity、reversibility、irreducibility、periodicity；附錄：recurrence、ergodic average | 全文讀取（文字層完整，無掃描圖） | **核心** |
| AR1 | `AR1_Markov_Properties.pdf`（標題：Gaussian AR(1) and Markov chain theory） | 50 | AR(1) 的 kernel、n-step law、invariant law、stationarity、correlation、reversibility、λ-irreducibility、aperiodicity、Harris recurrence、weak/TV convergence、coupling、ergodic theorem、樣本平均的變異數；附錄：Feller、small set、drift、$\lvert a\rvert\ge 1$ 無不變分布、CLT | 全文讀取 | **核心** |
| H10 | `Handout10_2026.pdf`（Markov Chains Monte Carlo Methods） | 74 | Metropolis、Metropolis–Hastings、Gibbs、Bivariate normal 條件分布、MALA、Burn-in、HMC、NUTS、RJMCMC | 全文讀取；p.21、35、41、48、55、58、61、64、65、72 主要是圖（無可辨識公式） | **核心（應用）** |
| H8 | `Handout08_2026.pdf`（Monte Carlo Integration） | 54 | Bayes estimator、Empirical Bayes、Classical MC、Importance Sampling、**Example 3.14 Transition matrix estimation（p.40–48）**、Laplace approximation | 全文讀取 | 只有 Example 3.14 與 Markov Chain 直接相關；Classical MC（p.19–20）是 ergodic average 的對照組 |
| H6 | `Handout06_2026.pdf`（Monte Carlo Statistical Methods: Introduction） | 36 | Censored data、mixture、MLE、exponential family、Bayes estimator、credible/HPD region | 全文讀取 | 非 Markov Chain 主題（前置統計背景） |
| H7 | `Handout07_2026.pdf`（Random Variable Generation） | 31 | Uniform generator、inverse transform、Box–Muller、Poisson 生成與 Poisson process 到達間隔、Accept–Reject、ARS | 全文讀取 | 非 Markov Chain 主題；Inverse transform 是 MC p.15「用 Uniform 生成路徑」的基礎 |
| H9 | `Handout09_2026.pdf`（Bayes estimation under non-quadratic loss、Empirical Bayes、Classical MC、Example 3.10） | 62 | L1 loss、noncentrality parameter、EB 推導、MC 收斂 | 全文讀取 | 非 Markov Chain 主題；p.10–11 說明「需要 MCMC 抽 posterior」作為動機 |

### 1.1 公式辨識與教材疑點（未自行猜測，僅標示）

| 位置 | 問題 | 處理方式 |
|---|---|---|
| H10 p.33 | MALA 程式碼最後一行被截斷：`.../epsilon*`，應為 `/epsilon**2`（與前一行對稱） | 文件中以數學式寫出完整 acceptance ratio，程式碼僅作參考 |
| H10 p.6、p.12 | 投影片只寫「Symmetry of q ensures detailed balance」，沒有給證明 | 03c 補上標準證明，標示為「補充推導」 |
| H10 p.40 | 投影片說 burn-in「helps ensure that estimates are unbiased」 | 嚴格說只能「降低」初始偏差，MCMC 估計量一般不是精確 unbiased；見 02e |
| H8 p.40 | 「$p_1+p_2<1$ ... strictly sub-stochastic chain」描述不精確（此 $T$ 每列和為 1） | 照投影片說法視為模型假設，不延伸解釋 |
| H8 p.12 vs H9 p.9–12 | EB 估計式分母：H8 寫 $(\hat\lambda+1)^2$，H9 推導為 $\hat\lambda/(\hat\lambda+1)$ | 非 Markov 範圍，僅記錄 |
| MC p.25 | 「irreducible chain 所有 state 的 period 相同」只陳述未證明 | 列為「需背誦的定理」 |

### 1.2 教材沒有涵蓋的內容（不列入正式考試範圍）

以下常見 Markov Chain 主題在 7 份教材中**沒有出現**：Absorbing Markov chain 的 canonical form 與 fundamental matrix $N=(I-Q)^{-1}$、Continuous-Time Markov Chain、Generator matrix、Kolmogorov forward/backward equations、Birth–death chain 的一般理論、Gambler's ruin。Poisson process 只在 H7 p.9–10 作為生成 Poisson 變數的工具出現。01 與 02c 中若提到，均標註「補充背景，不一定屬於考試範圍」。

## 2. 課程知識地圖（依實際教材）

```
[前置] 機率空間 (Ω,F,P)、條件機率、期望、Normal 分布、Φ、SLLN/CLT (H8 p.19-20)
   │
   ▼
Ch1 State space (E,ℰ) 與 Transition kernel K(x,A)          [MC p.4-7, AR1 p.5]
   │      兩個條件：(1) A↦K(x,A) 是機率測度 (2) x↦K(x,A) 可測
   ▼
Ch2 Markov property 與 Time homogeneity                     [MC p.8-9, 16; AR1 p.4, 9]
   │
   ├────────────────────────────┐
   ▼                            ▼
Ch3 有限狀態：Bernoulli–Laplace   Ch5 連續狀態：Gaussian AR(1)
   矩陣 K、K(i,A)=Σ K_ij           density k(x,y)、K(x,A)=∫_A k
   [MC p.10-18]                    [MC p.35-44, AR1 p.6-10]
   │                            │
   ▼                            ▼
Ch4 多步：K^n、Chapman–Kolmogorov、初始分布 νK^n、算子 Kf
   [MC p.20-23, 45-49; AR1 p.10-14]
   │
   ▼
Ch6 長期行為的「結構」：Accessibility / Irreducibility (ψ-irreducible)、Period / Aperiodic、Lazy chain
   [MC p.24-26, 54; AR1 p.24-25]
   │
   ▼
Ch7 平衡：Invariant distribution πK=π → Strict stationarity (X0~π) → Reversibility / Detailed balance
   [MC p.27-32, 50-53; AR1 p.15-23]
   │
   ▼
Ch8 收斂與遍歷：Recurrence (τ⁺, E τ⁺=1/π_i)、Harris recurrence、K^n→π（weak / TV / geometric）、
   uniqueness、coupling、ergodic theorem、樣本平均變異數
   [MC p.33, 56-58, 68, 73; AR1 p.26-36, 45-47, 50]
   │
   ▼
Ch9 應用 MCMC：Metropolis → Metropolis–Hastings → Gibbs → MALA → HMC → NUTS → RJMCMC、Burn-in
   [H10 全部]；Transition matrix 的 Bayesian 估計 [H8 p.40-48]
```

依賴關係重點：Ch7 的 detailed balance 是 Ch9 所有 MCMC 正確性的理論基礎；Ch6 的 irreducible + aperiodic 是 Ch8 收斂定理的假設；Ch4 的 $\nu K^n$ 是 Ch7「分布不變」與 Ch8「分布收斂」的共同語言。

## 3. 文件索引與建議學習順序

| 順序 | 文件 | 內容 | 狀態 |
|---|---|---|---|
| 1 | [01_prerequisites.md](01_prerequisites.md) | 機率空間、條件機率、全機率、條件期望、Normal 計算、矩陣乘法、極限定理 | 完成 |
| 2 | [02_concept_review.md](02_concept_review.md) | 概念章節索引 + **易混淆概念比較表** | 完成 |
| 2a | [02a_kernel_markov_property.md](02a_kernel_markov_property.md) | Ch1–2：State space、Kernel、Markov property、Homogeneity | 完成 |
| 2b | [02b_finite_chain_bernoulli_laplace.md](02b_finite_chain_bernoulli_laplace.md) | Ch3–4：Transition matrix、Bernoulli–Laplace、$K(i,A)$、$K^n$、C–K、初始分布、$Kf$ | 完成 |
| 2c | [02c_structure_equilibrium_convergence.md](02c_structure_equilibrium_convergence.md) | Ch6–8（有限狀態）：Irreducibility、Period、Lazy、Invariant、Stationarity、Reversibility、Recurrence、收斂、遍歷平均 | 完成 |
| 2d | [02d_gaussian_ar1.md](02d_gaussian_ar1.md) | Ch5–8（連續狀態）：AR(1) 的全部性質 | 完成 |
| 2e | [02e_mcmc.md](02e_mcmc.md) | Ch9：MCMC 演算法與 Example 3.14 | 完成 |
| 3 | [03_formula_derivations.md](03_formula_derivations.md) | 推導索引（03a/03b/03c） | 完成 |
| 3a | [03a_derivations_finite_and_general.md](03a_derivations_finite_and_general.md) | C–K、$\nu K^n$、Bernoulli–Laplace 轉移機率、超幾何不變分布、detailed balance ⇒ invariance、stationarity ⇔ $\nu K=\nu$ | 完成 |
| 3b | [03b_derivations_ar1.md](03b_derivations_ar1.md) | AR(1) n-step law、invariant law、correlation、reversibility、收斂、uniqueness、coupling、樣本平均、$\lvert a\rvert\ge1$ | 完成 |
| 3c | [03c_derivations_mcmc.md](03c_derivations_mcmc.md) | MH detailed balance、Gibbs、bivariate normal 條件分布、MALA ratio、HMC acceptance、Example 3.14 likelihood | 完成 |
| 4 | [04_solved_examples.md](04_solved_examples.md) | 教材全部例題與練習完整解答 + 額外反例 | 完成 |
| 5 | [05_problem_solving_guide.md](05_problem_solving_guide.md) | 題型辨識與 SOP、失分點 | 完成 |
| 6 | [06_formula_cheatsheet.md](06_formula_cheatsheet.md) | 公式總表 | 完成 |
| 7 | [07_last_minute_review.md](07_last_minute_review.md) | 考前清單 | 完成 |

## 4. 記號約定（全目錄統一，依老師記號）

| 記號 | 意義 | 常見教科書寫法 |
|---|---|---|
| $E$、$\mathcal E$ | State space 與其 σ-algebra | $S$ |
| $K(x,A)$ | 現在在 $x$，下一步落在集合 $A$ 的機率 | $P(x,A)$ |
| $K_{ij}=K(i,\lbrace j\rbrace)$ | 有限狀態的一步轉移機率 | $p_{ij}$ |
| $K^n(x,A)$、$(K^n)_{ij}$ | n 步轉移機率 | $p_{ij}^{(n)}$ |
| $\nu=\mathcal L(X_0)$ | 初始分布，**row vector** | $\pi_0$、$\mu$ |
| $\nu K^n=\mathcal L(X_n)$ | 時間 $n$ 的分布 | $\pi_n=\pi_0P^n$ |
| $\pi$ | Invariant（stationary）distribution | 同 |
| $P_x$、$E_x$ | 從 $X_0=x$ 出發的機率與期望 | $P(\cdot\mid X_0=x)$ |
| $\mathcal F_n=\sigma(X_0,\dots,X_n)$ | 到時間 $n$ 的觀測歷史 | 同 |
| $k(x,y)$ | 連續狀態的 transition density | $p(x,y)$ |

**矩陣慣例**：本課程一律是 row-stochastic（列 = 現在狀態，欄 = 下一狀態，每列和為 1），分布是 row vector，乘法方向為 $\nu K$（向量在左）。全目錄不混用 column 慣例。

## 5. 品質檢查紀錄（初版）

| 檢查項目 | 結果 |
|---|---|
| 教材盤點 | 7 份 PDF 全部讀取；Markov 相關內容（MC、AR1、H10、H8 Ex.3.14）全部涵蓋 |
| 公式辨識 | 文字層完整；不確定處列於 §1.1，未自行補成原文 |
| 推導完整性 | 26 個推導，每個附考試精簡版；教材未證明者標「補充推導」 |
| 假設與條件 | 收斂定理標明為充分條件；$\lvert a\rvert<1$ 與 kernel 合法性分開敘述 |
| Stationary vs limiting | 02c §7、02_concept_review 比較表、反例 $m=1$ |
| 矩陣方向 | 全目錄 row-stochastic、row vector 在左；未混用 column 慣例 |
| 範圍控制 | 教材外主題（absorbing chain、CTMC、generator 等）只在 02c §9 以「補充背景」出現 |
| 數值驗證 | $K^2$、$K^3$、$K^4$、$\pi$、回返時間、$m=4$ 列與 $\pi$、$\Phi$ 數值、AR(1) 變異數、MALA 化簡式、例 17 與例 20 皆以程式重算 |
| LaTeX | 2571 個公式以 KaTeX 0.16.11 全數解析無錯誤 |
| 來源標註 | 每節標註 `[檔案 p.頁]` |
