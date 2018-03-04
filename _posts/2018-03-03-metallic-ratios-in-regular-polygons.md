---
layout: post
---

<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['$','$'],['\\(','\\)']],processEscapes:true}});</script>

In [Beyond on the Golden Ratio](https://youtu.be/MIxvZ6jwTuA) from [PBS Infinite Series](https://www.youtube.com/channel/UCs4aHmggTfFrpkPcWSaBN9g), Gabe introduces the metallic ratios, a sequence of ratios $(\sigma_n)$ generalizing the golden ratio $\sigma_1$. At the end of the video, Gabe notes two metallic ratios in regular polygons.

* In a regular pentagon, the ratio of the length of a diagonal to the length of a side is the golden ratio $\sigma_1$.
* In a regular octagon, the ratio of the length of a second diagonal to the length of a side is the silver ratio $\sigma_2$.

His follow up question is whether there are other examples of metallic ratios in regular polygons, especially higher order metallic ratios like the bronze ratio $\sigma_3$.

I did not find any higher order metallic ratios, and, apparently, there aren't any (see Gabe's comment responses at the end of the subsequent video [How to Divide by "Zero"](https://youtu.be/uxpowBoPieQ)). I did, however, find three more "metallic" ratios worth mentioning.

* The golden ratio $\sigma_1$ appears twice in a regular $30$-gon.
* The half-golden, half-silver ratio $\sigma_{1.5}$ appears in a regular hexagon.

## Get in

Let's take as the vertices of our regular $n$-gon the $n$th roots of unity $z_k$, for $k$ from $0$ to $n - 1$, given by
\\[ z_k = \left( \cos 2 \pi k / n, \sin 2 \pi k / n \right). \\]

I'll use the term $k$-diagonal to refer to a segment joining two vertices $k$ indices apart, i.e. $z_i$ and $z_j$ where $i - j$ is congruent to $k$ modulo $n$. Note that a $1$-diagonal what you'd typically call a side, a $2$-diagonal is what you might call a first diagonal, and a $3$-diagonal is what you might call a second diagonal.

With trig identities on hand, one can compute that the length of a $k$-diagonal is
\\[ \left| z_k - z_0 \right| = 2 \sin \pi k / n. \\]

The ratio of the length of a $k_2$-diagonal to the length of a $k_1$-diagonal is then an expression of the form
\\[ \frac{\sin \pi k_2 / n}{\sin \pi k_1 / n}. \\]

One can characterize the metallic ratios as the positive real numbers $x$ for which $x - 1 / x$ is a positive integer, in which case that positive integer is the index of the metallic ratio. So, for example, if $x > 0$ and $x - 1 / x = 7$, then $x = \sigma_7$.

So what we're asking, effectively, is for which values of $n$, $k_2$, and $k_1$ the quantity
\\[ \frac{\sin \pi k_2 / n}{\sin \pi k_1 / n} - \frac{\sin \pi k_1 / n}{\sin \pi k_2 / n} \\]
is an integer.

The experiment is then, for each triple $(n, k_2, k_1)$, to compute a candidate index by the above formula, and, if the candidate index is within a specified error threshold of an integer, print the triple followed by the candidate index.

Notice that $k$-diagonals have the same length as $(n - k)$-diagonals. As such we restrict our parameter space to tripes $(n, k_2, k_1)$ such that $1 \leq k_1 < k_2 \leq n / 2$.

<script src="https://gist.github.com/ezgrapes/3b4de6250a5c4a58f065035ef5c56e2e.js?file=list_candidates.py"></script>

The above code produces a *long* list of results of the form $(n, k_2, k_1, i)$ where $i$ is the candidate index of the metallic ratio. Here is the output, which I've truncated at $n = 50$ since the full output up to $n = 1000$ has $391$ entries.

```
(5, 2, 1, 1)
(8, 3, 1, 2)
(10, 4, 2, 1)
(15, 6, 3, 1)
(16, 6, 2, 2)
(20, 8, 4, 1)
(24, 9, 3, 2)
(25, 10, 5, 1)
(30, 5, 3, 1)
(30, 9, 5, 1)
(30, 12, 6, 1)
(32, 12, 4, 2)
(35, 14, 7, 1)
(40, 15, 5, 2)
(40, 16, 8, 1)
(45, 18, 9, 1)
(48, 18, 6, 2)
(50, 20, 10, 1)
```

Notice two things:

* The first two results are precisely the ones Gabe noted.
* There appear to be many more results than I had initially suggested when I said I found three more.

## Get primitive

Regarding that second point ... take a look at the third result in the list $(10, 4, 2, 1)$ and compare it to the first $(5, 2, 1, 1)$. Those results are effectively the same because one can inscribe a $5$-gon in a $10$-gon by skipping every other vertex, in which case a $4$-diagonal and a $2$-diagonal of the $10$-gon are literally a $2$-diagonal and a $1$-diagonal, respectively, of the $5$-gon. Results $(n, k_2, k_1, i)$ where $n$, $k_2$, and $k_2$ have a nontrivial common factor can be reduced to a primitive result by dividing out the greatest common factor.

Taking another look at the results, it becomes clear fairly quickly that *most* of the results can be reduced. Let's rerun the previous experiment but only output results for which $n$, $k_2$, and $k_1$ are relatively prime.

<script src="https://gist.github.com/ezgrapes/3b4de6250a5c4a58f065035ef5c56e2e.js?file=list_primitive_candidates.py"></script>

The full output is shockingly short by comparison.

```
(5, 2, 1, 1)
(8, 3, 1, 2)
(30, 5, 3, 1)
(30, 9, 5, 1)
```

In addition to the original two, we gain two more primitive results, both in the $30$-gon, and neither using a side length.

One can prove the two results on the $30$-gon painlessly using the identities $\sin \pi / 10 = (\sqrt{5} - 1) / 4$, $\sin \pi / 6 = 1 / 2$, and $\sin 3 \pi / 10 = (\sqrt{5} + 1) / 4$.

## Get rational

The characterization of the metallic ratios as positive reals $x$ for which $x - 1 / x$ is a positive integer got me thinking ... what if we loosen the restriction that the index be an integer to something more general like a rational? After all, the equation $x - 1 / x = b$ has a unique positive real solution $\sigma_{b}$ for all reals $b$.

Let's rerun the previous experiment but instead of only reporting candidate indices that are within an error threshold of an integer, let's report all candidate "indices" that are within an error threshold a rational with a fixed denominator. I've chosen a denominator, namely $2520$, that allows us to consider simultaneously all rationals with denominator up to and including $10$, i.e. it's the least common multiple of the first $10$ positive integers.

<script src="https://gist.github.com/ezgrapes/3b4de6250a5c4a58f065035ef5c56e2e.js?file=list_rational_candidates.py"></script>

We get one additional result.

```
(5, 2, 1, 1.0)
(6, 3, 1, 1.5)
(8, 3, 1, 2.0)
(30, 5, 3, 1.0)
(30, 9, 5, 1.0)
```

The new result is underwhelming when you parse it out: in a regular hexagon, a main diagonal has length twice that of a side. This result doesn't feel as connected to metallicity as the previous results do, a sense I think is due to the ubiquity of $\sigma_{1.5} = 2$ outside the context of metallic ratios. Nonetheless, a result worthy of note.
