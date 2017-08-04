
# MCMCregressの事前分布とパラメータ設定
R_MCMCpack.pdfの110ページの記載によると､RのMCMCregressの事前分布はセミ共役事前分布で､
$\boldsymbol{\beta} $は
$$
\boldsymbol{\beta} \sim \mathcal{N}(b_0,B_0^{-1}) 
$$
であり､$\sigma^2$は
$$
(\sigma^2)^{-1} \sim Gamma(c_0/2,d_0/2) 
$$
もしくは､
$$
\sigma^2 \sim IGamma(c_0/2,d_0/2) 
$$
である｡

* b0

    $\boldsymbol{\beta}$の期待値ベクター｡スカラーの値を定義する場合は､全部の$\beta$にそのスカラーの値を期待値と利用する｡

* B0

    $\boldsymbol{\beta}$の精度の行列を定義する｡
    B0は0.2と設定する場合に､K=3の
    $\boldsymbol{\beta}$の分散共分散行列は対角要素はB0の逆数になるもの｡
    $$
      \begin{bmatrix}
       5 &amp; 0 &amp; 0 \\
       0 &amp; 5 &amp; 0 \\
       0 &amp; 0 &amp; 5
      \end{bmatrix} 
    $$

* c0, d0

  $c_0/2,d_0/2$は$\sigma^2$の分布を決めるパラメータ｡
  
* sigma.mu,sigma.var

  sigma.mu,sigma.varは$\sigma^2$の期待値と分散から分布を決めるパラメータ｡
  例えば､sigma.mu=5とsigma.var=25を設定する場合は､逆ガンマ分布のパラメータと以下の関係で対応できる｡
  $$
   \frac{c_0/2}{d_0/2-1} = \text{sigma.mu}=5
  $$
  $$
   \frac{(c_0/2)^2}{(d_0/2-1)^2(d_0/2-2)} = \text{sigma.var}=25
  $$
連立方程式を解けば､ $c_0=20, d_0=6$だと分かる｡

* beta.start

    $\boldsymbol{\beta}$の初期値設定｡デフォルトでは最小2乗推定を用いる｡



# K=3での回帰モデル

本節では､K=3のモデルでギブスサンプリングによる回帰モデルを記述する｡
設定したモデルから乱数を用いて発生させたデータを観測データとする｡
説明変数は2つの属性があり､それぞれ$x_{1i}$と$x_{2i}$で記録する｡切片を加えて､K=3のモデルとなる｡

## 事前の設定
切片付きのモデルを下記の用に設定する｡
$$
y_i = \beta_0 + x_{1i} \beta_1 + x_{2i} \beta_2 + \epsilon_i, \quad \epsilon_i \sim \mathcal{N}(0,\sigma^2) \quad i = 1,...,n
$$
興味あるパラメータは係数$\boldsymbol{\beta} = (\beta_0,\beta_1,\beta_2) $と誤差項の分散$\sigma^2$である｡
観測データは$\mathbf{y} = (y_1,...,y_n)'$と$\mathbf{X}$である｡$\mathbf{X}$は下記のようである｡ただし､$x_{0i}=1$である｡
$$
  \begin{bmatrix}
   x_{01} &amp; x_{11} &amp; x_{21} \\
   x_{02} &amp; x_{12} &amp; x_{22} \\
   \vdots &amp; \vdots &amp; \vdots \\
   x_{0n} &amp; x_{1n} &amp; x_{2n}
  \end{bmatrix} 
$$

K=3での回帰モデル
=================

本節では､K=3のモデルでギブスサンプリングによる回帰モデルを記述する｡
設定したモデルから乱数を用いて発生させたデータを観測データとする｡
説明変数は2つの属性があり､それぞれ$x_{1i}$と$x_{2i}$で記録する｡切片を加えて､K=3のモデルとなる｡

事前の設定
----------

切片付きのモデルを下記の用に設定する｡
$$y_i = \beta_0 + x_{1i} \beta_1 + x_{2i} \beta_2 + \epsilon_i, \quad \epsilon_i \sim \mathcal{N}(0,\sigma^2) \quad i = 1,...,n$$
興味あるパラメータは係数$\boldsymbol{\beta} = (\beta_0,\beta_1,\beta_2) $と誤差項の分散$\sigma^2$である｡
観測データは$\mathbf{y} = (y_1,...,y_n)'$と$\mathbf{X}$である｡$\mathbf{X}$は下記のようである｡ただし､$x_{0i}=1$である｡
$$\begin{bmatrix}
   x_{01} &amp; x_{11} &amp; x_{21} \\
   x_{02} &amp; x_{12} &amp; x_{22} \\
   \vdots &amp; \vdots &amp; \vdots \\
   x_{0n} &amp; x_{1n} &amp; x_{2n}
  \end{bmatrix}$$

最小2乗推定値
-------------

本節では､以降の数式変形の準備として､係数の最小2乗推定値
$(\hat{\beta_0},\hat{\beta_1},\hat{\beta_2})$ を求める｡
モデルの予測値と観測値の差$e_i$である残差の2乗和は
$$\sum_{i=1}^{n} e_i^2 = \sum_{i=1}^{n}(y_i-(x_{0i}\hat{\beta_0} + x_{1i}\hat{\beta_1} +  x_{2i}\hat{\beta_2}))$$
である｡ 最小化するには､下記の連立方程式が得られる｡ $$\begin{aligned}
\frac{\partial\sum_{i=1}^ne_i^2}{\partial \hat{\beta_j}} &amp; = 
-2\sum_{i=1}^n x_{ji}(y_i-(x_{0i}\hat{\beta_0}+x_{1i}\hat{\beta_1}+x_{2i}\hat{\beta_2}))
\\
&amp; = -2 \sum_{i=1}^n x_{ji}e_i\\
&amp;=0 \qquad j=0,1,2\end{aligned}$$
