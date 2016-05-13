~ MathDefs
\newcommand{\zo}{\{0,1\}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\getsr}{\leftarrow_R\;}
~

# Homework 3- CPA and CCA security

Total of 125 points.

1. (60 points) In the following questions, $\{ p_k \}$ denotes a pseudorandom permutation collection where for
every $k \in \zo^n$, $p_k$ is a permutation on $n$ bits. For each one of the following schemes for 
encrypting $2n$ bits, say whether it is _necessarily_ CPA secure, _necessarily_ not CPA secure, 
or the answer depends on the particular choice of $\{ p_k \}$. Prove your assertions.

    a. (ECB Mode, 15 points): $E(m_1,m_2)= p_k(m_1)\|p_k(m_2)$ (as usual $\|$ denotes concatenation[^concat])
    ~Proof
    The scheme is _necessarily_ not CPA secure. Note that the scheme is deterministic, 
    therefore by the material covered in Lecture 5, it _cannot_ be CPA secure. Note that all 
    pseudorandom permutation collections are deterministic, therefore there is no ifs ands or buts here.
    Paraphrasing the proof from lecture, we can show its insecurity by constructing the adversary Eve
    which plays the CPA game as follows. It first sends as input the message $m_0 = 0^{2n}$ to 
    receive the ciphertext $c_0 = E_k(m_0)$. It then choses$m_0 = 0^{2n}$ and $m_1 = 1^{2n}$ to 
    receive the encryption $c^* = E_k(m_b)$. Eve then outputs $0$ if and only if $c* = c_0$ otherwise $1$. Then note
    that she is always correct. 
    ~
    b. (CTR Mode, 15 points): $E(m_1,m_2) = IV\|y_1\|y_2$ where $IV$ is chosen at random in $\zo^n$,  and $y_i=p_k(IV+i) \oplus m_i$ where addition is done modulo $2^n$.
    ~Proof
    This scheme is _necessarily_ secure. We follow a proof which is inspired by that presented in "Introduction
    to Modern Cryptography" in page 113. We first show that the encryption scheme is CPA secure if we
    use a truly random permutation as opposed to one selected from our collection. In other words, we use
    $p_R: \zo^{2n} \to \zo^{2n}$ where $p_R$ is invertible (we do not particularly care about it being efficiently
    invertible) as opposed to $p_k$. Then consider the adversary $Eve_{\Pi}$ which plays the CPA game 
    upto $T(n)$ times where $T$ is polynomial (given input $1^{2n}$) with the scheme $\Pi$. Note
    that $\Pi$ is the schme described in the question, and we let $\Pi_R$ be the same schem with $p_R$.
    Then note that this adversary cannot differentiate between the truly random encryption scheme 
    $\Pi_R$ and our encryption scheme $\Pi$ with more than negligible probability. 
    That is to say:
    ~
    ~Math
      |\Pr[Eve_{\Pi_R}(1^{2n}) = 1] - \Pr[Eve_{\Pi}(1^{2n})] | < \epsilon(n)
    ~
    for some negligible $\epsilon$. We proof this by contradiction. Suppose such an adversary 
    $Eve$ exists. Then we can use $Eve$ to construct a distinguisher $D$ which differentiates between 
    a pseudorandom permutation and a completely random permutation. Give $D$ inputs $1^n$ and access 
    to oracle $\mathcal{O}: \zo^n \to \zo^n$. $D$ will output $1$ if it believes $\mathcal{O}$ to be a 
    pseudorandom function and $0$ otherwise. It proceeds as follows:
      
      1. Simulate the adversary $Eve$ internally by feeding it input $1^{2n}$. 
      When $Eve$ makes a query to it's oracle with a message $m_1 || m_2 \in \zo^{2n}$, 
      choose uniformly at random an $IV$ and query $y_i = \mathcal{O}(IV + i)$ for 
      $i \in \{1,2\}$ (addition done modulo $2^n$) and return the cipher text $IV || y_1 || y_2$. 
      2. When $Eve$ requests to be tested with messages $m_0,m_1 \in \zo^{2n}$, choose a uniform bit
      $b \in \zo$ and then repeat the process from (1) above with the message $m_b$.
      3. Continue to answer such queries from $Eve$ until $Eve$ outputs the bit $b'$. Output $1$ if $b' = b$
      and output $0$ otherwise.
      
    Note that $D$ runs in polynomial time since $Eve$ runs in $T(n)$ time. Then note that in the above,
    we are simulating exactly what $Eve$ expects. If the oracle $\mathcal{O}$ is a completely random
    function, then the distribution of Eve is equivalent to our distinguisher
    ~Math
      \E[Eve_{\Pi_R}(1^{2n})] = \E[D^{p(\cdot)}(1^n)]
    ~
    and similary, if the $\mathcal{O}$ is a pseudorandom function $p_s: \zo^n \to \zo^n$:
    ~Math
      \E[Eve_{\Pi}(1^{2n})] = \E_{s \gets \zo^n}[D^{p_s(\cdot)}(1^n)]
    ~
    Combining the above equations:
    ~Math
      |\E[D^{p(\cdot)}(1^n)] -  \E_{s \gets \zo^n}[D^{p_s(\cdot)}(1^n)] | = |\E[Eve_{\Pi_R}(1^n)] - \E[Eve_{\Pi}(1^n)] | > p(n)
    ~
    where $p(n)$ is non-negligible. This is a contradiction on $\{f_s\}$ being a pseudorandom 
    permutation, therefore our assumption that $Eve$ exists must be false. 
    
    Next, we show that any adversary $Eve'$ that makes $T(n)$ queries to a completely random oracle
    cannot win at the CPA game with proability greater than $\frac{1}{2} + \frac{4T(n)}{2^n}$.
    ~Math
      \E[Eve'_{\Pi_R}(1^{2n})] = \frac{1}{2} + \frac{4T(n)}{2^n}
    ~
    which, combined with the previous result, will give us:
    ~Math
    \begin{aligned}
      \E[Eve'_{\Pi}(1^{2n})] &= |\E[Eve'_{\Pi}(1^{2n})]  - \E[Eve'_{\Pi_R}(1^{2n})] + \E[Eve'_{\Pi_R}(1^{2n})]  | \\
      &\leq \frac{1}{2} + \frac{4T(n)}{2} + \epsilon(n)
    \end{aligned}
    ~
    Since $T(n)$ is a polynomial, the additonal factor is negligible, and therefore we
    complete the proof if we can show our previous claim. Let $IV^*$ be the random value used in
    computing the challange ciphertext. Then let $IV_i$ be the be the random value used in 
    computing the ciphertext for the $i$-th oracle query. Note that on the $i$-th query, we apply
    $p_k$ to $IV_i + 1$ and $IV_i + 2$ and XOR each with the respective message. Similarly, the challenge
    ciphertext applies $p_k$ to $IV^* + 1$ and $IV^*$ + 2. Then we have two cases to consider:
    
      1. There do not exists $i,j,j^*$ for which $IV_i + j = IV^* + j^*$. In this case, the inputs to
      $p_k$ on each use are distributed uniformly, and therefore $Eve'$ gains no information from its 
      previous queries. The result is that $Eve'$ has exactly $\frac{1}{2}$ probability of guessing 
      the correct decryption.
      2. There _do_ exists $i,j,j^*$ for which $IV_i + j = IV^* + j^*$ (ie, there's an overlap in the 
      inputs to $p_k$ by luck). In this case, we can image that $Eve'$ can use the results from the 
      overlapping oracle query to have a good chance at guessing which of the two challenge messages
      has been encrypted. Therefore, we need to analyze the probability that this event will occur. By a 
      union bound, we can calculate the probability of an overlap by summing the probabilities of on
      overlap on the $i$-th oracle query.
      ~Math
        \Pr[Overlap] = \sum_{i=1}^{T(n)} \Pr[Overlap_i]
      ~
      Let us fix $IV^*$. Then an overlap on the $i$-th query occurs if and only if 
      $IV^* - 2 \leq IV_i \leq IV^* + 1$. Given that $IV_i$ is chosen uniformly from $\zo^n$ and there
      are only $4$ values that satisfy the above, we have $\Pr[Overlap_i] = \frac{4}{2^n}$. This gives us:
      ~Math
      \Pr[Overlap] = \frac{4T(n)}{2^n}
      ~
      With the above, we can bound the success probability directly as:
      ~Math
      \E[Eve'_{\Pi_R}(1^{2n})] = \frac{1}{2} + \frac{4T(n)}{2^n}
      ~
      This proves our claim and completes the overall proof. 
    
    c. (CTR' Mode, 15 points): $E(m_1,m_2) = IV\|y_1\|y_2$ where $IV$ is chosen at random in $\zo^n$,  and $y_i = p_k(IV+i+m_i)$ where addition is done modulo $2^n$.
    ~Proof
    This scheme is _necessarily_ not CPA secure. We show this by constructing an adversary $Eve$ who can
    win the CPA game with probability $\frac{1}{2} + p(n)$ where $p$ is not negligible. On input $1^{2n}$, 
    $Eve$ does the following:
    
      1. Select as challange messages $m_0 = 0^{2n}$ and $m_1 = 0^{n-1}||1||0^n$. 
      2. On receiving the ciphertext $c^*$, if the last two $n$-length chunks are the same, Eve outputs $1$.
        Otherwise, it outputs $b \in \zo$ uniformly at random.
    
    Now we analyze the probability of $Eve$ correctly guessing the input message. We do this by considering
    two cases, when the challange ciphertext corresponds to $m_0$ and when it corresponds to $m_1$.
    
      1. Suppose $c^* = E(m_0) = E(0^n, 0^n) = IV^* || p_k(1) || p_k(2)$. Note that $p_k$ is a one-to-one 
      function, therefore the $p_k(1) = p_k(2)$ if and only if $n = 1$ (becase then we mod by $2$). Let us
      assume that $n > 1$. In that case, $p_k(1) \neq p_k(2)$. Therefore $Eve$ will be correct (output $0$)
      with probability $\frac{1}{2}$. 
      2. Suppose $c^* = E(m_1) = E(0^{n-1}|| 1|| 0^{n}) = IV^* || p_k(2) || p_k(2)$. Note that in this 
      case, $Eve$ will always output $1$. Therefore, $Eve$ succeed with probability $\frac{1}{2}$.
      
   Then combining the above, the success probability is $\frac{1}{2} + \frac{1}{4} = \frac{3}{4}$ which
   is non-negligibly above $\frac{1}{2}$. Therefore, the scheme is not CPA secure.
    ~

    d. (CBC Mode with random $IV$, 15 points): $E(m_1,m_2)=IV\|y_1\|y_2$ where $IV$ is chosen at random in $\zo^n$, $y_1 = p_k(m_1 \oplus IV)$ and $y_2 = p_k(m_2 \oplus y_1)$.
    ~Proof
    This scheme is necessarily secure. We first show that the scheme is CPA secure if we use a truly
    random pernumation as opposed to one selected from our collection. Consider the adversary
    $Eve_{\Pi}$ which plays the CPA game and queries the oracle up to $T(n)$ times where $T$ is polynomial
    in $1^{2n}$ with our scheme $\Pi$ (as described in the problem statement).
    Let $\Pi_R$ be the scheme equivalent to the previous but with a truly random permutation. Then note
    that the adversary cannot differentiate between the truly random encryption scheme $\Pi_R$ and 
    our encryption schme $\Pi$ with more than negligible probability. That is to say:
    ~
    ~Math
     |\E[Eve_{\Pi_R}(1^{2n})] - \E[Eve_{\Pi}(1^{2n})] | < \epsilon(n)
    ~
    for some negligible $\epsilon$. We proof by contradiction. Suppose an adversary $Eve$ exists where
    it can distinguish with some non-negligible probability $p(n)$. Then we can use $Eve$ to construct
    a distinguisher $D$ between pseudorandom and completely random permutations. Give $D$ inputs
    $1^n$ and access to oracle $\mathcal{O}: \zo^n \to \zo^n$. $D$ will output $1$ if it believes that
    $\mathcal{O}$ is pseudorandom and $0$ otherwise. It proceeds as follows:
      
      1. Simulate the adversary $Eve$ internally by feeding input $1^{2n}$. When $Eve$ makes a query
      to a it's oracle with a message $m_1 || m_1 \in \zo^{2n}$, choose uniformly at random $IV$ and
      query $y_i = \mathcal{O}(y_{i-1} \oplus m_i)$ where we define $y_0 = IV$, and return the ciphertext
      $IV || y_1 || \cdots || y_i$.
      2. When $Eve$ requests to be tested with messages $m_0,m_1 \in \zo^{2n}$, chose a uniform bit
      $b \in \zo$ and then repeat the process from $1$ above, on messages $m_b$.
      3. Continue to answer queries until $Eve$ returns $b'$. Output $1$ if $b' = b$ and $0$ otherwise.
      
    Note that $D$ runs in poly-time. Then we note that the above simulates exactly what $Eve$ expects to
    receive so that we have:
    ~Math
    \begin{aligned}
    \E[Eve_{\Pi_r}(1^{2n})] &= \E[D^{p(\cdot)(1^n)}] \\
    \E[Eve_{\Pi}(1^{2n})] &= \E_{s \gets \zo^n} [D^{p_s(\cdot)}(1^n)]
    \end{aligned}
    ~
    We combine the equations to receive:
    ~Math
      |\E[D^{p(\cdot)}(1^n)] -  \E_{s \gets \zo^n}[D^{p_s(\cdot)}(1^n)] | = |\E[Eve_{\Pi_R}(1^n)] - \E[Eve_{\Pi}(1^n)] | > p(n)
    ~
    where $p(n)$ is non-negligible. This is a contradiction of $\{p_s\}$ being a pseudorandom 
    permutation, therefore our assumption that $Eve$ exists must be false. 
    
    Now we show that any adversary $Eve'$ that makes $T(n)$ queries to a completely random oracle cannot
    win the CPA game with probability greater than $\frac{1}{2} + \frac{4 T(n)}{2^n}$. Let $IV^*$ be the
    random seed used for the challange ciphertext, and let $IV_i$ be the random seed used for the 
    encryption process during the $i$-th query to the oracle. Note that on the $i$-th query, we apply
    $p_k$ to $IV_i \oplus m^i_1$ and to $p_k(IV_i \oplus m^i_1) \oplus m_2$. Similarly, the challenge ciphertext
    applies $p_k$ to $IV^* \oplus m_1^*$ and to $p_k(IV^* \oplus m_1^*) \oplus m_2^*$. Then we have two cases
    to consider:
    
      1. There does not exist $i, m^i_1, m_1^*, m^i_2$, $m_2^*$ for which $IV_i \oplus m^i_1 = IV^* \oplus m_1^*$
      or $p_k(IV_i \oplus m^i_1) \oplus m^i_2 = p_k(IV^* \oplus m_1^*) \oplus m_2^*$ or $IV_i \oplus m^i_1 = 
      p_k(IV^* \oplus m^i_1) \oplus m_2^*$ or $IV^* \oplus m_1^* = p_k(IV_I \oplus m^i_1) \oplus m^i_2$. In this
      case, the inputs to $p_k$ on each use are distributed uniformly, and therefore $Eve'$ gains no 
      information from its previous queries. The result is that $Eve'$ has exactly $\frac{1}{2}$ probability
      of guessing the correct decryption for the challange message.
      2. There do exist $i,m^i_1,m_1^*,m^i_2,m_2^*$ for which at least one of the previous is true. In 
      this case, we can imagine that $Eve'$ will use this information to its advantage. Therefore, we 
      need to analyze the probability that something like the above occurs. Let's fix $i$, $IV^*$, $m_1^*$
      and $m_2^*$, and consider the $i$-th oracle input. Then a collision occurs if and only if
         
         a. $IV_i \oplus m^i_1 = IV^* \oplus m_1^*$.
         b. $p_k(IV_i \oplus m^i_1) \oplus m^i_2 = p_k(IV^* \oplus m_1^*) \oplus m_2^*$
         b. $IV_i \oplus m^i_1 = p_k(IV^* \oplus m_1^*) \oplus m_2^*$
         c. $p_k(IV_i \oplus m^i_1) \oplus m^i_2  = IV^* \oplus m_2^*$

      For each of the above cases, we have the following:
        
        a. Given that the RHS is fixed, there is only one string $IV_i \oplus m_0$ for which this holds.
        Since $IV_i$ is chosen uniformly at random, there's a $\frac{1}{2^n}$ probability of this occurring.
        b. Again, the RHS is fixed. Then noting that $p_k$ is one-to-one and that $IV_i$ is chosen uniformly,
          there is again a $\frac{1}{2^n}$ chance of this occuring.
        c. Again, the RHS is fixed. Then by the randomness of $IV_i$, the probability if $\frac{1}{2^n}.
        d. Similar to the above, with probability $\frac{1}{2^n}$. 
        
    From the above, for fixed $i$, we can bound the probability of collision at $\frac{4}{2^n}$. Then
    using the union bound:
    ~Math
      \Pr[Collision] = \sum_{i=1}^{T(n)} \Pr[Collision_i] = \frac{4T(n)}{2^n}
    ~
    With the above bound, we have the success probability as:
    ~Math
      \E[Eve'_{\Pi_R}(1^{2n})] = \frac{1}{2} + \frac{4T(n)}{2^n}
    ~
    Then we can use the triangle inequality and the above two results to calculate the expected success
    of $Eve'$:
    ~Math
    \begin{aligned}
      \E[Eve_{\Pi}(1^{2n})] &= |\E[Eve_{\Pi}(1^{2n})] - \E[Eve_{\Pi_R}(1^{2n})] + \E[Eve_{\Pi_R}(1^{2n})]| \\
      &\leq |\E[Eve_{\Pi}(1^{2n})] - \E[Eve_{\Pi_R}(1^{2n})]| + \E[Eve_{\Pi_R}(1^{2n})] \\
      &= \frac{1}{2} + \frac{4T(n)}{2^n} +  \epsilon(n)
    \end{aligned}
    ~
    Since $T(n)$ is polynomial, the additional factor is negligible, and therefore we've complted the proof
    showing that $Eve'$ gains no significant advantage under this scheme, and therefore the scheme is
    secure. 

[^concat]: Throughout this homework assignment we'll assume that if the lengths of the different parts are not known then we use some encoding to mark the point of concatenation so it's possible to parse the different parts.

2. (40 points) In the following questions, $(S,V)$ is a secure MAC with $n$ bit keys and messages in $\zo^*$, $(E,D)$ is a CPA-secure encryption scheme with $n$ bit keys and messages in $\zo^*$.
For each one of the following schemes $(E',D')$ for encrypting an $n$ bit message $m$, say whether it is _necessarily_ CCA secure, _necessarily_ not CCA secure, or the answer depends on the particular choice of the primitives. Prove your assertions.

    a. (10 points) $E'_{k_1,k_2}(m) = E_{k_1}(m\|S_{k_2}(m))$. $D'_{k_1,k_2}(c)$ is obtained by letting $m\|\sigma=D_{k_1}(c)$ and outputting $m$ if $V(m,\sigma)=1$ and ```error```  otherwise.
    ~Proof
    The scheme is secure depending on the particular choice of the primitives. In particular, we show that
    the scheme is insecure if $E_{k_1}(m || \sigma )$ is a CTR mode cipher performed on $F(m || \sigma)$ where
    $F$ is defined as the bitwise function which maps $0 \to 00$ and $1 \to 01$ or $1 \to 10$ with equal
    probabilities. Note that CTR schemes are secure, therefore the previous is secure. Let $S_{k_2}$ be
    a standard MAC secure verification such that $\sigma = S_{k_2}(m)$.
    
    More formally, we have an adversary $M$ which can win the CCA game with probability $\frac{1}{2} + p(n)$
    for non-negligible $p(n)$. $M$ does the following:
    
      1. On input $1^n$, it selects random $m_0 = 0^*$ and $m_1 = 1^*$ as challange texts. 
      2. After receiving the ciphertext $c^* = CTR(F(m_b || S_{k_2}(m_b)))$, $M$ will take $c^*$ and 
      flip two bits. It will flip the two bits that follow the $IV$ (we assume that we know the length
      of $IV$).
      3. We now have two cases to consider. If the decryption oracle successfully returns $m$, then
      $M$ simply sees if $m$ is all $0$s or all $1$s and returns the corresponding $b$ for $m_b$. 
      4. In the case where the oracle returns an error, $M$ return $0$ (the message is the all zeros
      message)
      
    Note that by the above, $M$ always succeeds. To see why, consider the two cases. 
    
      1. If the encrypted 
      message was $m_0 = 0^*$, then the decryption oracle first undoes CTR. Note that $c^* = IV || y_1 || \cdots || y_t$
      where $y_i = p_k(IV + i) \oplus F(m^i_0)$ (where we break up $F(m_0)$ into $t$ chunks). Then
      what does flipping the first two bits of $y_1$ do? Well, given that $p_k(IV + 1)$ remains the
      same, flipping the first two bits means that the first two bits of $F(m^1_0)$ are flipped. Then
      note that because the original message was $0^n$, the first two bits were $00\cdots$ and have now become
      $11\cdots$. However, the decryption system will now fail when it attempts to undo the $F$ transformation,
      and so the oracle returns an error, and $M$ succeeds.
      2. In the encrypted message was $m_1 = 1^n$, then the decryption oracle first undoes CTR. By
      similar logic to the above, flipping the first two bits after the IV leads to flipping the first two
      bits of $F(m^1_1)$.  Then note that because the orignal message was $1^n$, the first two bits have
      now changed from $01 \to 10$ or from $10 \to 01$, both of which correctly map to $1$. Therefore,
      the original message is left unchanged. In this scenario, the decryption oracle will successfully 
      decrypt the message $m$ and the integrity check will succeed. Therefore, $M$ will win at the 
      game.
      
    By the above, we now have a specific construction which leads to a non CCA secure $(E',D')$ scheme.
    
    The scheme can be secure for other primitives that don't allow for the tweaking of the ciphertext
    in the way we've tweaked it. We proof this by contradiction. Suppose that there exists an
    adversary $M'$ that wins the CCA game with (E',D') with probability at least $\frac{1}{2} + \epsilon$
    for some non-negligible $\epsilon$. We consider the following two cases:
    
      1. With probability at least $\epsilon /10$, at some point during the CCA game, $M'$ sends the 
      decryption box a $$ that is not idential to one of the ciphertexts it previously 
      obtained from it's encryption box, and obtains from it a non-error response. 
      2. The event above happens with probability less that $\epsilon /10$.
      
    We will derive a contradiction in either case. In the first case, we will use $M'$ to obtain
    a forger $F$ for $(S,V)$ and in the second case we will use $M'$ to obtain an $Eve$ for $(E,D)$, 
    contradicting the security of $(S,V)$ and $(E,D)$ respetively. 
    
    Let's start with case $1$. As per a previous exercise, $F$ has access to both an signing and a 
    verification oracle for some fixed, unknown $k_2$. $F$ will randomly select $k_1$ and a value
    $i \in \{1, \cdots T(n)\}$ where $T(n)$ is the number of queries made by $M'$ to its decryption box.
    $F$ then simulates the CCA games with $M'$ which is to say that when $M'$ asks for the encryption
    of message $m$, $F$ will return $E_{k_1}(m || S_{k_2}(m))$, where it uses the oracle to calculate
    the signature for $m$ and explicitly calculate the encryption, given that we know $k_1$. 
    Similarly, when $M'$ asks for a decryption
    of $c$, $F$ decrypts it first explicitly using the picked key $k_1$ to obtain $m || \sigma$,
    and then verifies $\sigma$ by checking
    with the verification oracle that it is a valid signature for the decrypted message $m$. It then either
    returns error or $m$ depending on whether the verification succeeded. 
    This continues unitl the $i$-th query $M$ makes to its decryption box, $c$
    , at which point $F$ explicitly decrypts $c$ as $m || \sigma$ and outputs $(m,\sigma)$. 
    Now because we are in the first case, the probability 
    that this message is a message not queried by $M'$ before but which passes the verification 
    algorithm is $\frac{1}{10 \epsilon T(n)}$. Hence, with at least this probability, $F$ succeeds in 
    creating a message $(m,\sigma)$ which passes the verification process, a contradiction
    on the MAC security of $(S,V)$.
    
    Now for the second case. We construct $Eve$ which choses a random key $k_2$, and then plays
    the CCA game internally with access to an encryption oracle for $E_{k_1}$. 
    When $M'$ makes an encryption query, $Eve$ will first explicitly obtain the signature
    $\sigma = S_{k_2}(m)$ and then forward $m || \sigma$ to its encryption oracle, returning whatever
    encryption $c$ it provides. When $M'$ makes a decryption query, Eve will just wing it. When receiving
    the decryption query $c$, Eve will check to see if it is the encryption of some previous
    plaintext query encryption $m$, and if so, return $m$. Note that we are implicitly taking
    advantage of the fact that we can re-order the queries made by $M'$ so that all encryption queries
    occur before all decryption queries. In the scenario where $Eve$ receives
    a query $c$ which is never encrypted by $M'$, note that $Eve$ simply returns error. Given
    that we are in case 2, $Eve$ is wrong (she returns error when she shouldn't) with probability
    at most $\epsilon/10$. Hence, we still get an overall success of $\frac{1}{2} + \frac{9}{10}\epsilon$,
    contradicting the CPA security of $(E,D)$.
    ~

    b. (10 points) $E'_{k_1,k_2}(m)= E_{k_1}(m)\|S_{k_2}(m)$. $D'_{k_1,k_2}(c\|\sigma)$ is obtained by letting $m = D_{k_1}(c)$ and outputting $m$ if $V_(m,\sigma)=1$ and ```error``` otherwise.
    ~Proof
    The scheme is secure depending on the particular choice of the primitives. In particular, we show that
    the scheme is insecure if $E_{k_1}$ is a CPA secure system and $S_k(m) = m|| S'_k(m)$ where $S'_k(m)$ is 
    a secure MAC. Note that $S_k(m)$ is a secure MAC if $S'_k$ is a secure MAC. However, under this 
    mechanism, note that $(E',D')$ is not CCA secure because the plaintext message is revealed by $S_k$.
    
    More formally, we have an adversary $M'$ which can win the CCA game with probability $\frac{1}{2} + p(n)$
    for non-negligible $p(n)$. $M'$ does the following:
    
      1. On input $1^n$, it selects random $m_0,m_1 \in \zo^*$ as challange texts. 
      2. After receiving the ciphertext $c^*$, Eve simply parses it as $c^* = E_{k_1}(m) || m || S'_k(m)$.
      3. $M'$ then returns whichever $b \in \zo$ for whichever $m_b = m$. 
      
    Note that $M'$ always wins this game. 
    The above shows that the scheme is insecure under this scenario. However, it is secure in any
    scenario where $S_k$ _does not_ leak information about the message itself. Assuming that $S_k$ is
    such an MAC, we proof our claim by contradiction. Suppose that there exists an
    adversary $M'$ that wins the CCA game with (E',D') with probability at least $\frac{1}{2} + \epsilon$
    for some non-negligible $\epsilon$. We consider the following two cases:
    
      1. With probability at least $\epsilon /10$, at some point during the CCA game, $M'$ sends the 
      decryption box a $(c,\sigma)$ that is not identical to one of the ciphertexts it previously 
      obtained from it's encryption box, and obtains from it a non-error response. 
      2. The event above happens with probability less that $\epsilon /10$.
      
    We will derive a contradiction in either case. In the first case, we will use $M'$ to obtain
    a forger $F$ for $(S,V)$ and in the second case we will use $M'$ to obtain an $Eve$ for $(E,D)$.
    
    Let's start with case $1$. As per a previous exercise, $F$ has access to both an signing and a 
    verification oracle for some fixed, unknown $k_2$. $F$ will randomly select $k_1$ and a value
    $i \in \{1, \cdots T(n)\}$ where $T(n)$ is the number of queries made by $M'$ to its decryption box.
    $F$ then simulates the CCA games with $M'$ which is to say that when $M'$ asks for the encryption
    of message $m$, $F$ will return $E_{k_1}(m) || S_{k_2}(m)$ where the first part is calculated
    explicitly and the second using the signing oracle. Similarly, when $M'$ asks for a decryption
    of $c || \sigma$, $F$ decrypts the first part explicitly and then verifies $\sigma$ by checking
    with the verification oracle that it is a valid signature for the decrypted message. It then either
    returns error or $m$. This continues unitl the $i$-th query $M$ makes to its decryption box, $(c,\sigma)$
    , at which point $F$ explicitly decrypts $c$ and outputs $(m,\sigma)$. Now because we are in the first case, the probability 
    that this message is a message not queried by $M'$ before but which passes the verification 
    algorithm is $\frac{1}{10\epsilon T(n)}$. Hence, with at least this probability, $F$ succeeds in 
    creating a message $(m,\sigma)$ which passes the verification process, a contradiction
    on the security of $(S,V)$.
    
    Now for the second case. We construct $Eve$ which choses a random key $k_2$, and then plays
    the CCA game internally. When $M'$ makes an encryption query, $Eve$ will forward the plaintext
    $m$ to its encryption oracle and use $k_2$ to explicitly construct the signature $\sigma = S_{k_2}(m)$,
    returnign $c || \sigma$. When $M'$ makes a decryption query, Eve will just wing it. When receiving
    the decryption query $(c,\sigma)$, Eve will check to see if it is the encryption of some previous
    plaintext query encryption $m$, and if so, return $m$. Note that we implicitly use the fact that
    we can reoder the queries of $M'$ in such a way that all encryption queries occur before all decryption
    queries. In the scenario where $Eve$ receives
    a query $(c,\sigma)$ which is never encrypted by $M'$, note that $Eve$ simply returns error. Given
    that we are in case 2, $Eve$ is wrong (she returns error when she shouldn't) with probability
    at most $\epsilon/10$. Hence, we still get an overall success of $\frac{1}{2} + \frac{9}{10}\epsilon$,
    contradicting the CPA security of $(E,D)$.
    ~

    c. (10 points) $E'_{k_1,k_2,k_3}(m) = c\|\sigma\|\sigma'$ where $c=E_{k_1}(m)$, $\sigma=S_{k_2}(c)$, $\sigma'=S_{k_3}(c)$. $D'_{k_1,k_2,k_3}(c\|\sigma\|\sigma')=D_{k_1}(m)$ if either $V_{k_2}(c)=\sigma$ or $V_{k_3}(c)=\sigma'$; otherwise $D'_{k_1,k_2,k_3}(c\|\sigma\|\sigma')=$```error```.
    
    We assume that a type occurred and the intended $D'_{k_1,k_2,k_3}(c || \sigma || \sigma') = D_{k_1}(c)$.
    ~Proof
    The scheme is necessarily insecure. We show this by explicitly constructing an adversary $\mathcal{A}$
    that wins in the CCA-game on input $1^n$ with probability $\frac{1}{2} + p(n)$ with non-negligible
    $p(n)$. The adversary does the following:
      
      1. We choose arbitrary $m_0$ and $m_1$ (for concreteness, we can let $m_0 = 0^n$ and $m_1 = 1^n$).
      2. On receiving the challenge ciphertext $c^* = c|| \sigma || \sigma' = E'_{k_1,k_2,k_3}(m_b)$, construct
      the ciphertext $c' = c || \sigma || (\sigma' \oplus 1^n)$ (ie, flip the bits of the last $\sigma'$).
      3. Use the decripytion oracle on  $c'$ to obtain the message $m_{b'}$. 
      4. Compare the message $m_{b'}$ to $m_0$ and $m_1$ and return whichever it matches.
    
    Note that our adversary always wins the game for the simple reason that $c' \neq c^*$, and therefore
    it is allowed to query its decryption oracle. Then as defined, the $D'_{k_1, k_2, k_3}$ will
    succeed as long as either signature of the encryption $c$ is correct. Since we leave $c$ and $\sigma$
    unmodified in $c'$, we have that $V_{k_2}(c) = \sigma$, so the oracle will return the descryption of
    $D_{k_1}(c)$ and then our adversary will know exactly what the encrypted message is.
    ~

    d. (10 points) $E'_{k_1,k_2,k_3}(m) = c\|\sigma\|\sigma'$ where $c=E_{k_1}(m)$, $\sigma=S_{k_2}(c)$, $\sigma'=S_{k_3}(c)$. $D'_{k_1,k_2,k_3}(c\|\sigma\|\sigma')=D_{k_1}(m)$ if both $V_{k_2}(c)=\sigma$ and $V_{k_3}(c)=\sigma'$; otherwise $D'_{k_1,k_2,k_3}(c\|\sigma\|\sigma')=$```error```.
    
    Again, we assume a typo was made.
    ~Proof
    The scheme is necessarily secure. We proof this by showing that the $S'_{k_1,k_2}(c) = S_{k_1}(c) || S_{k_2}(c)$
    is MAC secure. Note that if the above is the case, then the encryption scheme consists of simply
    $E'_{k_1,k_2,k_3}(m) = c || S'_{k_2,k_3}(c)$ which is the cononical CCA secure scheme. 
    
    For a contradiction, suppose that $S'_{k_2,k_3}$ is not MAC secure. This means that there exists an
    adversary $M$ who can forge a signature for a new message $m'$ (ie, submit $(m',\tau')$ where
    $V_{k_2,k_3}(m', \tau') = 1$ after quering $S'_{k_2,k_3}(m_i) = \tau_i$ for $1,\cdots,i,\cdots, T(n)$
    where $T$ is polynomial. Then note that we can use $M$ to construct a new adversary $M'$ which can
    forge a signature for the MAC $(S_{k_2},V_{k_2})$. The constrution proceeds as follows:
    
      1. In input $1^n$, $M'$ will simulate the forging game played by $M$. We have access to the 
      signature oracle for $S_{k_2}$, and then we choose random $k_3$. On a query by $M$ for the 
      tag of $m$, we return query our oracle to obtain $\tau_1 = S_{k_2}(m)$ and then using $k_3$ 
      compute $\tau_2 = S_{k_3}(m)$, returning $\tau_1 || \tau_2$. 
      2. When $M$ finally outputs a pair $(m',\tau')$ where $\tau = tau_1' || tau_2'$, not seen before,
      $M'$ simply outputs $(m',\tau_1')$.
      
    Note that $M'$ succeeds with probability at least equal to $M$. In order for $(m', \tau_1')$ to be 
    a valid signature for $m'$ under $S_{k_2}$, it must be the case that $\tau$ is a valid signature
    for $m'$ under $S'_{k_2,k_3}$. 
    
    Therefore $M'$ can break the security of $S_{k_2}$, which is a contradiction on its CPA security. 
    We conclude that $M$ does not exists, and therefore the scheme $S'_{k_2,k_3}$ is CPA secure. 
    ~

3. (25 points) Prove the security of the simplified GCM mode described in the lecture notes for two blocks. That is, let $H$ be a collection of functions from $\zo^{3n}$ to $\zo^n$ such that for every $x\neq x'\in\zo^{3n}$ and  $y,y'\in\zo^n$, $\Pr_{h\getsr H}[ h(x)=y \wedge h(x')=y' ]=2^{-2n}$. Let $\{ p_k \}$ be a pseudorandom permutation collection on $n$ bits. Prove that the following encryption on $2n$ bits is CCA secure:

    * $E_{k,h}(m_1,m_2) = (IV,y_1,y_2,y_3)$ where $IV$ is chosen at random in $[2^n]$, $y_1=p_k(IV+1)\oplus m_1$, $y_2=p_k(IV+2) \oplus m_2$ and $y_3=p_k(IV+3)\oplus h(IV\|y_1\|y_2)$. (Addition is done modulo $2^n$ as usual.)

    * $D_{k,h}(IV\|y_1\|y_2\|y_3)$ is obtained by outputting ```error''' if $m_3 \neq h(IV\|y_1\|y_2)\oplus p_k(IV+3)$ and otherwise outputting $(m_1,m_2)$.
