# 05 題型辨識與解題流程

## 複習優先度與判斷依據

教材沒有附歷屆考題，以下優先度依「在 MC 與 AR1 兩份投影片中反覆出現、有 Exercise / Final exercise / Exit question」推估，不代表實際出題頻率。

| 優先 | 題型 | 依據 |
|---|---|---|
| 高 | A 建矩陣、B 集合機率、C n-step、D 初始分布、F 求 $\pi$、G detailed balance、J AR(1) 機率計算、K AR(1) 平衡與相關 | 兩份投影片都有練習；Final exercise 直接考 |
| 高 | L 概念判斷（對錯題） | AR1 Exercise 2、Exit questions |
| 中 | E 條件期望、H irreducible/period、I 收斂與 recurrence、N MCMC 接受率、O chain likelihood | 主體投影片有，但練習較少 |
| 低 | M 證明題（uniqueness、$\lvert a\rvert\ge1$、樣本平均） | 主要在附錄或只出現一次 |

依賴順序：A → B/C/D/E → F → G；H 是 I 的前提；J → K。

---

## 題型 A：建立 Transition Matrix

**辨識**：文字描述一個實驗或系統，問「write down the transition matrix / kernel」。

1. 列出所有狀態，固定順序（寫在矩陣旁邊）。
2. 對**每一個**現在狀態 $i$：列出所有可能的隨機結果與機率（樹狀圖或表格）。
3. 把導致同一個新狀態的結果機率相加 → $K_{ij}$。
4. 填入第 $i$ 列（列 = 現在，欄 = 下一步）。
5. 檢查 $0\le K_{ij}\le1$。
6. 檢查**每列**和為 1。
7. 檢查狀態是否包含足夠資訊（Markov property 是否成立）。

**範例**：Bernoulli–Laplace [MC p.11–12, 17]。**失分點**：只算一部分結果；把 $K_{ij}$ 放到 $K_{ji}$；狀態定義不足以讓過程 Markov。

## 題型 B：集合事件的一步機率 $K(i,A)$

1. 確認現在狀態 $i$。2. 確認目標集合 $A$（「至少」、「極端」轉成集合）。3. 取第 $i$ 列。4. 把 $j\in A$ 的元素相加。

**失分點**：加成一欄；漏掉 $A$ 中不可達（機率 0）的狀態沒關係，但不能多加。

## 題型 C：n-Step Transition Probability

1. 起始狀態 $i$、目標 $j$（或集合 $A$）、步數 $n$。
2. 小 $n$（2、3）且只要一格：用 C–K 列路線 $(K^2)_{ij}=\sum_zK_{iz}K_{zj}$。
3. 要整列或多格：算 $K^n$（$K^2=KK$、$K^3=K^2K$）。
4. 讀第 $i$ 列第 $j$ 欄；集合則加總。
5. 檢查 $K^n$ 每列和為 1、元素在 $[0,1]$。

**失分點**：$(K^2)_{ij}=K_{ij}^2$；矩陣乘法列欄弄反。**變形**：問「第一次在第 $n$ 步到 $j$」時不能直接用 $K^n$（需要首達分析，教材未教）。

## 題型 D：給初始分布，求時間 $n$ 的分布

1. 把 $\nu$ 寫成 row vector。2. $\mathcal L(X_1)=\nu K$，逐欄計算 $\sum_i\nu_iK_{ij}$。3. 重複或用 $\nu K^n$。4. 檢查和為 1。

**快速判斷**：若 $\nu$ 是 invariant，答案就是 $\nu$（例 8 [MC p.62]）。**失分點**：$K\nu^\top$。

## 題型 E：條件期望 / 條件變異數

1. 寫 $f$。2. $(Kf)(i)=\sum_jf(j)K_{ij}$（連續：$\int f(y)k(x,y)dy$ 或直接用 $E[f(ax+\sigma Z)]$）。3. 變異數 = $K(f^2)-(Kf)^2$。

## 題型 F：求 Stationary / Invariant Distribution

1. 設 $\pi=(\pi_0,\dots,\pi_N)$ row vector。
2. **先看是否只有相鄰轉移**（birth–death 型或二狀態）：直接用 detailed balance $\pi_iK_{i,i+1}=\pi_{i+1}K_{i+1,i}$ 遞推，最快。
3. 否則寫 $\pi K=\pi$，**按欄**展開：$\pi_j=\sum_i\pi_iK_{ij}$。
4. 丟掉一條方程（冗餘），加入 $\sum\pi_i=1$。
5. 解聯立。
6. 檢查：非負、和為 1、代回被丟掉的方程。
7. 說明唯一性的依據（finite + irreducible）。

**連續 AR(1)**：猜 $N(0,v)$，解 $v=a^2v+\sigma^2$。

**失分點**：按列展開（那是 $K\mathbf 1=\mathbf 1$）；忘記正規化；reducible 時誤稱唯一。

## 題型 G：檢查 Detailed Balance / Reversibility

1. 先求或給定 $\pi$。2. 對每一對 $i<j$ 算 $\pi_iK_{ij}$ 與 $\pi_jK_{ji}$。3. 全部相等 ⇒ reversible（並推出 invariant）。4. 有一對不等 ⇒ 不 reversible（但仍可能 invariant）。

連續：寫 $\varphi_\pi(x)k(x,y)$，展開指數看是否對 $x,y$ 對稱。

**失分點**：檢查 $K_{ij}=K_{ji}$；忘了 $K_{ij}=0$ 的對也要兩邊為 0。

## 題型 H：Irreducibility 與 Periodicity

1. 畫狀態圖（只畫 $K_{ij}>0$ 的箭頭）。
2. Irreducible：每對狀態雙向有路。找到一對不能到達即可否定。
3. Period：選一個狀態，找回返迴路長度；有自環 ⇒ $d=1$；找兩個互質長度 ⇒ $d=1$；所有迴路長度都是 $d$ 的倍數 ⇒ period $d$。
4. Irreducible 時所有狀態 period 相同，只需算一個。

連續狀態：說明 $k(x,y)>0$ 處處 ⇒ λ-irreducible（一步）且 aperiodic。

**失分點**：只看 $K$ 不看多步；把 $a<0$ 的振盪當 period。

## 題型 I：長期行為（收斂、ergodic、recurrence）

1. 檢查 finite？irreducible？aperiodic？
2. 三者皆是 ⇒ 唯一 $\pi$、$K^n(i,\lbrace j\rbrace)\to\pi_j$；$\lim P(X_n=j)=\pi_j$ 與起點無關。
3. 只有 irreducible（periodic）⇒ $\pi$ 唯一、時間平均 → $\pi$，但 $K^n$ 不收斂。
4. 平均回返時間 $E_i\tau_i^+=1/\pi_i$（finite irreducible）。
5. AR(1)：$\lvert a\rvert<1$ ⇒ positive Harris、$K^n(x,\cdot)\Rightarrow\pi$、TV 幾何收斂；$\lvert a\rvert\ge1$ ⇒ 無 invariant probability。

**失分點**：把充分條件說成必要；混淆分布收斂與時間平均。

## 題型 J：AR(1) 機率計算

1. **先寫三件事**：現在狀態（或初始分布）、經過步數、目標集合 [AR1 p.13 的要求]。
2. 寫出條件分布：一步 $N(ax,\sigma^2)$；$n$ 步 $N(a^nx,\sigma^2\sum_{k<n}a^{2k})$；隨機初始 $X_0\sim N(\mu_0,s_0^2)$ 時 $X_1\sim N(a\mu_0,a^2s_0^2+\sigma^2)$。
3. 標準化：$\Phi\!\left(\frac{c-\text{mean}}{\text{sd}}\right)-\Phi\!\left(\frac{b-\text{mean}}{\text{sd}}\right)$。
4. 單點機率直接寫 0。
5. 用對稱化簡（$\Phi(-t)=1-\Phi(t)$）。

**失分點**：sd 寫成變異數；平均寫成 $x$；兩步變異數寫成 $2\sigma^2$。

## 題型 K：AR(1) 平衡、stationarity、相關

1. Invariant：$\pi=N(0,\sigma^2/(1-a^2))$（需 $\lvert a\rvert<1$）。
2. Stationary 初始化：$X_0\sim\pi$ 且與未來雜訊獨立。
3. $\mathrm{Corr}_\pi(X_0,X_h)=a^h$；$\mathrm{Cov}=va^h$。
4. Ergodic limit：$\frac1N\sum f(X_n)\to\int fd\pi$；$\frac1N\sum X_n^2\to v$。
5. $N\mathrm{Var}(\bar X_N)\to\sigma^2/(1-a)^2$。

## 題型 L：概念對錯題

SOP：(1) 找出敘述把哪兩個概念混在一起（查 [02_concept_review](02_concept_review.md) 比較表）；(2) 寫出正確定義的公式；(3) 給一個具體反例數字。

常見陷阱敘述：「依賴 $x$ 所以 inhomogeneous」「平均不變所以 stationary」「stationary 所以獨立」「reversible 要 $K$ 對稱」「有 $\pi$ 所以收斂」「$a<0$ 所以 periodic」「$\lvert a\rvert<1$ 是 kernel 條件」「$k>1$ 不合法」。

## 題型 M：證明題

| 要證 | 骨架 |
|---|---|
| C–K | 對 $X_r$ 全機率 + Markov + homogeneity |
| Detailed balance ⇒ invariant | $\sum_i\pi_iK_{ij}=\sum_i\pi_jK_{ji}=\pi_j$ |
| Stationary ⇔ $\nu K=\nu$ | 必要取 $r=0,h=1$；充分用路徑公式 |
| AR(1) invariant | 猜 Normal，比較變異數 |
| AR(1) reversible | 展開 $(1-a^2)x^2+(y-ax)^2=x^2+y^2-2axy$ |
| Uniqueness | $\int fd\mu=\int K^nf\,d\mu\to\pi(f)$（DCT） |
| $\lvert a\rvert\ge1$ 無平衡 | 特徵函數迭代 |
| MH 正確 | $\pi(x)q(y\mid x)\alpha(x,y)=\min(\cdot,\cdot)$ 對稱 |

書寫格式：先寫「To show …」、列出使用的假設、每步標註理由（by the Markov property / by independence / by normalization）。

## 題型 N：MCMC 接受率與演算法

1. 判斷 proposal 是否對稱（$q(y\mid x)=q(x\mid y)$？random walk $N(x,\sigma^2)$ 對稱；shifted $N(x+0.5,1)$、MALA 不對稱）。
2. 對稱 ⇒ $\alpha=\min(1,\pi(y)/\pi(x))$；不對稱 ⇒ 加 $q(x\mid y)/q(y\mid x)$。
3. 用未正規化的 $\pi$ 即可。
4. 拒絕時 $X_{n+1}=X_n$。
5. Gibbs：寫出每個條件分布（bivariate normal 公式），注意用最新值。
6. HMC：$\alpha=\min(1,e^{H-H^*})$。

## 題型 O：由觀測路徑估計轉移矩陣

1. 數轉移次數 $m_{ij}$。2. Likelihood $\prod_{i,j}K_{ij}^{m_{ij}}$。3. MLE：$\hat K_{ij}=m_{ij}/\sum_lm_{il}$（每列的比例）。4. Bayesian：每列 Beta/Dirichlet kernel；有限制時用 importance sampling [H8 p.46–48]。

---

## 題目如何改變條件讓方法失效

| 原方法 | 改變的條件 | 後果 |
|---|---|---|
| $K^n(i,\cdot)\to\pi$ | 鏈 periodic（$m=1$） | 不收斂；改問時間平均 |
| 唯一 $\pi$ | 鏈 reducible | 多個 invariant law |
| $\pi=N(0,\sigma^2/(1-a^2))$ | $\lvert a\rvert\ge1$ | 無 invariant probability |
| C–K 用同一 $K$ | 係數 $a_n$ 隨時間變 | 用 $K_0K_1\cdots K_{n-1}$ |
| Metropolis ratio | proposal 不對稱 | 必須加 Hastings correction |
| $\mathcal L(X_1)=\pi$ | 初始不是 $\pi$ | 要實際算 $\nu K$ |
| $\mathrm{Var}(\bar X_N)=v/N$ | 樣本相依（MCMC、AR(1)） | 用 $\sigma^2/((1-a)^2N)$ |
| 用 $K$ 判斷可達 | 需要多步 | 看 $K^n$ 或狀態圖 |
