# 02d Ch5–8（連續狀態）：Gaussian AR(1) 的完整理論

來源：[MC p.35–63, 66, 70–73]、[AR1 全部]

模型 [AR1 p.3]：

$$
X_{n+1}=aX_n+\sigma Z_{n+1},\qquad Z_n\overset{\text{i.i.d.}}\sim N(0,1)\ \text{independent of }X_0,\quad a\in\mathbb R,\ \sigma>0 .
$$

Running example：$a=1/2$、$\sigma=1$，即 $X_{n+1}=\tfrac12X_n+Z_{n+1}$。研究平衡時才加上 $\lvert a\rvert<1$。

---

## 2d.1 直覺：一個會「回到中心」的彈簧

想像一顆綁在彈簧上的球，每期位置先被彈簧拉回中心一半（$aX_n$，可預測的部分），再被隨機風吹一下（$\sigma Z_{n+1}$，新鮮雜訊）。
- 彈簧把舊位置的影響逐期縮小（$a^n\to0$）；
- 風持續注入新的不確定性，但被縮小的舊雜訊抵消，變異數最後穩定在 $\sigma^2/(1-a^2)$。

**比喻對應**：彈簧強度 = $a$；風力 = $\sigma$；位置 = 狀態 $x\in\mathbb R$；時間仍是離散的 [MC p.35]。

---

## 2d.2 Kernel 與 Transition Density

固定 $X_n=x$，$ax$ 是常數 ⇒

$$
X_{n+1}\mid X_n=x\sim N(ax,\sigma^2),\qquad k(x,y)=\frac{1}{\sigma\sqrt{2\pi}}\exp\!\left[-\frac{(y-ax)^2}{2\sigma^2}\right],
$$

$$
K(x,dy)=k(x,y)\,dy,\qquad K(x,A)=\int_Ak(x,y)\,dy\qquad[\text{MC p.37, AR1 p.6}].
$$

**Interpretation**：A matrix row becomes a density curve. Summing selected entries becomes integrating over a set. 不同的 $x$ 給出不同的曲線（$a=1/2$ 時 $x=-2,0,2$ 的曲線中心在 $-1,0,1$，變異數都是 1）[MC p.38]。

### Interval formula [MC p.40]

$$
K(x,(b,c])=P(b<ax+\sigma Z\le c)=\Phi\!\left(\frac{c-ax}{\sigma}\right)-\Phi\!\left(\frac{b-ax}{\sigma}\right),\qquad K(x,(-\infty,c])=\Phi\!\left(\frac{c-ax}{\sigma}\right).
$$

| 項目 | 說明 |
|---|---|
| 用途 | 一步落在區間的機率 |
| 條件 | Gaussian AR(1)，$\sigma>0$ |
| 步驟 | 固定現在狀態 → 認出目標事件 → 標準化下一步的 Gaussian |
| 常見錯誤 | 用 $x$ 當平均（應為 $ax$）；用 $\sigma^2$ 當分母（應為 $\sigma$） |

數值 [MC p.39]：$x=2$，下一步 $N(1,1)$，$K(2,[0,2])=P(-1\le Z\le1)\approx0.6827$。

### Density 值 vs 點機率 [MC p.43, AR1 p.8]

$K(x,\lbrace y\rbrace)=0$ 對每個 $y$，即使 $k(x,y)>0$，甚至 $k(x,y)>1$（$\sigma=0.1$ 時 $k(x,ax)\approx3.99$）。$K(x,[y,y+h])\approx k(x,y)h$。**Probability is area.** 這是 Exit question 4 [MC p.64]。

### Kernel conditions 的驗證 [MC p.41, AR1 p.7]
1. 固定 $x$：$k\ge0$、$\int k=1$，積分對互斥集合可加 ⇒ 機率測度。
2. 固定 $A$：$x\mapsto\int\mathbf 1_A(y)k(x,y)\,dy$，被積函數聯合可測且非負 ⇒ 可測。

**No condition $\lvert a\rvert<1$ is needed to verify the kernel axioms.** 每個 $a\in\mathbb R$、$\sigma>0$ 都給出合法 kernel [MC p.58]。

### Homogeneity 再提醒 [AR1 p.9]

$K(x,(0,\infty))=\Phi(x/2)$ 隨 $x$ 變，但同一個 $x$ 在任何時間都一樣 ⇒ homogeneous。若改為 $a_n$ 隨時間變，則 kernel 為 $K_n$，不 homogeneous。

---

## 2d.3 $Kf$ 與隨機初始狀態

$$
(Kf)(x)=\int f(y)\,K(x,dy)=E[f(ax+\sigma Z)]\qquad[\text{AR1 p.10}]
$$

| $f(y)$ | $(Kf)(x)$ | 意義 |
|---|---|---|
| $1$ | $1$ | 總機率 |
| $\mathbf 1_A(y)$ | $K(x,A)$ | 事件機率 |
| $y$ | $ax$ | 條件平均 |
| $y^2$ | $a^2x^2+\sigma^2$ | 條件二階動差 |

⇒ $\mathrm{Var}(X_{n+1}\mid X_n=x)=\sigma^2$（與 $x$ 無關）。

**隨機初始** [MC p.49]：$X_0\sim N(2,4)$ 與 $Z_1$ 獨立 ⇒ $EX_1=\tfrac12(2)=1$、$\mathrm{Var}X_1=\tfrac14(4)+1=2$，$X_1\sim N(1,2)$。對比 $X_0=2$ 固定時 $X_1\sim N(1,1)$。**The kernel is unchanged. The unconditional next-state law depends on the initial law.**

---

## 2d.4 n-Step Law

反覆代入 [MC p.45, 47; AR1 p.12]：

$$
X_2=a(ax+\sigma Z_1)+\sigma Z_2=a^2x+a\sigma Z_1+\sigma Z_2,\qquad X_n=a^nx+\sigma\sum_{j=1}^na^{n-j}Z_j,
$$

$$
K^n(x,\cdot)=N\!\left(a^nx,\ \sigma^2\sum_{k=0}^{n-1}a^{2k}\right)\overset{\lvert a\rvert<1}=N\!\left(a^nx,\ \frac{\sigma^2(1-a^{2n})}{1-a^2}\right).
$$

- $a^nx$：**記得起點**的部分（Discuss [MC p.47]）。
- $\sigma^2\sum a^{2k}$：**累積的新雜訊**。

$x=2$、$a=1/2$、$\sigma=1$：

| $n$ | 1 | 2 | 3 |
|---|---|---|---|
| 條件平均 | 1 | 1/2 | 1/4 |
| 條件變異數 | 1 | 5/4 | 21/16 |

C–K 的 density 版 [MC p.46]：$k_2(x,y)=\int_{\mathbb R}k(x,z)k(z,y)\,dz$（矩陣乘法的「加總」變成「積分」）。

推導見 [03b §1](03b_derivations_ar1.md)。

---

## 2d.5 Invariant Law

設 $\lvert a\rvert<1$，試 $X\sim N(0,v)$ 與 $Z$ 獨立 [MC p.50, AR1 p.17]：

$$
aX+\sigma Z\sim N(0,a^2v+\sigma^2)\ \overset{\text{要等於}}{=}\ N(0,v)\iff v=a^2v+\sigma^2\iff v=\frac{\sigma^2}{1-a^2}.
$$

$$
\pi=N\!\left(0,\frac{\sigma^2}{1-a^2}\right);\qquad a=\tfrac12,\sigma=1:\ \pi=N(0,4/3).
$$

意義：$X\sim\pi$、$X\perp Z$ ⇒ $aX+\sigma Z\sim\pi$ [AR1 p.16]。**One update preserves the entire probability law. Individual states continue to change.**

**注意**：這個計算只證明「存在」。唯一性要另外用收斂證明（2d.9）[AR1 p.17]。$\lvert a\rvert\ge1$ 時 $v=a^2v+\sigma^2$ 無正解，且可以證明**任何**（包括非 Gaussian、無限變異數）invariant probability 都不存在（2d.12）。

**非零平衡平均** [MC p.71, AR1 p.49]：$X_{n+1}=\mu+a(X_n-\mu)+\sigma Z_{n+1}$ ⇒ $\pi=N(\mu,\sigma^2/(1-a^2))$，$K^n(x,\cdot)=N(\mu+a^n(x-\mu),\sigma^2(1-a^{2n})/(1-a^2))$。減去 $\mu$ 即回到中心化模型。

---

## 2d.6 Stationarity 與相依性

### 兩種初始化 [AR1 p.19]

| | $X_0=2$ 固定 | $X_0\sim N(0,4/3)$ |
|---|---|---|
| 時間 0 | $\delta_2$ | $N(0,4/3)$ |
| 時間 1 | $N(1,1)$ | $N(0,4/3)$ |
| Time homogeneous? | Yes | Yes |
| Strictly stationary? | **No** | **Yes** |

Discuss：只知道 kernel 不能判斷 stationarity，還要知道初始分布。

**陷阱** [AR1 p.41]：從 $X_0=0$ 出發，平均一直是 0，但 $X_1\sim N(0,1)$、$X_2\sim N(0,5/4)$，分布在變 ⇒ **不是** stationary。

### Stationary 仍然相依 [MC p.52, AR1 p.20]

$X_0\sim\pi$，$v=\sigma^2/(1-a^2)$：

$$
\mathrm{Cov}(X_0,X_1)=\mathrm{Cov}(X_0,aX_0+\sigma Z_1)=av,\qquad\mathrm{Corr}_\pi(X_0,X_h)=a^h .
$$

- $a>0$：正相關、遞減。
- $a<0$：正負交替、遞減。
- $a=0$：stationary 觀察值是獨立的 $N(0,\sigma^2)$（Discuss [MC p.52] 的答案）。

Running example：每個邊際都是 $N(0,4/3)$，但相鄰相關 1/2。**Stationarity permits dependence. It requires that the dependence structure stays the same after a time shift.** 這是 Exit question「Why can a stationary chain have dependent observations?」[AR1 p.40]。

---

## 2d.7 Reversibility

$\lvert a\rvert<1$，$\varphi_\pi$ 為 $\pi$ 的 density [MC p.53, AR1 p.22]：

$$
\varphi_\pi(x)k(x,y)=\frac{\sqrt{1-a^2}}{2\pi\sigma^2}\exp\!\left[-\frac{(1-a^2)x^2+(y-ax)^2}{2\sigma^2}\right],\qquad(1-a^2)x^2+(y-ax)^2=x^2+y^2-2axy .
$$

指數與常數都對 $x,y$ 對稱 ⇒ $\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$（detailed balance，density form），**including when $a<0$**。

另一證法 [MC p.70]：stationary 時 $(X_0,X_1)$ 是中心化 bivariate Gaussian，共變異矩陣 $v\begin{pmatrix}1&a\\a&1\end{pmatrix}$，交換座標不變 ⇒ $(X_0,X_1)\overset d=(X_1,X_0)$。

**陷阱** [AR1 p.41]：reversibility **不需要** $k(x,y)=k(y,x)$。$k(0,2)=\varphi(2)$，$k(2,0)=\varphi(-1)=\varphi(1)$，兩者不同。Detailed balance 是**加權**對稱。

---

## 2d.8 Irreducibility、Aperiodicity、Harris Recurrence（連續版）

### ψ-irreducibility [MC p.54, AR1 p.24]

點的機率都是 0，所以「每個狀態可到達」沒有意義。改問「每個**有正測度的集合**可到達」：

$$
\psi(A)>0\implies K^n(x,A)>0\ \text{for some }n\ge1,\quad\forall x .
$$

取 $\psi=\lambda$（Lebesgue）：因 $k(x,y)>0$，$\lambda(A)>0\Rightarrow K(x,A)=\int_Ak>0$，**一步就夠**。有限空間取 counting measure 即回到熟悉的 irreducibility。

### Aperiodicity [AR1 p.25]

Periodic irreducible chain：存在不相交 cyclic sets $D_0,\dots,D_{d-1}$（正測度，$d\ge2$），$K(x,D_{j+1\bmod d})=1$ for $x\in D_j$。AR(1)：$\lambda(D_j)>0$ ⇒ $K(x,D_j)>0$ for $x\in D_j$，不可能以機率 1 跳到另一個不相交集合 ⇒ aperiodic。**$a<0$ 的正負交替相關不構成 period 2**（AR1 final exercise Q4）。

### Harris recurrence [AR1 p.26]

$\tau_A^+=\inf\lbrace n\ge1:X_n\in A\rbrace$。λ-irreducible chain 是 Harris recurrent 若

$$
P_x(\tau_A^+<\infty)=1\quad\forall x,\ \forall A\text{ with }\lambda(A)>0,
$$

等價於每個這樣的集合 a.s. 被造訪無限多次。**Positive Harris recurrent**：再加上存在 invariant probability。

**結果**：$\sigma>0$、$\lvert a\rvert<1$ ⇒ Gaussian AR(1) positive Harris recurrent。

**Interpretation**：「有正機率造訪」（irreducibility）與「機率 1 最終造訪」（recurrence）不同。Exit question：How can a chain revisit every interval without ever hitting one prescribed point? → 每個區間正測度、會被無限次造訪；單點測度 0，$K(x,\lbrace y\rbrace)=0$ [AR1 p.40]。

---

## 2d.9 收斂：Weak、Uniqueness、Coupling、TV

### Weak convergence [MC p.56, AR1 p.27]

$\lvert a\rvert<1$：$a^nx\to0$、$v(1-a^{2n})\to v$，整個 law 是 Gaussian：

$$
K^n(x,(-\infty,c])=\Phi\!\left(\frac{c-a^nx}{\sqrt{v(1-a^{2n})}}\right)\to\Phi(c/\sqrt v),\qquad K^n(x,\cdot)\Rightarrow\pi .
$$

定義：$\mu_n\Rightarrow\mu$ ⇔ $\int f\,d\mu_n\to\int f\,d\mu$ 對每個有界連續 $f$。

圖 [MC p.57, AR1 p.15]：$X_0=4$ 時 $n=1,2,5$ 的 density 依序靠近虛線 $N(0,4/3)$。

### Uniqueness [AR1 p.28]

設 $\mu$ 是任意 invariant probability（不假設 Gaussian），對有界連續 $f$：$\int f\,d\mu=\int K^nf(x)\,\mu(dx)$；$K^nf(x)\to\int f\,d\pi$ 且 $\lvert K^nf\rvert\le\lVert f\rVert_\infty$；dominated convergence ⇒ $\int f\,d\mu=\int f\,d\pi$ ⇒ $\mu=\pi$。推導見 [03b §6](03b_derivations_ar1.md)。

### Coupling [AR1 p.29]

兩條鏈用**同一組**雜訊：$X_n-Y_n=a^n(X_0-Y_0)$。$a=1/2$ 時差距每步減半（例 $X_0=4$、$Y_0=-3$）。**Both paths keep fluctuating, but the effect of different initial states fades.** Exit question：Which formula shows that the initial state loses influence? → $a^nx$ 與 $X_n-Y_n=a^n(X_0-Y_0)$ [AR1 p.40]。

### Total variation 與 geometric ergodicity [AR1 p.30]

$$
\lVert\mu-\nu\rVert_{TV}=\sup_{A}\lvert\mu(A)-\nu(A)\rvert=\tfrac12\int\lvert p-q\rvert ;
$$

Geometrically ergodic：$\lVert K^n(x,\cdot)-\pi\rVert_{TV}\le M(x)\rho^n$，$\rho\in(0,1)$，$M(x)<\infty$。AR(1) $0<\lvert a\rvert<1$：$\rho=\lvert a\rvert$；$a=0$：一步後就是 $\pi$。顯式上界（附錄，Pinsker）[AR1 p.45]：

$$
\lVert K^n(x,\cdot)-\pi\rVert_{TV}\le\left(\frac{\lvert x\rvert}{2\sqrt v}+\frac{1}{2\sqrt{2(1-a^2)}}\right)\lvert a\rvert^n .
$$

**不是 uniform** [AR1 p.46]：任何固定 $n$，$\sup_x\lVert K^n(x,\cdot)-\pi\rVert_{TV}=1$（起點夠遠時平均 $a^nx$ 任意大）。**There is no single finite burn-in that works for arbitrarily distant starts.**

（附錄 Feller / strong Feller [AR1 p.42]、small set [AR1 p.43]、drift $KV\le\beta V+b\mathbf 1_C$ with $V=1+x^2$ [AR1 p.44] 是證明 geometric ergodicity 的一般工具；屬 optional material。）

---

## 2d.10 Ergodic Theorem 與樣本平均

### Ergodic theorem [AR1 p.32]

Stationary process 是 ergodic 若每個「平移不變事件」機率為 0 或 1。Stable AR(1)：

$$
\frac1N\sum_{n=0}^{N-1}f(X_n)\to\int f\,d\pi\ \text{a.s.},\qquad\int\lvert f\rvert\,d\pi<\infty,
$$

從**任一固定起點**或 stationary 起點都成立。例：$\frac1N\sum X_n\to0$、$\frac1N\sum X_n^2\to\frac{\sigma^2}{1-a^2}$、$\frac1N\sum\mathbf 1_A(X_n)\to\pi(A)$。

**兩種極限不同** [AR1 p.39]：$K^n(x,\cdot)\to\pi$（分布）vs $N^{-1}\sum f(X_n)\to\pi(f)$（路徑平均）。

### 樣本平均的初等證明 [AR1 p.33]

把 $X_{n+1}-aX_n=\sigma Z_{n+1}$ 對 $n=0,\dots,N-1$ 加總：

$$
(1-a)\sum_{n=0}^{N-1}X_n+X_N-X_0=\sigma\sum_{j=1}^NZ_j\ \Rightarrow\ \bar X_N=\frac{\sigma}{1-a}\cdot\frac1N\sum_{j=1}^NZ_j-\frac{X_N-X_0}{(1-a)N}\to0\ \text{a.s.}
$$

### 相依改變變異數 [AR1 p.34]

Stationary 下 $\mathrm{Cov}(X_i,X_j)=va^{\lvert i-j\rvert}$：

$$
\mathrm{Var}_\pi(\bar X_N)=\frac vN\left[1+2\sum_{h=1}^{N-1}\left(1-\frac hN\right)a^h\right],\qquad N\,\mathrm{Var}_\pi(\bar X_N)\to v\frac{1+a}{1-a}=\frac{\sigma^2}{(1-a)^2}.
$$

| $a$ | 邊際 $\pi$ | $\sigma^2/(1-a)^2$ | 對比 i.i.d. $v$ |
|---|---|---|---|
| $1/2$ | $N(0,4/3)$ | 4 | 4/3 |
| $-1/2$ | $N(0,4/3)$ | 4/9 | 4/3 |

**同一個邊際分布，精度卻差 9 倍** [AR1 p.35]。正相關使平均更不準（有效樣本數變小），負相關反而更準。這直接關係到 MCMC 的效率（H10 的 autocorrelation、thinning）。

CLT（附錄）[AR1 p.50]：$\sqrt N\bar X_N\Rightarrow N\!\left(0,\frac{\sigma^2}{(1-a)^2}\right)$，固定起點也成立。

---

## 2d.11 參數的角色 [MC p.58, AR1 p.36]

| 參數（$\sigma>0$） | 行為 |
|---|---|
| $a=0$ | 下一步是新的 $N(0,\sigma^2)$，與現在無關；一步就是 $\pi$ |
| $0<\lvert a\rvert<1$ | 唯一 invariant Gaussian law、positive Harris recurrent、reversible、geometric convergence |
| $a=1$ | Gaussian random walk，$K^n(x,\cdot)=N(x,n\sigma^2)$，變異數發散 |
| $\lvert a\rvert\ge1$ | 合法、homogeneous、irreducible、aperiodic kernel，但**沒有 invariant probability** |

**Interpretation：Checking the kernel conditions and studying long-term behaviour are different tasks.**

Mean reversion [MC p.55]：$X_0=4$，條件平均 $a^n\cdot4$；$a=1/2$ 單調趨近 0，$a=-1/2$ 正負交替趨近 0（這些是**平均**，不是樣本路徑）。

## 2d.12 $\lvert a\rvert\ge1$ 無 invariant probability [AR1 p.47]

設 invariant law 存在，特徵函數 $\chi(t)=E[e^{itX}]$。由 $X\overset d=aX+\sigma Z$ 與獨立性：$\chi(t)=\chi(at)e^{-\sigma^2t^2/2}$。迭代 $n$ 次：$\chi(t)=\chi(a^nt)\exp\!\left(-\frac{\sigma^2t^2}2\sum_{j=0}^{n-1}a^{2j}\right)$。$\lvert a\rvert\ge1$ 時和發散，$\lvert\chi\rvert\le1$ ⇒ $\chi(t)=0$ 對所有 $t\ne0$，與 $\chi$ 在 0 連續、$\chi(0)=1$ 矛盾。**此論證也排除無限變異數的 invariant law。**

## 2d.13 OU 連結 [MC p.63, AR1 p.48]

$dY_t=-\kappa Y_tdt+\eta\,dW_t$，每隔 $\Delta$ 觀察 $X_n=Y_{n\Delta}$ ⇒ AR(1) with $a=e^{-\kappa\Delta}$、$\sigma^2=\frac{\eta^2}{2\kappa}(1-e^{-2\kappa\Delta})$；invariant variance $\sigma^2/(1-a^2)=\eta^2/(2\kappa)$。只產生 $0<a<1$ 的部分。（連續時間不屬考試主體。）

---

## 2d.14 兩個模型的對照表 [MC p.59]

| 問題 | Bernoulli–Laplace | Gaussian AR(1) |
|---|---|---|
| 現在狀態 | 整數紅球數 $i$ | 實數 $x$ |
| 下一步描述 | 機率列 $(K_{ij})_j$ | density 曲線 $k(x,y)$ |
| 集合機率 | $K(i,A)=\sum_{j\in A}K_{ij}$ | $K(x,A)=\int_Ak(x,y)\,dy$ |
| 兩步 | 對中間狀態加總 | 對中間狀態積分 |
| $f$ 的期望 | $(Kf)(i)=\sum_jf(j)K_{ij}$ | $(Kf)(x)=\int f(y)k(x,y)\,dy$ |
| 初始 $\nu$ | 列的平均 $\nu K$ | 條件分布的平均 $\nu K$ |
| Invariant | $\pi K=\pi$，超幾何 | $\pi K=\pi$，$N(0,\sigma^2/(1-a^2))$ |
| Irreducible | counting measure | Lebesgue measure，一步 |
| Aperiodic | gcd = 1（$m\ge2$） | 正 density 無 cyclic 分解 |
| Reversible | $\pi_iK_{ij}=\pi_jK_{ji}$ | $\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$ |

## 2d.15 性質與檢驗方法 [AR1 p.39]

| 性質 | AR(1) 的檢驗 | 需要特定初始分布？ |
|---|---|---|
| Time homogeneity | 每期同一 $K(x,\cdot)=N(ax,\sigma^2)$ | 否 |
| Invariant law | $\pi K=\pi$，$\pi=N(0,\sigma^2/(1-a^2))$ | 否 |
| Strict stationarity | $X_0\sim\pi$ 且與未來雜訊獨立 | **是** |
| Reversibility | $\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$ | **是**（定義在 $P_\pi$ 下） |
| Irreducibility / aperiodicity | 正 density 一步到任何正測度區域 | 否 |
| Positive Harris recurrence | 每個正測度集合被重訪，且有 invariant probability | 否 |
| Convergence / ergodic averages | $K^n(x,\cdot)\to\pi$ 與 $N^{-1}\sum f(X_n)\to\pi(f)$ 是不同極限 | 否（任一起點） |

（以上全部在 $\lvert a\rvert<1$、$\sigma>0$ 下成立。Discuss「Which rows also require a particular initial law?」的答案即最右欄。）

## 2d.16 Step 6 考試重點

**必背**：$K(x,\cdot)=N(ax,\sigma^2)$；$K^n(x,\cdot)=N(a^nx,\sigma^2\sum_{k<n}a^{2k})$；$\pi=N(0,\sigma^2/(1-a^2))$；$\mathrm{Corr}_\pi(X_0,X_h)=a^h$；$N\mathrm{Var}(\bar X_N)\to\sigma^2/(1-a)^2$；參數表。

**必須能推導**：n-step law（代入 + 獨立 Normal 相加）；invariant variance；detailed balance 展開平方；$\mathrm{Cov}(X_0,X_1)=av$；樣本平均恆等式。

**必會計算**：用 $\Phi$ 表示 $K(x,A)$、$K^2(x,A)$；Exercise 1、2 與 final exercise（見 [04](04_solved_examples.md)）。

**失分點**：標準化時用變異數不用標準差；誤以為 $\lvert a\rvert<1$ 是 kernel 條件；以為平均為 0 就 stationary；以為 stationary 就獨立；以為 reversible 要 $k$ 對稱；把 $a<0$ 說成 period 2。
