# 04 例題詳解

教材中的練習題與 Discuss 問題全部收錄，另加幾題對照用的例題與反例（標「補充」）。重要題目使用完整十步格式；短題使用精簡格式。

---

## 例 1 Bernoulli–Laplace Exercise 1（$m=4$、現在在 1）[MC p.18–19]

1. **題目**：(1) 列出兩個 urn 的紅藍球數；(2) 算 $K(1,\lbrace0\rbrace),K(1,\lbrace1\rbrace),K(1,\lbrace2\rbrace)$；(3) 檢查和為 1；(4) 求 $K(1,\lbrace2,3,4\rbrace)$；(5) 求期望下一計數。
2. **已知**：$m=4$，$X_n=1$。
3. **目標**：第 1 列的轉移機率與條件期望。
4. **選擇公式**：$K_{i,i+1}=\frac{(m-i)^2}{m^2}$、$K_{i,i-1}=\frac{i^2}{m^2}$、$K_{ii}=\frac{2i(m-i)}{m^2}$；$K(i,A)=\sum_{j\in A}K_{ij}$；$(Kf)(i)=\sum_jf(j)K_{ij}$。
5. **原因**：狀態 1 決定兩 urn 組成；集合機率加列元素；期望用 $f(j)=j$。
6. **代入**：左 urn 1 紅 3 藍；右 urn 3 紅 1 藍。
7. **計算**：
   - $K(1,\lbrace0\rbrace)=\frac14\cdot\frac14=\frac1{16}$（左取紅、右取藍）
   - $K(1,\lbrace2\rbrace)=\frac34\cdot\frac34=\frac9{16}$（左取藍、右取紅）
   - $K(1,\lbrace1\rbrace)=\frac{2\cdot1\cdot3}{16}=\frac6{16}$
   - 和 $=\frac{1+6+9}{16}=1$ ✓
   - $K(1,\lbrace2,3,4\rbrace)=\frac9{16}+0+0=\frac9{16}$（一步最多變 1）
   - $E[X_{n+1}\mid X_n=1]=0\cdot\frac1{16}+1\cdot\frac6{16}+2\cdot\frac9{16}=\frac{24}{16}=\frac32$
8. **答案**：$(1/16,6/16,9/16)$；$9/16$；$3/2$。
9. **意義**：低於平衡計數 $m/2=2$ 時，期望計數上升（mean reversion）[MC p.19, 69]。
10. **合理性**：用 restoring-mean 公式 $E[X_{n+1}-X_n\mid X_n=i]=1-\frac{2i}m=\frac12$ ⇒ $1+\frac12=\frac32$ ✓。

---

## 例 2 Discuss 問題（$m=2$）[MC p.5, 11, 13, 20]

| 問題 | 答案 |
|---|---|
| $K(1,\lbrace0,2\rbrace)$ 的文字意義？ | 「現在左 urn 有 1 顆紅球，下一次交換後紅球數是 0 或 2（極端狀態）的機率」$=1/2$ |
| 為什麼兩種不同交換得到同一狀態？ | 紅換紅與藍換藍都不改變計數；狀態只記錄計數 |
| $K(0,\lbrace0,2\rbrace)$、$K(2,\lbrace0,2\rbrace)$？ | 都是 0（0 與 2 只能去 1） |
| 為什麼 $K(0,A)=0$ 但 $K^2(0,A)>0$（$A=\lbrace0,2\rbrace$）？ | 一步只能到 1，兩步可經 1 到 0 或 2：$K^2(0,A)=K_{01}(K_{10}+K_{12})=\frac12$ |

---

## 例 3 隨機初始狀態 [MC p.22]

$\nu=(1/2,1/2,0)$，求 $\mathcal L(X_1)$。

$$
\nu K=\tfrac12(0,1,0)+\tfrac12(\tfrac14,\tfrac12,\tfrac14)+0\cdot(0,1,0)=(\tfrac18,\tfrac34,\tfrac18).
$$

檢查：和為 1。**常見錯**：寫成 $K\nu^\top=(1/2,3/8,1/2)^\top$（和 $11/8$，不是分布）。

---

## 例 4 條件期望與條件變異數 [MC p.23]

$m=2$，現在 1：$Kf(1)=1$（$f(j)=j$）、$Kg(1)=\frac32$（$g(j)=j^2$）、$\mathrm{Var}(X_{n+1}\mid X_n=1)=\frac32-1=\frac12$；$(K\mathbf 1_{\lbrace2\rbrace})(1)=\frac14$。

---

## 例 5 求 Invariant Distribution（$m=2$）[MC p.28]

1. **題目**：求 $\pi$ 使 $\pi K=\pi$。
2. **已知**：$K=\begin{pmatrix}0&1&0\\1/4&1/2&1/4\\0&1&0\end{pmatrix}$。
3. **目標**：機率向量 $\pi$。
4. **公式**：$\pi_j=\sum_i\pi_iK_{ij}$、$\pi_i\ge0$、$\sum\pi_i=1$。
5. **原因**：invariant 的定義。
6. **代入（展開成方程）**：欄 0：$\pi_0=\frac14\pi_1$；欄 2：$\pi_2=\frac14\pi_1$；欄 1：$\pi_1=\pi_0+\frac12\pi_1+\pi_2$。
7. **計算**：正規化 $\frac14\pi_1+\pi_1+\frac14\pi_1=\frac32\pi_1=1\Rightarrow\pi_1=\frac23$，$\pi_0=\pi_2=\frac16$。
8. **答案**：$\pi=(1/6,2/3,1/6)$。
9. **意義**：長期約 2/3 的時間兩 urn 各有 1 紅 1 藍；等於超幾何 $\binom2i^2/\binom42$。
10. **合理性**：欄 1 方程 $\frac16+\frac13+\frac16=\frac23$ ✓；detailed balance $\frac16\cdot1=\frac23\cdot\frac14$ ✓。

---

## 例 6 平均回返時間 [MC p.68]

$E_i\tau_i^+=1/\pi_i=(6,3/2,6)$。首步分析驗證見 [03a §9](03a_derivations_finite_and_general.md)。意義：從 0 出發平均 6 次交換回到 0。

---

## 例 7 收斂表與「平均先收斂」[MC p.33]

從 $X_0=0$：$\mathcal L(X_2)=(1/4,1/2,1/4)$（$K^2$ 第 0 列）、$\mathcal L(X_3)=(1/8,3/4,1/8)$、$\mathcal L(X_4)=(3/16,5/8,3/16)$，極限 $(1/6,2/3,1/6)$；平均從 $n=1$ 起恆為 1。

---

## 例 8 Final exercise A（urn）[MC p.61–62]

1. $K^2(0,\lbrace0,2\rbrace)=\frac14+\frac14=\frac12$（$K^2$ 第 0 列為 $(1/4,1/2,1/4)$）。
2. $X_0\sim(1/6,2/3,1/6)$：此為 invariant，所以 $\mathcal L(X_1)=\pi$，$P(X_1=2)=\frac16$。直接驗算：$\frac16\cdot0+\frac23\cdot\frac14+\frac16\cdot0=\frac16$ ✓。

---

## 例 9 Gaussian Exercise 2 [MC p.44, 66]

$X_{n+1}=X_n/2+Z_{n+1}$，$\Phi(1)\approx0.8413$。

| 題 | 現在狀態 | 下一步分布 | 目標集合 | 計算 | 答案 |
|---|---|---|---|---|---|
| 1 | 2 | $N(1,1)$ | $(2,\infty)$ | $P(1+Z>2)=1-\Phi(1)$ | $0.1587$ |
| 2 | $-2$ | $N(-1,1)$ | $(-\infty,0]$ | $P(-1+Z\le0)=\Phi(1)$ | $0.8413$ |
| 3 | 0 | $N(0,1)$ | $\lbrace0\rbrace$ | 單點 | $0$ |

第 4 題（先辨認再算）：題 1 的現在狀態 2，下一步平均 1，目標集合 $(2,\infty)$。

---

## 例 10 AR(1) Exercise 1（兩步）[AR1 p.13–14]

1. **題目**：$X_0=2$。(1) $X_1,X_2$ 的條件分布；(2) 用 $\Phi$ 表示 $K^2(2,[0,1])$；(3) $E_2X_2$、$\mathrm{Var}_2X_2$；(4) 依賴 $X_0$ 是否違反 homogeneity？
2. **已知**：$a=\frac12,\sigma=1$。
3. **目標**：兩步分布與區間機率。
4. **公式**：$K^n(x,\cdot)=N(a^nx,\sigma^2\sum_{k<n}a^{2k})$；標準化。
5. **原因**：n-step law 是 Normal。
6. **代入**：$n=1$：$N(1,1)$；$n=2$：平均 $\frac14\cdot2=\frac12$，變異數 $1+\frac14=\frac54$。
7. **計算**：

$$
K^2(2,[0,1])=\Phi\!\left(\frac{1-1/2}{\sqrt{5/4}}\right)-\Phi\!\left(\frac{0-1/2}{\sqrt{5/4}}\right)=\Phi(1/\sqrt5)-\Phi(-1/\sqrt5)=2\Phi(1/\sqrt5)-1 .
$$

   （$\frac{1/2}{\sqrt5/2}=\frac1{\sqrt5}$；對稱 $\Phi(-t)=1-\Phi(t)$。）
8. **答案**：$X_1\sim N(1,1)$、$X_2\sim N(1/2,5/4)$；$K^2(2,[0,1])\approx2(0.6726)-1=0.3453$；$E_2X_2=\frac12$、$\mathrm{Var}_2X_2=\frac54$。
9. **意義**：(4) 不違反——任何時間從狀態 2 開始，兩步後分布都相同。
10. **合理性**：區間 $[0,1]$ 包含平均 $1/2$，長度約 $0.9$ 個標準差，機率約 0.35 合理。

---

## 例 11 Final exercise B（Gaussian）[MC p.61–62]

1. $K^2(2,[0,1])=2\Phi(1/\sqrt5)-1\approx0.3453$（同例 10）。
2. 使鏈 stationary 的初始分布：$X_0\sim N(0,4/3)$，且與未來 innovation 獨立。

---

## 例 12 隨機初始的 Gaussian [MC p.49]

$X_0\sim N(2,4)\perp Z_1$：$X_1=\frac12X_0+Z_1\sim N(1,\frac14\cdot4+1)=N(1,2)$。對比固定 $X_0=2$：$N(1,1)$。多出的變異數 1 來自初始狀態的不確定。

---

## 例 13 AR(1) Exercise 2（判斷對錯）[AR1 p.23, 41]

$a=\frac12,\sigma=1$。**四題全錯**。

| 敘述 | 判斷 | 理由（公式） |
|---|---|---|
| $K(x,(0,\infty))$ 隨 $x$ 變，所以 inhomogeneous | 錯 | $K(x,(0,\infty))=\Phi(x/2)$ 依賴 $x$ 但每期公式相同 |
| 從 $X_0=0$ 出發平均保持 0，所以 stationary | 錯 | $X_1\sim N(0,1)$、$X_2\sim N(0,5/4)$，分布在變 |
| 從 $\pi$ 出發使相鄰觀察獨立 | 錯 | $\mathrm{Corr}_\pi(X_0,X_1)=a=\frac12$ |
| Reversibility 需要 $k(x,y)=k(y,x)$ | 錯 | 條件是 $\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$；$k(0,2)=\varphi(2)\ne k(2,0)=\varphi(1)$ |

---

## 例 14 AR(1) Final exercise（$a=-\frac12$）[AR1 p.37–38]

$X_{n+1}=-\frac12X_n+Z_{n+1}$。

1. $K(x,\cdot)=N(-x/2,1)$；$\pi=N\!\left(0,\frac1{1-1/4}\right)=N(0,4/3)$。
2. $K^2(2,\cdot)$：平均 $a^2\cdot2=\frac14\cdot2=\frac12$；變異數 $1+a^2=\frac54$ ⇒ $N(1/2,5/4)$（與 $a=\frac12$ 相同，因為只依賴 $a^2$）。
3. $\mathrm{Corr}_\pi(X_0,X_1)=a=-\frac12$；$\mathrm{Corr}_\pi(X_0,X_2)=a^2=\frac14$。
4. **不是** period 2：Gaussian density 處處為正，aperiodic。Reversible：detailed balance 對所有 $\lvert a\rvert<1$ 成立。
5. Ergodic theorem：$\frac1N\sum_{n<N}X_n^2\to\int x^2\pi(dx)=\frac43$ a.s.

延伸：$\sqrt N\bar X_N$ 的漸近變異數 $\frac{\sigma^2}{(1-a)^2}=\frac1{(3/2)^2}=\frac49$，比 $a=\frac12$ 的 4 小很多 [AR1 p.35]。

---

## 例 15 Exit questions 的標準答案 [MC p.64, AR1 p.40]

| 問題 | 答案要點 |
|---|---|
| $\Omega$ 與 $E$ 的差異？$K(x,A)$ 的兩個輸入？ | $\Omega$ 是隨機結果、$E$ 是觀察值；輸入是一個現在狀態 $x$ 與一個目標集合 $A$ |
| Kernel 的兩個條件？ | $A\mapsto K(x,A)$ 是機率測度；$x\mapsto K(x,A)$ 可測 |
| $K^2$ 與「把元素/density 平方」的差別？ | $K^2$ 對中間狀態加總/積分路線機率乘積 |
| $k(x,y)>0$ 但 $K(x,\lbrace y\rbrace)=0$？ | 可以，機率是面積 |
| Homogeneous 為何可能不 stationary？ | 初始分布不是 invariant（例 $X_0=0$） |
| Stationarity 與 reversibility 檢查聯合分布的什麼？ | 時間平移不變 vs 時間反轉不變 |
| 依賴 $x$ 為何保持 homogeneity？ | homogeneity 只禁止依賴時間 $n$ |
| Stationary 為何可相依？ | 只要求相依結構平移不變；$\mathrm{Corr}=a^h$ |
| 為何能重訪每個區間卻不命中指定點？ | 區間正測度、Harris recurrent；單點測度 0 |
| 哪個公式顯示初始狀態失去影響？ | $a^nx\to0$；coupling $X_n-Y_n=a^n(X_0-Y_0)$ |

---

## 例 16 Metropolis 與 MH 單步計算（補充，依 H10 p.7, 13 設定）

**Metropolis**：$\pi(x)\propto e^{-x^2/2}$，$X_n=0$，$Y=1$：$\alpha=\min(1,e^{-1/2})=0.6065$。$U=0.4$ ⇒ 接受；$U=0.8$ ⇒ $X_{n+1}=0$。

**MH**（$q(y\mid x)=N(y;x+0.5,1)$）：$X_n=0$，$Y=1$：

$$
\alpha=\min\!\left(1,\frac{e^{-1/2}\cdot\varphi(0-1.5)}{e^{0}\cdot\varphi(1-0.5)}\right)=\min(1,e^{-0.5}e^{-(2.25-0.25)/2})=e^{-1.5}\approx0.2231 .
$$

**反例**：若誤用 Metropolis ratio，得 $0.6065$——高估了往右移動的接受率，鏈會偏右。

---

## 例 17 離散 Metropolis：建 kernel 並驗證（補充）

1. **題目**：$E=\lbrace0,1,2\rbrace$，目標 $\pi=(0.2,0.3,0.5)$。Proposal：從 $i$ 以各 1/2 提議另外兩個狀態（對稱）。寫出 $K$ 並驗證 $\pi K=\pi$。
2. **公式**：$K_{ij}=q(j\mid i)\min(1,\pi_j/\pi_i)$（$j\ne i$），$K_{ii}=1-\sum_{j\ne i}K_{ij}$。
3. **計算**：
   - 列 0：$K_{01}=\frac12\min(1,1.5)=\frac12$，$K_{02}=\frac12$，$K_{00}=0$。
   - 列 1：$K_{10}=\frac12\cdot\frac{0.2}{0.3}=\frac13$，$K_{12}=\frac12$，$K_{11}=1-\frac13-\frac12=\frac16$。
   - 列 2：$K_{20}=\frac12\cdot0.4=0.2$，$K_{21}=\frac12\cdot0.6=0.3$，$K_{22}=0.5$。

$$
K=\begin{pmatrix}0&1/2&1/2\\1/3&1/6&1/2\\0.2&0.3&0.5\end{pmatrix}.
$$

4. **Detailed balance**：$\pi_0K_{01}=0.1=\pi_1K_{10}$；$\pi_0K_{02}=0.1=\pi_2K_{20}$；$\pi_1K_{12}=0.15=\pi_2K_{21}$ ✓。
5. **$\pi K$**：$(0.1+0.1,\ 0.1+0.05+0.15,\ 0.1+0.15+0.25)=(0.2,0.3,0.5)$ ✓。
6. **意義**：$K_{11},K_{22}>0$ 來自拒絕；鏈 irreducible 且有自環 ⇒ aperiodic ⇒ $K^n\to\pi$。

---

## 例 18 Gibbs 單步（補充，依 H10 p.19 設定）

$\rho=0.8$，現在 $Y^{(t)}=1$。$X^{(t+1)}\sim N(0.8,0.36)$，標準差 0.6。$P(X^{(t+1)}>0)=\Phi(0.8/0.6)=\Phi(4/3)\approx0.909$。**常見錯**：把 0.36 當標準差。

---

## 例 19 Example 3.14 的 MLE（H8 p.46–47）

$(m_{11},m_{12},m_{21},m_{22})=(68,28,17,4)$：$\hat p_1=\frac{68}{96}\approx0.708$，$\hat p_2=\frac{4}{21}\approx0.190$。Posterior 形狀：$\mathrm{Be}(69,29)\times\mathrm{Be}(5,18)$ 截斷於 $p_1+p_2<1$；此截斷區的 posterior 機率為 0.21 [H8 p.46]。

---

## 例 20 二狀態系統：完整流程（補充）

$K=\begin{pmatrix}0.9&0.1\\0.6&0.4\end{pmatrix}$（0 正常、1 故障）。

- 兩步：$K^2=\begin{pmatrix}0.87&0.13\\0.78&0.22\end{pmatrix}$；今天正常、後天故障 $=0.13=0.9(0.1)+0.1(0.4)$。
- Stationary：二狀態時 detailed balance $\pi_0K_{01}=\pi_1K_{10}$ ⇒ $0.1\pi_0=0.6\pi_1$ ⇒ $\pi=(6/7,1/7)$。
- 收斂：irreducible、$K_{00}>0$ ⇒ aperiodic ⇒ $K^n$ 每列 → $(0.857,0.143)$（程式驗證 $K^{50}$）。
- 平均回返：$E_1\tau_1^+=7$。

**反例**：$K=\begin{pmatrix}0&1\\1&0\end{pmatrix}$（$m=1$ urn）：$\pi=(1/2,1/2)$，但 $K^n$ 在 $I$ 與 $K$ 間交替，**沒有極限**——不能把例 20 的收斂結論套用到 periodic chain。
