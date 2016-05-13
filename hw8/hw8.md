% Homework 8: Fully homomorphic encryption
~MathDefs
\newcommand{\zo}{\{0,1\}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\getsr}{\leftarrow_R\;}
\newcommand{\Gp}{\mathbb{G}}
\newcommand{\iprod}[1]{\langle #1 \rangle}
\newcommand{\Epubcca}{E^{pub,cca}}
\newcommand{\Epubcpa}{E^{pub,cpa}}
\newcommand{\Epriv}{E^{priv,cca}}
\newcommand{\Sign}{S}
\newcommand{\Ver}{V}

\newcommand{\floor}[1]{\lfloor #1 \rfloor}
\newcommand{\ceil}[1]{\lceil #1 \rceil}

\newcommand{\cF}{\mathcal{F}}

\newcommand{\onand}{\overline{\wedge}}
~

### Total of 145 points

1. (45 points) The _Pallier  cryptosystem_ is defined as follows:

    * **Key generation:** Choose $m=pq$ where $p,q$ are random $n$ bit primes (e.g., chosen randomly in the interval $[2^{n-1},2^n]$). Choose $g$ to be a random element in $\Z^*_{m^2}$. The public key is $m,g$ and the secret key is the factorization $p,q$.  

    * **Verifying keys:** Define  $\lambda$ be the least common multiple of $(p-1)$ and $(q-1)$ and for every $a\in\Z^*_{m^2}$ let
    $\Lambda(a)= \floor{\tfrac{(a^\lambda \mod m^2)-1}{m}} (\mod m)$. If $gcd(\Lambda(g),m)\neq 1$ then regenerate the keys.  

    * **Encryption:** To encrypt a message $x\in\Z*_m$, choose $r$ at random in $\Z^*_m$ and output $g^xr^m (\mod m^2)$.

    * **Decryption:** To decrypt the ciphertext $c$, output $\Lambda(c)\Lambda(g)^{-1} (\mod m)$.  

    It can be shown to be CPA secure under reasonable number theoretical assumptions, similar to those underlying RSA.

      a. (15 points) Prove that if the key passes the check then the order of $g$ in $\Z^*_{m^2}$ is a multiple of $m$.

      We begin by first recalling the definition of Euler's $\phi$-function. Let $x = \prod_i p_i^{k_i}$
      for distinct prime. Then the function phi is defined as:
      ~Math
      \phi(x) = \prod_{i} p_i^{k_i - 1}(p_i - 1)
      ~
      ~Lemma
      First,we prove a relaxed version of Carmichael's Theorem. This is to say, we prove that $\forall x \in Z_{m^2}^*$,
      $x^{m\lambda} = 1 \pmod {m^2}$. First, note that the order for each of the following: 
      ~
      ~Math
      \begin{aligned}
         |\Z_{p^2}^*| = \phi(p^2) = p(p-1) \\
         |\Z_{q^2}^*| = \phi(q^2) = q(q-1)
      \end{aligned}
      ~
      Then note that $x^{m\lambda} = x^{pq\lambda} = x^{p(p-1)c_1q} = (x^{p(p-1)})^{c_1q} = 1^{c_2p} = 1 \pmod {p^2}$ and similarly $x^{m \lambda} =
      x^{pq\lambda} = x^{q(q-1)c_2p} = (x^{q(q-1)})^{c_2p} = 1^{c_2p} = 1 \pmod {q^2}$, which by the Chinese remainder theorem, implies that
      $x^{m\lambda} = 1 \pmod {p^2q^2}$, where we note that $m^2 = p^2q^2$. 
      
      We now continue the proof. Note that $gcd(\Lambda(g), m) = 1 \implies \Lambda(g) \neq 0$. This
      means that $g^{\lambda} \pmod {m^2} \neq 1 \pmod{m}$. Then it must be the case the order of $g$
      is a multiple of $m$. Suppose this is not the case, and we have in fact that $g^a = 1 \pmod {m^2}$
      where $a = order(g)$ and $a = km^2 + q$ for some $k,q \neq 0$. Then by Lemma 1, this implies that 
      $g^{\lambda} =1 \pmod {m^2}$, a contradiction on the fact that $\Lambda{g} \neq 0$. Therefore, it must be the case that the order of $g$ is a multiple of $m^2$. 
      
      [&box;]{float=right}
      
      b. (15 points) Prove that decryption works: for every message $x\in \Z^*_m$, the decryption of the encryption of $x$ is $x$ (i.e., $D_d(E_e(x))=x$).
     ~Proof
     We first define the function $E_g: \Z_m \times \Z^*_m \to \Z_{m^2}^*$ as the map 
     which computes the encryption, aka $E_g \mapsto g^xy^n \pmod n^2$. Then note
     that our encryption for message $m$ is simply given by $E_g(m,r)$ for $r \in_R \Z_m^*$. First,
     we show that the function is bijective.
     ~
     ~Lemma
     We show that $E_g$ is bijective. Note that $|Z_m \times Z_m^*| = m\phi(m) = |\Z_{n^2}^*|$ so it
     suffices to show that $E_g$ is injective. Pick some $(m_1,r_1),(m_2,r_2) \in \Z_m \times Z_m^*$
     such that $E_g(m_1, r_1) = E_g(m_2,r_2)$. Then note the following:
     ~
     ~Math
      \begin{aligned}
        g^{m_1}r_1^n = E_g(m_1,r_1) = E_g(m_2,r_2) = g^{m_2}r_2^n \pmod{m^2}\\
        \implies g^{m_1 - m_2}r_1^m = r_2^m \pmod{n^2} \\
        \implies  g^{(m_1 - m_2)\lambda} r_1^{\lambda m} = r_2^{m\lambda} \pmod{m^2} \\
        \implies g^{(m_1 - m_2)\lambda} = 1 \pmod {m^2}
      \end{aligned}
     ~
     where the last line follows from Lemma 1. The above means that the order of $g$ must divide
     $(m_1 - m_2)\lambda$, which by part (a) given our choice of $g$, implies that $m \mid (m_1 - m_2)\lambda$
     because we know that the order of $g$ is a multiple of $m^2$ and therefore a multiple of $m$,
     which means $m \mid (m_1 - m_2) \lambda$. Now, since $gcd(\lambda,m) = 1$, it must be the 
     case that $m \mid (m_1 - m_2)$. This implies that:
     ~Math
      \begin{aligned}
      (m_1 - m_2) &= 0 \pmod m \\
      \implies m_1 &= m_2 \pmod m \\
      \implies m_1 &= m_2
      \end{aligned}
     ~ 
     This givens us that $r_1^m = r_2^m$, which by the bijection of exponentiation for $r \in \Z^*_m$,
     we must necessariyl have that $r_1 = r_2$. Therefore, $(m_1,r_1) = (m_2, r_2)$, and this concludes
     the proof that the function is bijective. Given that the function is bijective, it is therefore
     invertible, so we introduce the following notation for the decryption:
     ~Math
     [c]_g = E_g^{-1}(c)_1
     ~
     which consists of inverting the above function and selecting the message (ignoring the second tupler
     parameter). We now show that for any $w \in \Z_{m^2}^*$:
     ~Math
      \Lambda(w) = \lambda [w]_{m+1} \pmod {m}
     ~
     This follows easiy:
     ~Math
     \begin{aligned}
        w &= E_{n+1}([w]_{m+1}, r) \pmod{m^2} \\ 
        &= (n+1)^{[w]_{m+1}}r^m \pmod{m^2} \\
        \implies w^{\lambda} &= (n+1)^{[w_{m+1}\lambda]}r^{m\lambda} \pmod{m^2} \\
        &= (n+1)^{[w_{m+1}\lambda]} \pmod{m^2} \\
        &= 1 + \lambda[w]_{m+1}m \pmod{m^2} \\
        \implies \frac{w^{\lambda} - 1}{m} &= \lambda[w]_{m+1} \pmod m \\
        \implies \Lambda(w) &= \lambda[w]_{m+1} \pmod m 
     \end{aligned} 
     ~
     which concludes our sub-claim. Correctness then continues by noting that:
     ~Math
     \begin{aligned}
     \Lambda(c)\Lambda^{-1}(g) &=  \lambda[c]_{m+1} (\lambda [g]_{m+1})^-1 \pmod m\\
     &= [c]_{m+1}[g]_{m+1}^{-1}\pmod m \\
     &= [c]_g \pmod m
     \end{aligned}
     ~
     where we now spend our efforts on showing why the last line is true. First, consider the following 
     three relations for any $c \in \Z_{n^2}^*$ and $g_1,g_2$ valid keys. 
     ~Math
     \begin{aligned}
        c &= E_{g_1}([c]_{g_1},r_1) = g_1^{[c]_{g_1}}r_1^m \pmod m \\
        c &= E_{g_2}([c]_{g_2},r_2) = g_2^{[c]_{g_2}}r_2^m \pmod m\\
        g_1 &= E_{g_2}([g_1]_{g_2},r_3) = g_2^{[g_1]_{g_2}}r_3^m \pmod m\\
        g_2 &= E_{g_1}([g_2]_{g_1},r_4) = g_1^{[g_2]_{g_1}}r_4^m \pmod m\\
     \end{aligned}
     ~
     Plugging equation 4 into 2 and equation 3 into 1, we have
     ~Math
     \begin{aligned}
        c &= (g_1^{[g_2]_{g_1}}r_4^m)^{[c]_{g_2}}r_2^m = E_{g_1}([c]_{g_2}[g_2]_{g_2}, r_2r_4^{[c]_{g_2}}) \pmod m\\
        c &= (g_2^{[g_1]_{g_2}}r_3^m)^{[c]_{g_1}}r_1^m = E_{g_2}([c]_{g_1}[g_1]_{g_2}, r_1r_4^{[c]_{g_1}}) \pmod  m
     \end{aligned}
     ~
     However, equation Equation i with i for $i\in \{0,1\}$, from above, we arrive at the following,
     using the fact that $E_g$ is injective:
     ~Math
     \begin{aligned}
      [c]_{g_2}[g_2]_{g_2} &= [c]_{g_1} \pmod m \\
      [c]_{g_1}[g_1]_{g_2} &= [c]_{g_2} \pmod m
     \end{aligned}
     ~
     If we substitute the second above into the first, we have:
     ~Math
      [c]_{g_1}[g_1]_{g_2}[g_2]_{g_2} = [c]_{g_1} \implies [g_1]_{g_2}[g_2]_{g_2} = 1 \pmod m
     ~
     Which means that $[g_1]_{g_2}$ is invertible. Therefore, we can substitute this result into
     the second equation above:
     ~Math
     [c]_{g_1} = [c]_{g_2}[g_1]_{g_2}^{-1} \pmod m
     ~
     with the above in place, letting $g_1 = g, c = c, g_2 = m+1$, the last line from our original
     proof follows immediately. Note that $m+1$ is a valid key. 
     
      c. (15 points) Prove that it is _additively homomorphic_:  for every $x,x' \in \Z^*_m$, $D_d(E_e(x)E_e(x') (\mod m^2)) = x+x' (\mod m)$.
      
      ~Proof
      Note that $E_e(x)E_e(x') = g^xr^mg^{x'}r'^m = g^{x + x'}(rr')^{m} = E_e(x+x',rr')$ where 
      $r,r' \in \Z_m^{*} \pmod m \implies rr' \in Z_m^{*} \pmod m$. Then the decrpytion algorithm
      will still succeed since none of the above properties have changed, and this reuslts in
      $D(E_e(x)E_e(x')) = m_1 + m_2$. 
      ~
     After you finish this exercise you might want to look at [this document](http://security.hsr.ch/msevote/seminar-papers/HS09_Homomorphic_Tallying_with_Paillier.pdf) from 2009 showing how Pallier encryption can be used for electronic voting. Note the list of "joke candidates" for the world parliament they used on page 8.  

2.  (60 points) Consider the following zero knowledge protocol based on a homomorphic encryption scheme $(G,E,D,EVAL)$ for a family $\cF$:

    The public input is $f\in\cF$ where $|f|=poly(n)$ and maps $\zo^n$ to $\zo$. Alice has an input $x\in\zo^m$ such that $f(x)=1$ and wants to prove to Bob that there exists such an $x$.
    We consider the following protocol for this task:

    __Protocol 1:__

    * Alice generates $(e,d)$ keys for the encryption scheme, and sends $e$, together with $(c_1,\ldots,c_n)$ where $c_i =E_e(x_i)$ to Bob

    * Bob chooses $b\getsr\zo$. If $b=1$ then Bob sends $c'=EVAL(c_1,\ldots,c_n,f)$ to Alice and if $b=0$ then Bob sends $c'=E_e(0)$.

    * Alice responds with $b'=D_d(c')$ and Bob accepts if $b=b'$.

      Lets say that the scheme has the _identical ciphertexts_ property if for every string $e$ there exists a unique private key $d$ such that $(e,d)$ has nonzero probability to be  output by the generator $G$  and for every ciphertexts  $c_1,\ldots,c_n$ and function $f\in\cF$, the distribution of $EVAL(c_1,\ldots,c_n,f)$ and $E_e(f(D_d(x_1),\ldots,D_d(x_n)))$ is _identical_. (The randomness in this distribution is only over the potential internal coins of $EVAL$ and $E$.[^note])

      [^note]: The $EVAL$ procedures we saw in the lectures were deterministic but we can get probabilistic variants of them that satisfy a close enough property to the weak identical ciphertexts property.
      a. (20 points) Prove that if the scheme has the identical ciphertexts property then Protocol 1 is honest verifier zero knowledge, in the sense that it satisfies completeness (with probability $1$), soundness (success in cheating is at most $0.9$) and the zero knowledge property is guaranteed if the verifier follows the protocol.
      ~Proof
      For completeness, we need to show that if $f(x) = 1$, then Bob accepts with probability at least
      $0.9$. We condition on the results of $b$. Suppose $b = 0$, then Alice receives $c' = E_e(0)$. 
      In this scenario, Alice will always return $b = 0$, and therefore Bob will always accept. Now
      consider the scenario where $b = 1$. Then Bob sends $c' = EVAL(c_1, \cdots, c_n, f)$. By the
      identical ciphertext property, assuming that Alice is using the unique private key $(e,d)$ which
      was generated with non-zero probability by $G$, then the probability that $c' \neq E_e(f(D_d(c_1),\cdots,D_d(c_n)))$ is 
      negligible because the distributions are identical. Then note that:
      ~
      ~Math
       E_e(f(D_d(c_1),\cdots,D_d(c_n))) = E_e(f(x))
      ~
      which is $1$, by assumption, and therefore with very high probability, $Bob$ verifies the result
      correctly. 
      We now prove soundness. Suppose $f(x) \neq 1$. Then no matter what Alice does to cheat, she
      cannot trick Bob into verifiying a result that is not correct. Alice is unaware of whether 
      she is receiving $E_e(0)$ (in which case she would want to return $0$), or $c'$,
      in which case she would want to respond with $1$. In the given protocol, Alice has no 
      way of guessing which is which because in both scenarios, Bob returns an encryption which
      encrypts to $0$. Therefore, there's only a $50/50$ chance that Alice wil succeed in tricking
      Bob, and Bob can minimize this to a negligle amount by repeating the protocol.
      Note that the zero knowledge property follows from the fact that if Bob follows the protocol,
      then he never gains any knowledge as to the value of $x$. This is guaranteed by the fact that
      $x$ is encrypted when sent to Alice, and the calcualtions that Bob performs on it are done
      under a fully homomorphic encryption schema and therefore he learns not the value of $x$. 
      ~
      b. (20 points) Prove that Protocol 1 is _not_ zero knowledge with respect to a malicious verifier by giving a verifier strategy that always succeeds in learning the 17th bit of $x$.
      ~Proof 
      The malicious verifier simply submits $c_{17}$ to the prover. Then following the protocol,
      the prover will return $D_d(c_{17})$, which is equivalent to $x_{17}.$ In this way, the
      malicious verifier always suceeds in learning the 17-th bit of $x$.  
      ~
      Consider now the following protocol:

      __Protocol 2:__

      * Alice generates $(e,d)$ keys for the encryption scheme, and sends $e$, together with $(c_1,\ldots,c_n)$ where $c_i =E_e(x_i)$ to Bob

      * Bob chooses $b\getsr\zo$ and $z\getsr\zo^{3n}$. If $b=1$ then Bob sends $z$ and $c'=EVAL(c_1,\ldots,c_n,f)$ to Alice and if $b=0$ then Bob sends $z$ and $c'=E_e(0)$.

      * Alice computes $b'=D_d(b)$ and chooses $w\getsr\zo^n$ and sends to Bob $y=PRG(w)+ b'z (\mod 2)$.

      * Bob sends to Alice all the internal randomness used to compute $c'$.

      * Alice checks that $c'$ was indeed computed as desired and if so sends to Bob $w$ and $b'$. Bob accepts if it indeed holds that $y=PRG(w)+b'z (\mod 2)$ and $b'=b$.

      c. (20 points) Prove that if the scheme has the identical ciphertexts property then Protocol 2 is a zero knowledge protocol (without the need for zero knowledge)
      ~Proof
      Not done :(
      ~
3. (40 points) Hash functions and public key encryption seem like very different creatures, but here we will show a relation between the two. Recall that a  collection of functions $H$ mapping $10n$ bits to $n$ bits is a _collision resistant hash function_ collection if for every efficient adversary $A$, if $A$ is given $h$ chosen at random from $H$, then the probability that $A$ outputs $x \neq x' \in \zo^{10n}$ such that $H(x)=H(x')$  is negligible.
Let $(G,E,D,EVAL)$ be an XOR homomorphic encryption scheme with respect to XOR's of $10n$ ciphertexts, where $n$ is the length of the ciphertext.
Consider the following collection of functions $H$: to choose a random $h\in H$, we generate keys $(e,d)$ for the homomorphic crypto system, and let $c_1,\dots,c_{10n}$ be $n$ independent encryptions of zero. The function $h$ will map $x\in\zo^{10n}$ to $EVAL(f,c_1,\ldots,c_{10n}$ where $f(b_1,\ldots,b_{10n}) = \oplus_{i:x_i=1} b_i$.
Prove that if the scheme is CPA secure then this  hash function collection is collision resistant. See footnote for hint[^hint]
~ Proof
Following the hint, suppose that there exists an efficent adversary that can find a collision in
some $h$ chosen at random. Then we use this adversary to construct an adversary to our homorphic
encryption scheme which breaks CPA security. When given a ciphertext $c$ that we don't know whether
it is the encryption of $0$ or $1$, we first compute $10n -1$ encryptions of $0$ and set our 
encryption as $1$ for some random bit index $i$. Suppose this is how we define $h$, which is a homomorphic
scheme evaluating $f$ in our input where $f$ is defined to be the XOR of the ciphertext bits.
Then we run the adversary $A$ with this hash function and it will return two values, $x,x'$ such 
that $h(x) = h(x')$.
~

[^hint]: If there is such an adversary $A$ and we are given a ciphertext $c$ that we don't know if it is an encryption of $0$ or an encryption of $1$, we can make $10n-1$ encryptions of $0$, and plug $c$ in some random index $i$ among them to obtain $c_1,\ldots,c_{10n-1}$ such that $c_i=c$, and use this tuple to define $h$. Then we could argue that the probability that the adversary outputs $x$ and $x'$ that satisfy $h(x)=h(x')$ but disagree on the $i^{th}$ bit is different depending on whether  $c$ was an encryption of $0$ opposed to when it was an encryption of $1$.
