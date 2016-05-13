Homework 6: Public key, RSA/Dlog and lattices
~ MathDefs
\newcommand{\zo}{\{0,1\}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\getsr}{\leftarrow_R\;}
\newcommand{\Gp}{\mathbb{G}}
\newcommand{\iprod}[1]{\langle #1 \rangle}
~ 

### Total of 120 points

1. (KL Ex 8.10, 15 points) Prove that for every $x\in \{0,\ldots, m-1\}$ (even if $x$ is not in $\Z^*_m$) if $ed = 1 \pmod{|\Z^*_m|}$ then $(x^e)^d = x \pmod{m}$.
The statement from from KL 8.10 actually reads "Corollary 8.21 shows that if $m = pq$ and $ed = 1 \pmod{\phi(m)}$
 then for all $x \in Z^∗_m$ we have $(x^e)^d = x \pmod{m}$ . Show that this holds for all
$x \in \{0,\cdots, m - 1\}$.
~ Proof
We first generalize the statement in Corollary 8.21. We prove that $\forall m > 1$ such 
that $m = pq$ where $p,q$ are prime and $x \in \Z_m$,
then $x^{\phi(m)} = 1 \mod m$. Note that once we have this, the result follows immediately because 
$ed = 1 \pmod {\phi(m)} \implies ed = k\phi(m) + 1$ for some $k \in \Z$: 
~ 
~ Math
  \begin{aligned}
(x^e)^d &= x^{ed} \pmod{m} \\
&= x^{k\phi(m) + 1} \pmod{m}\\
&=  x^{k\phi(m)}x \pmod{m}\\
&= (x^{\phi(m)})^kx\pmod{m} \\
&= 1^kx \pmod{m}\\
&= x \pmod{m}
\end{aligned}
~
We know that $m = pq$ where $p,q$ are distinct primes. Then applying 
the Chinese Remainder Theorem (since $gcd(p,q) = 1$), we know that there exists  isomorphism 
$f: \Z_m \to \Z_p \times \Z_q$ defined as 
$f(x) = ([x \mod p], [x \mod q])$ and similarly defined isomorphism 
$f^*: \Z_m^* \to \Z_p^* \times \Z_q^*$. Then we have:
~ Math
x^{\phi(m)} \leftrightarrow (x_{p}^{\phi(m)}, x_{q}^{\phi(m)})
~
Then because $p,q$ are prime, we have that $\Z_p^* = \Z_p$ and $\Z_q^* = \Z_q$ due to the fact that
$\forall a \in Z_p$, $gcd(a,p) = 1$ and similarly for $q$. Then we can define the trivial isomorphism
$t_k: \Z_k \to Z_k^*$ for prime $k$. Then by composition of isomorphisms, note that the function
defined as $h: \Z_m \to \Z^*_p \times Z^*_q$ where $h(x) = (t_p(x), t_q(x'))$ where $(x,x') = f(x)$ is
also an isomorphism whose inverse $h^{-1}$ simply perform the reverse trivial isomorphism on each
coordinate $t_p^{-1}$ and $t_q^{-1}$ and then performs $f^{-1}$. We then have:
~ Math
\underbrace{x^{\phi(m)} \leftrightarrow (x_p^{\phi(m)}, x_q^{\phi(m)})}_{\Z_m \to \Z_p \times \Z_q} \underbrace{\leftrightarrow (x_p^{*^{\phi(m)}}, x_q^{*^{\phi(m)}})}_{\Z_m \to \Z_p \times \Z_p^* \times \Z_q^*} = \underbrace{(1,1) \leftrightarrow 1 \pmod{m}}_{Z_p^* \times Z_q^* \to Z_m}
~ 
where equality follows from the original statement of Corollary 8.21. Therefore, we have that $\forall x \in Z_m$,
$x^{\phi(m)} = 1 \pmod{m}$, which is sufficient to proof our statement. 
Note that the above readily generalizes to $m = \prod_i p_i$ for prime $\{p_i\}$. I have had some difficulty generalizing
to the case where $m = \prod_i p_i^{e_i}$, though since the question does not ask for this, I do not show it.

2. (KL Ex 8.20, 25 points) Let $m,e$ be as in the RSA problem, let $y\in\Z^*_m$, and let $f_0$ be the RSA function $f_0(x)=x^e$ and $f_1$ be its "shifted by $y$" variant $f_1(x)=y\cdot x^e$.
     a. (10 points) Prove that given two inputs $x \neq x' \in \Z^*_m$ such that $f_0(x)=f_1(x')$, one can find $y^{1/e} \pmod{m}$.
     ~Proof
     To calculate $y^{1/e} = y^{e^{-1}} = (y^{-1})^e$, note that we just need to calculate $y^{1/e} = x(x')^{-1} \pmod{m}$.
     Note that $x \neq x'$ implies that $y^{1/e} \neq 1$. Since $x' \in \Z_m^*$, then the inverse
     $x'^{-1} \in \Z_m^*$ exists and is well-defined. 
     ~
     We're given that $f_0(x) = f_1(x')$.
     ~Math
     \begin{aligned}
      f_0(x) = f_1(x') &\implies x^e =yx'^e \\
      &\implies y = x^e(x'^{-1})^e \\
      &\implies y = (xx'^{-1})^e \\
      &\implies y^{1/e} =xx'^{-1} 
     \end{aligned}
     ~
     where the last step follows from the fact that $y = (y^{1/e})^e$.
     b. (15 points) Conclude that $\ell = 10\log m$, if we pick $m,e,y$ as above and let $H_{m,e,y}(z_1,\ldots,z_\ell)$ be defined as $f_{z_1}(f_{z_2}(\cdots (f_{z_{\ell}}(1))\cdots))$ then this collection is a  _collision resistant hash family_ mapping $\zo^\ell$ to $\Z^*_m$ if the RSA function is hard to invert. That is, if there is an algorithm that given a random hash function $H$ from this collection finds $z\neq z' \in \zo^\ell$ such that $H(z)=H(z')$ then there is an algorithm to invert the RSA function.
     ~Proof
     We first show that we can compute $H_{m,e,y}(x)$ efficiently. The reason for this is that we can
     compute $f_i(x)$ efficiently for all $x \in \Z_m^*$ and we perform this computation only $l = O(\log m)$
     times, which is polynomial in the size of the input. Now we show that the family is collision resistant
     given that RSA function is hard to invert.
     Suppose there exists an efficient adversary $\mathcal{A}(H_{m,e,y}) = (x,'x)$ such that $x \neq x'$
     yet $H_{m,e,y}(x) = H_{m,e,y}(x')$. Then we use this adversary to construct $\mathcal{A'}$ which 
     inverts the $RSA_{m,e}: \Z_m^* \to \Z_m^*$ function. The adversary $\mathcal{A'}$ does the following, 
     with the purpose of finding $y^{1/e} = x \in \Z_m^*$ where $y = x^e = RSA_{m,e}(x)$:
      
      1. On input $y$, check if $y = 1 \pmod m$. If so, return $y$.
      2. Otherwise, construct $H_{m,e,y}$.
      3. Feed $H_{m,e,y}$ to $\mathcal{A}$ to produce $(z,z')$.
      4. Find the rightmost bit of $z$ and $z'$ which differs, call it $i_R$.
      5. Compute $h_{i_R}(1) \neq h_{i_R}'(1)$. ($h$ is defined below)
      6. Continue computing $f_{z_i}$ as defined until we reach a point such that
      $f_{z_{i_L}}(f_{z_{i_L+1}}(\cdots h_{i_R}(1)\cdots)) = f_{z'_{i_L}}(f_{z'_{i_L+1}}(\cdots h'_{i_R}(1)\cdots))$.
      7. Let $a = f_{z_{i_L+1}}(\cdots h_{i_R}(1)\cdots)$ and $a' = f_{z'_{i_L+1}}(\cdots h_{i_R}(1)\cdots)$
      and output $aa'^{-1}$.
  
     We argue that $\mathcal{A'}$ is efficient. Note that the construction of $H_{m,e,y}$ is efficient,
     and that by assumption, $\mathcal{A}$ is efficient. Furthermore, note that because calculating
     $f_k$ is efficient, and we repeat this process at most $\ell = 10 \log m$ times which is polynomial
     in the size of the input, then we have an efficient algorithm.
     Now, we prove that $\mathcal{A'}(y) = y^{1/e}$ which is the inverse of the RSA function. The key is
     to note that $z \neq z'$ yet $H_{m,e,y}(z) = H_{m,e,y}(z')$. Let $h_i = f_{z_{i}} \circ \cdots \circ f_{z_{\ell}}$
     and $h_i' = f_{z_{i}'} \circ \cdots \circ f_{z_{\ell}'}$. Note that if $z \neq z'$, there exists
     some rightmost bit $i_R$ such that $z_{i_R} \neq z_{i_R}'$ where for all $j > i$, we have $z_j = z_j'$ 
     (this is the bit we find in step 4 above). 
     Then note that it must be the 
     case that $h_{i_R}(1) \neq h'_{i_R}(1)$, as claimed in step $5$. The reason for this is that, by determinism
     of $f_0,f_1$, $h_{i_R+1}(1) = h'_{i_R+1}(1) = x_R$. Then $h_{i_R} = f_{0}(x_R) = x_R^e \neq yx_R^e = f_1(x_R)$
     since we know that $y \neq 1 \pmod m$. However, we know that eventually, $H_{m,e,y}(z) = h_1(1) = h'(1) = H_{m,e,y}(z')$,
     therefore there exists some rightmost bit $z_{i_L} \neq z'_{i_L}$ such that $h_{i_L}(1) = h_{i_L}(1)$. Note that we
     find the rightmost bit in step 6 above. Then we have just found inputs $a = h_{i_L + 1}(1)$ and $a' = h'{i_L + 1}(1)$
     such that $a \neq a'$ (by construction) where $f_{z_{i_L}}(a) = f_{z'_{i_L}}(a')$. These are precisely
     the conditions needed in part (a) above, therefore, we know that $y^{1/e} = aa'^{-1}$,
     which is exactly what our adversary returns. This is a contradiction on the hardness
     of inverting RSA, therefore it must be the case that $H_{m,e,y}$ is collision resistent. 
     
3. (One time signatures, 25 points) As I mentioned it is in fact possible to get digital signatures based on only private key cryptography. In this exercise we will show a baby version of this. We say that a signature scheme $(G,S,V)$ is a _one time signature scheme_ if it satisfies the security definition of digital signatures (with a public verification key) with the restriction that the adversary is only allowed to make a _single query_ $m$ to the signing oracle, and needs to output a signature on a messahe $m' \neq m$. Let $PRG:\zo^n\rightarrow\zo^{2n}$ be a pseudorandom generator. Prove that the following scheme is a secure one-time signature scheme for messages of length $\ell$:

    * _Key generation_: Pick $2\ell$ independent random strings in $\zo^n$ which we'll denote by $x^0_1,\ldots,x^0_\ell,x^1_1,\ldots,x^1_\ell$. The secret signing key is the tuple $( x^b_i )_{b\in\zo,i\in[\ell]}$  while the public verification key is the tuple $( y^b_i )_{b\in\zo,i\in[\ell]}$ where $y_i^b = PRG(x_i^b)$.

    * _Signing_: To sign a message $m\in \zo^\ell$, output the $\ell$-tuple $(x^{m_1}_1,\ldots,x^{m_\ell}_\ell)$.

    * _Verification_: To verify a message $m$ w.r.t. signature $(x'_1,\ldots,x'_\ell)$ and public key $( y^b_i )_{b\in\zo,i\in[\ell]}$, check that $PRG(x'_i)=y^{m_i}_i$ for all $i\in[\ell]$.
    
    ~ Proof
    For the sake of contradiction, suppose the above scheme is not a secure one-time signature scheme for
    messages of length $\ell$. Then $\exists$ an efficient adversary $\mathcal{A}$ that on input, the
    public key $v$, and black box access to $S_s(\cdot)$ can, by querying the box only once, produce a
    pair $(m^*,\sigma^*)$ such that $V_s(m^*,\sigma^*) = 1$. Then we construct the distinguisher $D'$ as 
    follows:
    On input $y \in \zo^{2n}$:
      
      1. Use $G$ to generate $(s,v) = ((x_i^b)_{b \in zo, i \in [\ell]}, (y_i^b)_{b \in \zo, i \in [\ell]})$,
      the secret signing key and the public signing key.
      2. Pick $b' \in_R \zo$ and $j \in_R [\ell]$ and replace $y_{j}^{b'}$ with $y$, creating the modified
      public key $v'$.
      3. Feed $v'$ to $\mathcal{A}$.
      4. If $\mathcal{A}$ makes a single query to the decryption box with input $m$, then check
      if $m_j = b'$. If so, chose output $o \in_R \zo$. If not, then continue.
      5. When $\mathcal{A}$ outputs $(m^*,\sigma^*)$, return $1$ if $V_{v'}(m^*,\sigma^*) = 1$.
    ~
    We claim that the above can distinguish successfully between input distributions $U_{2n}$ and 
    $PRG(U_n)$ (ie, with non-negligible probability). We have two cases to consider:
      
      1. The adversary $\mathcal{A}$ makes no queries to the signing oracle. In this case,
      we condition on whether $\exists x \in U_n$ such that $PRG(x) = y$ (the event $\mathcal{E}$). 
      Note that in the case
      where $y \in_R PRG(U_n)$, such an $x$ always exist. In that case, $1$ is returned
      iff $V_{v'}(m^*,\sigma^*) = 1$ which occurs with probability $1 - negl(n)$. In the case
      where $y \in_R U_{2n}$, $x$ exists with probability $\frac{2^n}{2^{2n}} = \frac{1}{2^n}$. In the
      case where it does exists, success again occurs iff $V_{v'}(m^*,\sigma^*) = 1$. In the case where
      it does not exist, then we are guaranteed that $V_{v'}(m^*,\sigma^*) = 0$, so we always output $0$.
      ~Math
      \begin{aligned}
      |\E[D(PRG(U_n)] - \E[D(U_n)]| &=  |\E[D(PRG(U_n) \mid \mathcal{E}]\Pr[\mathcal{E}] + \E[D(PRG(U_n) \mid \mathcal{E}^c]\Pr[\mathcal{E}^c] \\
        &-\E[D(U_n) \mid \mathcal{E}]\Pr[\mathcal{E}]- \E[D(U_n) \mid \mathcal{E}^c]\Pr[\mathcal{E}^c]| \\
       &= |(1-negl(n)) - \frac{1}{2^n}(1 - negl(n)) - (1 - \frac{1}{2^n})(0)| \\
       &= 1 - negl'(n)
      \end{aligned}
      ~
      which is non-negligible. Therefoere, $D$ distingishes succesfully with non-negligible probability.
      2. In the second case, $\mathcal{A}$ makes a single query. In this case, we can condition
      on whether or not $m_j = b'$ and reduce the problem to case 1. Note that when $m_j = b'$, which
      occurs with probability $\frac{1}{2}$ because $b' \in_R \zo$, $D$ returns $1$ with probability 
      $\frac{1}{2}$.
      ~Math
      \begin{aligned}
      |\E[D(PRG(U_n)] - \E[D(U_n)]| &=  |\E[D(PRG(U_n) \mid m_j = b']\Pr[m_j = b'] \\
        &+\E[D(PRG(U_n) \mid m_j \neq b']\Pr[m_j \neq b'] \\
        &-\E[D(U_n) \mid m_j = b']\Pr[m_j = b']\\
          &- \E[D(U_n) \mid m_j \neq b']\Pr[m_j \neq b']| \\
       &= |\frac{1}{4} + \frac{1}{2}\E[D(PRG(U_n) \mid m_j \neq b'] - \frac{1}{4} \\
       &- \frac{1}{2}\E[D(PRG(U_n) \mid m_j \neq b']| \\
       &= \frac{1}{2} - \frac{negl'(n)}{2}
      \end{aligned}
      ~
      The last line follows from the fact that if $m_j \neq b'$, $\mathcal{A}$ has no information as to
      $y$, and might have as well not asked the query, so we're in Case 1 again.
      
    The above shows that the distinguisher $D$ succeeds with non-negligible probability, a contradiction
    on the fact that $PRG$ is a pseudo-random generator. Therefore, it must be the case that 
    the scheme is a secure one-time scheme. 
      
4. (30 points) Consider the following variant of the DSA signature scheme:
    * _Key generation:_ Let $\Gp$ be a cyclic group. Pick generator $g$ for $\Gp$ and $a\in \{0,\ldots,|\Gp|-1\}$ and let $h=g^a$. Pick $H:\zo^\ell\times \{0,\ldots,|\Gp|-1\}\rightarrow \{0,\ldots,|\Gp|-1\}$ and $F:\Gp\rightarrow\{0,\ldots,|\Gp|-1\}$ to be some  functions that we consider as random oracles. The public key is $(g,h)$  (as well as the functions $H,F$) and secret key is $a$.
    * _Signature:_ To sign a message $m$, pick $b$ at random,  let $f=g^b$, let $c=F(f)$ and $d=H(m,c)$  and then let $s= b^{-1}[d+a\cdot c]$ where all computation is done modulo $|\Gp|$. The signature is $(f,s)$.
    * _Verification:_ To verify a signature $(f,s)$ on a message $m$, compute $c=F(f)$ and $d=H(m,c)$  and then check that $s\neq 0$ and $f^s=g^{d}h^{c}$.

    a. (20 points) Prove that this is a secure one-time signature scheme in the random oracle model, assuming the difficulty of the discrete logarithm problem in $\Gp$. See footnote for hint[^hintDSA]
    ~Proof
      Following the hint, suppose this is not a secure one-time signature scheme in the random oracle model.
      Then there exists some efficient adversary $\mathcal{A}$ that with at most one use of the signing oracle,
      can construct $(m^*, \sigma^*)$ given the public key $(g,h,H,F)$ such that $V_a(m^*,\sigma^*) = 1$ with
      probability $1 - negl(\ell)$. Then we use this adversary to solve the discrete logarithm problem. 
      The algorithm to solve dlog works as follows:
      
        1. On input $h$, we generate the key as described in the modified DSA signature scheme, where
        we set the public key to be $(g,h)$ and the secret key as $a$ where $h$ is our input ($H,F$ are 
        also public.
        2. We then feed the public key $(g,h)$ to the adversary $\mathcal{A}$, providing it with access 
        to $F$ and $G$. 
        3. When $F$ eventually queries the $F$ and $H$ oracles in order to construct the result
        $(m^*, s^*)$, we intercept the query to oracle $F$, $f$. If $c = F(f)$ has not get been computed,
        we compute it. However, we then pick $c^* \in_R \{0, \cdots, |\Gp|\}$ and return this as the
        result to $\mathcal{A}$. 
        4. $\mathcal{A}$ will return to us $(m^*,s^*)$. 
        5. We calculate $d = H(m,c)$ with $c$ the original input fed to oracle $F$ by the adversary.
        6. We output $(d-d^*)(c^* - c)^{-1} \pmod{\Gp}$. 
        
    Note that the above algorithm is efficient. Key generation is efficient, randomly selecting elements
    is efficient, $H$ and $F$ are efficient, and computing the result in step 6 is efficient. We now 
    prove that we have $a = (d-d^*)(c^* - c)^{-1}$. This follows from the fact that
    with high probability, $s^*b = d^* + ac^*$ and $s^*b = d + ac$. The reason for this is that 
    is that the signature scheme $(f,s^*)$ passes with high probability by the fact that 
    the $\mathcal{A}$ adversary has generated it as a forge for the chosen value for the message
    $m$. This means $s^*b = d + ac$. This is by construction since we know that $f = g^b$ and
    therefore $f^{s^*} = (g^b)^{b^{-1}(d + ac)} = g^{d+ac} = g^dh^c$ as required by the signature.
    However, also note that the adversary successfully procuded this signature, and with high
    likelihood, it should pass the verifier. However, the result $\mathcal{A}$ received for $F(f)$ 
    was $c^*$, a completely random value and therefore $d^* \neq d$, so yet the verifier must pass,
    so it must be the case that $s^*b = d^* + ac^*$. We therefore now have two linearly independent equations,
    and subtracting one from the other, we have $0 = d^{*}  - d + a(c^* - c) \implies a = (d - d^*)(c^* + c) $.
    Therefore,  the above algorithm successfully computes $a$ such that $g^a = h$, thereby solving 
    the discrete logarithm problem.
    ~
    b. (10 points) Prove that this is a secure (many times) signature scheme in the random oracle model, assuming the difficulty of the discrete logarithm problem in $\Gp$.
    ~Proof
    I'm not entirely sure why the above proof is not sufficient.
   ~

[^hintDSA]: You need to design a reduction that takes $h=g^a$ and returns $a$ using "in its belly" an adversary for the signature scheme. You can use $h$ as the public key. The scheme ensures that to produce a valid signature the adversary will first need to ask $F$ on the query $f$, and then ask $H$ on the query $m,F(f)$. The idea is that once an adversary makes a query $f$ to the oracle $F$, then they have "committed" to the value $b$ such that $g^b=f$ even if they didn't disclose it. Now, if they are able to successfully sign the message $m$ with decent probability over the output of $H(m,c)$ then we'll be able to find two different responses $d \neq d'$ for which they can sign successfully. This will yield two linearly independent equations on the two unknowns $b$ and $a$.

5.  (25 points) Prove that under the LWE assumption, the following variant of our lattice based encryption scheme is secure: (you can use the assumption of security of the scheme presented in class if it helps.)

     * _Parameters:_ Let $\delta(n)=1/n^4$ and let $q=poly(n)$ be a prime such that LWE holds w.r.t. $q,\delta$. We let $m = n^2\log q$. _(Same as before)_

     * _Key generation:_ Pick $x\in\Z_q^n$. The private key is $x$ and the public key is $(A,y)$ with $y=Ax+e$ with $e$ a $\delta$-noise vector and $A$ a random $m\times n$ matrix. _(Same as before)_

     * _Encrypt:_ To encrypt $b\in\zo$ given the key $(A,y)$, pick $w\in\zo^m$ and output $2w^\top A, 2\iprod{w,y}+b$ (all modulo $q$ of course). The difference is that instead of adding either $0$ or $q/2$, we add either $0$ or $1$, but multiply this by $2$ so the result would be _even_ or _odd_ as needed.

     * _Decrypt:_ To decrypt $(a,\sigma)$, output $0$ iff $|\iprod{a,x}-\sigma|$ is even. (Instead of asking this to be smaller than $q/10$.
     
     ~Proof
     We proof that this variant is CPA secure by reducing to the variant presented in class, which under the 
     LWE assumption, we can assume to be CPA secure. Suppose the above lattice encryption scheme is not 
     CPA secure. Then there exists an efficient distinguisher $\mathcal{D}$ that with non-negligible
     probability can distinguish between the distributions of the encryption of $b \in \zo$ given the 
     public key. Formally, it can distinguish with non-negligible probability between 
     $Z_0 = \{((A,y = Ax + e),(2w^\top A, 2\iprod{w,y})) \}$ and $Z_1 = \{(A, y = Ax + e),(2 \iprod{w,y}) + 1 \}$. Then
     we construct a distinguisher $D'$ which can distinguish between the distributions of the encryption
     of $b \in \zo$ given the public key in the lattice scheme presented in class. Formally, we 
     construct $D'$ so it can distinguish between $Z_0' = \{(A,y= Ax + e),(w^{\top}A, \iprod{w,y}) \}$ and 
     $Z_1' = \{(A, y = Ax e),(w^\top A, \iprod{w,y} + \lfloor q/2 \rfloor) \}$. The distinguisher works as follows:
     
        1. On input $((A,y),(a,\sigma))$, calculate $((A,y), (2a, 2\sigma + 1))$ (all modulo $q$) and 
        feed this input int $D$.
        2. Return whatever $D$ returns.
     
     Now we show that $D'$ distinguish between $Z_0'$ and $Z_1'$ with non-negligible probability. It 
     is immediately obvious that if $D$ is efficient, then $D'$ is efficient since multiplication
     modulo $q$..
     ~
     ~Math
      |\E[D'(Z_0')] - \E[D'(Z_1')]| =  |\E[D(Z_1)] - \E[D(Z_0)]| > p(n)
     ~
     The inequality follows from the assumption that $D$ is a successful distinguisher between $Z_0$ and
     $Z_1$ distributions. Now we explain the equality. Given input from $Z_1'$ to $D'$, we know that
     it must be of the form $((A,y), (w^\top A, \iprod{w,y}))$. $D'$ converts this into the form
     $((A,y), 2w^\top A, 2\iprod{w,y} + 1)$, all modulo $q$, which is precisely an element from $Z_0$.
     Therefore, $D'(Z_1') = D(Z_0)$. Similarly, given input from $Z_0'$ to $D'$, we know that
     it must be of the form $((A,y), (w^\top A, \iprod{w,y} + \lfloor q/2 \rfloor))$, and $D'$ converts
     this into an element of the form $((A,y), (2w^\top A, 2[\iprod{w,y} + \lfloor q/2 \rfloor] + 1))$. Note that
     if $q = 2$, then $2[\iprod{w,y} + \lfloor q/2 \rfloor] + 1 = 2\iprod{w,y} \pmod q$. In the case
     where $q > 2$, we know that $q = 2k + 1$ for some $k \in \mathbb{N}$ since $q$ is prime. Therefore:
     ~Math
     \begin{aligned}
      2[\iprod{w,y} + \lfloor q/2 \rfloor] + 1 &= 2\iprod{w,y} + 2\lfloor k +  1/2 \rfloor + 1 \pmod{q}\\
      &= 2\iprod{w,y} + 2k + 1 \pmod{q} \\
      &= 2\iprod{w,y} + q \pmod{q} \\
      &= 2\iprod{w,y} \pmod{q}
      \end{aligned}
     ~
     Then in either case, we have an element from $Z_1$. Therefore, $D'(Z_0') = D(Z_1)$. However,
     the above is a contradiction on the CPA security of the encryption scheme presented in class, 
     therefore our modified encryption must be secure. 
     
     ~Proof
     Now we proof that with high probability, the decryption of the encryption $b$ equals $b$. We do 
     this to show that this is a valid probabilistic encryption scheme. Then note that:
     ~
     ~Math
     \begin{aligned}
     |\iprod{a,x} - \sigma| &= |\iprod{2w^\top A, x}- 2\iprod{w,y} +- b| \\
     &= |2\iprod{w,Ax} - 2\iprod{w,Ax} - 2\iprod{w,e} - b|| \\
     &= |2\iprod{w,e} + b| 
     \end{aligned}
     ~
     Then note that $\iprod{w,e} < q/10$, therefore if $b = 0$, $2\iprod{w,e}$ is even, and if $b = 1$,
     then $2\iprod{w,e} + 1$ is odd.