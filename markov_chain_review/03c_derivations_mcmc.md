# 03c 推導講義（三）：MCMC 與 Transition Matrix 估計

教材 [H10] 對多數 MCMC 演算法只陳述「ensures detailed balance」而沒有證明。本檔 §1、§2、§4、§5 為**補充推導**（標準結果，用課堂已教的 detailed balance 定理 [MC p.30–31] 完成），§3、§6 對應教材原有推導。

---

## §1 Metropolis–Hastings 滿足 detailed balance（補充推導）

**1. 目標**：MH 的 transition kernel 對 $\pi$ 滿足 detailed balance，因此 $\pi K=\pi$。Metropolis 是 $q$ 對稱的特例。

**2. 已知**：Detailed balance 的積分形式與「detailed balance ⇒ invariance」[MC p.30–31]；$\alpha(x,y)=\min\!\left(1,\frac{\pi(y)q(x\mid y)}{\pi(x)q(y\mid x)}\right)$ [H10 p.12]。

**3. 策略**：先寫出真正的 kernel（包含拒絕），分成「移動部分」與「留在原地部分」分別驗證。

**4. 逐步推導**

真正的 transition kernel：

$$
K(x,dy)=q(y\mid x)\alpha(x,y)\,dy+r(x)\,\delta_x(dy),\qquad r(x)=1-\int q(y\mid x)\alpha(x,y)\,dy .
$$

說明：從 $x$ 有兩種結果——提議 $y$ 並接受（密度 $q\alpha$），或拒絕而留在 $x$（總機率 $r(x)$，是在 $x$ 的點質量）。

**移動部分**：

$$
\pi(x)q(y\mid x)\alpha(x,y)=\pi(x)q(y\mid x)\min\!\left(1,\frac{\pi(y)q(x\mid y)}{\pi(x)q(y\mid x)}\right)=\min\big(\pi(x)q(y\mid x),\ \pi(y)q(x\mid y)\big)
$$

說明：把 $\pi(x)q(y\mid x)$ 乘進 min 的兩個參數（正數乘進 min 不改變大小關係）。

右邊交換 $x,y$ 不變，所以 $\pi(x)q(y\mid x)\alpha(x,y)=\pi(y)q(x\mid y)\alpha(y,x)$。

**留在原地部分**：$\int_Ar(x)\delta_x(B)\pi(x)\,dx=\int_{A\cap B}r(x)\pi(x)\,dx$，交換 $A,B$ 不變。

**合併**：

$$
\int_AK(x,B)\pi(x)\,dx=\int_A\int_B\pi(x)q(y\mid x)\alpha(x,y)\,dy\,dx+\int_{A\cap B}r\pi=\int_BK(y,A)\pi(y)\,dy,
$$

即 detailed balance [MC p.30]，因此 $\pi K=\pi$。

**5. 關鍵**：
- $\pi$ 只以比值 $\pi(y)/\pi(x)$ 出現 ⇒ 正規化常數不需要。
- 拒絕時「留在原地」是 kernel 的必要部分；若拒絕時改成重新提議而不記錄，就不再是這個 kernel。
- Detailed balance 只保證 invariance；收斂還需要 irreducible + aperiodic [H10 p.15]。
- Metropolis：$q(x\mid y)=q(y\mid x)$ 消掉，$\alpha=\min(1,\pi(y)/\pi(x))$ [H10 p.6]。

**6. 考試版**：For $y\ne x$, $\pi(x)q(y\mid x)\alpha(x,y)=\min\lbrace \pi(x)q(y\mid x),\pi(y)q(x\mid y)\rbrace $, which is symmetric in $(x,y)$; the rejection part is a point mass at $x$ and trivially symmetric. Hence $\pi(dx)K(x,dy)=\pi(dy)K(y,dx)$ and $\pi$ is invariant.

---

## §2 Gibbs sampling 保留 $\pi$、接受率為 1（補充推導）

**1. 目標**：(a) 更新一個座標保留 $\pi$；(b) 把它視為 MH，$\alpha\equiv1$ [H10 p.16]。

**4. 推導**

(a) 設 $X=(X_i,X_{-i})\sim\pi$。新值 $X_i'\sim\pi(\cdot\mid X_{-i})$。則

$$
P(X_i'\in dx_i,X_{-i}\in dx_{-i})=\pi_{-i}(dx_{-i})\,\pi(dx_i\mid x_{-i})=\pi(dx_i,dx_{-i}),
$$

說明：$X_{-i}$ 的邊際仍是 $\pi_{-i}$（沒被改動）；條件 × 邊際 = 聯合。所以一次座標更新後分布仍為 $\pi$；依序更新所有座標是這些 kernel 的合成，仍保留 $\pi$。

(b) Proposal $q(y\mid x)=\pi(y_i\mid x_{-i})$，$y_{-i}=x_{-i}$：

$$
\frac{\pi(y)q(x\mid y)}{\pi(x)q(y\mid x)}=\frac{\pi(y_i\mid x_{-i})\pi_{-i}(x_{-i})\cdot\pi(x_i\mid x_{-i})}{\pi(x_i\mid x_{-i})\pi_{-i}(x_{-i})\cdot\pi(y_i\mid x_{-i})}=1 .
$$

說明：$\pi(y)=\pi(y_i\mid y_{-i})\pi_{-i}(y_{-i})$ 且 $y_{-i}=x_{-i}$；分子分母完全相同。

**6. 考試版**：A Gibbs update draws $X_i'\sim\pi(\cdot\mid X_{-i})$; if $X\sim\pi$ then $(X_i',X_{-i})\sim\pi_{-i}\otimes\pi(\cdot\mid\cdot)=\pi$. As an MH step with $q=\pi(\cdot\mid x_{-i})$, the ratio equals 1.

---

## §3 Bivariate normal 的條件分布 [H10 p.24–28]

**1. 目標**：

$$
X\mid Y=y\sim N\!\left(\mu_X+\frac{\sigma_{XY}}{\sigma_Y^2}(y-\mu_Y),\ \sigma_X^2-\frac{\sigma_{XY}^2}{\sigma_Y^2}\right).
$$

**2. 已知**：聯合 density $\propto\exp[-\frac12 w^\top\Sigma^{-1}w]$，$w=(x-\mu_X,y-\mu_Y)^\top$；$\Sigma^{-1}=\frac1{\lvert\Sigma\rvert}\begin{pmatrix}\sigma_Y^2&-\sigma_{XY}\\-\sigma_{XY}&\sigma_X^2\end{pmatrix}$，$\lvert\Sigma\rvert=\sigma_X^2\sigma_Y^2-\sigma_{XY}^2$ [H10 p.24]。

**3. 策略**：$f_{X\mid Y}(x\mid y)=f_{X,Y}(x,y)/f_Y(y)\propto f_{X,Y}(x,y)$（$y$ 固定），把指數整理成 $x$ 的完全平方 [H10 p.25–26]。

**4. 逐步推導**（令 $u=x-\mu_X$、$s=y-\mu_Y$）

$$
w^\top\Sigma^{-1}w=\frac{\sigma_Y^2u^2-2\sigma_{XY}us+\sigma_X^2s^2}{\lvert\Sigma\rvert}
$$

說明：二次型展開。

$$
=\frac{\sigma_Y^2}{\lvert\Sigma\rvert}\left(u^2-2\frac{\sigma_{XY}}{\sigma_Y^2}us\right)+\frac{\sigma_X^2s^2}{\lvert\Sigma\rvert}=\frac{\sigma_Y^2}{\lvert\Sigma\rvert}\left(u-\frac{\sigma_{XY}}{\sigma_Y^2}s\right)^2+C(s)
$$

說明：提出 $u^2$ 係數後配方；$C(s)$ 不含 $x$，被正規化吸收。

所以 $X\mid Y=y$ 是 Normal，平均 $\mu_X+\frac{\sigma_{XY}}{\sigma_Y^2}(y-\mu_Y)$，變異數

$$
\frac{\lvert\Sigma\rvert}{\sigma_Y^2}=\frac{\sigma_X^2\sigma_Y^2-\sigma_{XY}^2}{\sigma_Y^2}=\sigma_X^2-\frac{\sigma_{XY}^2}{\sigma_Y^2}=\sigma_X^2(1-\rho^2),\quad\rho=\frac{\sigma_{XY}}{\sigma_X\sigma_Y}.
$$

**例** [H10 p.28]：$\mu=0$、$\sigma_X^2=\sigma_Y^2=1$、$\sigma_{XY}=0.8$ ⇒ $X\mid Y=y\sim N(0.8y,0.36)$。

**6. 考試版**：Completing the square in $x$ in $w^\top\Sigma^{-1}w$ gives a Gaussian kernel with mean $\mu_X+\sigma_{XY}(y-\mu_Y)/\sigma_Y^2$ and variance $\lvert\Sigma\rvert/\sigma_Y^2=\sigma_X^2-\sigma_{XY}^2/\sigma_Y^2$.

---

## §4 MALA 在 standard normal 的接受率（補充推導）

**目標**：$\pi(x)\propto e^{-x^2/2}$，$q(y\mid x)=N(y;cx,\epsilon^2)$，$c=1-\epsilon^2/2$ [H10 p.32]。求 $\alpha$ 的簡化式。

**推導**

$$
\log\frac{\pi(y)q(x\mid y)}{\pi(x)q(y\mid x)}=-\frac{y^2-x^2}2-\frac{(x-cy)^2-(y-cx)^2}{2\epsilon^2}
$$

說明：Normal density 的常數相同而抵消；取 log 後只剩指數。

$$
(x-cy)^2-(y-cx)^2=x^2-2cxy+c^2y^2-y^2+2cxy-c^2x^2=(1-c^2)(x^2-y^2)
$$

$$
1-c^2=\epsilon^2-\frac{\epsilon^4}4\ \Rightarrow\ \log\text{ratio}=\frac{x^2-y^2}2\left(1-\frac{1-c^2}{\epsilon^2}\right)=\frac{x^2-y^2}2\cdot\frac{\epsilon^2}4=\frac{\epsilon^2(x^2-y^2)}8 .
$$

$$
\alpha(x,y)=\min\!\left(1,\exp\!\left[\frac{\epsilon^2(x^2-y^2)}8\right]\right).
$$

**意義**：$\epsilon\to0$ 時 $\alpha\to1$；梯度把 proposal 推向高密度區，所以接受率遠高於同步長的 random walk。（已用數值驗證：$\epsilon=0.5,x=1.3,y=-0.4$，兩邊皆為 $0.0478$。）

---

## §5 HMC 的接受率為什麼是 $\min(1,e^{H-H^*})$（補充推導）

**目標**：說明 [H10 p.47] 的 $\alpha=\min\big(1,\exp[-H(x^*,p^*)+H(x,p)]\big)$。

**推導思路**：
1. 在擴充空間 $(x,p)$ 定義聯合分布 $\pi(x,p)\propto e^{-H(x,p)}=e^{-U(x)}e^{-\frac12p^\top M^{-1}p}$，其 $x$-邊際就是目標 $\pi(x)\propto e^{-U(x)}$，$p$-邊際是 $N(0,M)$。
2. 步驟「抽 $p\sim N(0,M)$」是對 $p$ 的 Gibbs 更新（§2），保留 $\pi(x,p)$。
3. Leapfrog（加上最後把動量反號）是**可逆**（time-reversible）且**保體積**（Jacobian 行列式為 1）的確定性映射，所以作為 proposal 是「對稱」的，Hastings 比值中不出現 proposal 密度。
4. Metropolis 比值 $=\pi(x^*,p^*)/\pi(x,p)=e^{-H(x^*,p^*)+H(x,p)}$。
5. 連續時間 Hamilton 方程保守 $H$，leapfrog 近似保守 ⇒ $H^*\approx H$ ⇒ $\alpha$ 接近 1（高接受率的來源）[H10 p.45, 52]。

**考試版**：HMC is a Metropolis step on the joint target $\propto e^{-H(x,p)}$ with a volume-preserving, reversible (leapfrog + momentum flip) proposal, so $\alpha=\min\lbrace 1,e^{-H(x^*,p^*)+H(x,p)}\rbrace $; resampling $p$ is an exact Gibbs step.

---

## §6 Example 3.14：二狀態鏈的 likelihood、MLE 與 posterior [H8 p.40–46]

**1. 目標**：$L(p_1,p_2)\propto p_1^{m_{11}}(1-p_1)^{m_{12}}(1-p_2)^{m_{21}}p_2^{m_{22}}$。

**2. 已知**：Path probability（[03a §1](03a_derivations_finite_and_general.md)）。

**4. 逐步推導**

$$
P(X_1=x_1,\dots,X_m=x_m)=P(X_1=x_1)\prod_{t=1}^{m-1}T_{x_tx_{t+1}}
$$

說明：Markov chain 的路徑機率。把 $P(X_1=x_1)$ 視為與 $(p_1,p_2)$ 無關的常數（或對 $X_1$ 條件化）。

$$
\prod_{t=1}^{m-1}T_{x_tx_{t+1}}=\prod_{i,j}T_{ij}^{m_{ij}}=p_1^{m_{11}}(1-p_1)^{m_{12}}(1-p_2)^{m_{21}}p_2^{m_{22}}
$$

說明：把 $m-1$ 個因子依「從 $i$ 到 $j$」分組，每組出現 $m_{ij}$ 次；代入 $T_{11}=p_1$、$T_{12}=1-p_1$、$T_{21}=1-p_2$、$T_{22}=p_2$。所以 $D$ 是 sufficient statistic。

**MLE**（不考慮限制 $p_1+p_2<1$）：

$$
\frac{\partial}{\partial p_1}\log L=\frac{m_{11}}{p_1}-\frac{m_{12}}{1-p_1}=0\ \Rightarrow\ \hat p_1=\frac{m_{11}}{m_{11}+m_{12}},\qquad\hat p_2=\frac{m_{22}}{m_{21}+m_{22}} .
$$

說明：$\log L$ 對 $p_1,p_2$ 可分離。這就是 [H8 p.47] 的 $\hat p_i=m_{ii}/(m_{ii}+m_{i(3-i)})$。直覺：「從 1 出發的轉移中，留在 1 的比例」。

數值 $(68,28,17,4)$：$\hat p_1=68/96\approx0.708$、$\hat p_2=4/21\approx0.190$，和 $0.899<1$。

**Posterior**：均勻 prior $2\,\mathbf 1\lbrace p_1+p_2<1\rbrace$ × likelihood。$p_1^{m_{11}}(1-p_1)^{m_{12}}$ 是 $\mathrm{Be}(m_{11}+1,m_{12}+1)$ 的 kernel；$p_2^{m_{22}}(1-p_2)^{m_{21}}$ 是 $\mathrm{Be}(m_{22}+1,m_{21}+1)$ 的 kernel [H8 p.46]。

**6. 考試版**：By the Markov property, $L=\prod_tT_{x_tx_{t+1}}=\prod_{i,j}T_{ij}^{m_{ij}}$; maximizing the separable log-likelihood gives $\hat p_1=m_{11}/(m_{11}+m_{12})$, $\hat p_2=m_{22}/(m_{21}+m_{22})$.
