

<!--- 
The macros below work fine for both Madoko and Pandoc. If you are using some other way to compile 
markdown to PDF you may need to use a different syntax or simply do "search and replace" to
avoid using these macros.
--->

~ MathDefs
\newcommand{\zo}{\{0,1\}}
\newcommand{\getsr}{\leftarrow_{\text{\tiny R}}}
\newcommand{\E}{\mathbb{E}}
~

_CS 127: Cryptography / Boaz Barak_


# Homework 1

Total of 141 points. (Note that while this exercise is long, 100 points are a perfect score, so you 
don't _have_ to solve all questions if you don't have the time for it.)

0. (10 points + 5 points bonus) Log in to canvas and:
__(a)__ Signed up, left description, and included a photo of myself. 

__(b)__ Answered the questionnaire on Canvas!



## Exercises from the "mathematical background" handout.

1. (16 points) In the following exercise $X,Y$ denote random variables over some sample space $S$.
You can assume that the probability on $S$ is the uniform distribution--- every point
$s$ is output with probability $1/|S|$. 
Thus $\E[X]= (1/|S|)\sum_{s\in S}X(s)$.
We define the variance and standard deviation of $X$ and $Y$ as above (e.g.,
$Var[X] = \E [ (X-\E[X])^2 ]$ and the standard deviation is the square root of the variance).

    a. (2 points) Prove that $Var[X]$ is always non-negative.
~ Proof
We follow the definition of variance. Take $\mu = E[X]$, which is constant. We therefore have: 
~
~ Math
\begin{aligned} 
  Var[X] &= E[(X - \mu])^2] \\
  &= \left(\frac{1}{|S|}\right) \sum_{s \in S} (X(s) - \mu)^2 \\
  &\geq 0 
\end{aligned}
~
Note that the last line follows from the fact that $(X(s) - \mu)^2 \geq 0$ for all $s \in S$, 
and in fact, $(X(s) - \mu)^2 = 0 \iff X(s) = \mu$. Therefore the sum of finite number of
non-negative terms is non-negative.
[&box;]{float=right}

    b. (2 points) Prove that $Var[X] = \E[X^2] - \E[X]^2$.
~ Proof
We again simply follow the definition of variance. Letting $\mu = E[X]$ constant. 
~
~ Math
\begin{aligned}
  Var[X] &= E[(X - \mu)^2] \\
    &= E[X^2 - 2X\mu + \mu^2]  \\
    &= E[X^2] - 2\mu E[X] + \mu^2 \\
    &= E[X^2] - \mu^2 \\
    &= E[X^2] - E[X]^2
\end{aligned}
~
The first step is just definitional, while the second expand the inner term. Then we use linearity of
expectation in the next step. Next, we take note that $E[X] = \mu$ and subtitute and simplify. Then 
for the final step, we simply reverse substitute $\mu = E[X]$. 
[&box;]{float=right}

    c. (2 points) Prove that always $\E[X^2] \geq \E[X]^2$.
~Proof
By the proofs of (a) and (b), this follows immediately. By (b), we have that $Var[X] = E[X^2] - E[X]^2$ 
and by (a) we have that $Var[X] \geq 0$. Therefore, 
$Var[X] \geq 0 \implies E[X^2] - E[X]^2 \geq 0 \implies E[X^2] \geq E[X]^2$ always.
~
[&box;]{float=right}
    d. (2 points) Give an example for a random variable $X$ such that $\E[X^2] \neq \E[X]^2$.
~Proof
Let $X$ be the r.v. with uniform support such that $X(S) = \{1,2\}$. Then we have:
~
~ Math
\begin{aligned}
  E[X^2] &= \frac{1}{2}[1^2 + 2^2] = \frac{5}{2}\\
  E[X]^2 &= (\frac{1}{2}[1 + 2])^2 = \frac{9}{4}
\end{aligned}
~
Note that in general, $E[X^2] = E[X]^2$ if and only if the variance is $0$.
[&box;]{float=right}
    e. (2 points) Give an example for a random variable $X$ such that its standard deviation is _not 
    equal_ to $\E[ | X - \E[X] | ]$.
~Proof
Take $X$ to be a r.v. with uniform support such that $X(S) = \{-1,1,6\}$. Note that $E[X] = 2$. 
Then we have:
~
~ Math
\begin{aligned}
  SD[X] &= \sqrt{Var[X]} = \sqrt{E[X^2] - E[X]^2} = \sqrt{\frac{38}{3} - \frac{12}{3}} = 
  \sqrt{\frac{26}{3}} = \frac{\sqrt{78}}{3} \\
  E[|X -E[X]|] &= \frac{8}{3}
\end{aligned}
~
[&box;]{float=right}
    f. (2 points) Give an example for two random variables $X,Y$ such that $\E[XY] = \E[X]\E[Y]$.
~ Proof
Trivially, you could just take $X = 0$ always and $Y = 0$ always (actually, just one suffices). 
However, for the sake of doing some work, we take $X$ with uniform support such that 
$X(S) = \{1,2\}$ and let $Y$ be with uniform support such that $Y(S) = \{3,4\}$ independent of 
$X$. Then note that the new r.v $XY$ has uniform support such that $XY(S \times S) = \{3,4,6,8\}$. 
Then we have:
~
~ Math
\begin{aligned}
  E[XY] &= \frac{1}{4} (3 + 4 + 6 + 8) = \frac{21}{4} \\
  E[X]E[Y] &= \left[\frac{1}{2}(1 + 2) \right] \left[\frac{1}{2} ( 3 + 4) \right] = 
  \frac{3}{2}\frac{7}{2} = \frac{21}{4}
\end{aligned}
~
In general, the above identity holds when $X,Y$ are independent.
[&box;]{float=right}

    g. (2 points) Give an example for two random variables $X,Y$ such that $\E[XY] \neq \E[X]\E[Y]$.
~ Proof
Let $X$ be the same r.v. as defined in (d). Then take $Y = X$. By (c), it immediately follows that:
~
~Math
  E[XY] = E[X^2] \neq E[X]^2 = E[X]E[Y]
~
[&box;]{float=right}

    h. (2 points) Prove that if $X$ and $Y$ are independent random variables (i.e., for every $x,y$,
    $\Pr[X=x \wedge Y=y]=\Pr[X=x]\Pr[Y=y]$) then
    $\E[XY]=\E[X]\E[Y]$ and $Var[X+Y]=Var[X]+Var[Y]$.
~Proof
We prove the general case hinted at above. We have:
~
~Math 
  \begin{aligned}
    E[XY] &= \sum_{x \in S, y \in S} Pr[X = x \wedge Y = y]xy \\
    & = \sum_{x \in S} \sum_{y \in S} (Pr[X = x]x) (Pr[Y= y]y) \\
    &= \sum_{x \in S} Pr[X = x]x \sum_{y \in S} Pr[Y = y]y \\
    &= E[X]E[Y] 
  \end{aligned}
~
Note that we only used the definition of expected value (general version) and the independence of 
$X,Y$ (going from first to second line). Therefore the above holds for any $X,Y$ independent. 
[&box;]{float=right}

2. (15 points) Suppose that $H$ is chosen to be a random function mapping the numbers
$\{1,\ldots,n\}$ to the numbers $\{1,..,m \}$. That is, for every $i\in
\{1,\ldots,n\}$, $H(i)$ is chosen to be a random number in $\{ 1,\ldots, m \}$ and that choice is done 
independently for every $i$. For every $i < j \in \{1,\ldots,n\}$, define the random variable $X_{i,j}$ to equal $1$ if there
was a _collision_ between $H(i)$ and $H(j)$ in the sense that
$H(i)=H(j)$ and to equal $0$ otherwise.

    a. (3 points) For every $i < j$, compute $\E[ X_{i,j} ]$.
~Proof
    Note that by symmetry, $E[X_{i,j}]$ is equal for all $i < j \in \{1,...,n\}$. Therefore, WLOG, 
    we pick $i=1,j=2$. Then:
~
~Math
  E[X_{1,2}] = 0 \cdot Pr[X_{1,2} = 0] + 1 \cdot Pr[X_{1,2} = 1] = Pr[X_{1,1} = 2] = 
  Pr[H(1) = H(2)] = \frac{1}{m}
~ 
The last part comes from the fact that there's a $1$ in $m$ chance that the randomized function $H$ takes $j$ into the same output as it took $i$. The above shows that $E[X_{i,j}] = \frac{1}{m}$.
[&box;]{float=right}

    b. (3 points) Define $Y = \sum_{i < j} X_{i,j}$ to be the total number of collisions. Compute $\E[ Y ]$ as a function of $n$ and $m$.
      In particular your answer should imply that if $m < n^2/1000$ then $\E[Y]>1$ and hence in expectation
      there should be at least one collision and so the function $H$ will not be one to one.
~ Proof
We simply use linearity of expectation and the results from part (a) above.
~
~ Math
E[Y] = E\left[\sum_{i < j} X_{i,j}\right] = \sum_{i < j} E[X_{i,j}] = \frac{n(n-1)}{2m}
~
Note that the last line follows from the fact that the total number of summands such that 
$i < j \in \{1,\cdots, n\}$ is given by ${n \choose 2} = \frac{n(n-1)}{2}$ combined with 
the results from part (a). We can verify the claim made earlier too.
~ Math
m < n^2/1000 \implies E[Y] > \frac{n^2-n}{2n^2 / 1000} = 500(1 - \frac{1}{n}) > 1   
~ 
The above holds for all $n \in \mathbb{N}$. Additionally, note the $> 1$ lower bound is preserved as \
long as  $m < n^2/2$.
[&box;]{float=right}
    c. (3 points) Prove that if $m > 1000\cdot n^2$ then the probability that $H$ is one to one is at 
    least $0.9$.
~ Proof
First, note that the probability that $H$ is one to one is equivalent to the probability of _no_ 
collisions. If we wish to lower bound this probability, we can upper bound the complement. That is to 
say, we can find an upper bound for the probability that the function $H$ is _not_ one to one (ie, at 
least one collision occurs). Mathematically:
~
~Math
  \Pr[Y = 0] = 1 - \Pr[Y \geq 1]
~
Note that we are given that $m > 1000\cdot n^2 \implies m > 5 n^2$. Then in that scenario, we have:
~Math
  E[Y] = \frac{n^2-n}{2m} < \frac{n^2 - n}{10 n^2} = \frac{1}{10} - \frac{1}{10n} < \frac{1}{10}
~
for all $n > 0$. Let $k$ be a constant. Then by Markov's Inequality:
~ Math
\Pr[Y \geq kE[Y]] \leq \frac{1}{k}
~
Note that we're trying to upper bound $\Pr[Y \geq 1]$. In this case, 
$kE[Y] \geq 1 \implies k \geq 10 \implies \frac{1}{k} \leq \frac{1}{10}$.
The first implication is due to the results above where $E[Y] \leq \frac{1}{10}$. 
Then by Markov's Inequality, we have that 
$\Pr[Y \geq 1] < \frac{1}{10} \implies \Pr[Y = 0] \geq 0.9$, as desired.
[&box;]{float=right}

    d. (3 points) Give an example of a random variable $Z$ (unrelated to the function $H$) that is always equal to a non-negative integer, and such that
      $\E[Z] \geq 1000$ but $\Pr[ Z > 0] < 0.001$.
~ Proof
Simply take the r.v. Z with support $\{0, 10^7\}$ where $\Pr[Z = 0] = 1 - \frac{1}{10^4}$. Then we have that the expected values is:
~
~Math
 E[Z] = (1 - \frac{1}{10^4})(0) + \frac{10^7}{10^4} = 1000
~
And note that by definition, $\Pr[Z > 0] = \frac{1}{10^4} < 0.001$.
[&box;]{float=right}
    e. (3 points) Prove that if $m  < n^2/1000$ then the probability that $H$ is one to one is at most $0.1$.
~ Proof
We follow a relatively straightforward approach for this proof. The first step is in noting that $H$ is one to one if and only if
there are no collisions. Then directly calculating the result:
~
~Math
  \Pr[Y = 0] = (1-\frac{1}{m})(1 - \frac{2}{m})\cdots(1 - \frac{n}{m}) 
~
Each of the terms in the product comes from considering the probability of the $j$-th results from the $H$
function not being equivalent to any of the previous $j - 1$ results (ie, we consider the function to output values in a sequence
which can be done WLOG). Next, we take advantage of the following fact:
~ Math
e^x = \sum_{k=0}^{\infty} \frac{x^k}{k!} \geq 1 + x
~
With the above, we can replace each term in the product to optain an upper bound:
~ Math
\Pr[Y = 0] \leq e^{-\frac{1}{m} - \frac{2}{m} - \cdots - \frac{n}{m}} = e^{-\frac{n^2-n}{2m}}
~
The first inequality simply transforms products of exponentials into the exponentiation of the a sum. 
The first equality then follows immediately from a simple summation of $(1 + 2 \cdots n)$.
Then note that for $m < n^2 /1000$, the above simplifies further (with approximations):
~ Math
\Pr[Y = 0] \leq e^{-500} \leq 0.1
~
as desired. The simplification is done by ignoring the linear term $n$ (which we can do for large enough $n$) and plugging in $m = n^2/1000$. For all 
$m' \leq m$, the exponent will be more negative, which will lead to a smaller value. Therefore $e^{500}$ is an upper bound
for large enough $n$.
[&box;]{float=right}


3. (15 points) In this exercise we we will work out an important special case of the Chernoff bound.
   You can take as a given the following facts
    I) The number of $x \in \zo^n$ such that $\sum x_i = k$ is $\binom{n}{k} = \tfrac{n!}{k!(n-k)!}$.
    II) Stirling's approximation formula: for every $n\geq 1$,
        $\sqrt{2\pi n}\left(\tfrac{n}{e}\right)^n \leq n! \leq 2\sqrt{2\pi n}\left(\tfrac{n}{e}\right)^n $
          where $e=2.7182\ldots$ is the base of the natural logarithm.

    Do the following:
      a. (5 points) Prove that for every $n$, $\Pr_{x\getsr \zo^n} [ \sum x_i \geq 0.6n ] < 2^{-n/1000}$
~Proof
    In this proof, we the Chernoff bound theorem from the notes. We simply rewrite the given statement:
~
~ Math
  \Pr_{x \getsr \zo^n} [\frac{\sum x_i}{n} \geq 0.6n] = \Pr_{x \getsr \zo^n} [(\frac{\sum x_i}{n} - 0.5) \geq 0.1]
~
Then note that we have $E[X_i] = 0.5$, so looking at Chernoff's bounds, we have:
~ Math
\Pr_{x \getsr \zo^n} [|\frac{\sum x_i}{n} - 0.5| > 0.1 ] < 2^{-0.0025n}
~
The above is extremely close to the original statement. The main difference lies in the absolute value. Note that by symmetry (because we have a Bin($n,\frac{1}{2})$), the probability of the distance from the mean being $\geq 0.1$ is precisely twice the probability of the sample mean being $\geq 0.1$. Therefore:
~ Math
\Pr_{x \getsr \zo^n} [\sum x_i \geq 0.6n] < 2^{-0.00125n} < 2^{-0.001n} 
~
as desired.
~ Proof
Now we proof without using Chernoff. The idea behind the proof is simple, though instead of proving the result
for any $n$, we just prove the result for large enough $n$ (ie, using big-Oh notation because I didn't want to keep
track of the constants)! The first step in the proof is to use the naive definition of probability:
~
~ Math
\Pr_{x \getsr \zo^n}\left[\sum x_i \geq 0.6n \right] = \frac{\sum_{k=0}^{\lfloor 0.4n \rfloor} {n \choose k}}{2^n}
~
The denominator is assuming that the bit strings are uniformly distributed, and the numerator counts the
number of bit strings with more than $\lceil 0.6n \rceil$ ones by counting the number of strings with less
than $\lfloor 0.4n \rfloor$ zeros (which is counted in the same way!). Continuing, we first note that
the sum is dominated by its final term, so for large enough $n$ and a large enough constant $c_1$, we'll have:
~ Math
\frac{\sum_{k=0}^{\lfloor 0.4n \rfloor} {n \choose k}}{2^n} \leq c_1\frac{ {n \choose 0.4n }}{2^n}
~
Now we make use of Stirling's approximation to simplify the factorials into something a little more manageable.
Note that once again I collect all the constants into a single $c_2$ term because who wants to keep track
of them? Also, note that the $e^n$ on the bottom and the $e^{0.4n}e^{0.6n}$ on the top cancel! Additionally,
the $n^n$ and the $n^{0.4n}n^{0.6n}$ also cancel. This leaves us with a very simple form:
~ Math
{n \choose 0.4n} \leq c_2\frac{\sqrt{n}}{n}
~
Putting the above together, we immediately arrive at the conclusion:
~ Math
\Pr_{x \getsr \zo^n}\left[\sum x_i \geq 0.6n \right] \leq \frac{c_1c_2}{2^{n}\sqrt{n}} = \frac{1}{2^{n + \frac{1}{2} \log n - \log c_1 c_2}} < \frac{1}{2^{n/1000}}
~
If we really want to go out of our way to make it so that it holds true for all $n$, then note that the
tighest bound occurs when $n = 1$, so we'd just have to pick $c_1,c_2$ to satisfy the above constraints (though
note that $c_2$ is essentially fixed as we're likely to use the constants determined by Stirling, so really we just need to pick
a small enough $c_1$). It's likely that $c_1 = 2$ works. Working it out in explicit detail:
~ Math
  \begin{aligned}
1/1000 < 1 - \log c_1 c_2 \\
c_1 < \frac{2^{1 - \frac{1}{1000}}}{c_2}
\end{aligned}
~
Note that we can calculate $c_2$ explicitly to be:
~ Math
  c_2 = \frac{\sqrt{2\pi}}{\pi} \approx 0.8
~
Therefore, note that the value $c_2 = 2 < \frac{2^{1 - \frac{1}{1000}}}{0.8}$. Note that the first term in the
sume dominates exponentially, therefore it is certainly the case that:
~ Math
{n \choose 0.4n } \leq 2 \frac{\sqrt{n}}{n}
~
which concludes the proof for the specified constants. 
[&box;]{float=right}

      The above shows that if you were given a coin of bias at least $0.6$, you should only need some constant number of samples to be  able to reject the "null hypothesis" that
      the coin is completely unbiased with extremely high confidence. In the following somewhat more challenging questions (which can be considered as bonus exercise) we try to show a converse to this:

        b. Let $P$ be the uniform distribution over $\zo^n$ and $Q$ be the $1/2+\epsilon$-biased distribution corresponding to tossing $n$ coins in which each one
      has a probability of $1/2+\epsilon$ of equalling $1$ and probability $1/2-\epsilon$ of equalling $0$. Namely the probability of $x\in\zo^n$ according to $Q$ is equal to
      $\prod_{i=1}^n (1/2 - \epsilon + 2\epsilon x_i)$.

         i. (5 points) Prove that for every threshold $\theta$ between $0$ and $n$, if $n < 1/(100\epsilon)^2$ then the probabilities that $\sum x_i \leq \theta$ under $P$ and $Q$ respectively
            differ by at most $0.1$. Therefore, one cannot use the test whether the number of heads is above or below some threshold to reliably distinguish between these two possibilities unless the number
            of samples $n$ of the coins is at least some constant times $1/\epsilon^2$.
         ii. (5 points) Prove that for _every_ function $F$ mapping $\zo^n$ to $\zo$, if $n < 1/(100\epsilon)^2$ then the probabilities that $F(x)=1$  under $P$ and $Q$ respectively differ by at most $0.1$.
           Therefore, if the number of samples is smaller than a constant times $1/\epsilon^2$ then there is simply _no test_ that can reliably distinguish between these two possiblities.


## Exercises from Lecture 1

4. (20 points) Prove that every encryption scheme $(E,D)$ is perfectly secret if and only if for every plaintexts $m,m' \in \zo^\ell$, the two
random variables $\{ E_k(m) \}$ and $\{ E_{k'}(m') \}$ (for randomly chosen keys $k$ and $k'$)
have precisely the same distribution.
~ Proof
We first proof the "if" condition. The condition implies that for every plaintext $m_0,m_1$ and for $K \gets \zo^{n}$ (key is distributed uniformly random)
we have:
~
~Math
 \Pr[E_K(m_0) = c] = \Pr[E_K(m_1) = c] 
~
Therefore, almost directly, it follows that seeing the ciphertext gives no information to Eve.
We can work this more formally by taking letting $M$ be the distribution over possible messages and $C$ the distribution ove possible cipher-texts.
We prove that seeing the cipher-text $C = c$ gives no additional information to as to the message $M = m$.
The proof follows an outline similar to that from the recommended textbook.
~ Math
\begin{aligned}
  \Pr[M = m \mid C = c] &= \frac{\Pr[C = c \mid M = c] \Pr[M = m]}{\Pr[C = c]} \\
  &= \frac{\Pr[C = c \mid M = c] \Pr[M = m]}{\sum_{m' \in \mathcal{M}} \Pr[C = c \mid M = m'] \Pr[M = m']} \\
  &= \frac{\Pr[M = m]}{\sum_{m' \in \mathcal{M} \Pr[M = m']}} \\
  &= \Pr[M = m]
\end{aligned}
~
The first line is by Baye's rule.
The second simply uses LOTP to expand the bottom.
In the third step is the most involved, but it consists of simply noting that by our assumption, $\forall m' \in \mathcal{M}$, we have:
~ Math
 \begin{aligned}
\Pr[C = c \mid M = m'] &= \Pr[E_K(M) = c \mid M = m'] \\
 &= \Pr[E_K(m') = c] = \Pr[E_K(m) = c] \\
 &= \Pr[C = c \mid M = m]
\end{aligned}
~
The first equality is definition and the second is done by conditioning on $M = m'$. The last equality is given
by our assumption about the distributions being equal. The last equality is just a reversal of the aforementioned steps.
Therefore, with the above in mind, we can factor out the common term from the sum ($\Pr[C = c \mid M = m]$) and cancel 
with the top factor. We're then left with a summation over a complete sample space on the bottom, which the last 
equality resolves by noting that it sums to $1$. 
With the above complete, it is evident that Eve gains no additional information after seeing the ciphertext
and therefore we conclude that for every set $M \subset \zo^l$ if we choose $m \in M$ and a random key
$k \in \zo^n$, then:
~ Math
 \Pr[Eve(E_k(m)) = m] \leq \frac{1}{|M|}
~
which is the definition of perfect secrecy.
[&box;]{float=right}
~ Proof
We now proof the "only if" condition. We do this by supposing that there exists some encryption scheme $(E,D)$
that we assume to be perfectly secret but for which the distributions as described in the problem statement are
not equal. This implies that $\exists m,m' \in \mathcal{M}$ such that, WLOG:
~
~Math
 \Pr[E_K(m) = c] > \Pr[E_K(m') = c]
~
We use these two messages to show that the scheme is not perfectly secret through the Two to Many Theorem. Recall 
that a scheme is perfectly secret if and only if for every two distinct $\{m_0,m_1\}$ and every strategy used 
by Eve, if we chose a message and a key at random, the probability that Eve guesses the message after seeing 
$C = E_K(M)$ is at most $\frac{1}{2}$. Indeed, simply define $Eve(C) = m$ if $C = c$ and otherwise $Eve(C)$ outputs a uniformly random
message in $\{m,m'\}$. Then note that by the above, the probability that $Eve(c) = m$ is higher than the probability that
$Eve(c) = m'$ (by our assumption that the distributions are not equal) when $E_K(m) = c$. Therefore, since $Eve$ outputs either
$m$ or $m'$, it follows that the probability that the output message is the correct message is higher that $\frac{1}{2}$.
[&box;]{float=right}
5. (20 points- a bit harder bonus question)  In the lecture we saw that any perfectly secret private key encryption
scheme needs to use a key as large as the message. But we
actually made an implicit subtle assumption: that the encryption
process is _deterministic_. In a _probabilistic encryption scheme_, the encryption function $E$ may be probabilistic: 
that is, given a message $m$ and a key $k$, the value $E_k(x)$ is not fixed but is distributed according to some distribution $C_{x,k}$. 
The decryption function is still given only the key $k$ and not the internal randomness used by $E$, and we require that for every message $m$, $\Pr[D_k(E_k(m))=m]>0.99$
where this probability is taken both over the choice of the key $k$ and the internal randomness used by $E$. Prove that even a probabilistic encryption scheme cannot be perfectly secret with a key that's significantly shorter than the message. That is, show
that for every probabilistic encryption scheme $(E,D)$  using $n$-length keys and $n+10$-length messages, there exist two messages $m,m'\in \zo^{n+10}$
such that the distributions $\{ E_k(m) \}$ and $\{ E_{k'}(m') \}$ are not identical.
~ Proof
The first proof presented is the one we believe to be the strongest proof of this claim. However, this
proof does not use the intended method based on the phrasing of the question, so we present a second
proof which is more directly drawn from the question but which leads me unsatisfied (though it's
simplicity is breathtaking).
Now for the first proof.
The outline of the proof is relatively simple, and it follows the deterministic proof presented in lecture
with some small modifications to handle the internal randomness of $E$. First, note that a probabilistic
encryption $E$ scheme can be thought of as a collection of _deterministic_ encryption schemes $\{E^i\}$
each with probability $p_i$ of being selected. Now suppose that we choose two messages $m_0$ and $m_1$ 
at random, and then encrypt $m_1$ to obtain the cipher-text $c_1$. Then let us ask what is 
the probability that there exists some key $k$ such that $c_1 = E_k(m_0)$. Let us fix $m_0$, and consider
the set $C_0^i = \{E^{i}_k(m_0) \mid k \in \zo^n\}$ and the set $C_0 = \bigcup_i C_0^i$. Then
conditioning on the randomly chosen deterministic algorithm when running the encryption:
~
~Math
\Pr[E_k(m_0) = c_1] = \sum_{i} \Pr[E^i_k(m_0) = c_1] p_i
~
Now that we've reduced the problem to a deterministic algorithm, note that for fixed $i$, the probability
that the encryption is equal to $c_1$ for fixed $m_0$ is at most $\frac{1}{2^10}$. This is because for 
fixed messages, there are at most $2^n$ distinct outputs for $E^i_k(m_0)$ (one per key, or in other words,
$|C_0^i| \leq 2^n$), yet for the fixed key and random message, we have up to $2^{n+10}$ distinct ciphertexts 
(one per message, since the function $E^i_k$ must be one to one as it's deterministic). 
Therefore we can place an upper bound on the sum above:
~ Math
\Pr[E_k(m_0) = c_1] = \sum_i \Pr[E^i_k(m_0) = c_1] p_1 \leq \frac{1}{2^10} \sum_i p_i = \frac{1}{2^10}
~
Hence, similarly to the deterministic argument, there must be some choice of $m_0,m_1$ such that Eve
decides to output $m_0$ if $c \in C_0$ and output $m_1$ otherwise. By the arguments above, she will be 
successful with probability $> \frac{1}{2}$ and therefore the encryption scheme is not secure.
The insecurity of the encryption scheme implies that $\{ E_k(m_0) \} \not\approx \{E_{k'}(m_1) \}$ as desired.
[&box;]{float=right}
Now for the second proof.
~ Proof
In this scenario, we tackle the task of showing that $\{E_k(m) \} \not\approx \{E_{k'}(m')\}$. What we wish to
show is that $\Pr[E_k(m) = c] \neq \Pr[E_{k'}(m') = c]$ for some fixed ciphertext $c$. Note that by 
conditioning on $i$ (the deterministic algorithm), we have:
~
~ Math
\Pr[E_k(m) = c] = \sum_i \Pr[E^i_k(m) = c] p_i \neq \sum_{i} \Pr[E^i_{k'}(m') = c] p_i = \Pr[E_{k'}(m') = c]
~
The equalities arise from a simple application of LOTP, and the middle inequality arises from the fact that
for fixed $i$, we know from lecture that there exists $m,m'$ such that the encryption scheme is 
$E^i_k$ is insuecure and therefore the distributons are not equal. 
[&box;]{float=right}

## Exercises from Lecture 2

6. (20 points) Prove the Computational Indistinguishability phrasing of computational security Theorem.
For the sake of completeness, we repeat the theorem: Let $(E,D)$ be a valid encryption scheme. Then $(E,D)$ 
is computationally secure if and only if for every two messages $m_0,m_1 \in \zo^l$,
~ Math
\{E_k(m_0)\} \approx \{E_k(m_1)\}
~
where each of thse distributions is obtained by sampling a random $k \gets_R \zo^n$. We now continue with 
the proof. 
~ Proof
We first proof the "if" direction. We are attempting to get an upper bound on the probability of Eve
guessing the correct message given the cipher text, assuming she is computationally limited. Then take
our encryption scheme $(E,D)$ and any two distinct $\{m_0, m_1\} \subseteq \zo^l$ and any efficient 
strategy $Eve$. Choose at random $b \in \zo$ and a key $k \in \zo^n$. Then note that:
~
~Math
  \Pr[Eve(E_k(m_b)) = b] = \frac{1}{2}\Pr[Eve(E_k(m_0)) = 0] + \frac{1}{2}\Pr[Eve(E_k(m_1)) = 1]
~  
by the law of total probability conditioning on the randomly chosen $b$. If we then add and subtract 
the term $\frac{1}{2}\Pr[Eve(E_k(m_0)) = 1]$, and rearranging terms:
~Math
  \begin{aligned}
  \Pr[Eve(E_k(m_b)) = b] &= \frac{1}{2} (\Pr[Eve(E_k(m_0)) = 0] + \Pr[Eve(E_k(m_0)) = 1]) \\
  &+\frac{1}{2}(\Pr[Eve(E_k(m_1)) = 1] - \Pr[Eve(E_k(m_0)) = 1])
  \end{aligned}
~
The above is starting to look promising. Next step is to note that $x + y \leq x + |y|$, along with the
fact that $\Pr[Eve(E_k(m_0)) = 0] + \Pr[Eve(E_k(m_0)) = 1] = 1$ and then we make direct use of the 
indistinguishability assumption:
~ Math
  \begin{aligned}
\Pr[Eve(E_k(m_b)) = b] &\leq \frac{1}{2} + \frac{1}{2}|\Pr[Eve(E_k(m_1)) = 1] - \Pr[Eve(E_k(m_0)) = 1]| \\
&\leq \frac{1}{2} + \frac{\epsilon}{2} 
\end{aligned}
~
for some negligible epsilon. The above holds in the general case, and therefore our encryption scheme is computationally secure. 
[&box;]{float=right}
~ Proof
Now we prove the "only if" part. Suppose we have a computationally secure system $(E,D)$ for which
the theorem doesn't hold. Then that means that there exists $m_0,m_1 \in \zo^l$ and computationally 
efficient $Eve$ such that:
~
~ Math
|\Pr[Eve(E_K(m_1)) = 1] - \Pr[Eve(E_K(m_0)) = 1]| \geq \epsilon
~
for some non-negligible $\epsilon$. Then we follow the same pattern as above:
~Math
  \begin{aligned}
  \Pr[Eve(E_k(m_b)) = b] &= \frac{1}{2} (\Pr[Eve(E_k(m_0)) = 0] + \Pr[Eve(E_k(m_0)) = 1]) \\
  &+\frac{1}{2}(\Pr[Eve(E_k(m_1)) = 1] - \Pr[Eve(E_k(m_0)) = 1])
  \end{aligned}
~
At this point, we first handle the case where $(\Pr[Eve(E_k(m_1)) = 1] - \Pr[Eve(E_k(m_0)) = 1]) > 0$ since 
it is the most direct. In that scenario, note that we have equality when adding the absolute value bars,
and then we apply the results from the fact that we can computationally distinguish the distributions.
~ Math
  \begin{aligned}
\Pr[Eve(E_k(m_b)) = b] &= \frac{1}{2} + \frac{1}{2}|\Pr[Eve(E_k(m_1)) = 1] - \Pr[Eve(E_k(m_0)) = 1]| \\
&> \frac{1}{2} + \frac{\epsilon}{2} 
\end{aligned}
~
for some non-negligible epsilon. The above holds for the specified case. In the other case where 
$(\Pr[Eve(E_k(m_1)) = 1] - \Pr[Eve(E_k(m_0)) = 1]) < 0$, a similar inequality should hold and we are done.
[&box;]{float=right}

7. (20 points) Give a direct proof (not going through computational indistinguishability) in your own words for the length extension theorem in the special case $t=2$ and when
the messages are $m^0 = 00$ and $m^1 = 01$. That is, show how to transform an adversary $Eve$ that
can distinguish between the distribution $C^0 = (E'_{k_0}(k_1,0),E'_{k_1}(k_2,0))$ and $C^1 = (E'_{k_0}(k_1,0),E'_{k_1}(k_2,1))$
(for random $k_0,k_1,k_2$) with advantage $\epsilon$ into an adversay $Eve'$ that runs in time polynomial in the running time of $Eve$
and can distinguish between $E'_k(m')$ and $E'_k(m'')$ for two messages $m',m''\in\zo^{n+1}$ with advantage at least, say, $\epsilon/10$.
~ Proof
The proof is similar to the presented in lecture for the security of PRGs. Let us consider the two
distinct messages $m, m' \in \zo ^ {n+1}$ and the task of constructing an $Eve$ that can guess correctly,
given a ciphertext $c$ of either $m$ or $m'$, which message is more likely to be with probability
non-neglibly greater than $\frac{1}{2}$. By the Two to Many theorem, this shows the insecurity of the
encryption scheme $E'_k$.
For any two distinct random messages $m,m'$, and relabel them $m_0,m_1$. Now $Eve$, on input $c$, does the following. 
We interpret $c$ as $E'_{k_1}(m_b)$ and feed 
$(E'_{k_0}(k_1,0), c)$ to $Eve'$ for random $k_0,k_1$ 
which will output $b \in \zo$, meaning that the ciphertext is more likely to be from the distribution 
$C^b$ which means the input message was more likely to be $m^b$. Note that this gives $Eve'$ information
that the original message is more likely than not to contain an initial bit $b$. Therefore, $Eve'$, 
there exists two messages $m,m'$ between which $Eve$ can non-negligibly distinguish. Since $Eve'$ is 
computationally efficient, $Eve$ is computationally efficient. Similarly, since $Eve'$ can distinguish
with non-negligible probability, $Eve$ can also distinguish with the same non-negligible probability.
~
[&box;]{float=right}