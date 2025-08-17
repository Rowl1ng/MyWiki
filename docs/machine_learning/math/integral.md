# 定積分

積分はイメージとして「関数の値を足しまくったもの」です。

$x$が$a$から$b$まで動く時、この区間を$N$等分して足したもの
$$ \sum_{i=0}^N f\left(a+(b-a)\frac{i}{N}\right)\frac{b-a}{N} $$
を考えると次のグラフの面積のようなものになります。正確にはx軸より上の棒グラフの面積は正、x軸より下にある棒グラフの面積は負とするような「符号付き面積」値が上の式の値です。

あまり変な関数でなければ$N \to \infty$とすると上の式の値は一定値に近づいていきます。この値を
$$ \int_a^b f(x)dx $$
と書きます。これを定積分と呼びます。
これはグラフでいうと斜線部の「符号付き面積」にあたります。

--- ここにグラフを書く---

# 微積分学の基本定理

上の定義からも分かるように積分は基本的に難しい演算なのですが、微積分学の基本定理を使うことでいくつかの関数の積分がそこそこ簡単にできるようになります。

定積分の区間を$a$から$x$までと考えると$x$の値を決めると定積分の値が一つに決まるので $\int_a^x f(t)dt$は$x$の関数であると考えられます。このとき、
$$\left(\int_a^x f(t)dt\right)' = f(x)$$
が成立します。つまり積分してから微分すると元の関数が現れるということです。

微分の定義に戻って考えてみます。
$$\left(\int_a^x f(t)dt\right)' = \lim_{h \to 0} \frac{\int_a^x f(t)dt - \int_a^{x+h} f(t)dt }{h}$$
この差をグラフ上で表すとこのようになります。

--- ここにグラフを書く---


$h$が十分小さいのでこの範囲では変化がないと考えると分子にあたる差の面積は$f(x) \cdot h$と表されます。これを代入して計算すれば微積分学の基本定理の公式になります。

# 不定積分

$f(x)$ に対して $F'(x) = f(x)$ となるような関数のことを $f(x)$ の原始関数といい、原始関数を求めることを不定積分と言います。

$F(x)=\int_0^xf(t)dt$とすると $\int_a^b f(t)dt = \int_0^b f(t)dt - \int_0^a f(t)dt = F(b)-F(a)$ が成り立ちます。従って定積分を求めるには原始関数を求めて範囲の境界同士での値の差を取ればよいことが分かります。

$F(x)=\int_0^xf(t)dt$ も $F(x)=\int_1^xf(t)dt$ も微分すれば $f(x)$ になるためどちらも$f(x)$ の原始関数となります。原始関数には定数分だけ無限の可能性がありますが、どれを使っても引き算する際に相殺されるので定積分を求める際には一番簡単な原始関数を使うのがよいでしょう。

例: $(\frac{1}{3}x^3)' = x^2$ より、$\frac{1}{3}x^3$ は$f(x)=x^2$ の不定積分の一つであることが分かります。
同様にして $\frac{1}{3}x^3+5$ も$f(x)$ の不定積分です。この定数ぶんの違いを含めて全ての不定積分を表すために積分定数$C$と呼ばれる記号を使って $\frac{1}{3} x^3+C$ のように書くことがあります。

# 積分の公式

## 和・差の公式

$$\int f(x)\pm g(x)dx = \int f(x)dx\pm \int g(x)dx$$

例:
$$ 
\begin{aligned}
\int 3x^2+2x-1 = \int 3x^2 + \int 2x - \int x \\
= x^3+x^2-x+C
\end{aligned}
$$

## 置換積分の公式

$x=g(t)$, $a=g(\alpha)$, $b=g(\beta)$ のとき、
$$\int_a^b f(x)dx = \int_{\alpha}^{\beta}f(g(t))g'(t)dt$$

例:
$x = \sqrt{t+1}$ とおくと、$x=t^2-1$となることを利用して、
$$
\begin{aligned}
\int_0^1\frac{x}{\sqrt{x+1}}dx = \int_1^{\sqrt{2}} \frac{t^2-1}{t}\cdot 2t dt \\
= \int_1^{\sqrt{2}} 2(t^2-1)dt
\end{aligned}
$$

## 部分積分の公式

$$\int f(x)g'(x)dx = f(x)g(x)-\int f'(x)g(x)dx$$

---そもそも使わない気がするから後で検討する---

# 多変数関数の積分

関数を積分して得た関数をさらに積分する場合があります。このときは
$$\int \int f(x, y) dx dy$$
のように書きます。


# Signal Representation
## Useful orthogonality relations

- cos mωt and cos nωt are sinusoids at a multiple of some fundamental frequency ω
- If we integrate the various products of sin and cos over the period of the fundamental waveform then:
    - cos mωt integrates to zero always 
    - sin mωt integrates to zero always
    - sin mωt . cos nωt  integrates to zero always
    - cos mωt . cos nωt  integrates to zero when m≠n and to T/2 when m=n
    - sin mωt . sin nωt  integrates to zero when m≠n and to T/2 when m=n


