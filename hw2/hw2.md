~ MathDefs
\newcommand{\zo}{\{0,1\}}
\newcommand{\getsr}{\leftarrow_{\text{\tiny R}}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\N}{\mathbb{N}}
~

_CS 127: Cryptography / Boaz Barak_


# Homework 2

Total of 128 points.


## Exercises from Lecture 3

1. (impossibility of statistically testing randomness, 15 points) Let $T_1,\ldots,T_M:\zo^n\rightarrow\zo$ be a collection of function that are supposed to be statistical tests for randomness. Prove that if $n$ is large enough and  $M<2^{100n}$ there exists a distribution $X$ that passes all these tests but is very far from the uniform distribution. Concretely, show that
there exists a random variable $X$ over $\zo^n$ such that:
  * For every $i\in [M]$, $| \E[ T_i(X)]-\E[T_i(U_n)]|<0.001$ where $U_n$ is the uniform distribution over $n$ bits.
  * But, there exists some $T^*:\zo^n\rightarrow\zo$ such that $| \E[ T^*(X)] - \E[T^*(U_n)] |>0.999$.
~ Proof
Take $S \subset \zo^n$ such that $|S| < 2^{n + \log \epsilon}$
for some small $\epsilon \in (0,1]$. Let $X$ be any distribution with support $S$ (ie, $P[X = x] > 0$ for $s \in S$
and $\Pr[X = x] = 0$ for $x \notin S$). Then consider the test $T^*: \zo^n \to \zo$ defined as the characteristic
or indicator function of the set $S$:
~
~ Math
 T^*(x) = \begin{cases} 
      1 & x \in S \\
      0 & x \notin S 
   \end{cases}
~
Then note that this test distinguishes between $U_n$ and $X$. 
~ Math
  \begin{aligned}
|\E[ T^*(X)] - \E[T^*(U_n)| &= \left|\sum_{x \in S} T^*(x) - \frac{1}{2^n} \sum_{x\in \zo^n} T^*(x) \right| \\
  &= |1 - \frac{|S|}{2^n}| \\
  &= 1 - \frac{|S|}{2^n} \\
  &> 1 - 2^{\log \epsilon} \\
  &= 1 - \epsilon
\end{aligned}
~
The first expectation is simplified because it's simply the sum of the probabilities of the distribution
$X$ over it's support (which is $1$) and the second expectation is simply the proportion of $|S|$ to $\zo^n$.
Note that we can make epsilon arbitrarily small, and in particular, we can take $\epsilon = 0.001$. Now
let $X$ be the uniform distribution over $S$, the restricted sample space. Then consider, for fixed $i$, 
the probability of $\E[T_i(X)]$ deviating from the $\mu = \E[T_i(U_n)]$ by more than $\epsilon$. 
~ Math
  \begin{aligned}
\Pr[|\E[T_i(X)] - \E[T_i(U_n)]|  > \epsilon ] &= \Pr \left[ |\sum_{x \in S} \frac{T_i(x)}{|S|}  - \mu | > \epsilon \right]  \\
  &< 2^{\frac{-\epsilon^2 n}{4}}
\end{aligned}
~ 
The above inequality follows immediately from the Chernoff bound. Note that $\E[T_i(X)]$ is essentially
a sample mean of $T_i(U_n)$ (we're restricted $S \subset \zo^n$). With the above, we now have a tight
upper bound on the probability that the test _does_ distinguish between $X$ and $U_n$. Then simply note
that the probability that none of the tests distinguish is the complement of at least one of the tests
distingushing. Letting $D_i$ be the event where $T_i$ successfully distinguishes between $X$ and $U_n$
by some $\epsilon$, the probability of successfully distinguishing, $D$ is given by:
~ Math
  \begin{aligned}
    \Pr[D] &= \Pr[ \bigcup_i D_i ] \\
    &\leq \sum_{i} \Pr[D_i] \\
    &< \sum_{i} 2^{\frac{-\epsilon^2 n}{4}} \\
    &= \frac{M}{2^{\frac{\epsilon^2 n}{4}}} 
  \end{aligned}
~
The first inequality is simply using the union-bound, and the second uses the Chernoff bound shown
previously. From the above we can immediately see that for any given $\epsilon > 0$, we can find
a large enough $n$ so that the probability of any of the tests $T_i$ successfully distinguishing 
the two distributions $X$ and $S$ can be made arbitrarily small. This shows that for large enough $n$
and for fixed $M$, then no test will successfully distinguish between $X$ and $U_n$. However, we showed
before that the indicator test $T^*$ will always successfully distinguish between $U_n$ and any distribution
$X$ with support $S \subset \zo ^n$. 
[&box;]{float=right}

(No points, just food for thought.) Based on this exercise, what do you believe can we say about a distribution $X$ if it passes the FIPS 140-2 testing suite for randomness?
Based on the above, we can't say much. Testing for randomness appears to be impossible.

2.  (20 points) We call a sequence $\{ X_n \}_{n\in\N}$ where $X_n$ is a distribution over $\zo^n$  \emph{pseudorandom} if it's computationally indistinguishable from the sequence $\{ U_n \}$ where $U_n$ is the uniform distribution over $\zo^n$. Are the following sequences pseudorandom? prove or refute.
   a. (10 points) $\{ X_n \}$ where  $X_n$ be the following distribution: we pick $x_1,\ldots,x_{n-1}$
     uniformly at random in $\zo^{n-1}$, and let $x_n$ be the parity (i.e. XOR) of
     $x_1,\ldots,x_{n-1}$, we output $x_1,\ldots, x_n$.
     ~Proof
     The sequence $\{X_n\}$ is computationally distinguishable from $\{U_n\}$. Note that this follows from 
     the fact that for any well-defined $n$, $X_n$ is computationally distinguishable from $U_n$. A successful distinguisher
     can be defined as $D$, where on input $s \in \zo^n$, $D$ will take the parity of the first $n-1$ bits
     and output $1$ if $s_n = \oplus_{i=1}^{n-1}x_i$ and $0$ otherwise. Then note:
     ~
     ~Math
      |\E[D(X_n)] - \E[D(U_n]| = |1 - \frac{1}{2}| = \frac{1}{2}
     ~
     Note that the first expectation is $1$ because it holds for all strings drawn from $X_n$ while the
     second expected value only holds half of the time for strings drawn from $U_n$. Furthermore, note that
     $D$ is linear in the input, and therefore is computationally efficient. 
     [&box;]{float=right}
    b. (10 points) $\{Z_n \}$ where for $n$ large enough, with probability $2^{-n/10}$ we output an $n$ bit  string encoding the text \texttt{''This is not a pseudorandom distribution''} (say encode
    in ASCII and pad with zeros), and with probability $1-2^{-n/10}$ pick a random string. For
    $n$ that is not large enough to encode the text, $Z_n$ always outputs the all zeroes
    string.
    ~Proof
    We proof that the sequence $\{Z_n\}$ is computationally distinguishable from $\{U_n\}$. We do this by
    showing that for large enough $n$ and a computationally efficient distinguisher $D$, 
    $|\E[D(Z_n)] - \E[D(Z_n)]|$ is non-negligible. The construction of this distinguisher is very 
    straight forward. Let $t$ be the number of bits required to encode the given phrase in 
    ASCII (note that this is fixed). On input $s \in \zo^n$ for $n \geq t$, $D$ looks at the first $t$ bits of the input
    string $s$. If the first $t$ bits are equal to the phrase, $D(s) = 1$, otherwise $D(s) = 0$. 
    Then note the following for $n \geq t$:
    ~
    ~Math
    \begin{aligned}
    | \E[D(Z_n)] - \E[D(U_n)] | &= | 1 - \frac{2^{n-t}}{2^n} | \\
    &= \frac{1}{2^t}
    \end{aligned}
    ~
    The first expectation is simplified because the property holds for all $s \in_R Z_n$, 
    and the second expectation is just using the naive definition of probability to count 
    the number of $s \in_R U_n$ such that the first $t$ bits are fixed to the given ASCII encoding.
    Then note that $2^{-t}$ is fixed and non-negligible, therefore the two distributions are
    distinguishable. Furthermore, note that the running time of the algorithm is actually constant,
    depending only on $t$, and is therefore computationally efficient. 
    [&box;]{float=right}

3. (24 points) Suppose that $G:\zo^n\rightarrow\zo^{3n}$ is a secure pseudorandom generator. For each one of the constructions $G^1,G^2,G^3$ below either prove that they are necessarily a secure pseudorandom generator or give a counterexample (which is a construction, based on the Cipher, PRG or PRG conjectures, of a generator $G$ such that $G_i$ would not be secure.)
   a. (8 points) $G^1(s)=G(s)_{1,\ldots,2n}$ (i.e., the first $2n$ bits of $G(s)$).
   ~Proof
   $G^1$ is secure. Suppose this is not the case. Then there exists a distinguisher 
   $D: \zo^{2n} \to \zo$ which can distinguish
   between $G^1(U_n)$ and $U_{2n}$. Formally, $\exists D$ computationally efficient such that
   ~
   ~Math
    |\E[D(G^1(U_n))] - \E[D(U_{2n})]| > \epsilon(n)
   ~ 
   for non-negligible $\epsilon(n)$.
   Then note that we can construct a distinguisher $D': \zo^{3n} \to \zo$
   that can distinguish between $G(U_n)$ and $U_{3n}$, contradicting our assumption that $G$ is secure.
   Let $D'$ be the function which on input $s \in \zo^{3n}$ discards the final $n$ bits and feeds
   the remaining string to $D$, outputing the results of $D$. Then note that,
   ~Math
     \begin{aligned}
      D'(G(U_n)) &= D(G(U_n)_{1,\cdots,2n}) = D(G^1(U_n)) \\
      D'(U_{3n}) &= D(U_{2n})   
     \end{aligned}
   ~
   The second equality follows almost immediately, and the first is from the fact that we retain only
   the first $2n$ bits, which is the same process that $G^1$ performs. With the above, we have:
   ~Math
     |\E[D'(G(U_n))] - \E[D'(U_{3n})]| = | \E[D(G^1(U_n)] - \E[D(U_{2n})] | > \epsilon(n) 
   ~
   a contradiction on the security of $G$. Therefore, $G^1$ must be secure.
   [&box;]{float=right}
   b. (8 points) $G^2(s)=G(0,s_2,\ldots,s_n)$ (i.e., the output of $G(s)$ but setting the first seed bit to zero)
   ~Proof
    $G^2$ is not secure. Consider $G: \zo^n \to \zo^{3n}$ which is constructed as follows. By the PRG 
    conjecture (combined with the length extension theorem), there exists a secure $G': \zo^n \to \zo^{3n - 1}$.
    Then on input $s \in \zo^n$, $G$ feeds $s$ to $G'$ producing a $y \in \zo^{3n-1}$ which is pseudorandom. Then
    it simply appends $s_1$ to $y$ and gives as output $y \circ s_1$. Note that if $y$ is pseudorandom, 
    then because $s_1$ is uniformly distributed, $y \circ s_1$ is also pseudorandom. However, this 
    leads to a $G^2$ which is not pseudorandom. Given the above construction, we can easily construct
    a distinguisher $D$ which on input $s \in \zo^{3n}$ checks the last bit of $s$. If $s_{3n}$ is $0$, 
    then it outputs $1$, otherwise it outputs $0$. Then we have: 
    ~
    ~Math
    |\E[D(G^2(U_n))] - \E[D(U_{3n})] | = |1 - \frac{1}{2}| = \frac{1}{2}
    ~
    which is non-negligible, with $D$ computationally efficient, and therefore $G^2$ is insecure despite
    the fact that $G$ is secure.
   [&box;]{float=right}
   c. (8 points) $G^3(s_1,\ldots,s_n)=G(s_n,\ldots,s_1)$ (i.e., the output of $G(s')$ where $s'$ is obtained by reversing $s$).
   ~Proof
     $G^3$ is secure. Suppose not. Then there exists a computationally efficient distinguisher $D: \zo^{n} \to \zo$
     between $G^3(U_n)$ and $U_{3n}$. However, note that this same distinguisher can distinguish between
     $G(U_n)$ and $U_{3n}$. The reason for this is that if the inputs $s$ are distributed uniformly, than
     the reverse inputs are also distributed uniformly. Therefore $G^3(U_n)$ has the same distribution as
     $G(U_n)$, which implies that 
   ~
   ~Math
     |\E[D(G(U_n))] - \E[D(U_{3n})| = |\E[D(G^3(U_n))] - \E[D(U_{3n})]| > \epsilon(n)  
    ~
    for some non-negligible $\epsilon(n)$ where $D$ is computationally efficient. 
   [&box;]{float=right}


## Exercises from Lecture 4

4. (24 points) In these three questions you'll show that if we have
a pseudorandom function family with particular input and output sizes, we can easily obtain a
family that handles different inputs and outputs. 

   a. (Padding inputs and outputs, 8 points) Suppose that $\{ f_s \}$ is a pseudorandom function collection where for every $s\in\zo^n$, $f_s$ maps   $\zo^n$ to $\zo^n$. Prove that if we define $f'_s$ to be function that on input $i\in [2^{n/2}]$ outputs the first bit of $f_s(2^{n/2}+i)$ then $\{ f'_s \}$ is a pseudorandom function collection (with one bit output).
   ~Proof
   Suppose $\{f_s\}$ is a pseudorandom function collection but $\{f_s'\}$ is not. Then there exists a 
   computationally efficient adversary $A'$ with access to a black box $B_s'(\cdot): [2^{n/2}] \to \{0,1\}$
   which can distinguish with non-negligible probability $\epsilon(n/2)$ between $B_s'(\cdot) = f'_s(\cdot)$
   and $B_s'(\cdot) = H'(\cdot)$ where $H' \gets [2^{n/2}] \to \zo$ (so $A'$ can tell whether 
   we're using a completely random function or a pseudorandom function from our collection $\{f_s^{\prime}\}$). 
   Then note that we can construct an 
   adversary $A$ with access to a black box $B_s(\cdot): [2^n] \to [2^n]$ which can distinguish between
   $B_s(\cdot) = f_s(\cdot)$ or $B_s(\cdot) = H(\cdot)$ for $H \gets [2^n] \to [2^n]$. We do this as
   follows, by allowing $A$ access to $A$'. When $A$' queries $B_s'(\cdot)$ with some value 
   $i \in [2^{n/2}]$, $A$ forwards the value $2^{n/2} + i$ to $B_s(\cdot)$, which then returns a 
   string $s \in \zo^n$. A the proceeds to forward the value $s_1$ (the first bit) to $A'$. After
   $A'$ is finished testing this simulated black-box, $A$ will return whatever $A'$ returns. 
   Then note that if the $B_s(\cdot)$ is completely random, the bits fed to $A'$ will be completely
   random. Similarly, if $B_s(\cdot)$ is $f_s(\cdot)$, then the bits fed to $A'$ will have the
   same distribution as $f'_s(\cdot)$.
   ~
   ~Math
     \begin{aligned}
    &|\E_{s\in\zo^n}[A^{F(s,\cdot)}(1^n)] - \E_{H \gets [2^n] \to [2^n]}[A^{H}(1^n)]|\\
    &= |\E_{s \in \zo^n}[A'^{F'(s,\cdot)}(1^n)] - \E_{H' \gets [2^{n/2}] \to \zo}[A'^{H}(1^n)]| \\
    &> \epsilon(n/2)
    \end{aligned}
   ~
   The above simple expresses the fact that if $B_s(\cdot)$ is from the collection $\{f_s\}$, then 
   $A$ will be able to distinguish that scenario from a completely random function with the same
   non-negligible probability $\epsilon(n/2)$ with which $A'$ can distinguish $\{f_s^{\prime}\}$ from 
   a random function. This is a contradiction on $\{f_s\}$ being a pseudorandom function collection,
   however, and therefore it must be the case that $\{f'_s\}$ is a pseudorandom function collection.
   [&box;]{float=right}
   b. (Increasing output size, 8 points) Prove that if there exists a collection $\{ f_s \}$ where $f_s:\zo^{|s|}\rightarrow \zo$ (i.e., one bit output), then then there exists a collection $\{ f'_s \}$ with  $f'_s:\zo^{|s|}\rightarrow\zo^{|s|}$. See footnote for hint.[^hint-prf-expand]
   ~Proof
    We follow a construction which mimicks the PRF-login protocol presented in lecture
    and that uses the ideas from part (a) above. Note that given our collection of functions 
    $\{f_s \}$ which maps from $[2^{|S|}] \to [2^{|S|}]$, we define $f'_s$ to be the function that 
    on input $i \in [2^{|s|/l}]$ for $l < |s|$ (where $l$ is such that $\exists k \in \mathbb{N}$ st.
    $kl = |s|$) outputs the the $t$-th bit of the result as
    $f_s(i + t2^{|s|/l})$. Then note that by the proof shown in (a), it's clear that this new
    $\{f'_s \}$ collection consists of PRFs. Then note that each $f'_s: [2^{|s|/l}] \to [2^l]$ for
    the specified $l$. Then note that the existence of PRFs implies the existence of PRGs, in the
    sense that we can simulate a PRG that doubles its input by querying $f'_s$ twice (the construction
    is very direct). By repeating this process, we can actually create a PRG that takes an input 
    of any size and expands it polynomially (this is the PRG Conjecture with Lengh Extension). Using
    this fact, we can create the collection we were seeking originally. The first step consists
    of using the PRFs to create a PRG $G: \zo^n \to \zo^{kn}$ (expands by a constant factor of $k$).
    Then we define the final collection $f''_s(x) = f'_{G(s)}(x)$. Therefore, note that $f''_s$ first
    expands its seed using the PRG G, which by the PRG Conjecture, is a distribution 
    indistinguishable from the uniform distribution $G(U_{|s|}) \approx U_{k|s|}$. It then uses 
    the construction presented above for $f'_s$ with $l = |s|$ (which is now possible, since we have
    $|G(s)| = k|s|$!). Then with this construction, we have a collection of functions $f'_s: [2^{|s|}] \to [2^{|s|}]$.
    The fact that this construction is a PRF immediately follows from the fact that $G$ is a valid
    PRG, $l = |s|$, $|G(s)| = k|s|$, and that part (a) above is proven.
    [&box;]{float=right}
    ~
   c. (Changing PRFs input size, 8 points) Prove that if there exists a collection $\{ f_s \}$ of pseudorandom functions with $f_s:\zo^{|s|}\rightarrow\zo^{|s|}$ then there exists a collection $\{ f'_s \}$ with $f'_s:\zo^{*}\rightarrow\zo^{|s|}$ (i.e., $f'_s$ for a random $s\in\zo^n$ is indistinguishable from a random function from $\zo^*$ to $\zo^n$. (If it makes your life easier, it's fine to construct a collection $\{ f'_s \}$ with a single output bit.)
   ~Proof
    We lay out the general sketch for the proof first, for the sake of time. The general idea is 
    that given an input of arbitrary size (the sketch is similar to construction 4.11 in the 
    recommended textbook). On an input of the string $m$ where $m = poly(|s|)|s|$, we split the
    message into $m_1,m_2, \cdots, m_{poly(|s|)}$ where each chunk we have $|m_i| = |s|$. Then 
    we set $t_0 = 0^{l(|s|)}$, and for $i = 1$ to $i = |l(|s|)|$, we set $t_i = f_s(t_{i-1} \oplus 
    m_i)$ for some random seed $s$. Finally, our algorithm outputs $t_{poly(|s|)}$. 
    
    I'm actually not sure this is secure, and I've run out of time, so I'll have to pass on it.
    
    Looking forward to the solutions, though!
   ~
   [&box;]{float=right}
[^hint-prf-expand]: First come up with a pseudorandom family with output longer than $1$ but shorter than $|s|$. For example, if $s \in \zo^{n^2}$ then the output can be $n$. Then show that existence of PRF implies existence of pseudorandom generators and use that to expand your output.

5. (20 points) Suppose that $\{ f_s \}$ is a collection of secure pseudorandom functions where $f_s$ maps $\zo^{|s|+1}$ to $\zo$. For each of the following constructions $f^1$,$f^2$ below of function collections mapping $\zo^{|s|}$ to $\zo^2$, either prove that they are necessarily secure or show a counterexample (i.e., a construction of PRF's $\{ f_s \}$ based on the PRF conjecture such that the corresponding construction is insecure)
  a. (10 points) $f^1_s(x)=f_s(0\circ x) \circ f_s(1\circ x)$ 
  ~Proof
 Intuitively, this is a valid and secure construction. The reason for this is that no matter what the
 chosen $x$ attacks on $f^1_s$ are, we always see the pairs of values $f_s(0 \circ x)$ and $f_s(1 \circ x)$
 which are completely unrelated by the security of $f_s$. More formally,
 Assume $\{f^1_s\}$ is not a PRF.  Then there exist a computationally efficient adversary $A^1$ such
 that for random seed $s$: 
  ~
  ~Math
   |\E_{s \gets_R \zo^{|s|}} [A^{f^1_s(\cdot)}(1^{|s|})] - \E_{H^1 \gets \zo^{|s|} \to \zo^2}[A^{H^1}](1^{|s|}) | < \epsilon
  ~
  Then note that we can use $A^1$ to construct an adversary $A$ the breaks $\{f_s\}$. The construction
  is simple as $A$ simply simulates $A^1$ and captures the queries it sends to its black box 
  $B^1_s(\cdot): \zo^{|s|} \to \zo^2$ and transforms them into queries to $A$'s black box, 
  $B_s(\cdot):\zo^{|s| + 1} \to \zo$. On any query $i \in [2^{|s|}]$ by $A^1$, $A$ forwards two queries
  to $B_s(\cdot)$, the first being $i$ and the second $i + 2^{|s|}$, outputting two bits, and then
  returns the value $B_s(i) \cdot B_s(i + 2^{|s|})$ as the answer to $A^1$. When $A^1$ is finished,
  $A$ simply returns the result of $A^1$. Then note that if $B_s(\cdot)$ is instantiated with 
  an $f_s$ for random $s$, then $A$ simulates to $A^1$ exactly the conversations that $A^1$ would
  have had with the function $f^1_s$. On the other hand, if $B_s(\cdot)$ is a completely random
  function $r: \zo^{|s|+1} \to \zo$, then the simulation by $A$ is that of a completely random function.
  Therefore:
  ~Math
   |\E_{s \gets_R \zo^{|s|}} [A^{f_s(\cdot)}(1^{|s|+1})] - \E_{H \gets \zo^{|s|+1} \to \zo}[A^{H}](1^{|s|+1}) | < \epsilon
  ~ 
  [&box;]{float=right}
  b. (10 points) $f^2_s(x)=f_s(0\circ x) \circ f_s(x\circ 1)$
  ~Proof
  The construction is not secure. We directly construct an computationally efficient
  adversary $A$ that can distinguish between
  a completely random function $H \gets [2^{|s|}] \to \zo^2$ and a function $f^2_s$ selected 
  from our collection for random $s$. On input $1^{|s|}$, $A$ queries its box $B_s(\cdot)$ with inputs
  $i \in [2^{|s|-1} - 1]$ and $2*i + 1$ to obtain $x = B_s(i)$ and $y = B_s(2*i+1)$. It then
  checks to see if $x_2 = y_1$ (the last bit of the first result matches the first bit of the 
  last result). If that is the case, then it outputs $1$, otherwise $0$. Note that for our given
  construction $f^1$, the last bit of the $f^2(i)$ always matches the first bit of $f^2(2i+1)$. 
  The reason for this is clear. Let $x$ be the bit representation of $i$. Then note that 
  $0 \circ x$ is still just $i$, and $x \circ 1$ is really $i << 1 + 1$ (right shift $i$ and add $1$)
  , which is equivalent to $2*i + 1$. Therefore, $f^2(i) = f(i) \circ f(2i + 1)$ and 
  $f^2(2i + 1) = f(2i + 1) \circ f(4i + 3)$. However, in the case where $B_s(\cdot)$ is a completely 
  random function, then $x_2 = y_1$ with probability $\frac{1}{2}$. Therefore:
  ~
  ~Math
   |\E_{s \gets \zo^{|s|}}[A^{f^2_s(\cdot)}(1^{|s|})] - \E_{H \gets [2^{|s|}] \to \zo^2}[A^H(1^{|s|})] | = |1 - \frac{1}{2}| = \frac{1}{2}
  ~
  which is non-negligible. Furthermore, note that $A$ makes simply two queries to its black box
  and compares two bits, which implies it is therefore computationally efficient. 
  
  
6. (25 points) For the sake of this question, let's say that a pair of algorithms $(S,V)$ is an _enhanced message authentication code_ if it is a secure message authentication code (as per the definition given in the lecture notes) with the following addition--- in the attack game Mallory is given not just oracle (i.e., black box) access to the signing oracle $S$ but also to the verification oracle $V$. That is, Mallory can put forward a pair $(m,\sigma)$ to the oracle and find out whether or not the pair passes verification. Prove that every $(S,V)$ that is a secure message authentication code is also an enhance message authentication code.
~Proof
 We assume a shared private key. 
 We proof by contradiction. Suppose we have a pair of algorithms such that $(S,V)$ is a secure
 message authentication code but is _not_ an enhanced message authentication code. Therefore,
 there exists an adversary $A$ which for random, unknown key $k$
 queries $S_k(\cdot)$ and $V_k(\cdot)$ some polynomial
 number of times and outs the message $(m',\tau')$ un-queried such that $V_k(m',\tau') = 1$. Then we
 can use this adversary to construct and adversary $A'$ with access only to $S_{k'}(\cdot)$
 for some random and unknown key $k'$ as follows.
 The adversary $A'$ simulates $A$. When $A$ queries the signing oracle with message $m$, $A$ forwards
 the message to its own oracle $S_{k'}$ and gives the result to $A$. When $A$ attempts to query
 the verification oracle $V_k(\cdot)$, $A$ takes the input $(m,\tau)$, and obtains the tag
 $\tau_r = S_{k'}(m)$ and then compares $\tau_r$ and $\tau$. If equal, it gives $1$ to $A$. Otherwise,
 it gives $0$ to $A$. Then $A$ outputs whatever $(m',\tau')$ $A'$ comes up at the end of its' 
 computation. Note that the verification procedure carried out by $A'$ is always correct (it
 verifies the message as a true verification procedure would). Therefore, from the perspective of
 $A$, it is interacting with $(S_{k'}, V_{k'})$. Since we assumed that $A$ can break this 
 enhanced security, we know that $V_{k'}(m',\tau') = 1$. However, this breaks the MAC assumption
 of $(S,V)$, a contradiction.
~
[&box;]{float=right}

