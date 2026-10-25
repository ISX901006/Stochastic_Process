# 07 考前快速複習

## 1. 十個一定要會寫的式子

1. Kernel 兩條件：$A\mapsto K(x,A)$ 是機率測度；$x\mapsto K(x,A)$ 可測。
2. Markov：$P(X_{n+1}\in A\mid\mathcal F_n)=K(X_n,A)$ a.s.
3. C–K：$(K^{r+s})_{ij}=\sum_z(K^r)_{iz}(K^s)_{zj}$；$\mathcal L(X_n)=\nu K^n$。
4. Urn：$K_{i,i+1}=\frac{(m-i)^2}{m^2}$、$K_{i,i-1}=\frac{i^2}{m^2}$、$K_{ii}=\frac{2i(m-i)}{m^2}$；$\pi_i=\binom mi^2/\binom{2m}m$。
5. Invariant：$\pi K=\pi$、$\pi\ge0$、$\sum\pi=1$；Detailed balance $\pi_iK_{ij}=\pi_jK_{ji}$ ⇒ invariant。
6. Period $d(i)=\gcd\lbrace n\ge1:K^n(i,\lbrace i\rbrace)>0\rbrace$；$E_i\tau_i^+=1/\pi_i$。
7. AR(1)：$K^n(x,\cdot)=N(a^nx,\sigma^2\sum_{k<n}a^{2k})$；$\pi=N(0,\frac{\sigma^2}{1-a^2})$。
8. $\mathrm{Corr}_\pi(X_0,X_h)=a^h$；$N\mathrm{Var}(\bar X_N)\to\frac{\sigma^2}{(1-a)^2}$。
9. Reversibility 展開：$(1-a^2)x^2+(y-ax)^2=x^2+y^2-2axy$。
10. MH：$\alpha=\min\big(1,\frac{\pi(Y)q(X_n\mid Y)}{\pi(X_n)q(Y\mid X_n)}\big)$。

## 2. 必背數字（running examples）

| 量 | 值 |
|---|---|
| urn $m=2$ 的 $K$ 第 1 列 | $(1/4,1/2,1/4)$ |
| $K^2$ | 列 $(1/4,1/2,1/4)$、$(1/8,3/4,1/8)$、$(1/4,1/2,1/4)$ |
| $\pi$（$m=2$） | $(1/6,2/3,1/6)$；回返時間 $(6,3/2,6)$ |
| $m=4$、狀態 1 | $(1/16,6/16,9/16)$，期望 $3/2$ |
| AR(1) $a=\frac12,\sigma=1$ | $\pi=N(0,4/3)$；$X_0=2$：$X_1\sim N(1,1)$、$X_2\sim N(1/2,5/4)$ |
| $K^2(2,[0,1])$ | $2\Phi(1/\sqrt5)-1\approx0.3453$ |
| $K(2,[0,2])$ | $\Phi(1)-\Phi(-1)\approx0.6827$ |
| $\sqrt N\bar X_N$ 漸近變異數 | $a=\frac12$：4；$a=-\frac12$：$4/9$；i.i.d. 對照 $4/3$ |

## 3. 八個最容易混淆的觀念（一句話版本）

1. **Homogeneous ≠ Stationary**：前者 kernel 不隨時間變；後者還要 $X_0\sim\pi$。
2. **依賴 $x$ 不違反 homogeneity**；依賴 $n$ 才違反。
3. **Stationary ≠ 獨立**：AR(1) 相鄰相關 $a$。
4. **Invariant ≠ Limiting**：$m=1$ 有 $\pi$ 但不收斂（period 2）。
5. **Reversible ≠ $K$ 對稱**：是 $\pi$ 加權對稱。
6. **$k(x,y)$ ≠ 機率**：$K(x,\lbrace y\rbrace)=0$；$k$ 可 $>1$。
7. **Kernel 合法 ≠ 有平衡**：任何 $a$ 都是合法 kernel，$\lvert a\rvert\ge1$ 無 invariant probability。
8. **Irreducible ≠ 一步可達**；**$a<0$ 振盪 ≠ periodic**。

## 4. 定理的假設清單

| 結論 | 需要的假設 |
|---|---|
| C–K、$\mathcal L(X_n)=\nu K^n$ | Markov + homogeneous |
| Strictly stationary ⇔ $\nu K=\nu$ | homogeneous Markov |
| 唯一 $\pi$、positive recurrent、$E_i\tau_i^+=1/\pi_i$ | finite + irreducible |
| $K^n(i,\lbrace j\rbrace)\to\pi_j$ | finite + irreducible + **aperiodic**（充分條件） |
| 時間平均 → $\pi_j$ | finite + irreducible（不需 aperiodic） |
| AR(1) 平衡、Harris、reversible、幾何收斂、ergodic | $\lvert a\rvert<1$、$\sigma>0$ |
| AR(1) kernel 合法、λ-irreducible、aperiodic | 只要 $\sigma>0$ |
| MH 的 $\pi$ invariant | detailed balance（自動成立） |
| MH 收斂到 $\pi$ | 再加 irreducible + aperiodic |

## 5. 看到關鍵字就想到

| 關鍵字 | 工具 |
|---|---|
| "transition matrix", 文字描述實驗 | 題型 A：列結果表、檢查列和 |
| "probability that next state is in A" | 加第 $i$ 列 |
| "after $n$ steps", "two-step" | $K^n$ 或 C–K |
| "initial distribution", "$X_0\sim$" | $\nu K^n$ |
| "expected next", "conditional variance" | $Kf$ |
| "stationary/invariant distribution" | $\pi K=\pi$ + 正規化；birth–death 用 detailed balance |
| "reversible", "detailed balance" | 逐對檢查 $\pi_iK_{ij}$ |
| "irreducible", "aperiodic", "period" | 狀態圖、gcd |
| "limit as $n\to\infty$" | 檢查三個假設 |
| "mean return time" | $1/\pi_i$ |
| AR(1) "$\Phi$" | 寫現在狀態、步數、目標集合 → 標準化 |
| "stationary correlation" | $a^h$ |
| "time average", "almost surely" | ergodic theorem |
| "acceptance probability" | 先判斷 proposal 對稱否 |
| "Gibbs", "conditional distribution" | bivariate normal 公式 |
| "count transitions", "likelihood of the chain" | $\prod K_{ij}^{m_{ij}}$ |

## 6. 作答檢查清單

- [ ] 矩陣每列和為 1？元素在 $[0,1]$？
- [ ] 分布是 row vector、放在 $K$ 左邊？
- [ ] 求 $\pi$ 時加了正規化、代回檢查？
- [ ] 說明唯一性/收斂時寫出假設（finite、irreducible、aperiodic）？
- [ ] AR(1) 標準化用標準差而非變異數？平均是 $ax$ 不是 $x$？
- [ ] 單點機率寫 0？
- [ ] 判斷 stationary 時檢查的是**分布**而不只是平均？
- [ ] MH 有沒有 Hastings correction？拒絕時保留現值？
- [ ] 證明每一步標明使用的性質（Markov / homogeneity / independence / normalization）？

## 7. 五分鐘口述練習（能說出來才算懂）

1. 用 urn 解釋為什麼「狀態 = 紅球數」使過程 Markov。
2. 用「6000 份平行複本」解釋 $\pi K=\pi$。
3. 用 $m=1$ 解釋為什麼需要 aperiodic。
4. 用 AR(1) 的 $a^nx$ 與 $\sigma^2\sum a^{2k}$ 解釋「記憶」與「新雜訊」。
5. 用 $a=\pm\frac12$ 解釋為什麼相同邊際分布的鏈，樣本平均精度可以差 9 倍。
6. 用 detailed balance 解釋 MH 為什麼只需要 $\pi$ 的比值。
