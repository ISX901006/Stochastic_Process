# 03b 推導講義（二）：Gaussian AR(1)

模型：$X_{n+1}=aX_n+\sigma Z_{n+1}$，$Z_n$ i.i.d. $N(0,1)$ 獨立於 $X_0$，$\sigma>0$。記 $v=\sigma^2/(1-a^2)$（$\lvert a\rvert<1$ 時）。

---

## §1 n-step law $K^n(x,\cdot)=N\big(a^nx,\ \sigma^2\sum_{k=0}^{n-1}a^{2k}\big)$ [MC p.45, 47; AR1 p.12]

**1. 目標**：從 $X_0=x$ 出發，求 $X_n$ 的分布。

**2. 已知**：遞迴式；獨立 Normal 的線性組合仍為 Normal，平均與變異數可加。

**3. 策略**：反覆代入把 $X_n$ 寫成「$x$ 的確定項 + 獨立雜訊的線性組合」，再讀出平均與變異數。

**4. 逐步推導**

$$
X_1=ax+\sigma Z_1
$$

說明：$n=0$ 代入，$X_0=x$ 已知。

$$
X_2=aX_1+\sigma Z_2=a(ax+\sigma Z_1)+\sigma Z_2=a^2x+a\sigma Z_1+\sigma Z_2
$$

說明：把 $X_1$ 的式子代入下一步。

歸納假設 $X_n=a^nx+\sigma\sum_{j=1}^na^{n-j}Z_j$，則

$$
X_{n+1}=aX_n+\sigma Z_{n+1}=a^{n+1}x+\sigma\sum_{j=1}^na^{n+1-j}Z_j+\sigma Z_{n+1}=a^{n+1}x+\sigma\sum_{j=1}^{n+1}a^{n+1-j}Z_j .
$$

說明：$\sigma Z_{n+1}$ 正好是 $j=n+1$ 那一項（$a^0=1$）。

$$
E_x[X_n]=a^nx,\qquad\mathrm{Var}_x(X_n)=\sigma^2\sum_{j=1}^na^{2(n-j)}=\sigma^2\sum_{k=0}^{n-1}a^{2k}
$$

說明：$E Z_j=0$；$Z_j$ 獨立 ⇒ 變異數相加，係數平方；令 $k=n-j$ 重新編號。

$\lvert a\rvert<1$ 時等比級數：$\sum_{k=0}^{n-1}a^{2k}=\frac{1-a^{2n}}{1-a^2}$，所以 $K^n(x,\cdot)=N\big(a^nx,\ v(1-a^{2n})\big)$。

**5. 關鍵**：Normal 性來自「Normal 的線性組合仍 Normal」；**不需要** $\lvert a\rvert<1$。$a=1$ 時和為 $n$，$K^n(x,\cdot)=N(x,n\sigma^2)$ [MC p.58]。

**6. 考試版**：Iterating, $X_n=a^nx+\sigma\sum_{j=1}^na^{n-j}Z_j$, a linear combination of independent Gaussians; hence $X_n\sim N(a^nx,\sigma^2\sum_{k=0}^{n-1}a^{2k})$.

**數值**：$a=\frac12,\sigma=1,x=2$：$n=2$ 時 $N(\frac12,1+\frac14)=N(\frac12,\frac54)$ ✓ [MC p.45]。

---

## §2 Chapman–Kolmogorov 的 density 版一致性

**目標**：說明 $k_2(x,y)=\int k(x,z)k(z,y)\,dz$ 與 §1 的 $N(a^2x,\sigma^2(1+a^2))$ 一致。

**推導**：給定 $X_1=z$，$X_2\sim N(az,\sigma^2)$；$X_1\sim N(ax,\sigma^2)$。所以 $X_2=aX_1+\sigma Z_2$，$X_1$ 與 $Z_2$ 獨立：平均 $a\cdot ax=a^2x$，變異數 $a^2\sigma^2+\sigma^2$。積分 $\int k(x,z)k(z,y)\,dz$ 正是「對中間狀態 $z$ 加權平均條件 density」，結果就是此 Normal density。

**考試版**：$k_2(x,\cdot)$ is the density of $aX_1+\sigma Z_2$ with $X_1\sim N(ax,\sigma^2)\perp Z_2$, i.e. $N(a^2x,\sigma^2(1+a^2))$.

---

## §3 Invariant law $\pi=N(0,\sigma^2/(1-a^2))$ [MC p.50, AR1 p.17]

**1. 目標**：找出 $\pi$ 使 $X\sim\pi$、$X\perp Z$ ⇒ $aX+\sigma Z\sim\pi$。

**2. 已知**：$\pi K=\pi$ 的意義 [AR1 p.16]；Normal 線性組合。

**3. 策略**：猜一個 Normal 家族 $N(\mu,v)$，要求「輸入分布 = 輸出分布」，解參數。

**4. 逐步推導**

$$
X\sim N(\mu,v),\ X\perp Z\ \Rightarrow\ aX+\sigma Z\sim N(a\mu,\ a^2v+\sigma^2)
$$

說明：線性組合的平均與變異數。

$$
N(a\mu,a^2v+\sigma^2)=N(\mu,v)\iff a\mu=\mu\ \text{且}\ a^2v+\sigma^2=v
$$

說明：Normal 由平均與變異數唯一決定。

$a\ne1$ ⇒ $\mu=0$；$\lvert a\rvert<1$ ⇒ $v=\frac{\sigma^2}{1-a^2}>0$。

**5. 關鍵**：
- $\lvert a\rvert\ge1$ 時 $v(1-a^2)=\sigma^2>0$ 無正解（左邊 $\le0$）。但這只排除 Normal 候選；完整排除見 §10。
- 這只證明**存在**，唯一性見 §6 [AR1 p.17]。

**6. 考試版**：If $X\sim N(0,v)\perp Z$, then $aX+\sigma Z\sim N(0,a^2v+\sigma^2)$; invariance requires $v=a^2v+\sigma^2$, i.e. $v=\sigma^2/(1-a^2)$ for $\lvert a\rvert<1$.

---

## §4 Stationary 共變異與相關 $\mathrm{Corr}_\pi(X_0,X_h)=a^h$ [MC p.52, AR1 p.20]

**1. 目標**：$X_0\sim\pi$ 時 $\mathrm{Cov}(X_0,X_h)=a^hv$，$\mathrm{Corr}=a^h$。

**4. 逐步推導**

$$
X_h=a^hX_0+\sigma\sum_{j=1}^ha^{h-j}Z_j
$$

說明：§1 的展開式，起點換成隨機的 $X_0$。

$$
\mathrm{Cov}(X_0,X_h)=a^h\,\mathrm{Cov}(X_0,X_0)+\sigma\sum_{j=1}^ha^{h-j}\mathrm{Cov}(X_0,Z_j)=a^h\mathrm{Var}(X_0)+0=a^hv
$$

說明：共變異雙線性；未來雜訊與 $X_0$ 獨立 ⇒ 共變異 0；stationary ⇒ $\mathrm{Var}X_0=v$。

$$
\mathrm{Corr}(X_0,X_h)=\frac{a^hv}{\sqrt{v\cdot v}}=a^h
$$

說明：stationary ⇒ 每個邊際變異數都是 $v$。

**5. 關鍵**：同樣的計算給出 $\mathrm{Cov}(X_i,X_j)=va^{\lvert i-j\rvert}$（平移不變），是 §9 的基礎。

**6. 考試版**：$\mathrm{Cov}_\pi(X_0,X_h)=\mathrm{Cov}(X_0,a^hX_0+\sigma\sum_ja^{h-j}Z_j)=a^hv$; both variances equal $v$, so $\mathrm{Corr}=a^h$.

---

## §5 Reversibility：$\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$ [MC p.53, AR1 p.22]

**1. 目標**：證明 detailed balance（density form）。

**2. 已知**：$\varphi_\pi(x)=\frac{\sqrt{1-a^2}}{\sigma\sqrt{2\pi}}\exp\!\left[-\frac{(1-a^2)x^2}{2\sigma^2}\right]$；$k(x,y)=\frac1{\sigma\sqrt{2\pi}}\exp\!\left[-\frac{(y-ax)^2}{2\sigma^2}\right]$。

**3. 策略**：乘起來，證明結果是 $x,y$ 的對稱函數。

**4. 逐步推導**

$$
\varphi_\pi(x)k(x,y)=\frac{\sqrt{1-a^2}}{2\pi\sigma^2}\exp\!\left[-\frac{(1-a^2)x^2+(y-ax)^2}{2\sigma^2}\right]
$$

說明：常數相乘 $\frac{\sqrt{1-a^2}}{\sigma\sqrt{2\pi}}\cdot\frac1{\sigma\sqrt{2\pi}}$；指數相加。

$$
(1-a^2)x^2+(y-ax)^2=x^2-a^2x^2+y^2-2axy+a^2x^2=x^2+y^2-2axy
$$

說明：展開平方，$a^2x^2$ 抵消。

$x^2+y^2-2axy$ 交換 $x,y$ 不變，常數不含 $x,y$ ⇒ $\varphi_\pi(x)k(x,y)=\varphi_\pi(y)k(y,x)$。

**5. 關鍵**：沒有用到 $a>0$，所以 $a<0$ 也 reversible。整合後得 $\int_AK(x,B)\pi(dx)=\int_A\int_B\varphi_\pi(x)k(x,y)\,dy\,dx$，對稱 ⇒ 等於 $\int_BK(y,A)\pi(dy)$。

**6. 考試版**：$\varphi_\pi(x)k(x,y)\propto\exp[-(x^2+y^2-2axy)/(2\sigma^2)]$, symmetric in $(x,y)$ with a symmetric constant; hence detailed balance.

---

## §6 Invariant law 的唯一性 [AR1 p.28]

**1. 目標**：若 $\mu$ 是任一 invariant probability，則 $\mu=\pi$。

**2. 已知**：(a) $\mu K^n=\mu$；(b) 對有界連續 $f$，$K^nf(x)\to\int f\,d\pi$ 每個 $x$（由 §1 + weak convergence，見 §7）；(c) $\lvert K^nf\rvert\le\lVert f\rVert_\infty$（[03a §10](03a_derivations_finite_and_general.md)）；(d) dominated convergence；(e) 有界連續函數的期望決定 $\mathbb R$ 上的機率分布。

**3. 策略**：把 $\int f\,d\mu$ 寫成「$\mu$ 跑 $n$ 步後的期望」，令 $n\to\infty$。

**4. 逐步推導**

$$
\int f\,d\mu=\int f\,d(\mu K^n)=\int K^nf(x)\,\mu(dx)
$$

說明：第一個等號用 invariance $\mu K^n=\mu$；第二個是 Fubini：$\int f\,d(\mu K^n)=\int\int f(y)K^n(x,dy)\mu(dx)$。

$$
\lim_{n\to\infty}\int K^nf(x)\,\mu(dx)=\int\lim_{n\to\infty}K^nf(x)\,\mu(dx)=\int\left(\int f\,d\pi\right)\mu(dx)=\int f\,d\pi
$$

說明：dominated convergence（控制函數為常數 $\lVert f\rVert_\infty$，對機率測度可積）；逐點極限 (b)；常數對機率測度積分等於自己。

左邊不依賴 $n$，所以 $\int f\,d\mu=\int f\,d\pi$ 對所有有界連續 $f$ ⇒ $\mu=\pi$。

**5. 關鍵**：沒有假設 $\mu$ 是 Gaussian。「收斂到 $\pi$ 不論起點」⇒「不變分布唯一」是通用技巧。

**6. 考試版**：For bounded continuous $f$, $\int fd\mu=\int K^nf\,d\mu\to\int fd\pi$ by dominated convergence since $K^nf(x)\to\pi(f)$ and $\lvert K^nf\rvert\le\lVert f\rVert_\infty$. Thus $\mu=\pi$.

---

## §7 Weak convergence 與 coupling [MC p.56, AR1 p.27, 29]

**Weak convergence**：$\lvert a\rvert<1$。

$$
K^n(x,(-\infty,c])=\Phi\!\left(\frac{c-a^nx}{\sqrt{v(1-a^{2n})}}\right)\xrightarrow{n\to\infty}\Phi\!\left(\frac{c}{\sqrt v}\right)
$$

說明：§1 標準化；$a^nx\to0$、$1-a^{2n}\to1$；$\Phi$ 連續。CDF 在每點收斂（極限 CDF 連續）等價於 weak convergence，所以 $K^n(x,\cdot)\Rightarrow\pi$。

**Coupling**：用同一組 $Z$ 驅動兩條鏈：

$$
X_{n+1}-Y_{n+1}=(aX_n+\sigma Z_{n+1})-(aY_n+\sigma Z_{n+1})=a(X_n-Y_n)\ \Rightarrow\ X_n-Y_n=a^n(X_0-Y_0)\to0 .
$$

說明：雜訊相同所以相減消去；遞迴 $n$ 次。若 $Y_0\sim\pi$，則 $Y_n\sim\pi$ 對所有 $n$，而 $X_n$ 與 $Y_n$ 的差趨近 0 ⇒ $X_n$ 的分布趨近 $\pi$。

---

## §8 樣本平均 $\bar X_N\to0$ a.s.（初等證明）[AR1 p.33]

**1. 目標**：$\lvert a\rvert<1$ 時 $\bar X_N=\frac1N\sum_{n=0}^{N-1}X_n\to0$ a.s.

**4. 逐步推導**

$$
\sum_{n=0}^{N-1}(X_{n+1}-aX_n)=\sigma\sum_{n=0}^{N-1}Z_{n+1}=\sigma\sum_{j=1}^NZ_j
$$

說明：對遞迴式兩邊加總。

$$
\sum_{n=0}^{N-1}X_{n+1}-a\sum_{n=0}^{N-1}X_n=\Big(\sum_{n=0}^{N-1}X_n-X_0+X_N\Big)-a\sum_{n=0}^{N-1}X_n=(1-a)\sum_{n=0}^{N-1}X_n+X_N-X_0
$$

說明：$\sum_{n=0}^{N-1}X_{n+1}=X_1+\cdots+X_N=\sum_{n=0}^{N-1}X_n-X_0+X_N$（index 平移）。

$$
\bar X_N=\frac{\sigma}{1-a}\cdot\frac1N\sum_{j=1}^NZ_j-\frac{X_N-X_0}{(1-a)N}
$$

說明：兩式相等後除以 $(1-a)N$（$a\ne1$）。

第一項 → 0 a.s.（i.i.d. SLLN，$EZ=0$）；第二項 → 0 a.s.（投影片陳述 stable Gaussian chain 的邊界項 $X_N/N\to0$ a.s.）。

**6. 考試版**：Summing $X_{n+1}-aX_n=\sigma Z_{n+1}$ gives $(1-a)\sum_{n<N}X_n+X_N-X_0=\sigma\sum_{j\le N}Z_j$; divide by $(1-a)N$ and apply the SLLN.

---

## §9 樣本平均的變異數 [AR1 p.34]

**1. 目標**：stationary 下 $\mathrm{Var}_\pi(\bar X_N)=\frac vN\left[1+2\sum_{h=1}^{N-1}(1-\frac hN)a^h\right]$，且 $N\mathrm{Var}_\pi(\bar X_N)\to\frac{\sigma^2}{(1-a)^2}$。

**2. 已知**：§4：$\mathrm{Cov}(X_i,X_j)=va^{\lvert i-j\rvert}$。

**4. 逐步推導**

$$
\mathrm{Var}\Big(\sum_{n=0}^{N-1}X_n\Big)=\sum_{i=0}^{N-1}\sum_{j=0}^{N-1}\mathrm{Cov}(X_i,X_j)=\sum_{i,j}va^{\lvert i-j\rvert}
$$

說明：和的變異數 = 所有共變異的總和。

$$
=v\Big[N+2\sum_{h=1}^{N-1}(N-h)a^h\Big]
$$

說明：$N\times N$ 表格中，對角線 $i=j$ 有 $N$ 格（$a^0=1$）；距離 $\lvert i-j\rvert=h$ 的格子在對角線上下各 $N-h$ 格。

$$
\mathrm{Var}(\bar X_N)=\frac1{N^2}\cdot v\Big[N+2\sum_{h=1}^{N-1}(N-h)a^h\Big]=\frac vN\Big[1+2\sum_{h=1}^{N-1}\Big(1-\frac hN\Big)a^h\Big]
$$

說明：$\bar X_N$ 有 $1/N$，變異數乘 $1/N^2$。

極限：$\sum_{h\ge1}\lvert a\rvert^h<\infty$，且 $(1-h/N)\mathbf 1_{h<N}\to1$，dominated convergence ⇒ $\sum_{h=1}^{N-1}(1-\frac hN)a^h\to\frac{a}{1-a}$。

$$
N\mathrm{Var}(\bar X_N)\to v\Big(1+\frac{2a}{1-a}\Big)=v\frac{1+a}{1-a}=\frac{\sigma^2}{(1-a)(1+a)}\cdot\frac{1+a}{1-a}=\frac{\sigma^2}{(1-a)^2}.
$$

**5. 關鍵**：i.i.d. 時只有對角線，得 $v/N$。相依多出的 $2\sum(1-h/N)a^h$ 是「autocorrelation 懲罰」。$a=\frac12$：$\frac43\cdot3=4$；$a=-\frac12$：$\frac43\cdot\frac13=\frac49$。

**6. 考試版**：$\mathrm{Var}(\sum_{n<N}X_n)=\sum_{i,j}va^{\lvert i-j\rvert}=v[N+2\sum_{h=1}^{N-1}(N-h)a^h]$; divide by $N^2$. As $N\to\infty$, $N\mathrm{Var}(\bar X_N)\to v(1+2a/(1-a))=\sigma^2/(1-a)^2$.

---

## §10 $\lvert a\rvert\ge1$ 時沒有 invariant probability [AR1 p.47]

**1. 目標**：任何機率分布（不限 Normal、不限有限變異數）都不是 invariant。

**2. 已知**：特徵函數 $\chi(t)=E[e^{itX}]$：$\lvert\chi\rvert\le1$、$\chi(0)=1$、$\chi$ 連續；獨立和的特徵函數相乘；$E[e^{it\sigma Z}]=e^{-\sigma^2t^2/2}$。

**3. 策略**：反證。假設存在，迭代 invariance 方程，推出 $\chi\equiv0$（$t\ne0$），與連續性矛盾。

**4. 逐步推導**

$$
X\overset d=aX+\sigma Z\ \Rightarrow\ \chi(t)=E[e^{it(aX+\sigma Z)}]=E[e^{i(at)X}]\,E[e^{it\sigma Z}]=\chi(at)e^{-\sigma^2t^2/2}
$$

說明：invariance；$X\perp Z$ 使期望分解。

$$
\chi(t)=\chi(a^nt)\exp\!\Big(-\frac{\sigma^2t^2}2\sum_{j=0}^{n-1}a^{2j}\Big)
$$

說明：把 $t$ 換成 $at$ 代回，重複 $n$ 次：$\chi(at)=\chi(a^2t)e^{-\sigma^2a^2t^2/2}$，指數相加。

$\lvert a\rvert\ge1$ ⇒ $\sum_{j=0}^{n-1}a^{2j}\ge n\to\infty$；對 $t\ne0$，指數項 → 0；$\lvert\chi(a^nt)\rvert\le1$ ⇒ $\lvert\chi(t)\rvert\le e^{-\sigma^2t^2n/2}\to0$，故 $\chi(t)=0$。

但 $\chi$ 在 0 連續且 $\chi(0)=1$，$t\to0$ 時 $\chi(t)\to1$，矛盾。

**5. 關鍵**：這說明 kernel 合法（任何 $a$）與存在平衡（$\lvert a\rvert<1$）是不同問題 [AR1 p.36]。

**6. 考試版**：Invariance gives $\chi(t)=\chi(at)e^{-\sigma^2t^2/2}$, hence $\lvert\chi(t)\rvert\le\exp(-\frac{\sigma^2t^2}{2}\sum_{j<n}a^{2j})\to0$ for $t\ne0$ when $\lvert a\rvert\ge1$, contradicting continuity of $\chi$ at 0.
