% Homework 4

~ MathDefs

\newcommand{\zo}{\{0,1\}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\getsr}{\leftarrow_R\;}
~

### Total of 130 points

The following two questions show that a PRF does _not_ necessarily yield a cryptographic hash function that can be used as a proof of work or provide collision resistance.

1. (15 points) Show (under the PRF conjecture) that there exists a PRF $\{ f_k \}$ mapping $n$ bits to $n$ bits and an efficient algorithm $A$ such that
   $A(k)=x$ such that $f_k(x)=0^\ell$.
   ~Proof
    We beging by constructing an artificially "weak"" PRF $\{f_k \}$ family mapping $n$ bits to $n$
    bits which is nonetheless still a secure PRF. Given an arbitrary PRF $\{g_k \}$ family, define 
    the new PRF $\{f_k \}$ family as $f_k(x) = g_k(x)$ for $x \neq k$ and $f_k(x) = 0^l$ for $x = k$.
    Then intuitively, note that we've changed only a single output for each fixed function $g_k$
    and therefore, because $k$ is unknown to the adversary, then if $g_k(x)$ behaves like a random 
    function, then so does $f_k(x)$. The only input on which it does not behave randomly is $x =k$,
    and so an adversary could distinguish between $g_k$ and $f_k$ if he correctly gives input
    $x = k$, however, given that $k \in_R \zo^n$, the likelihood of this happening is $\frac{1}{2^n}$, 
    which is negligible. Therefore, the PRF $\{f_k \}$ is secure.
    
    However, note that now we can construct an efficient algorithm $A$ with the required properties
    that allows us to find a collision.  Simply define $A(k) = k$, which is constant time. Then it
    is immediately clear that $f'_k(k) = 0^n$. 
   ~

2. (15 points) Show (under the PRF conjecture) that there exists a PRF $\{ f_k \}$ mapping $n$ bits to $n$ bits and an efficient algorithm $A$ such that
   $A(k)=(x,x')$ such that $f_k(x)=f_k(x')$.
   ~Proof
     We take a very similar approach to the above to construct a PRF with the desired properties.
     Given an arbitrary PRF $\{g_k \}$ family, define the PRF $\{f_k \}$ family as follows: $f_k(x) = g_k(x)$
     for $x \neq k$ and $f_k(k) = g_k(k+1)$. Then note that $f_k$ differs from $g_k$ on a single 
     output, and therefore its distribution can only be distinguished from $g_k$ if an adversary
     picks as input either $k$ or $k+1$, which occurs with probability at most $\frac{1}{2^{n-1}}$ 
     because $k \in_R \zo^n$. Therefore, the PRF $\{f_k\}$ is secure if $\{g_k \}$ is secure.
     
     However, we can now construsct the adversary $A(k) = (k, k+1)$ and by construction, it is
     immediately clear that $f_k(k) = g_k(k+1) = f_k(k+1)$. 
   ~


3. (20 points) This question studies the security implications of including a unique "salt" value per user when hashing passwords.
   Suppose that $H$ is a random oracle, and an adversary is given a database $(y_1,\ldots,y_N)$ of the hashes of passwords of $N$ users, with each user choosing their
   password at random from a dictionary $D$. Compute (up to an order of magnitude)
   the expected number of queries (as a function of $N$ and $|D|$)  an adversary needs to make to recover the passwords for all users in the case (a) that the $i^{th}$  entry in the database is simply $H(p_i)$ where $p_i$ is the password of the $i^{th}$ user and the case (b) that the $i^{th}$ entry is $s_i\|H(s_i\|p_i)$ where $s_i$ is a "salt" value chosen at random in $\zo^n$.
    
    a. We make the assumption that each of the $y_i$ are distinct. Note that 
    we can do this without loss of generality since repeated $y_i$ give no additional information
    to the adversary. In the first case, we have $y_i = H(p_i)$. Let $X$ be the expected number
    of queries an 
    adversary needs to recover the passwords for all users in the database. Note that because 
    $H$ is completely random, the adversary has no choice but to linearly search through the
    space $D$ in order to find the passwords. Therefore, we pick some random permutation of $D$
    and select that as the search pattern for the adversary. Then let $X_i$ be the number of
    queries needed to recover $p_i$, and note that $X_i$ is precisely equivalent to the index
    of $p_i$ in the permutation chosen by the adversary for searching. Due to the fact that 
    the passwords are chosen at random, we have $X_i \stackrel{i.i.d}{\sim} \text{DUnif}(1,|D|)$,
    and therefore we have $X = \max_{i=1}^N\{ X_i \}$. Now that we have the above, we wish 
    to calculate $E[X]$. First, recall the CDF for $X_i$:
    ~Math
    \Pr[X_i \leq k ] = \frac{k}{|D|}
    ~
    Then note that we can calculate the PDF of $X$ as follows:
    ~Math
    \begin{aligned}
      \Pr[X \leq k-1] + \Pr[X \geq k+1] + \Pr[X = k] &= 1 \\
      \implies \Pr[X = k] &= 1 - \Pr[X \leq k - 1] - \Pr[X \geq k + 1] \\
      &= \Pr[X \leq k] - \Pr[X \leq k - 1] \\
      &= (\Pr[X_i \leq k])^n - (\Pr[X_i \leq k - 1])^n \\
      &= \frac{k^N - (k-1)^N}{|D|}    
    \end{aligned}
    ~
    Then we can use the definition of expected value to calculate our final result:
    ~Math
    \begin{aligned}
      E[X] &= \sum_{k=1}^{|D|} k \frac{k^N - (k-1)^N}{|D|}   \\
      &= \frac{1}{|D|} \sum_{k=1}^{|D|} k^{N+1} - k(k-1)^N 
    \end{aligned}
    ~
    At this point I'm basically stuck, so I instead I opt for an approximation to the solution by
    converting from discrete to uniform. Let $Y_i \stackrel{i.i.d}{\sim} \text{Unif}(0,|D|)$ and
    create a mapping where $X_i = \lceil Y_i \rceil$ so that we can use experiments on $Y_i$
    to simulate experiments on $X_i$. Then note that 
    $X = \max_i \{X_i \} = \max_i \{ \lceil Y_i \rceil \} = \lceil \max_i \{ Y_i \} \rceil$, so 
    let $Y = \max \{Y_i \}$ and let us explore the properties of $Y$ just like we did with $X$. 
    First, let's calculate the CDF of $Y$ (noting that the CDF for $Y_i$ is $F_{Y_i}(y) = \frac{y}{|D|}$)
    ~Math
    \begin{aligned}
    F_Y(y) &= F_{Y_i}(y)^N \\
    &= \frac{y^n}{|D|^N}
    \end{aligned}
    ~
    Then we can immediately calculate the PDF as $f_Y(y) = \frac{Ny^{N-1}}{|D|^N}$, and armed with
    this result, we can calculate the expected value:
    ~Math
    \begin{aligned}
      E[Y] &= \frac{N}{|D|^N} \int_0^{|D|} y^n \\
      &= \frac{N}{(N+1)|D|^N}y^{n+1} \biggr|_0^{|D|}\\
      &= \frac{N}{N+1}|D|
    \end{aligned}
    ~
    From the above we can deduce that for large $N$:
    ~Math
    E[X] \approx |D| 
    ~
    Therefore we expect the adversary to make approximately $|D|$ queries before get gets all 
    the passwords. 
    
    b. We make the assumption that each of the $y_i$ are distinct, as before. Therefore, we 
    have $y_i = s_i || H(s_i || p_i)$ distinct. Note that given $y_i$, the 
    attacker knows $s_i$ (the salt). However, the beauty of the salt in this case is that 
    because a different salt is used for each user (see note below), then the attacker must
    launch a new attack on each hash. Intuitively, hashes with salt $s_i$ give no information
    about hashes with salt $s_j$ for $i \neq j$. More formally, let $X$ be the number of 
    queries an adversary needs to make to recover the passwords for all users. Then note 
    that $X = \sum_{i=1}^N X_i$ where $X_i$ is the number of queries needed to recover $p_i$ 
    Because each query on a distinct salt is independent of other queries, we have that
    $X_i \stackrel{i.i.d}{\sim} \text{DUnif}(1,|D|)$. We can imagine arranging the set $D$ in 
    some permutation through which the adversary will sequentially guess. Then note that $X_i$
    is equivalent to the index of $p_i$ in this permutation, therefore it is distributed 
    uniformly over the support. Then note that $E[X_i] = \frac{|D| + 1}{2}$ which results in:
    ~Math
    E[X] = \sum_{i=1}^N E[X_i] = \sum_{i=1}^N \frac{|D| + 1}{2} = \frac{N(|D| + 1)}{2} 
    ~ 
    
    
    
    Note that above we made the assumption that the $s_i$ were distinct. If this is not the 
    case, then an attacker can take advantage of the duplicity in seeds to improve his attack 
    (similar to the results in part (a) where now he can attack all hashes which share a salt
    at the same time), which means that technically speaking, the above
    is an upper bound on the number of queries. However, note that if we make the assumption 
    that $N << 2^{n/2}$,
    then by the birthday problem, the likelihood of having a collision in the $s_i$ is 
    negligibly low and it is safe to assume that the $s_i$ are distinct. Furthermore, security
    can be improved by making sure that our system never reuses a seed, even in the case where
    $N >> 2^{n/2}$. A final point, even if the above is not true, note that part (a) gives us 
    a lower bound on the expected number of queries necessariy. Therefore, technically speaking,
    with no assumptions, all we can say is that th expected number of queries $E(X)$ satisfies:
    ~Math
    |D| \leq E(X) \leq \frac{N(|D| + 1)}{2} 
    ~



4. (30 points) Consider the following construction $(S,V)$ for a message authentication code: given some hash function collection $\{ h_k \}$, to sign the message $m$ the signing algorithm  $S_k(m)$ outputs a ```C```  program $P$ that on input $k,m$    outputs the constant $y=h_k(m)$. The verification algorithm $V$ on key $k$ and pair $(m,P)$ outputs $1$ iff $P(k,m)=h_k(m)$.

    a. (15 points) Prove that $(S,V)$ is secure in the random oracle model. That is, prove that with high probability if $H$ is a random function and the function $h_k(m)$ is defined as $H(k\|m)$ then there is no adversary that succeeds in a chosen message attack against $(S,V)$.
    ~Proof
    We proof by contradiction. Suppose such an adversary $A$ exists which after querying the 
    signature oracle $T$ times for some polynomial $T$ can construct a message and signature pair
    $(m',P')$ such that it verifies successfully with some $p(n)$ non-negligible probability. 
    Then we will use this adversary to construct an adversary $A'$ that can predict the output
    of $H$ for some $x'$ unqueried with non-negligible probability. 
    
    The adversary $A'$ has access to a completely random oracle, $H$, and the begins to simulate
    the chosen message attack performed by $A$. When $A$ queries for an signature, $A'$ intercepts
    the message $m$. Using a pre-picked $k \in_R \zo^n$, $A'$ requests the result of $y = H(k || m)$
    and then constructs the trivial C program $P$ which on every input returns $y$. The program
    is then given to $A$. After $T$ such requests, $A$ will produce a tuple $(m',P')$ such that
    for $m'$ unqueried such that $P'$ is a valid tag for $m'$ with non-negligible probability 
    $p(n)$. Then note that $A'$ then runs $P'$ to produce $y' = P'(k,m')$ and proceeds to output
    $(k || m', y')$ which, with high likelihood, is a correct prediction of $H$. 
    Formally:
    ~
    ~Math
    \begin{aligned}
    \Pr[H(k || m') = y'] &= \Pr[h_k(m') = P'(k,m')] \\
    &= \Pr[V_k(m,P) = 1] \\
    &= p(n)
    \end{aligned}
    ~
    which is non-negligible. Therefore, without querying input $x' = k || m'$, $A$ is able to
    predict with non-negligible probability the results of $H$, contradicting the fact that it is
    a random function. Therefore, the adversary $A$ cannot exist and it must be the case that $(S,V)$ is secure in the
    ranodm oracle model. 
    
    b. (15 points) Prove that $(S,V)$ is _insecure_ no matter _what_ hash function collection $\{ h_k \}$ we use as long as the map $(k,m)\mapsto h_k(m)$ is efficiently computable.
    
    This is one example of the potential dangers in the "random oracle heuristic". Stronger examples were given by [this paper of Canneti, Goldreich and Halevi](https://eprint.iacr.org/1998/011.pdf). The conclusions (Section 6) of this paper are particularly worth reading. While the technical contents of the paper are unambiguous, the three authors each had different opinions on their _meaning_ and so each author wrote his own conclusions.

    ~Proof
    We proof by construction. The adversary works as follows. It select random $m'$ and then 
    creates a C program $P'$ which on input $k,m$ uses the key to construct the hash function
    $h_k$ and then computes $h_k(m)$ and returns that value. 
    Note that $P$ is efficiently computable as long as $h_k$
    is efficiently computable. Then note that $P'$ is a universal signature. For any message, it
    is the case that $(m,P')$ will be validated by the verification algorithm since by 
    construction of $P'$, we will have that $P'(k,m) = h_k(m)$. Therefore, the adversary
    succeeds with probability $1$ no matter what has function collection we use as long as
    the map $(k,m) \mapsto h_k(m)$ (ie, constructing the hash function on the fly given a key and 
    message, which is what $P'$ does) is efficiently computable. 
    ~

5. (20 points) This question studies the need to use _min entropy_ as opposed to _Shannon entropy_ in cryptographic applications:

    a. (10 points) Show that there exists a distribution $X$ over $\zo^\ell$ such that $H_{Shannon}(X) \geq \ell/100$ but $H_{\infty}(X) \leq 5$.
    ~Proof
    We show by construction. Take $X$ to be a distribution over $\zo^\ell$ where we choose with
    probability $2^{-c_2}$ some fixed string $x' \in \zo^\ell$ and with probability $1 - 2^{-c_2}$ we chose 
    a string uniformly at random from a subset $S \subset \zo^\ell \setminus \{x'\}$ such that $|S| = 2^{c_1 \ell}$
    for some constants $c_1 \geq 0, c_2 \in [0,1]$. We never chose the strings $x \in (\zo^l \setminus S) \setminus \{x'\}$.
    Furthermore, let us impose the constraint that $2^{-c_2} \geq \frac{1-2^{-c_2}}{|S|} \implies
    2^{-c_2} \geq \frac{1}{1 + |S|} \implies c_2 \leq -\log \frac{1}{1 + |S|} \implies 
    c_2 \leq \log(1 + 2^{c_1 \ell})$.
    
    With the above set, note that the minimum entropy
    of this distribution is precisely $c_2$. More formally:
    ~
    ~Math
    \begin{aligned}
    H_{\infty}(X) &= \min_x \left\{\log \frac{1}{\Pr[X = x]} \right\} \\
    &= \log \frac{1}{\Pr[X = x']} \\
    &= \log 2^{c_2} \\
    &= c_2
    \end{aligned}
    ~
    The key above occurs when calculating the min. At that step, we note that by the constraint
    imposed, the most likely string to occur is precisely $x'$ since it occurs with probability
    $2^{-c_2}$ and all others strings in the support occurs with probability $\frac{1-2^{-c_2}}{|S|}$
    since they are chosen uniformly at random. Now let us calculate the Shannon Entropy of the 
    above distributions:
    ~Math
    \begin{aligned}
    H_{Shannon}(X) &= \sum_{x \in X} \Pr[X = x] \log \frac{1}{\Pr[X = x]} \\
    &= 2^{-c_2}c_2 + \sum_{x \in S}\frac{1-2^{-c_2}}{|S|} \log \frac{|S|}{1 - 2^{-c_2}} \\
    &= 2^{-c_2}c_2 + \frac{1-2^{-c_2}}{|S|} [\log(|S|) - \log (1 - 2^{-c_2})] \sum_{x \in S} 1 \\
    &= 2^{-c_2}c_2 + (1-2^{c_2})(c_1\ell - \log(1 - 2^{-c_2}))
    \end{aligned}
    ~
    Then note that we just need to find $c_1$ and $c_2$ to satisfy the following requirements:
    ~Math
    \begin{aligned}
    1 \leq c_2 &\leq 5 \\
    c_1 &\geq \frac{1}{50}
    \end{aligned}
    ~
   With the above constants, we will have that $H_{\infty}(X) = c_2 \leq 5$ yet:
   ~Math
     \begin{aligned}
     H_{Shannon}(X) &= \frac{1}{2^{c_2}} + (1 - \frac{1}{2^{c_2}})[c_1 \ell - \log [1 - \frac{1}{2^{c_2}}]] \\
     &\geq \frac{1}{2}c_1 \ell \\
     &\geq \frac{1}{100}\ell
      \end{aligned}
   ~  
   as desired. The second line follows from the fac tthat $1- \frac{1}{2^{c_2}}$ is at least 
   $\frac{1}{2}$ and that $c_1\ell - \log[1-\frac{1}{2^{c_2}}]$ is at least $c_1\ell$.
   
    b. (10 points) Show that for _every_ distribution $X \over \zo^{10n}$ with $H_{\infty}(X)\leq 5$ and for _every_ salt value $s\in\zo^n$, and _every_ efficiently computable function $h:\zo^{10n}\rightarrow\zo^n$ there is an efficient attacker $A$ and a pair of messages $m_0,m_1\in\zo^n$ such that given the salt value $s$, $A$ can distinguish between a sample from $h(s\|X)\oplus m_0$ and a sample from $h(s\|X)\oplus m_1$ with advantage at least $1/100$. That is, it is impossible to use a distribution with small min entropy to obtain a secure instantiation of the one time pad.
    ~Proof
    We proof by construction. Suppose we have the stated distribution whose min-entropy is at most
    $5$. First, note that there exists $x'$ in the support such that:
    ~
    ~Math
    \Pr[X = x'] = \frac{1}{2^5}
    ~
    The above follows almost immediately from the definition of min-entropy, since we have:
    ~Math
    \begin{aligned}
    H_{\infty}(X) = \min_x \{\log \frac{1}{\Pr[X = x]} \} = 5 \\
    \implies \exists x' \text{ st. } \log \frac{1}{\Pr[X= x']} = 5 \\
    \implies  \exists x' \text{ st. } \Pr[X = x'] = \frac{1}{2^5}
    \end{aligned}
    ~
    Then we construct the adversary $A$ as follows:
      1. On input $1^n$, the adversary first calculates the value $y = h(s || x')$, which it 
      can do because $s$ is given and $x'$ exists as shown above. Practically, we can imagine
      that $A$ samples the distribution $X$ some number of times in order to obtain the value
      $x'$ which occurs the most often. We need only do this if $A$ does not have a description
      of the distribution $X$. Note that in either scenario, given that $x'$ occurs with 
      probability $\frac{1}{2^5}$, we can pre-compute the value $x'$ with extremely high 
      probabiltiy in a constant number of cycles. Therefore, we ignore this problem and 
      simply assume that $A$ knows $x'$. 
      2. The adversary then selects the messages $m_0  = y$ and $m_1 = y \oplus 1^n$ on which to
      be tested. 
      3. On obtaining $c = h(s || X) \oplus m_b$, $A$ does the following. If $c = 0^n$, it outputs
      $0$. If $c = 1^n$, it outputs $1$. Otherwise, it outputs $b' \in_R \zo$. 
      
    We now show that $A$ will guess $b$ successfully with high probability. There are two 
    scenarios to consider, depending on whether $X = x'$ or $X \neq x'$. 
      1. In the case where $X = x'$, the adversary always guesses correctly. This is because
      $c = 0^n$ iff $m_b = m_0$ and $c = 1^n$ iff and $m_b = m_1$. 
      2. In the case where $X \neq x'$, we can essentially consider $h(s || x)$ to be uniformly
      distributed. Therefore $c$ is uniformly distributed, and the adversary guesses 
      correctly with probability $\frac{1}{2} + negl(n)$. 
      
    Putting the two cases above together, we have that the probability of success is:
    ~Math
    \begin{aligned}
    \Pr[b' = b] &= \Pr[b' = b \mid X = x']\Pr[X = x'] + \Pr[b' = b \mid X \neq x']\Pr[X \neq x'] \\
    &= (1)\frac{1}{2^5} + [\frac{1}{2} + negl(n)]\frac{2^5 - 1}{2^5} \\
    &= \frac{1}{2} + \frac{2^5 - 1}{2^5}negl(n) + \frac{1}{2^6} \\
    &= \frac{1}{2} + p(n)
    \end{aligned}
    ~
    where $p(n)$ is non-negligible since it is at least the constant value $\frac{1}{2^6}$. Therefore
    the adversary succeeds in distinguishing between the two messages with non-negligiblel 
    probability, so it successfully distinguishes between a sample from $h(s|| X) \oplus m_0$ and
    $h(s || X) \oplus m_1$.     


6. (30 points) We now show a few properties of min entropy, many of those hold for other entropies as well:

    a. (10 points) Show that the min entropy function is _concave_: that is for every two distributions $X$ and $Y$ and $p\in [0,1]$, $H_{\infty}(pX+(1-p)Y)\geq pH_{\infty}(X)+(1-p)H_{\infty}(Y)$, where the distribution $pX+(1-p)Y$ is obtained by taking this combination of the vectors of probabilities or, equivalently, choosing with probability $p$ to sample from $X$ and with probability $1-p$ to sample from $Y$. Note that this makes intuitive sense since if $X$ and $Y$ have a certain amount of uncertainty, we introduce only more uncertainty by choosing which one of them to sample from at random.
    ~Proof
    We can show this almost directly using the definition of min-entropy.
    ~
    b. (10 points) We have mentioned that in practical applications sometimes  operating systems accumulates an _entropy pool_ before refreshing the generator state. One approach is to simply XOR the hash of the new measurements into the old pool.
     This can sometimes be problematic but let us show that under the (not always realistic) assumption of _independence_ it at least does not hurt: show that for every two distributions $X$ and $Y$ over $\zo^n$, $H_{\infty}(X \oplus Y) \geq \max \{ H_{\infty}(X) , H_{\infty}(Y) \}$ where $X\oplus Y$ denotes the distribution obtained by picking independently $x\getsr X$ and $y\getsr Y$ and outputting $x\oplus y$.
   ~Proof
   We prove directly, Let us consider the min-entropy of the $X \oplus Y$ distribution. We carefully
   simplify each step:
   ~
   ~Math
    \begin{aligned}
    H_{\infty}(X \oplus Y) &= \min_z \left\{\log \frac{1}{\Pr[X \oplus Y = z]} \right\} \\
    &= \log \min_z \left\{ \frac{1}{\Pr[X \oplus Y = z]}\right\} \\
    &= \log \frac{1}{\max_z \left\{ \Pr[X \oplus Y = z]\right\}}
    \end{aligned}
   ~
   by a similar line of reasoning, we also have that:
   ~Math
    H_{\infty}(X) = \log \frac{1}{\max_x \left\{ \Pr[X = x] \right\}}
   ~
   Therefore, if we seek to prove that $H_{\infty}(X \oplus Y) \geq H_{\infty}(X)$, we must show
   that $\max_z \left\{ \Pr[X \oplus Y = z]\right\} \leq  \max_x \left\{ \Pr[X = x] \right\}$. We
   can do this easily by conditioning on $Y$:
   ~Math
    \begin{aligned}
      \max_z \left\{ \Pr[X \oplus Y = z]\right\} &= \max_z \left\{ \sum_y \Pr[X \oplus y = z \mid Y=y]\Pr[Y = y]\right\} \\
      &= \max_z \left\{ \sum_y \Pr[X = z \oplus y \mid Y=y]\Pr[Y = y]\right\} \\
      &\leq \max_z \left\{ \sum_y  \max_x \left\{ \Pr[X = x] \right\} \Pr[Y = y]\right\} \\
      &= \max_x \left\{ \Pr[X = x] \right\} \max_z \left\{ \sum_y \Pr[Y = y]\right\} \\
      &= \max_x \left\{ \Pr[X = x] \right\}
    \end{aligned} 
   ~
   and therefore we conclude that $H_{\infty}(X \oplus Y) \geq H_{\infty}(X)$. A simpler line of 
   reasoning, though less techincal, is that $H_{\infty}(X \oplus Y) \geq H_{\infty}(X \oplus Y \mid Y) = H_{\infty}(X)$
   where the first inequality follows from the fact that we're given more information and the second
   inequality from the fact that given $Y$, the only unknown is $X$. With either explanation,
   an exactly symetric argument for $Y$ will show that:
   ~Math
     \begin{aligned}
      H_{\infty}(X \oplus Y) &\geq H_{\infty}(X) \\
      H_{\infty}(X \oplus Y) &\geq H_{\infty}(Y) \\
      \implies H_{\infty}(X \oplus Y) &\geq \max \{ H_{\infty}(X), H_{\infty}(Y)\}
     \end{aligned}
   ~
   as desired. 
    c. (10 points) Give an example of two distributions $X$ and $Y$ over $\zo^n$ with min entropy $n/2$ such that $H_{\infty}(X\oplus Y)=n/2$.
    
    Let $X$ be the distribution that selects the string 