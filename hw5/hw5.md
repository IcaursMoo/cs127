% Homework 5: Private Key Recap

~ MathDefs
\newcommand{\zo}{\{0,1\}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\getsr}{\leftarrow_R\;}
~

### Total of 160 points

This homework consists of a review of the notions we learned about from _private key_ cryptography.
It is a little long, but many of the questions are not very hard and it has many points and
so is a chance for you to make up for lost points in previous homeworks.

Each of the following questions is taken from the Katz Lindell book (2nd ed)  and is worth 20 points:

1. Excercise 3.6
We let $G$ be a pseudorandom generator with expansion factor $l(n) > 2n$, and provide a construction 
for the following pseudoranom generators.
  
  a. Define $G'(s) = G(s_1\cdots s_{\lfloor n /2 \rfloor})$:
  ~Proof
  We proof that $G'(s)$ is a pseudorandom generator. Then we need to show that $G'(s) \approx U_{\ell{\lfloor n/2 \rfloor}}$.
  Note that the distributions $s \in U_{n}$ where we truncate the last $\lceil n/2 \rceil $ bits is indistinguishable
  from $s \in U_{\lfloor n/2 \rfloor }$. Therefore, the input to $G$ is distributed uniformly at random. Formally
  ~
  ~Math
   G'(U_{n}) \approx G(U_{\lfloor n/2 \rfloor}) \approx U_{\ell(\lfloor n/2 \rfloor)} 
  ~
  Where the first follows from the fact that $U_n \approx U_{\lfloor n / 2\rfloor}$ and the second
  step follows from the fact that $G$ is a pseudorandom generator. Then by the triangle inequality,
  we have that $G'(U_{n}) \approx U_{\ell(\lfloor n/2 \rfloor)}$ as desired.
  b. Define $G'(s) = G(0^{|s|}|| s)$.
  ~Proof
   We proof that $G': \zo^{n} \to \zo^{\ell(n)}$ is not secure. The way we demonstrate
   this is by constructing $G$ and an adversary $\mathcal{A}$ of $G'$ such that $\mathcal{A}$ can
   distinguish between $G'(s)$ and $U_{\ell(n)}$. First, the construction of $G$. Let $G'': \zo^{2n} \to \zo^{\ell(n)}$
   be a pseudorandom number generator. Then define $G$ as follows:
  ~
  ~Math
   G(s) = G''(s_1\cdots s_{n}) 
  ~
  By (a), we know that the above construction is secure (this is trivial, just take $n' = 2n$). 
  Therefore, $G$ is a secure. However, now we construct an adversary $\mathcal{A}$ that 
  will distinguish between $G'(s)$ for $s \in_R \zo^{n}$ and $U_{\ell(n)}$. On input 
  $y \in \zo^{\ell(n)}$, $\mathcal{A}$ compares the input $y$ to $G''(0^{|s|})$, and if they are
  equal, outputs $1$, otherwise it randomly outputs $1$ or $0$. Then note that now we have:
  ~Math
    \begin{aligned}
    \Pr[\mathcal{A}(G'(s)) = 1] &=  1\\
    \Pr[\mathcal{A}(U_{\ell(n)}) = 1] &= \frac{1}{2^{\ell(n)}}
    \end{aligned}
  ~
  where the first equality follows from the fact that $G'(s) = G(0^{|s|} || s) = G''(0^{|s|})$ for
  all $s \in_R \zo^{n}$ so $\mathcal{A}$ always outputs $1$, and the second follows from the
  fact that for uniformly random $y \in_R \zo^{\ell(n)}$, the probability that it equals $G''^{0^|s|}$
  is negligible. 
  Then we conclude:
  ~Math
   |\E[\mathcal{A}(G'(s))] - \E[\mathcal{A}(U_{\ell(n)})]| = 1 - \frac{1}{2^{\ell(n)}}  
  ~
  which is non-negligible for $\ell(n) > 2n$ and therefore we have tha $G'$ is not a pseudorandom
  generator. 
  c. Define $G'(s) = G(s) || G(s+1)$.
  ~Proof
    We proof thar $G': \zo^{n} \to \zo^{2\ell(n)}$ is not a pseudorandom generator. We do this
    by explicitly constructing an adversary that breaks an instance of $G'$. First, suppose we have
    the pseudorandom generator $G'': \zo^n \to \zo^{\ell(n)}$. Then define $G$ as follows:    
  ~
  ~Math
   \begin{aligned}
     G(s) &= \begin{cases}
       G''(s) &  s \mod 2 = 0\\
       G''(s-1) & s \mod 2 = 1
     \end{cases}
   \end{aligned} 
  ~
  Then note that the above $G$ is a pseudoranom generator. Suppose not, then there exists 
  $Eve$ such that it can distinguish between $U_{\ell(n)}$ and $G(s)$ for $s \in_R \zo^n$. 
  Then let us construct $Eve'$ which on input $y \in \zo^{\ell(n)}$ feeds the input $y$ to
  $Eve$ and outputs what $Eve$ outputs. Let $A$ be the event where $s \mod 2 = 0$. Then we have:
  ~Math
    \begin{aligned}
    &|\E_{s \in_R \zo^{n}}[Eve'(G(s))]  - \E[Eve'(U_{\ell(n)}) ]| \\
    &= |\E_{s \in_R \zo^{n}}[Eve'(G(s))| A] \Pr[A] + \E_{s \in R \zo^{n}}[Eve'(G(s)) |A^c] \Pr[A^c] - \E[Eve'[U_{\ell(b)}]| \\
    &= |\frac{1}{2}\E_{s \in_R \zo^{n}}[Eve(G''(s))] + \frac{1}{2}\E_{s \in_R \zo^{n}}[Eve'(G''(s-1))] - E[Eve(U_{\ell(n)}]| \\
    &= |\frac{1}{2}\E_{s \in_R \zo^{n}}[Eve(G''(s))] + \frac{1}{2}\E_{s' \in_R \zo^{n}}[Eve(G''(s'))] - \E[Eve[U_{\ell(n)}| \\
    &= |\E_{s \in_R \zo^{n}}[Eve(G''(s))] -\E[Eve(U_{\ell(n)})]| > \epsilon(n)
  \end{aligned}  
  ~
  for non-negligible $\epsilon(n)$. Therefore, $G(s)$ must be a pseudorandom number generator. 
  However, note that now we can construct an adversary to distinguish $G'(s)$ from $U_{2 \ell(n)}$.
  On input $y \in \zo^{2\ell(n)}$, our adversary $\mathcal{A}$ does the following: if the first half
  of $y$ is equivalent to the second half, then it outputs $1$, otherwise, it outputs randomly ouputs either $0$ or $1$. Then
  we have:
  ~Math
    \begin{aligned}
    \Pr[\mathcal{A}(G'(s)) = 1] &= \Pr[\mathcal{A}(G'(s)) = 1 \mid A] \Pr[A] + \Pr[\mathcal{A}(G'(s)) = 1 \mid A^c] \Pr[A^c] \\
    &=  \frac{1}{2}(1) + \frac{1}{2}((1- \frac{1}{2^{n-1}})\frac{1}{2} + \frac{1}{2^{n-1}}) \\
    &= \frac{1}{2} + \frac{1}{4} - \frac{1}{2^{n+1}} + \frac{1}{2^n}\\
    &= \frac{3}{4} + \frac{1}{2^n} - \frac{1}{2^{n+1}} \\
    &= \frac{3}{4} + \frac{1}{2^{n+1}}
    \end{aligned}
  ~
    The first term in the last line follows from the fact that given $A$, $G(s) = G''(s) = G(s + 1)$,
    and the second term from the fact that given $A^c$, $G(s) = G''(s-1)$ and $G(s+1) = G''(s+1)$ which
    are essentially independent by the security of $G''$. Therefore, the probability that the first
    $\ell(n)$ bits of $y$ match the second $\ell(n)$ bits is $\frac{1}{2^{\ell(n)}}$ and we use LOTP.
    Similarly, we have:
   ~Math
    \begin{aligned}
       \Pr[\mathcal{A}(U_{2\ell(n)}) = 1] &= \frac{1}{2^{2\ell(n) - 1}} \\
    \end{aligned} 
   ~
   Then we have:
   ~Math
    |\E[\mathcal{A}(G'(s))] - \E[\mathcal{A}(U_{2\ell(n)})]| = \frac{3}{4} + \frac{1}{2^{n+1}} - \frac{1}{2^{\ell(n)}} 
   ~
   which is non-negligible. Therefore, for $\ell(n) > 2n$ and large $n$. Therefore, $G'$ is not a
   pseudorandom generator.
2. Excercise 3.12
~ Proof
We show the equivalence of the the PRF indistinguishability experiment, $PRF_{\mathcal{A},F}(n)$ and
the normal definition of PRFs given in 3.25. We do this be showing a reducing from one to the 
other and vice versa. To begin, suppose we're given a collection $\{f_k\}$ of PRFs such that the 
collection satisfies Definition 3.25. We show this implies success in $PRF_{\mathcal{A},F}(n)$ with
negligible probability.  
Suppose not. Then then exists $\mathcal{A}$ such that it can play the $PRF_{\mathcal{A},F}(n)$ and
succeed with non-negligible probability. Then construct the distinguisher $D$ as follows:
  1. On input $1^n$, $D$ simulates the $PRF_{\mathcal{A},F}(n)$ game. We ignore the results 
  of the $b \in_R \zo$, and instead do the following:
  2. When $\mathcal{A}$ queries the unknown function, $D$ forwards that query to its oracle. 
  3. At the end of all queries, $D$ outputs the output of $\mathcal{A}$. This means that if 
  $b  = 0$, $D$ has access to an $f \in Func_n$, and if $b = 1$ then $D$ has access to $f_k$. 
Now we show that $D$ is a distinguisher for $\{f_k\}$ PRF and $f \in Func_n$. 
~
~ Math
  \begin{aligned}
|\E[D^{f_k(\cdot)}(1^n)] - E[D^{f(\cdot)}(1^n)]| &= |\Pr[PRF_{\mathcal{A},F}(n) = 1] - \Pr[PRF_{\mathcal{A},F}(n) = 0]) \\
&> |\frac{1}{2} + p(n) - (\frac{1}{2} - p(n)) | \\
&= 2p(n)
\end{aligned}                                              
~
The first line follows from the fact that the probability of $D$ outputing a $1$ given that the 
simulated game has access to a $f_k$ is precisely equal to the probability that $\mathcal{A}$
outputs $b' = 1 = b$. In other words, the probability that $PRF_{\mathcal{A},F}(n)$ succeeds.
Similarly, the probability that $D$ outputs a $1$ given that the 
simulated game has access to $f \in Func_n$ is precisely equal to the probability that $\mathcal{A}$
outputs $b' = 1 \neq 0 = b$, so this is the probability that $PRF_{\mathcal{A},F}(n)$ fails. The 
second line follows from the fact that $\Pr[PRF_{\mathcal{A},F}(n) = 1] > \frac{1}{2} + p(n)$ and 
$\Pr[PRF_{\mathcal{A},F}(n) = 0] < \frac{1}{2} - p(n)$ for non-negligible $p(n)$ by our assumption.
However, as shown above, this leads to the distinguisher $D$ distinguishing between $f_k$ and
$f \in Func_n$ with non-negligible probability $2p(n)$. Note that as long as the game is 
restricted to a polynomial number of queries, $D$ is computationally efficient. Therefore, 
our assumption must be incorrect and Definition 3.25 implies the PRF indistinguishibility experiment.
~ Proof
For the other direction, we show that the PRF indistinguishibility experiment implies that 
$\{f_k\}$ is a PRF collection. Again, for the sake of contradiction, suppose this is not the case.
Then there exists a distinguisher $D$ that can distinghish between access to $f \in Func_n$ and
$f_k$ with non-negligible probability $p(n)$. Then play the PRF indistinguishibility experiment with $\mathcal{A} = D$. Then we have that:
~
~ Math
  \begin{aligned}
\Pr[PRF_{\mathcal{D},F}(n) = 1] &= \Pr[PRF_{\mathcal{D},F}(n) = 1 \mid b = 0]\Pr[b = 0] + \Pr[PRF_{\mathcal{D},F}(n) = 1 \mid b = 1]\Pr[b = 1] \\
&= \frac{1}{2}\Pr[D^{f_k(\cdot)}(1^n) = 1] + \frac{1}{2}\Pr[D^{f(\cdot)}(1^n) = 0] \\
&= \frac{1}{2}\Pr[D^{f_k(\cdot)}(1^n) = 1] + \frac{1}{2} - \frac{1}{2}\Pr[D^{f(\cdot)}(1^n) = 1] \\
&=  \frac{1}{2} + \frac{1}{2}|\E[D^{f_k(\cdot)}(1^n)]| - \frac{1}{2}|\E[D^{f(\cdot)}(1^n)]| \\
&> \frac{1}{2} + \frac{1}{2}|\E[D^{f_k(\cdot)}(1^n)] - \E[D^{f(\cdot)}(1^n)]| \\
&= \frac{1}{2} + \frac{p(n)}{2}
\end{aligned}
~
which is non-negligible. Therefore, we have an adversary that succeeds at the game. For an 
explanation, the first line follows by LOTP on condition on the result of the random bit. The
second line is simplification along with nothing that given the information on the bit, the
game succeeds when $b=1$ if and only if the distinguisher outputs $1$, and succeeds with $b = 0$
if and only if the distinguisher outputs $0$. The next line just uses the complement rule for 
probability. The next step is converting to expectation, then lastly noting that the expectations
are always positive and using the triangle inequality. The last step uses the fact that $D$ 
can distinguish with non-negligible probability $p$. 
The above leads to a contradiction, and therefore it must be the case that security under the
PRF experiment implies definition 3.25. The two definitions are equivalent. 

3. Excercise 3.13
We show that the keyed function defined as $F_{A,b}(x) = Ax + b$ for $A \in_R \mathbb{Z}_2^{n \times n}$ and
$b \in_R \mathbb{Z}_2^n$ is not a pseudo random function. We do this explicitly by constructing
a distinguisher.
~ Proof
Consider the following distinguishing algorithm $D$, which on input $1^n$ does the following:
  1. The algorithm queries $\mathcal{O}(0), \mathcal{O}(e_1)$, and $\mathcal{O}(e_2)$.
  2. The algorithm then queries $\mathcal{O}(e_1 + e_2)$. 
  3. If $\mathcal{O}(e_1 + e_2) = \mathcal{O}(e_1) + \mathcal{O}(e_2) - \mathcal{O}(0)$, the output $1$. Otherwise, output $0$. 
Note that the above distinguisher distinguishes with high probability between a random function 
and the keyed function $F_{A,b}(\cdot)$. 
~
~ Math
  \begin{aligned}
|\E[D^{F_{A,b}(\cdot)}(1^n)] - \E[D^{F(\cdot)}(1^n)]| &= |1 - \frac{1}{2^n}| = 1 - \frac{1}{2^n}
  \end{aligned}
~
which is non-negligible. The result follows from the fact that if we're querying $F_{A,b}$, then
$\mathcal{O}(0) = A0 = b, \mathcal{O}(e_1) = Ae+1 + b = A_1 + b, \mathcal{O}(e_2) = Ae_2 + b = A_2 + b$ where 
$A_i$ is the $i$-th column of $A$. Therefore, we have that 
~ Math
\begin{aligned}
  \mathcal{O}(e_1 + e_2) &= A(e_1 + e_2) + b \\
  &= Ae_1 + A_2e_2 + b \\
  &= A_1 + A_2 + b \\
  &= A_1 + b + A_2 + b - b \\
  &= \mathcal{O}(e_1) + \mathcal{O}(e_2) - b
 \end{aligned}
~
In the case where $\mathcal{O}$ is truly random, then all inputs are independent, and therefore
we have the probability of success as $\frac{1}{2^n}$ (number of total possible outputs). 
4. Excercise 3.29
We define the new encryption scheme from $(E^1,D^1)$ and $(E^2,D^2)$ as $(E_k,D_k)$ where the
key to the system is such that $k \in \zo^{2n}$ where $k = k_1 || k_2$ where $k \in_R \zo^{n}$ and
$k_2 \in_R \zo^n$. Then: 
$E_{k_1,k_2}(m) = E_{k_1}^1(r) || E^2_{k_2}(m \oplus r)$ and the decryption is given by
$D_{k_1,k_2}(c_1 || c_2) =D^2_{k_2}(c_2) \oplus D_{k_1}^1(c_1)$ where we've chosen $r \in_R \zo^{|m|}$.
~ Proof
We claim $(E,D)$ is CPA-secure given that at least one of $(E^i,D^i)$ for $i \in \{1,2\}$ is CPA-secure.
Suppose not. Then there exists an $Eve$ computationally efficient which on input $1^n$ and with 
access to $E_k$ can generate $m_0,m_1$ such that probability $\frac{1}{2} + p(n)$ it wins the
CPA game. We use this to break the security of both encryption $E^i$. To break $E^1$, consider
$Eve_1$ which on input $1^n$ does the following:

  0. Pick $k_2 \in_R \zo^n$ and gain access to $k_2 \to E^2_{k_2}$.
  1. Simulates $Eve$. 
  2. When $Eve$ requests an encryption of $m$, $Eve_1$ picks $r \in_R \zo^{n}$ and calculates
  $c_2 = E^2_{k_2}(m \oplus (m \oplus r))$ and obtains $c_1$ by using the access to $E^1_k$ to calculate 
  $c_1 = E^1_{k_1}(m \oplus r)$. Note that if $r$ is random, then $m \oplus r$ is random, so this
  satisfied the property required for encrypting the message $m$ with random $r' = m \oplus r$.
  3. We return the input $c_1 || c_2$ to $Eve$.
  4. $Eve$ selects after $poly(n)$ rounds as the challenge
  messages the messages $m_0, m_1$. $Eve_1$ then selects the messages $m'_0 = m_0 \oplus r$ and $m'_1 = m_1 \oplus r$
  as the challenge messages for some $r \in_R \zo^{|m_i|}$. 
  5. On receipt of $c^* = E^1_{k_1}(m_b')$ for $b \in_R \zo$, construct $c^*|| y$ where 
  $y = E^2_{k_2}(r)$ where $r$ is the random value selected previously. Feed this as
  the challenge ciphertext to $Eve$. 
  6. We return $b'$ returned by $Eve$. 
  
Then note that $Eve_1$ will correctly output $b = b'$ whenever $Eve$ outputs correctly $b = b'$. 
This is because the cipher-text submitted 
$c^* || y = E^1_{k_1}(m'_b) || E_{k_2}^2(r) = E^1_{k_1}(m_b \oplus r) || E_{k_2}^2(m_b \oplus (m_b \oplus r))$
where we can treat $r' = m_b \oplus r$ as essentially randomly chosen becase $r$ wasn randomly
chosen. Then $c^* || y = E^1_{k_1}(r') || E^2_{k_2}(m_b \oplus r') = E(m_b)$, so if $Eve$ 
guesses $b'$ correctly, then $Eve_1$ also guess correctly. Therefore, $Eve_1$ succeeds at the CPA
game with probability $\frac{1}{2} + p(n)$ where $p(n)$ is non-negligible. To break $E^2$, consider
$Eve_2$ which on input $1^n$ does the following:

 0. Pick $k_1 \in_R \zo^n$ and gain access to $k_1 \to E^1_{k_1}$.
  1. Simulates $Eve$. 
  2. When $Eve$ requests an encryption of $m$, $Eve_1$ picks $r \in_R \zo^{n}$ and uses the 
  accees to $E^2_{k_2}$ to calculate
  $c_2 = E^2_{k_2}(m \oplus r))$ and manually calculates $c_1 = E^1_{k_1}(r)$. 
  3. We return the input $c_1 || c_2$ to $Eve$.
  4. $Eve$ selects after $poly(n)$ rounds as the challenge
  messages the messages $m_0, m_1$. $Eve_1$ then selects the messages $m'_0 = m_0 \oplus r$ and $m'_1 = m_1 \oplus r$
  for $r \in_R \zo^{|m_b|}$ (we assume $|m_0| = |m_b|$). 
  5. On receipt of $c^* = E^2_{k_1}(m_b')$ for $b \in_R \zo$, construct $y||c^*$ where 
  $y = E^1_{k_1}(r)$ ($r$ is the same random value as before). Feed this as the challenge ciphertext to $Eve$. 
  6. We return $b'$ returned by $Eve$. 
  
Then note that $Eve_2$ will correctly output $b = b'$ whenever $Eve$ correctly outputs $b = b'$.
This is because the ciphertext submitted $y || c^* = E^1_{k_1}(r) || E^2_{k_2}(m_b') = E^1_{k_1}(r) || E^2_{k_2}(m_b \oplus r) = E_{k_1,k_2}(m_b)$
which $Eve$ can successfully guess with probability $\frac{1}{2} + p(n)$ for non-negligible $p(n)$. However,
this means that the existence of $Eve$ implies the existence of both $Eve_1$ and $Eve_2$, contradicting
the fact that at least one of our original schemes is CPA secure. Therefore, our assumption
that the new scheme is not CPA secure ($Eve$ exists) leads to a contradiction, therefore 
$(E,D)$ must be secure.

5. Excercise 4.3
6. Excercise 4.7
We show that each of the below MACs is insecure for $l > 1$.
  
  a. Authenticate the message $m = m_1, \cdots, m_l$ by computing $\tau = F_k(m_1) \oplus \cdots \oplus F_k(m_l)$.
  ~Proof
  We construct an adversary $\mathcal{A}$. On input $1^{|m|}$, $\mathcal{A}$ simply selects 
  the message $m' = m_1, \cdots, m_l$ where $m_i = m_j$ for all $i,j$ and the verification $\tau' = 0$ and
  submits $(m',\tau')$ as a forged signature. Then note that $V(m',\tau') = 1$ with probability $1$,
  since $F_k(m_i) = F_k(m_j)$ for all $i,j,k$, therefore $\oplus_{i=1}^{l} F_k(m_i) = 0$.
  ~
  b. Authenticate the message $m = m_1, \cdots, m_l$ by computing 
  $\tau = F_k(\langle 1 \rangle || m_1) \oplus \cdots \oplus F_k(\langle l \rangle || m_l)$.
  ~Proof
   We show the above is insecure for $l = 2$ (originally I did this, but now I generalize to
   arbitrary $l > 1$). 
   We construct an adversary $\mathcal{A}$.
   On input $1^{|m|}$, the adversary selects $m_1,m_2,m_3 \in_R \zo^{n/2}$ such
   that they are all distinct and $r \in_R \zo^{|m| - n}$ (the "remaining" message). 
   Then the adversary computes the signature $\tau_1 = S_k(m_1 || m_2 || r)$
   and $\tau_2 = S_k(m_2 || m_3|| r)$ and $\tau_3 = S_k(m_1 || m_3 || r)$. It then
   submits the message $m' = m_2 || m_2|| r$ with the tag $\tau '= \tau_1 \oplus \tau_2 \oplus \tau_3$.
   Then note that the verification algorithm always returns $1$. To see this, first let us define
   $g(m) = F_k(\langle 3 \rangle || m_3) \oplus \cdots \oplus F_k(\langle l \rangle || m_l)$ where
   $m = m_3 || m_4 || \cdots|| m_l$ where each $m_2 \in \zo^{n/2}$. Then we have:
  ~
  ~Math
    \begin{aligned}
      \tau_1 \oplus \tau_2 \oplus \tau_3 &= S_k(m_1 || m_2 || r) \oplus S_k(m_2 || m_3 || r) \oplus S_k(m_1 || m_3 || r) \\
      &= F_k(\langle 1 \rangle || m_1) \oplus F_k(\langle 2 \rangle || m_2) \oplus g(r)\\
      & \oplus F_k(\langle 1 \rangle || m_2) \oplus F_k(\langle 2 \rangle || m_3) \oplus g(r) \\
      &\oplus F_k(\langle 1 \rangle || m_1) \oplus F_k(\langle 2 \rangle || m_3) \oplus g(r) \\
      &= g(r) \oplus g(r) \\
      &\oplus F_k(\langle 1 \rangle || m_1) \oplus F_k(\langle 1 \rangle || m_1) \\
      &\oplus F_k(\langle 2 \rangle || m_3) \oplus F_k(\langle 2 \rangle || m_3) \\
      &\oplus F_k(\langle 1 \rangle || m_2) \oplus F_k(\langle 2 \rangle || m_2) \oplus g(r) \\
      &= F_k(\langle 1 \rangle || m_2) \oplus F_k(\langle 2 \rangle || m_2) \oplus g(r)\\
      &= S_k(m_2 || m_2 || r)
    \end{aligned}
  ~
  Therefore, the signature we submit is always valid, and $\mathcal{A}$ succeeds with probability $1$.
  c. Authenticate the message $m = m_1, \cdots, m_l$ by computing by choosing $r \in_R \zo^{n}$
  and computing $t = F_k(r) \oplus F_k(\langle 1 \rangle || m_1)\oplus \cdots \oplus \cdots F_k(\langle l \rangle || m_l)$
  and let the tag be $\tau = (r, t)$.
  ~Proof
   We show the above is insecure for $l=1$. We construct an adversary $\mathcal{A}$. On input
   $1^{|m|}$ simply selects the message $m' =  \langle 1 \rangle || m$ for some $m \in_R \zo^{n/2}$
   and then submits the tag $\tau = (m', 0)$. Then note that the verification algorithm always
   returns $1$ since $F_k(m') \oplus F_k(\langle 1 \rangle || m) = 0$ for all $k$. I do not
   know how to generalize this for arbitrary $l$, but $l = 1$ should suffice to show insecurity.
  ~
7. Excercise 5.3
We show that for a collision resistant hash function $H^s$, $\hat{H}^s = H^s \circ H^s$ is also
collision resistant.
~Proof
 Suppose that $\hat{H}^s$ is not collision resistant. Then there exists an efficient adversary
 $\mathcal{A}$ such that $A(s) = (x,x')$ where $x \neq x'$ yet $\hat{H}^s(x) =  \hat{H}^s(x')$. 
 Then note:
 ~
 ~Math
  \begin{aligned}
  \hat{H}^s(x) &= \hat{H}^s(x') \\
  \implies H^s(H^s(x)) &= H^s(H^s(x'))
  \end{aligned}
 ~
 In the above, one of two scenarios is true. Either $H^s(x) \neq H^s(x')$ yet we have that
 $H^s(H^s(x)) = H^s(H^s(x'))$, in which case we have just found a collision in $H^s$ with
 inputs $y = H^s(x)$ and $y' = H^s(x')$ where $y \neq y'$. Or, $H^s(x) = H^s(x')$, in which case we have found
 a collision in $H^s$ with inputs $x,x'$ where we know $x \neq x'$. Note that by the above,
 we can construct an new adversary $\mathcal{A}'(s)$ which simply uses the adversary $\mathcal{A}$
 as a sub-routine to calculate $x,x'$, and then checks to see if $H^s(x) = H^s(x')$. It can
 do this efficiently since the map $(k,x) \to H^k(x)$ is efficient. If the previous is true,
 then it returns $(x,x')$, otherwise it returns $(H^s(x), H^s(x'))$. By the previous argument,
 the output leads to a collision and the outputs are not equivalent. Therefore, $H^s$ is not 
 collision resistent, a contradiction. This means that our assumption that $\hat{H}^s$ was not
 collision resitent is wrong, therefore $\hat{H}^s$ is collision resistent.
8. Excercise 5.10
We study the $MAC$ defined as $MAC_{s,k}(m) = H^s(k || m)$.
  a. We show that under the Merkel-Damgard transform, this MAC is not secure ($s$ is known, $k$ is secret).
  ~Proof
  We simply perform a length extension attack. Because $H$ is constructed using the Merkel-Dambard
  transform, we know that $H^s(x) = h(h(h(x_1 || s) || x_2), \cdots || x_t)$ when the input is 
  composed of $t$ blocks (padded if needed) where $h: \zo^{2n} \to \zo^n$ is  a compression function.
  Then our adversary $\mathcal{A}$ does the following:
    
    1. Choose random $t \in \mathbb{Z}$ and generate random $m \in \zo^{tn}$. 
    2. Request the signature $\tau = MAC_{s,k}(m)$.
    3. Select random $m_{t+1} \in \zo^n$ and compute $\tau' = h(\tau, m_{t+1})$ (this is efficient).
    4. Output $(m || m_{t+1}, \tau')$.
    
  Then note that our adversary succeeds in passing the verifcation algorithm with probability $1$.
  To see this, note:
  ~
  ~Math
    \begin{aligned}
   MAC_{s,k}(m || m_{t+1}) &= H^s(k || m || m_{t+1}) \\
   &= h(H^s(k || m) || m_{t+1}) \\
   &= h(MAC_{s,k}(m) || m_{t+1}) \\
   &= h(\tau, m_{t+1}) \\
   &= \tau'
   \end{aligned} 
  ~
  Therefore, the MAC is not secure. 
  b. If we model $H$ as a random oracle, then the above is secure. 
  ~Proof
  For contradiction, suppose it were not secure under the random oracle. Then we would
  be able to predict the output of $H$, contradicting the fact that it's a random function.
  To do this, take the adversary $\mathcal{A}$ which wins CPA game when given access to the 
  signing oracle. Then the new advesary $\mathcal{A}'$ does the following:
  
    1. Pick $k$ uniformly at random.
    2. On a signature request from $\mathcal{A}$ for message $m$, calculate $H(k || m)$, and return the result to $\mathcal{A}$.
    3. After some $poly(n)$ requests, $\mathcal{A}$ will output $(m,\tau)$ which with non-negligible
    probability succeeds at the verification process.
    4. Then note that $H(k || m) = \tau$ with non-negligible probability, so we can predict the outout
    of $H$ for unseen input $x = k || m$. 
  ~
  The above means we can non-negligibly predict the output of a completely random function, a
  contradiction on our assumption, so it must be the case that the MAC is secure.
  