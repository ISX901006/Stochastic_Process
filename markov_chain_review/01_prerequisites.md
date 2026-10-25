# 01 前置知識：後續章節一定會用到的工具

本檔只整理後面推導**真的會用到**的基礎。每一節最後都寫「在 Markov Chain 哪裡用到」。

## 1. 機率空間 $(\Omega,\mathcal F,P)$ 與 State space $(E,\mathcal E)$

**直覺**：擲一顆骰子。「宇宙所有可能的結果」是 $\Omega$；「你能問機率的問題」（例如「點數是偶數嗎？」）是事件，事件的集合是 $\mathcal F$；$P$ 給每個事件一個 0 到 1 的數字。

| 符號 | 意義 | 骰子例子 |
|---|---|---|
| $\Omega$ | Sample space，所有隨機結果 $\omega$ | $\lbrace 1,\dots,6\rbrace$ |
| $\mathcal F$ | σ-algebra，可以問機率的事件集合 | 所有子集合 |
| $P$ | 機率測度，$P(\Omega)=1$、對互斥事件可數可加 | $P(\lbrace 2,4,6\rbrace)=1/2$ |

老師特別區分兩個空間 [MC p.7, AR1 p.3]：

- $(\Omega,\mathcal F,P)$：**隨機性的來源**（看不到的「宇宙」）。
- $(E,\mathcal E)$：**我們觀察到的值**所在的空間，即 State space。urn 模型 $E=\lbrace 0,\dots,m\rbrace$、$\mathcal E=2^E$（所有子集）；AR(1) $E=\mathbb R$、$\mathcal E=\mathcal B(\mathbb R)$（Borel sets，可以想成「由區間組出來的所有合理集合」）。

$X_n:\Omega\to E$ 把一個結果 $\omega$ 對應到時間 $n$ 的狀態 $X_n(\omega)$。

**σ-algebra 白話**：「可以合法問機率的集合」清單，對補集、可數聯集封閉。有限狀態空間時取所有子集即可，不必擔心；連續狀態時用 Borel sets。

**Measurable（可測）白話**：函數 $f$ 可測，意思是對每個合法集合 $B$，「$f$ 的值落在 $B$」這件事也是合法事件，所以可以問它的機率或對它積分。有限空間上任何函數都可測 [MC p.6, 14]。

**用在哪**：kernel 定義第 2 條（$x\mapsto K(x,A)$ 可測）、Markov property 的 $\mathcal F_n$。

## 2. 條件機率、獨立、全機率公式、Bayes

**條件機率**：

$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)},\qquad P(B)>0 .
$$

白話：「已知 $B$ 發生，把宇宙縮小到 $B$，再看 $A$ 佔多少」。

**乘法規則**（推導 path probability 的核心）：

$$
P(A\cap B\cap C)=P(A)\,P(B\mid A)\,P(C\mid A\cap B).
$$

**獨立**：$P(A\cap B)=P(A)P(B)$，等價於 $P(A\mid B)=P(A)$。

**全機率公式（Law of Total Probability）**：若 $B_1,B_2,\dots$ 互斥且聯集為全部，

$$
P(A)=\sum_k P(A\mid B_k)\,P(B_k).
$$

白話：「先按中間情況分類，各類的機率加權平均」。這正是 Chapman–Kolmogorov「對中間狀態加總」的來源。

**Bayes**：$P(B_k\mid A)=\dfrac{P(A\mid B_k)P(B_k)}{\sum_j P(A\mid B_j)P(B_j)}$。用於 H10 posterior、H8 Example 3.14。

**數值檢查**：天氣今天晴的機率 0.6、雨 0.4；晴→明天晴 0.7，雨→明天晴 0.4。則明天晴 $=0.7(0.6)+0.4(0.4)=0.58$。這就是 $\nu K$ 的一個分量。

**用在哪**：Ch3 從 urn 實驗算 $K_{ij}$、Ch4 的 $K^2$、$\nu K$。

## 3. 隨機變數、PMF/PDF、期望、變異數、共變異數

| 概念 | 離散 | 連續 |
|---|---|---|
| 分布 | PMF $p(k)=P(X=k)$ | PDF $f(x)$，$P(X\in A)=\int_A f$ |
| 單點機率 | 可為正 | **一律為 0** |
| 期望 $E[g(X)]$ | $\sum_k g(k)p(k)$ | $\int g(x)f(x)\,dx$ |

- $\mathrm{Var}(X)=E[X^2]-(E X)^2$。
- $\mathrm{Cov}(X,Y)=E[XY]-E[X]E[Y]$；$\mathrm{Cov}(aX+bZ,X)=a\,\mathrm{Var}(X)+b\,\mathrm{Cov}(Z,X)$（雙線性）。
- $\mathrm{Corr}(X,Y)=\mathrm{Cov}(X,Y)/\sqrt{\mathrm{Var}X\,\mathrm{Var}Y}$。
- 獨立 ⇒ $\mathrm{Var}(X+Y)=\mathrm{Var}X+\mathrm{Var}Y$。

**指標函數** $\mathbf 1_A(x)=1$ 若 $x\in A$，否則 0。關鍵等式：$E[\mathbf 1_A(X)]=P(X\in A)$。老師用 $K^0(x,A)=\mathbf 1_A(x)$（走 0 步就留在原地）[MC p.20]。

**PDF 可以大於 1**：$N(\mu,0.1^2)$ 在中心的高度 $=1/(0.1\sqrt{2\pi})\approx 3.99$ [MC p.43]。機率是面積不是高度。

**用在哪**：$Kf$、AR(1) 的條件期望與相關係數。

## 4. 條件期望

**基礎版**：$E[Y\mid X=x]$ 是「已知 $X=x$ 時 $Y$ 的平均」，是 $x$ 的函數 $g(x)$；$E[Y\mid X]=g(X)$ 是隨機變數。

**σ-algebra 版**（老師寫法 [MC p.8]）：$P(X_{n+1}\in A\mid\mathcal F_n)$ 表示「已知到時間 $n$ 為止的所有歷史，下一步落在 $A$ 的機率」，它是一個由歷史決定的隨機變數。等式後面寫 **a.s.**（almost surely）表示「除了機率 0 的情況外都相等」。

**Tower property**：$E[E[Y\mid\mathcal G]]=E[Y]$。這是 $\mathcal L(X_{n+1})=\mathcal L(X_n)K$ 的根據。

**用在哪**：Markov property 的正式定義、$(Kf)(x)=E[f(X_{n+1})\mid X_n=x]$。

## 5. Normal 分布的計算工具

- $Z\sim N(0,1)$，CDF $\Phi(t)=P(Z\le t)$，對稱 $\Phi(-t)=1-\Phi(t)$。
- **標準化**：$Y\sim N(\mu,s^2)$ ⇒ $P(b<Y\le c)=\Phi\!\left(\frac{c-\mu}{s}\right)-\Phi\!\left(\frac{b-\mu}{s}\right)$。
- **線性組合**：獨立 Normal 的線性組合仍是 Normal：$aX+\sigma Z\sim N(a\mu_X,\ a^2 s_X^2+\sigma^2)$ 若 $X\sim N(\mu_X,s_X^2)$ 與 $Z$ 獨立。
- 常用數值：$\Phi(1)\approx 0.8413$、$\Phi(1)-\Phi(-1)\approx 0.6827$、$\Phi(2)\approx 0.9772$、$\Phi(1/\sqrt5)\approx 0.6726$。

**Bivariate normal 條件分布** [H10 p.27]：

$$
X\mid Y=y\sim N\!\left(\mu_X+\frac{\sigma_{XY}}{\sigma_Y^2}(y-\mu_Y),\ \sigma_X^2-\frac{\sigma_{XY}^2}{\sigma_Y^2}\right).
$$

**用在哪**：AR(1) 全部計算、Gibbs sampling。

## 6. 矩陣乘法與 row vector

Row vector $\nu$（$1\times N$）乘矩陣 $K$（$N\times N$）：

$$
(\nu K)_j=\sum_i \nu_i K_{ij}.
$$

白話：第 $j$ 個分量 = 「每個起點 $i$ 的權重 × 從 $i$ 走到 $j$ 的機率」再全部加起來。

矩陣乘矩陣：$(K^2)_{ij}=\sum_z K_{iz}K_{zj}$——第 $i$ 列與第 $j$ 欄的內積。

**維度檢查**：$(1\times N)(N\times N)=1\times N$。若寫成 $K\nu$（$N\times N$ 乘 $1\times N$）維度不合，代表方向錯了。

矩陣乘函數（column vector）：$(Kf)_i=\sum_j K_{ij}f(j)$，$f$ 放右邊。**分布在左、函數在右**是本課程的固定規則 [MC p.22–23]。

**用在哪**：所有有限狀態計算。

## 7. 最大公因數 gcd

$\gcd\lbrace 2,3\rbrace=1$、$\gcd\lbrace 2,4,6,\dots\rbrace=2$。只要集合裡有兩個互質的數，gcd 就是 1。**用在** Period $d(i)$ [MC p.25]。

## 8. 收斂的種類與極限定理

| 名稱 | 定義 | 白話 |
|---|---|---|
| Almost sure（a.s.） | $P(\lim_n Y_n=Y)=1$ | 幾乎每一條樣本路徑最終都會貼住極限 [H9 p.35] |
| In probability | $P(\lvert Y_n-Y\rvert>\epsilon)\to 0$ | 固定很大的 $n$，偏很遠的機率小 |
| Weak convergence $\mu_n\Rightarrow\mu$ | $\int f\,d\mu_n\to\int f\,d\mu$ 對所有有界連續 $f$ | 分布的形狀越來越像 [MC p.56] |
| Total variation | $\lVert\mu-\nu\rVert_{TV}=\sup_A\lvert\mu(A)-\nu(A)\rvert$ | 所有事件機率的最大誤差 [AR1 p.30] |

- **SLLN**：i.i.d. 且 $E\lvert h(X)\rvert<\infty$ ⇒ $\frac1m\sum h(X_j)\to E h(X)$ a.s. [H8 p.19]。
- **CLT**：$\sqrt m(\bar h_m-Eh)\Rightarrow N(0,\mathrm{Var}\,h(X))$ [H8 p.20]。
- **Dominated convergence**：若 $g_n(x)\to g(x)$ 且 $\lvert g_n\rvert\le G$ 可積，則 $\int g_n\,d\mu\to\int g\,d\mu$。用在 AR(1) uniqueness 證明 [AR1 p.28]。

Markov chain 的 ergodic theorem 是 SLLN 的「相依版本」：樣本不獨立仍然平均收斂，但變異數改變 [AR1 p.32–34]。

## 9. Lebesgue measure

$\lambda(A)$ = 集合 $A$ 的「長度」。$\lambda([0,2])=2$，$\lambda(\lbrace 1\rbrace)=0$。用於連續狀態的 irreducibility [MC p.54, AR1 p.24]。

## 10. 前置知識自我檢查

1. $P(X\in A)$ 與 $P(X\in A\mid Y=y)$ 差在哪？（後者的宇宙被縮小到 $Y=y$）
2. 連續變數 $P(X=1)$ 等於多少？PDF 在 1 的值可以是 3 嗎？（0；可以）
3. $(1/2,1/2,0)$ 乘矩陣時放左邊還是右邊？（左邊）
4. $\gcd\lbrace 2,3,4,5\rbrace=?$（1）
5. 若 $X\sim N(2,4)$ 與 $Z\sim N(0,1)$ 獨立，$X/2+Z$ 的分布？（$N(1,2)$，見 MC p.49）
