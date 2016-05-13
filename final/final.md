CS127 Take home final - Spring 2016
Boaz Barak

<!-- Good luck and I hope you also enjoy working on this, at least a little bit :) -->
~ MathDefs

\newcommand{\zo}{\{0,1\}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\R}{\mathbb{R}}
\newcommand{\getsr}{\leftarrow_R\;}

\newcommand{\floor}[1]{\lfloor #1 \rfloor}
\newcommand{\ceil}[1]{\lceil #1 \rceil}
\newcommand{\iprod}[1]{\langle #1 \rangle}

\newcommand{\cO}{\mathcal{O}}

\newcommand{\Gp}{\mathbb{G}}
~

## Instructions.

You have 48 hours or until 11:59pm on Monday May 9 (whichever is earlier) to complete this exam. You should submit your written exam as a PDF file.
**You can submit only once so don't submit the final until you have completed it.**
You should submit the final via canvas, but as a backup please **also email it to me** (I will ignore the emailed copy unless there is a technical problem with canvas).
If you have any questions, please contact me or the TF's via email or Piazza (please use private questions only since other students might have not started the exam).

For any question you solve, write out your solution **clearly** and **fully**.
If you give a construction of a cryptographic scheme, spell out all for each algorithm what it does- what is the input, what is the randomness, and what is the output.
If you are giving a reduction, say exactly how it operates - what is the input and the output.
If you are giving an analysis, be clear about your statements (e.g. "Claim: | Pr[ Eve wins in experiment 1] - Pr[ Eve wins in experiment 2 ] | < $negl(n)$" ) and your proofs.

Remember: the first step of writing a proof is writing a clear statement that _you are convinced of its truth yourself_.  
If you are not sure yourself, just explain the parts you conjecture but are unable to prove, and you will get partial credit.

State precisely every claim you use and either prove it or say where it was proven (e.g., KL book Lemma 2 (2nd ed, page 111), or lecture notes for lecture #14).
You can quote without proof results that were stated or proven in class, or in the Katz-Lindell or Boneh-Shoup textbooks, but you should give the source and make sure the result is stated as you used it.

You can use any of the following sources:

* The lecture notes, videos, homeworks, Piazza board.
* Any notes that you or your classmates prepared in advance to working on the exam, which can be based on anything (the class or any other sources).
* The Katz Lindell textbook.
* The Boneh Shoup textbook.

You should not use any other resource. Needless to say, you should work on this exam alone.

You can use the following assumptions if you need them:

* Existence of pseudorandom generators (and all that it implies, including pseudorandom functions,  CPA and CCA secure commitment schemes, zero knowledge proofs)

* The LWE assumption, difficulty of  factoring random integers of the form $m=pq$, discrete logarithm and Diffie-Hellman problem (for a prime order group in the multiplicative group modulo some other prime), and all that these imply (in particular public key encryption, fully homomorphic encryption, digital signatures, collision resistant hash functions).

* The random oracle heuristic.

For example, if you are asked to construct an object satisfying property X, then you can use any of the above objects as building blocks.
On the other hand, obviously if the question explicitly says what security properties a certain object satisfies, then you cannot assume it satisfies additional properties as well.
(e.g., just because you can assume that _some_ CCA secure encryption scheme exists, doesn't mean that if a question says that $(E,D)$ is a CPA-secure encryption then you can assume that it is also CCA secure.)

**Honor code:** Throughout the term I've been impressed by the dedication and integrity of the students in this class, and I have full faith that you will continue to do so while working on this exam.

_Good luck!!_



## Questions

This exam has 4 questions with a total of 135 points (plus one additional bonus question worth 20 points).


__Question 1 (40 points):__ Here is a technical abstraction of the security mechanism in the iPhone as relevant to the Apple vs FBI case.  Let $(E,D)$ be a private key encryption scheme taking $n$ bit keys and having $n$ bit messages. Let $\{ f_s \}$ be a collection of functions where for $s\in\zo^n$, $f_s$ is an efficiently computable map from $\zo^{1+n+20}$ to $\zo^n$.
Imagine that there is a processor that is treated as a black box $F$ and has keys $s, uid \in \zo^n$ embedded inside it, as well as a string $z\in\zo^n$ and a counter $count$ initiated to zero.
On input $p\in\zo^{20}$, if $count \geq 10$ then the processor $F$ always outputs ```error```. If $count<10$ then it checks if $f_s(0\|uid\|p)=z$ and if so outputs $k=f_s(1\|uid\|p)$. Otherwise it outpus ```error``` and increments the counter $count$.
The idea is that $k$ will be the key used to encrypt the memory.

We say that this mechanism is _secure_ if for every efficient adversary $A$,  the probability that $A$ wins the following game is at most $0.51$:

1.  The keys $s,uid$ are chosen at random and independently in $\zo^n$ and $p$ is chosen randomly in $\zo^{20}$. We set $z=f_s(0\|uid\|p)$ and $k=f_s(1\|uid\|p)$. The adversary does _not_ learn those keys nor $z$.
2. The adversary gets a polynomial in $n$ number of calls to the processor $F$ (but the processor updates its state between these calls). It does not get any other oracle calls, including to the encrytion or decryption box.
3. The adversary then chooses two messages $m_0,m_1$ and outputs them.
4. We choose $b\getsr \zo$ and send $c^* = E_k(m_b)$ to the adversary.
5. The adversary outputs $b'$ and _wins_ if $b=b'$.

In each one of the following questions, either prove that the scheme is secure or give a counterexample by instantiating the private key scheme $(E,D)$ and the collection of functions $\{ f_s \}$ in a way that satisfies the assumptions of the questions but such that the overall mechanism is insecure.

a. (8 points) Prove or give a counterexample: the mechanism is secure when $(E,D)$ is a CCA secure encryption and $\{ f_s \}$ is a pseudorandom function collection.
  
  We prove that the system is secure.
  
  The intuition behind the security of the system is that having access to $F$ is either useless, or
  useful in a way that violates the PRF assumption.
  
  We prove that the system is secure. We do this by contradiction. We begin by proving that the 
  distribution $k = f_s(1 \| uid \| p)$ is indistinguishable from the uniform distribution.
  ~Proof
   Suppose this is not the case. Then for $s,uid \in_R \zo^n$ and $p \in_R \zo^{20}$, there exists a distinguisher
   $D$ that can successfully distinguish between $U_n$ and $f_s(1 \| uid \| p)$ (output $1$ if $x = f_s(1 \| uid |\ p)$ 
   and output $0$ otherwise. More formally, we have:
  ~
  ~Math
    |\E[D(f_s(1 \| uid \| p))] - \E[D(U_n)]| > p(n)
  ~ 
  where $p(n)$ is non-negligible. 
  We use $D$ to construct
  an adversary $A$ that winds the PRF Game. On input $1^n$ the adversary does as follows:
  
    1. The adversary queries the blackbox $x \mapsto F(x)$.
    2. On receiving the result $y = F(x)$, $A$ submits this result $y$ to $D$.
    3. The adversary outputs the result output by $D$.
  
  We claim that the probability of success is $1/2 + p(n)$. We condition on the choice $b$ performed
  in the PRF game. Then the probability of success is:
  ~Math
   \begin{aligned}
     \Pr[A(1^n) = 1 \| b = 1]\frac{1}{2} + \Pr[A(1^n) = 0 \| b = 0] \frac{1}{2} &\geq (1/2 + p(n))(1/2) + (1/2 + p(n))(1/2) \\
     &= \frac{1}{2} + p(n)
   \end{aligned} 
  ~
  Yet, the above is a contradiction on the fact that $\{ f_s \}$ is a pseudorandom function collection. Therefore,
  we must have that the distribution $k = f_s(1 \| uid \| p)$ is indistinguishable from $U_n$ given that
  $s, uid \in_R \zo^n$ and $p \in_R \zo^{20}$. 
  [&box;]{float=right}
  
  We continue by showing a few properties about a successful adversary $A$. We prove that the system
  above is secure by assuming that it's not, and showing that this leads to an insecurity in a 
  CCA secure system. We can, broadly, speaking, categorize an efficient and successful adversary $A$
  into two types. Either the adversary $A$ succeeds in guessing a value $p$ such that the counter
  does not surpass $10$ and the $k$ is retrived, or the adversary $A$ succeeds without ever guessing
  such a value $p$. 
  
  In the first case, suppose the adversary $A$ succeeds by guessing correctly a value $p$ such 
  that it can retrieve the key. We show that this leads to a contradiction on the assumption that 
  $\{ f_s\}$ is a pseudorandom function collection.
  
  ~Proof
   Suppose the above mentioned adversary $A$ exists. Then we can use $A$ to construct an adversary $A'$
   which can successfully distinguish between $f_s(U_{n+21})$ and $U_{n}$. The adversary $A'$ works
   as follows, essentially simulating a black-box game with $A$. 
   
    1. On input $z \in \zo^n$, $A'$ executes $A$. 
    2. $A'$ picks $s,uid \in_R \zo^n$ uniformly at random. 
    3. On each request, $p$, sent by $A$, $A'$ checks to see if $f_s(0 \| uid \| p) = z$. In 
    the case where this is false, $A$ increases a local counter by 1. After 10 failed attempts,
    $A$ always replies with "invalid".
    4. In the case where the check succeeds, $A'$ terminates the simulation and outputs $1$.
    5. Once the counter reaches $10$ (all future queries will be invalid), $A'$ outputs $0$.
   
   The first claim is that in the case where $z = f_{s'}(0 \| uid' \| p')$ for some $s',uid' \in_R \zo^n$
   and $p' \in \zo^{20}$, $A$ cannot distinguish between the simulated responses and the responses
   that would have normally been given. This is because $A$ does now know $s',uid',p'$ since they were
   chosen at random, and given that $z$ is an output of $f_s$, the probability that the values 
   are equal leads to indistinguishable distributions. However, in this scenario, by assumption, it
   must be the case that $A$ succeeds in retrieving the key without surpassing the $10$ count, 
   and therefore $A'$ outputs $1$. 
   
   The second claim is that in the case where $z \in U_n$, the probability that $A$ succeeds 
   in $10$ attempts is at most $\frac{10}{2^n}$, which is negligible. Therefore, with
   probability $1 - negl$, $A$ will exceed the counter and $A'$ will output $0$.
   
   Putting the results above together, we have $
     |\E[A'(f(U_{n + 21}))] - \E[A'(U_n)]| = | 1 - negl | = 1 - negl$ 
   
   Note that we have already shown that the family of pseudorandom functions produces values 
   which are computationally indistinguishable from the uniform. Therefore, assuming the existence of
   $A$ leads to a contradiction because we can use $A$ to construct $A'$. We conclude that such an
   adversary $A$ cannot exist. 
   [&box;]{float=right} 
  ~
  
  We now consider the second scenarior where the adversary $A$ succeeeds in the game without correcting
  guessing the password $p$. We note that this also leads to a contradiction.
  ~Proof
    We construct the adversary $A'$ with the purposes of succeeding in the CCA-game. Even without access
    to the encryption and decryption boxes, the adversary $A'$ simply runs the adversary $A$ until 
    it successfully produces two messages, $m_0,m_1$. All queries to the black box $F$ are replied to
    with "error". On receiving the challenge ciphertext $c^* = E_k(m_b)$, we submit the ciphertext
    to $A$ and output the same result.
    
    It is immediate that $A'$ succeeds if and only if $A$ succeeds, by construction. we furthermore
    note that $A$ cannot distinguish between the simulated interaction and a normal interaction (in 
    the normal interaction, the box was completely useless!) as all replies were invalid after 
    some point. However, $A$, by assumption, still succeeds in deciphering the ciphertext $c^*$ with
    probability $1/2 + p(n)$ where $p(n)$ is non-negligible. 
    
    Therefore $A'$ succeeds with the same probability. This is a direct contradiction of the CCA security 
    of the encryption scheme.
  ~
    
b. (8 points) Prove or give a counterexample: the mechanism is secure when $(E,D)$ is a CPA secure encryption and $\{ f_s \}$ is a pseudorandom function collection.
  
  We simply note that the only change is the the encryption scheme is CPA rather than CCA secure. However,
  if we look a the proof above, we never make use of the decryption oracle. Therefore, we still have the
  same contradiction for the adversaries which correctly guess the password and obtain the key, and
  for those that don't, the box can be simulated in the exact same way.
  
  Therefore, by the prove from a, the mechanism is secure.
  
c. (8 points) Prove or give a counterexample: the mechanism is secure when $(E,D)$ is a CCA secure encryption and $\{ f_s \}$ is a collision resistant hash function collection.
  
  We note that the only difference between (c) and (a) is that the family of functions is now collison
  resistant as opposed to a PRF. We therefore show the following. The proof structure is similar to before,
  where in the case where the adverary $A$ does not guess the password, no change is required.
  
  However, we now consider the case of a successful adversary $A$ to the described system where $A$
  correctly guesses the password $p$. Then note that we can use this $A$ to construct an adversary
  which will find a collision.
  
  ~Proof
    $A'$ is given input $s \in_R \zo^n$. It then procceeds to simulate the interaction between $A$
    and the blackbox, where we select at random $uid \in \zo^n$ and $p \in \zo^{20}$. When $A$
    queries for some password p_p, we check to see if $f_s(0 \| uid \| p_p) = f_s(0 \| uid \| p)$ 
    Note that if $A$ succeeds as before, then by  capturing the input $p$ and because we know
    $uid$, we can actually output the results $uid \| p$, which with very high probability, 
    is actually in facts not equal to $uid \| p_p$, the originally chosen password.
    
    Therefore, we can simply output
    
    Note that the above is efficient as long as $A$ is efficient and with very probability $1 - negl$, the
    results are that we've found a collision. 
  ~
  
d. (8 points) Prove or give a counterexample: the mechanism is secure when $(E,D)$ is a CPA secure encryption and $\{ f_s \}$ is a collision resistant hash function collection.
  
  The only difference is, again, the decreased to CPA security. However, the exact same argument as 
  in (b) here
  holds, and therefore the system is still secure. 
  
  Intuitively, I have a feeling I've misunderstood the question, yet, I don't see why CPA vs CCA makes
  a difference. An adversary that can succeed in breaking the above system is extremely powerful, given
  that it doesn't even have access to the encryption/decryption boxes. It can essential find out
  the key on its own, which is equivalent.
  
e. (8 points)  Prove or give a counterexample: there exist _some_ valid private encryption scheme $(E,D)$ with $n$ bit keys and ciphertexts and _some_  collection of functions $\{ f_s \}$ where for $s\in\zo^n$, $f_s$ is an efficient mapping of  $\zo^{1+n+20}$ to $\zo^{n}$ such that for arbitrarily large $n$, the mechanism above is secure even when we require the probability of $A$'s success to be at most $1/2 + negl(n)$.

  We prove the statement to be true. The general idea is to prove by contradiction.
 
  ~Proof
    Suppose no such collection of objects to satisfy the above statement exists. Then this means
    that there exists some system which succeeds with probability $51\%$ (for concreteness). Then
    note that for sufficiently large $n$, by the law of large number and after repeated trials, 
    we can very increase this probability in a polynomial number of tries to be as arbitrariy large as 
    we'd like. Therefore, it must be the case that for large $n$, the new defininition of secuirity is
    successfull for all algorithms, in fact. 
  ~

**Question 2 (30 points):** Consider the following variant of the DSA signature scheme:

* _Key generation:_ Let $\Gp$ be a cyclic group of prime order $q$. Pick generator $g$ for $\Gp$ and $a\in \{0,\ldots,q-1\}$ and let $h=g^a$. Pick $H:\zo^\ell \rightarrow \{0,\ldots,q-1\}$ and $F:\Gp\rightarrow\{0,\ldots,q-1\}$ to be some  functions that we consider as random oracles. The public key is $(g,h)$  (as well as the functions $H,F$) and secret key is $a$.

* _Signature:_ To sign a message $m$, pick $b$ at random,  let $f=g^b$, let $c=F(f)$ and $d=H(m)$  and then let $s= b^{-1}[d+a\cdot c]$ where all computation is done modulo $q$. The signature is $(f,s)$.

* _Verification:_ To verify a signature $(f,s)$ on a message $m$, compute $c=F(f)$ and $d=H(m)$  and then check that $s\neq 0$ and $f^s=g^{d}h^{c}$.

(_Note:_ The difference between this scheme and the one in the homework is that $d$ is only obtained as a hash of the message $m$ and not also of $c$. This difference makes the current scheme more similar to the DSA scheme.)

Prove that this is a valid signature scheme secure against a chosen message attack in the random oracle model, assuming that  $(\Gp,g)$ satisfies that every efficient adversary $A$, if $a \getsr \{0,\ldots, q-1\}$ then the probability of $A(\Gp,g,g^a)=a$ is negligible in $\log |\Gp|$.

~Proof
  Similar to the homework assignment, suppose this is not a secure signature scheme in the random oracle model.
  Then there exists some efficient adversary $\mathcal{A}$ that with use of the signing oracle,
  can construct $(m^*, \sigma^*)$ given the public key $(g,h,H,F)$ such that $V_a(m^*,\sigma^*) = 1$ with
  probability $1 - negl(\ell)$. Then we use this adversary to solve the discrete logarithm problem. 
  The algorithm to solve dlog works as follows:

  1. On input $\Gp, g, g^a$, we generate the key as described in the modified DSA signature scheme, where
  we set the public key to be $(g,h)$ where $g$ is given and $h = g^a$. We don't know $a$ directly,
  but assume its existence. ($H,F$ are also public).
  2. We then feed the public key $(g,h)$ to the adversary $\mathcal{A}$, providing it with access 
  to $F$ and $G$. 
  3. When $F$ eventually queries the $F$ and $H$ oracles in order to construct the final result
  $(m^*, s^*)$, we intercept the query to oracle $F$, $f$. If $c = F(f)$ has not get been computed,
  we compute it. However, we then pick $c^* \in_R \{0, \cdots, |\Gp|\}$ and return this as the
  result to $\mathcal{A}$. Note that this is indistinguishable, given the random oracle model. 
  4. We do the same for the random oracle $H$, where when the final submission is given rabdomly, as
  $d^*$, rather than the true value, $d = H(m)$. 
  5. $\mathcal{A}$ will return to us $(m^*,s^*)$. 
  6. We calculate $d = H(m)$.
  7. We output $(d-d^*)(c^* - c)^{-1} \pmod{\Gp}$. 

Note that the above algorithm is efficient. Key generation is efficient, randomly selecting elements
is efficient, $H$ and $F$ are efficient, and computing the result in step 6 is efficient. We now 
prove that we have $a = (d-d^*)(c^* - c)^{-1}$. This follows from the fact that
with high probability, $s^*b = d^* + ac^*$ and $s^*b = d + ac$. The reason for this is that 
is that the signature scheme $(f,s^*)$ passes with high probability by the fact that 
the $\mathcal{A}$ adversary has generated it as a forge for the chosen value for the message
$m$. This means $s^*b = d + ac$. This is by construction since we know that $f = g^b$ and
therefore $f^{s^*} = (g^b)^{b^{-1}(d + ac)} = g^{d+ac} = g^dh^c$ as required by the verifcation algorithm
to succeed.

However, also note that the adversary successfully produced this signature by receiving incorrect
results from the random oracles -- the results are indistinguishable from the correct results. The result 
$\mathcal{A}$ received for $F(f)$ was $c^*$, a completely random value, and the result it received for 
$H(m)$ was $d^*$, again a completely random value. From this, we also know that it must be the 
case that $s^*b = d^* + ac^*$. 

We therefore now have two linearly independent equations,
and subtracting one from the other, we have $0 = d^{*}  - d + a(c^* - c) \implies a = (d - d^*)(c^* + c) $.
Therefore,  the above algorithm successfully computes $a$ such that $g^a = h$, thereby solving 
the discrete logarithm problem with high probability.

However, the discrete logarithm problem is not solvable with non-negligble probability, a contradiction.
This means the system must be secure.
[&box;]{float=right}
~
**Question 3 (30 points):** Let $(E',D')$ and $(E'',D'')$ be two private key encryption schemes using $n$ bit keys and $n$ bit messages.

a. (10 points) Prove or give a counterexample: If $(E',D')$ and $(E'',D'')$ are CPA secure, then the encryption scheme $(E,D)$ taking $n$ bit keys and messages which is defined for every $k,m\in\zo^n$ as $E_k(m)=E'_k(m)\|E''_k(m)$ and $D_k(c'\|c'')=D'_k(c')$ is CPA secure.

  We show that the encryption scheme is CPA secure. We do this by contradiction and showing that if we
  can break the above encryption scheme, then we can break $(E',D')$. 
  
  The first step is showing that, in the case where $(E',D') = (E'', D'')$, the encryption scheme 
  described above is secure. We then argue that that, in some sense, this would be the easiest 
  encryption to break. Therefore, in the case where $(E',D') \neq (E'',D'')$, the system can only 
  be more secure. The attempt at formalizing this intuition is found after the first, though I will
  be the first to admit that it isn't bullet-proof.
  
  We first focus in the casw where we have one encryption scheme $(E',D')$ that is CPA secure,
  and we let $(E'',D'') = (E',D')$ and construct $(E,D)$ as described above. The claim is that $(E,D)$
  is CPA-secure.
  
  ~Proof
  For a contradiction, suppose
  $(E,D)$ is not CPA secure. Then there exists an adversary $A$ which wins
  at the CPA-secure game with probability at least $\frac{1}{2} + p(n)$ where $p(n)$ is non-negligible
  with an encryption scheme $(E,D)$ that satisfies the definition above.
  Then we use this adversary $A$ to construct an adversary $A'$ against the encryption scheme
  $(E',D')$ as follows:
  
    1. On input $1^n$, we run $A'$.
    2. When $A'$ requests an encryption of $E_k(m)$, we return $E_k(m) = E_k'(m) \| E'_{k}(m)$.
    3. After a polynomial number of requests, $A'$ will output $m_0,m_1 \zo^n$.
    4. $A$ submits $m_0,m_1 \in \zo^n$ as the challenge messages.
    5. On receiving $c_1^* = E'_k(m_b)$, we submit the ciphertext $c^* = c_1^* \| c_1^*$ to $A'$.
    6. We output the same output as $A'$.
  
  The claim is that the above adversary succeeds with probability at least $\frac{1}{2} + p(n)$ and
  the adversary $A$ is efficient (done more rigorously below), and
  therefore the encryption scheme $(E',D')$ is not CPA-secure -- a contradiction. Therefore we conclude
  that such an adversary $A$ against $(E,D)$ cannot exists, and therefore $(E,D)$ is CPA-secure. 
  [&box;]{float=right}
  ~
  
  The first step is in showing that the encryption scheme simulated by $A'$ satisfies the properties
  expected by $A$.
  ~Proof
   The proof is straight forward. We have simply have $E'' = E'$ and $D'' = D'$. Then both $(E',D')$ and
   $(E'',D'') = (E',D')$ are CPA-secure schemes. Then the simulated encryption is simply
   $E_k(m) = E'_k(m) \| E_k'(m) =  E'_k(m) \| E_k''(m)$ and $D_k(c' \| c'') = D_k'(c')$, which is exactly as
   defined in the problem.
   [&box;]{float=right} 
  ~
  
  The next step is to calculate the probability of success. We claim that the probability of success
  of $A'$ is equivalent to that of $A'$. We show this by showing that $A'$ succeeds if and only if
  $A$ succeeds.
  ~Proof
    Suppose $A$ succeeds, this means it correctly outputs $b' = b$ for $c^* = E_k(m_b) = c_1^* \| c_1^* = E'_k(m_b) \| E_k'(m_b)$.
    Then if $A'$ outputs $b'$, it will also correctly identify $c_0^* = E_k'(m_b)$. In the other direction,
    our system outputs $b' = b$ for $c_0^* = E_k'(m_b)$, then this means that $A'$ must have also output
    $b'$.
  [&box;]{float=right}
  ~
  
  We now conclude by arguing that the constructed adversary is efficient.
  ~Proof
   We can immediately see that we make twice as many calls to the oracle, and as long as $A$ is efficient,
   since we only use $A$ poly times, $A'$ should be efficient.
   [&box;]{float=right} 
  ~
  
  With the above, we conclude our proof that in the case where we have a single CPA-secure encryption
  scheme and use it in the construction above, we arrive at a CPA-secure encryption scheme.
  
  Therefore, the last step in our argument is that having two distinct CPA-secure encrpytion schemes 
  will not weaken the security of the constructed scheme. The reason for this is that, in the simulation
  above, we can simply make the following modifications:
  
    1. Pick a $k'' \in_R \zo$ to be used as the key for the new encryption scheme $(E'',D'')$. Note
    that by the CPA-security of $(E'',D'')$, we have thet $\{ E''_{k''}(m) \} \approx \{ E_{k'''}(m) \}$,
    so the fact that we're using a different key is not of consequence.
    2. When submitting the encryption, rather than submitting $c_1^*$, we instead submit $E''_{k''}(m_R)$ for
    some arbitrary message $m_R$, since by CPA security, $\{E_{k''}(m)\} \approx \{E''_{k''}(m') \}$ for
    any $m,m'$ (otherwise it wouldn't be CPA-secure). Therefore, the adversary still receives inputs that
    are indistinguishable from what it expects.
    
  With the above modifications, we should be able to handle the case where $(E',D') \neq (E'',D'')$, which
  verifies our intuition that it should somehow not change the sucurity of the construction to simply add
  a second CPA-secure system.  
  
b. (10 points) Prove or give a counterexample: If $(E',D')$ and $(E'',D'')$ are CPA secure, then the encryption scheme $(E,D)$ taking $2n$ bit keys and $n$ bit messages which is defined for every $k\in \zo^{2n}$ and $m\in\zo^n$ as $E_k(m)=E'_{k_1\ldots k_n}(m)\|E''_{k_{n+1}\ldots k_n}(m)$ and $D_k(c'\|c'')=D_{k_1\ldots k_n}(c)$ is CPA secure.
  
  For succinctness, we let $k' = k_1k_2\cdots k_n$ and $k'' = k_{n+1}k_{n+1} \cdots k_{2n}$. 
  
  We proof that the scheme is secure. We do this by contradiction. We show that if $(E,D)$ is not secure,
  then we can win the CPA-game with $(E',D')$, a contradiction on the CPA security of $(E',D')$. 
  
  We now provide a construction.
  For contradiction, suppose $(E,D)$ is not CPA-secure. Then there exists an adversary $A$ which can 
  win the CPA game with probability $1/2 + p(n)$ where $p(n)$ is non-negligible. 
    
  Now, we use $A$ to construct an adversary $A'$ for the CPA-game with the encryption scheme $(E',D')$.
  The adversary works as follows, on input $1^n$:
  
    1. Select a random $k'' \in_R \zo^n$. 
    2. Simulate the CPA game between $A$ and $(E,D)$ as follows, where $(E,D)$ is defined as above.
    3. When $A$ makes a query to the encryption oracle $E_k(m)$, simply use our encryption oracle 
    to compute $c' = E'_{k'}(m)$ and then use the key $k''$ to compute $c'' = E''_{k''}(m)$, and
    respond with $c' \| c''$. 
    4. $A$ will eventually produce two messages, $m_0,m_1$. Submit these same messages as our
    challenge message.
    5. On receipt of $c^* = E'_{k'}(m_b)$ for some $b \in_R \zo$, construct the encryption 
    $c' = c^* \| E''_{k''}(m_0)$. Give this encryption to $A$.
    6. Output whatever $A$ outputs. 
 
  The main claim is that $A'$ succeeds with probability $1/2 + p(n)$. We note that $A'$ outputs whatever
  $A$ outputs, so as long as $A$ simulated above is correct, then $A'$ and $A$ will succeed with the 
  same probability. There are two modifications to our simulation. We prove that each cannot be 
  distinguished by $A$. 
  
  The first claim is that $k \in_R \zo^{2n}$ is equivalent to $k' \| k''$ where $k' \in_R \zo^n$ and $k'' \in_R \zo^n$.
  ~Proof
    The claim is trivial, since $k'$ and $k''$ are chosen independently. 
    [&box;]{float=right}
  ~
  
  With the above, we note that $A'$ simulates an interaction with the encryption scheme $(E,D)$ using 
  the key $k = k' \| k''$ where $k'$ is the key (unknown, but to which we have encryption oracle access),
  and $k''$ is chosen by $A'$.
  
  The second claim is that the encryption $c'$ is indistinguishable from $E_k(m_b)$. 
  ~Proof
   We do this by hybrid argument. 
  ~
  ~Math
   \begin{aligned}
     \{ c' \} &\approx \{ c^* \| E''_{k''}(m_0) \} \\
     &\approx \{ E'_{k'}(m_b) \| E''_{k''}(m_0) \} \\
     &\approx \{ E'_{k'}(m_b) \| E''_{k''}(m_b) \} \\
     &\approx \{ E_k(m_b) \}
   \end{aligned}
  ~
  the important step taking place in the second to last line. Here, we usue the fact that $E''$ is a CPA-secure
  encryption scheme, and therefore we know that for any messages $m,m'$, $\{E''_{k''}(m) \} \approx \{E''_{k''}(m')\}$. 
  [&box;]{float=right}
  
  With the above, we immediately see that $A$ cannot distinguish between the simulated input
  and the input it would normally expect from the scheme $(E,D)$ using key $k = k' \| k''$. We are now
  ready to prove the final claim.
  
  Lastly, we argume that $A'$ is efficient.
  ~Proof
    Given that $A$ is efficient, we only make polynomial number of calls to $A$ and to the oracles, along
    with many other computations that are mostly based on string manipulation. Therefore it must be possible
    to maek $A'$ efficient.  
  ~
  
  Supposing the existence of a successful adversary $A$ for $(E,D)$, we construct a successful 
  adversary $A'$ for $(E',D')$. Therefore $(E,D)$ must be secure.
  ~Proof
   The proof is almost completely laid out. From the above, we know that the simulation of $A$ done 
   by $A'$ has $A$ interacting with the encryption scheme $(E,D)$ with key $k = k' \| k''$,
   and therefore by the existence of $A$, $A$ will successfully craft two messages $m_0,m_1$ such that
   with probability $1/2 + p(n)$ it will output $b' = b$ where when fed $c^* = E_k(m_b)$. By the arguments
   above, $c'$ is indistinguishable from $c^*$, therefore the simulated $A$ will also successfully guess
   given our constructed input $c' = E'_{k'}(m_b) \| E''_{k''}(m_0)$. However, by construction, a successful
   guess is exactly equivalent to successfully guessing $c^* = E'_{k'}(m_b)$ (since this is the input we feed),
   and therefore $A'$ succeeds with probability $1/2 + p(n)$. Yet, this is a contradiction on the
   fact that $(E',D')$ is CPA secure, therefore we conclude that the adversary $A$ cannot exist.
   [&box;]{float=right} 
  ~
  
  The above shows that $A$ does not exists, and therefore $(E,D)$ must be CPA-secure.
  
c. (10 points) Prove or give a counterexample: If $(E',D')$ and $(E'',D'')$ are CCA secure, then the encryption scheme $(E,D)$ in item b. above is CCA secure.
  
  We give a counter example. The construction is actually relatively straight forward. Take any two
  CCA-secure schemes, $(E',D')$ and $(E'', D'')$ and define the encryption scheme $(E,D)$ as described in 
  part b. Now construct the adversary $A$ which on intput $1^n$ does the following:
  
    1. Use the encryption oracle to compute $c_0 = E_k(m_R)$ for some arbitrary $m_R \in \zo^n$. 
    2. Store the last $n$ bits of $c_0$ in memory. More formally, note that $c_0 = c_{01}c_{02} \cdots c_{0n} \cdots c_{0(n+1)}\cdots c_{0(2n)}$,
    and we store $s = c_{0(n+1)}c_{0(n+2)} \cdots c_{0(2n)}$.
    3. Chose any two messages $m_0, m_1 \in_R \zo^n$ and submit these as the challenge messages.
    4. On receiving the encryption $c^* = E_k(m_b)$ for some $b \in_R \zo$, modify $c^*$ so that the last
    $n$ bits are now the stored $n$ bits from $c_0$. This is, more formally, construct the ciphertext
    $c' = c^*_0c^*_1 \cdots c^*_ns_0s_1 \cdots s_n$.
    5. Submit $c'$ to the decryption oracle to obtain $m_{b'} = D_k(c')$. Note that this is allowed with 
    high probability since, with high probability, $s \neq c^*{n+1}c^*_{n+2} \cdots c^*_{2n}$. In the
    case where the above occurs, then simply output $b' \in_R \zo$ (randomly).
    6. Output $b'$.
    
  The main claims is that $b' = b$ with probability $1 - negl/2$. We now formally show this.
  ~Proof
    For succinctness, we let $k' = k_1k_2 \cdots k_n$ and $k'' = k_{n+1}k_{k+2} \cdots  k_{2n}$. 
    We work backwards. Note that in one scenario, $b'$ is chosen at random, and therefore it equals
    $b$ with probability $\frac{1}{2}$. However, this scenario occurs only in the case where 
    $s =  c^*_{n+1}c^*_{n+2} \cdots c^*_{2n}$. By construction, this occurs if and only if $E''_{k''}(m_R) = E''_{k''}(m_b)$,
    which by the internal randomness of $E''$, must occur with negligible probability. 
    
    In the other case, we know that $E''_{k''}(m_R) \neq E''_{k''}(m_b)$, and therefore we submit
    $c'$ to the decryption oracle (which is valid since $c' \neq c^*$). Note that $c' = E'_{k'}(m_b) \| E''_{k''}(m_R)$.
    As defined above, this means that $D_k(c') = D_k(E'_{k'}(m_b) \| E''_{k''}(m_R)) = D'_{k'}(m_b)$, and therefore,
    by the correctness of the encryption scheme, the result is the decryption of $m_b$ and so $b' = b$ with
    probability $1$. 
    
    Therefore, the overall probability that $b = b'$ is given by $1(1-negl) + 1/2(negl) = 1 - negl/2$.
    [&box;]{float=right} 
  ~ 
  
  The next step is to show that $A$ is efficient.
  ~Proof
   The proof is relatively straight forward. Analyzing each step that $A$ takes, we see that querying the
   encryption oracle once and the decryption oracle once is efficient. All other steps simply involve processing
   the input or manipulating strings, which is certainly efficient. Therefore, $A$ is efficient.
   [&box;]{float=right} 
  ~
  
  The above concludes the problem, as we've constructed an explicit and efficient adversary $A$ which
  breaks the encryption scheme from b. Therefore $(E,D)$ is not CCA-secure.

**Question 4 (35 points):** We define a public key encryption scheme $(G,E,D)$ taking $n$ bit messages and having $n$ length private and public keys  to be _key-dependent message (KDM) secure_ if for every efficiently computable function $f:\zo^n\rightarrow\zo^n$, every efficient adversary $A$ wins with probability at most $1/2+ negl(n)$ in the following game:

1. The keys $(d,e)$ are generated using $G(1^n)$.
2. The adversary gets $e$ and $c^* = E_e(f(d))$.
3. The adversary chooses $m_0,m_1 \in \zo^n$.
4. We choose $b\getsr\zo$ and give the adversary $c^{**}=E_e(m_b)$.
5. The adversary outputs $b'$ and _wins_ if $b'=b$.

That is, the scheme remains CPA secure even when the adversary gets an encryption of the message $f(d)$.

We call a scheme _circular secure_ if it is KDM secure with respect to the single function $f(m)=m$ (i.e., the ciphertext $c^*$ in Step 2 is an encryption of the secret key $d$).

a. (10 points) Prove or give a counterexample: every CPA secure encryption scheme is also KDM secure.                                                                                                                                                                                                                                                                                   

  We give an explicit counter-example. The intuition is that the encryption of some function of the secret
  key should leak information to the adversary. Note that CPA security has no restrictions on the 
  encryptions of the secret key itself, therefore there's no real reason to expect all CPA-secure 
  schemes to effectively hide information about the encryption of the secret key. Trivially, and 
  not in the public-key setting, we can think of a shared-key example as follows.
  
  Take any shared-key symmetric CPA secure encryption, $(G,E,D)$, and modify it as follows to construct
  $(G',E',D')$:
  
    1. Key generation algorithm $G$ is exactly the same as before, ($G = G'$)
    2. In the encryption, $E'_k(m)$ first checks to see if $k = m$, and if so, it outputs $m$. Otherwise,
    it outputs $E_k(m)$.
    3. The decryption algorithm works in a similar way. $D'_k(c)$ first checks to see if $c = k$, and 
    if so, returns $c$, otherwise returns $D_k(c)$. 
  
  It's clear from the above that the function $f(m) = m$ (circular secure, which is a weaker notion than
  KDM secure), does not hold. Therefore, KDM security won't hold, despite the fact that the encryption
  scheme is CPA secure. 
  
  We can use the idea above to construct something similar in the public-key setting. However, 
  we make use of the indistinguishability.
  
  As a reminder, we say a compiler $\cO$ is an indistinguishability obfuscator (IO) if for every two 
  circuits $C_0, C_1$ that have the same size and compute the same function, the random variables $\cO(C_0)$
  and $\cO(C_0)$ are computationally indistinguishable. 
  
  With the above assumption, we take any CPA secure public key encryption scheme $(G,E,D)$ and convert
  it into the following, CPA secure public key encryption scheme, $(G',E',D')$. We remove the restriction
  that the keys must be of length $n$ (same as the message), though we note that the keys can be made
  of fixed length. 
    1. Key generation algorithm $G$ is exactly the same as before, ($G = G'$), producing $(e,d)$ where
    $e$ is public, and $d$ is private. Furthermore, the system constructs an circuit $C_e(m)$ which on
    input $m$ compares $m$ to $d$ and returns either $1$ if equal or $0$ otherwise. The circuit 
    is the obfuscated using the indistinguishability obfuscator and is output as part of the public
    key, so we now have $e' = (e,\cO(C_e))$.
    2. In the encryption, $E'_{(e,\cO(C_e)}(m)$, we first run $cO(C_e)(m)$ and if it outputs $1$, we outout
    $c = m$. Otherwise, we simply output $c = E_e(m)$. 
    3. The decryption algorithm works in a similar way. $D'_k(c)$ first checks to see if $\cO(c) = 1$, and 
    if so, returns $c$, otherwise returns $D_k(c)$.
  
  We cam immediately see the similarities with the previous constructions in the shared-key setting.
  We now prove that the system is CPA secure.
  ~Proof
    The system behaves exactly the same as the CPA secure $(G,E,D)$ except for on the single input 
    of the private key, $d$, which is private and not accessible to the adversary.
    Therefore, the $\{E_e(m)\} \approx \{E'_e(m)\}$, and we have that both systems are CPA secure.
    [&box;]{float=right}
  ~
  
  With the above complete, we now show that the system $(G',E',D')$ is _not_ KDM secure. In fact,
  we can very explicitly construct the adversary $\mathcal{A}$ for which there exists a function
  $f$ such that $\mathcal{A}$ can win the KDM security game. In fact, we take the function $f$ to 
  be the indentity function (we discuss this in more detail in part (b)). Then we have the following
  results:
  
  On input $1^n$, the adversary $\mathcal{A}$ as follows:
    
    1. Upon receiving $e'$ and $c^* = E'_{e'}(d)$, the adversary choses random $m_0,m_1$ and submits these as the challenge.
    2. Upon receipt of $c^{**} = E'_{e'}(m_b)$, the adversary simply compute $m_{b'} = D'_{c^*}(c^{**})$ and outputs $b'$.
    
  We claim that the above adversary always wins the game. 
  ~Proof
   Note that winning the game occurs when $b = b'$. We claim that this is the case bceause $m_b = m_{b'}$. 
   By construction, 
  ~
  ~Math
   \begin{aligned}
     m_{b'} &= D'_{c^*}(c^{**})) \\
     &= D'_{ E'_{e'}(f(d))}( E'_{e'}(m_b)) \\
     &=D'_{ E'_{e'}(d)}( E'_{e'}(m_b)) \\
     &=  D'_d( E'_{e'}(m_b)) \\
     &= m_b
   \end{aligned}
  ~
  We thereby conclude that not all CPA secure schems are KDM secure.
  [&box;]{float=right}
  
b. (10 points) Prove or give a counterexample: every CPA secure encryption scheme is also circular secure.
  
  Given the results from part (a), this follows as a direct corollary. Note that the adversary we 
  constructed in part (a) against the modified CPA secure encryption scheme is in fact an adversary
  which wins the game when the function $f(m) = m$. Therefore, by the results from part (a), the
  same system fails circular security, which is a weaker notion than KDM security. 
  
  In fact, if I had more time, I would have proven this section first and then referenced the results
  in the section above, since showing that there exists a CPA secure encryption scheme that is not
  circular secure immediately implies that the same CPA secure encryption scheme is not KDM secure. 
  
c. (15 points) Prove or give a counterexample: every CPA secure strongly[^strongfhe] fully homomorphic encryption scheme that is circular secure is also KDM secure
  
  We first make the assumption that we can extend an FHE system to compute any function from $f: \zo^n \to \zo^n$
  by computing one bit at a time and by composing functions. We then have a more succinct definition
  for strong FHE scheme $(G,E,D)$ which is the existence of a function EVAL such that $EVAL(f, c) \approx E_e(f(x))$
  where $c= E_e(x)$ where $x \in \zo^n$ and $c \in \zo^n$. 
  
  We claim, then, that the statement is true. Every CPA secure strongly fully homomorphic encryption scheme
  that is circular secure is also KDM secure. 
  
  First, note that circular security implies CPA secure, so we drop the qualifier.
  ~Proof
    Suppose we had a circular secure scheme that is not also CPA secure. Then there exists an adversary
    $A$ which wins at the CPA game. Then we construct an adversary $A'$ which wins the KDM game. $A'$
    simply runs $A$ on the inputs, ignoring the additional encrypted information $c^* = E_e(f(d))$ and
    outputs the same result. Note that $A$ succeeds if an only if $A'$ succeeds.
    [&box;]{float=right}
  ~
  
  Therefore our statememet is simply that every strongly fully homomorphic and circular secure encryption scheme 
  is also KDM secure.
  ~Proof
   For a contradiction, suppose we have a strongly FHE circular secure scheme that is not KDM secure.
   Then there exists an adversary $A$ and a function $f: \zo^n \to \zo^n$ such that $A$ wins the 
   KDM secure game with probability $1/2 + p(n)$ where $p(n)$ is non-negligbile. 
   
   We construct the adversary $A'$ which wins at the circular secure game (KDM secure with $f(m) = m)$
   with the same probability:
   
    1. On input $1^n$, receive $e$ and $c^* = E_e(d)$. 
    2. Simulate the game the adversary $A$ would play. 
    3. Using EVAL, compute $c^{*'} = EVAL(f,c^*)$ and give $c^{*'}$ and $e$ to $A$.
    4. Chose $m_0,m_1 \in \zo^n$ the same as $A$ choses.
    5. On receiving the input $c^{**} - E_e(m_b)$, forward it to $A$ and output whatever it outputs.
    
  We now show that the probabilities of winning are equivalent. First, we note that $A$ cannot distinguish
  between the simulated game and the real game. The reason for this is because, by assumption,
  we have $EVAL(f,c^*) \approx E_e(f(d))$, and therefore $A$ is fed an equivalent result to $E_e(f(d))$. 
  
  From the above, we therefore know that $A$ correctly wins with high probability. Since $A'$ does the
  same as $A$, it must also win with high probability. 
  
  Given the above, we conclude the if $A$ exists and is efficient, then $A'$ exists and is efficient (it 
  simply simulates $A$).
  This contradicts the fact that our scheme is circular secure, and therefore it must be the case 
  that every strongl FHE and circular secure scheme is KDM secure. 
  ~

[^strongfhe]: By strong FHE I mean that the result of EVAL is distributed identically to the result of a fresh encryption; see the appendix below for the formal definition. I assume a bitwise FHE is extended to an encryption of $n$ messages by encrypting one bit at a time.

**Bonus question (20 points, please only do this if you finished all the others and have extra time):**
This question shows how to transform a _private key_ fully homomorphic scheme into a _public key_ scheme.
Suppose that $(D,E,EVAL)$ is a (not necessarily strong) fully homomorphic  CPA-secure _private key_ encryption scheme with $n$ bit keys, $1$ bit messages, and $n$ bit ciphertext. In Prove that the following encryption scheme $(G',E',D')$ is a CPA-secure _public key_ encryption:

* __Key generation:__  $G'(1^n)$ is defined as follows: choose $k\getsr\zo^n$. We let $m=100$.  The private key is $k$ and the public key is $(h,c_1,\ldots,c_m)$ where $h$ is chosen at random in $\zo^m$ and $c_i=E_k(h_i)$ for all $i=1..m$.

* __Encryption:__ For plaintext $b\in\zo$, and public key $(h,c_1,\ldots,c_m)$, pick a random $x\getsr\zo^m$ subject to $\sum_{i=1}^m x_i h_i = b \;(\mod\; 2)$, let $f(h_1,\ldots,h_m)=\sum_{i=1}^m h_ix_k (\mod\; 2)$ and output $c^* = EVAL(f,c_1,\ldots,c_m)$.

* __Decryption:__ For ciphertext $c\in\zo^n$ and keyt $k\in\zo^n$, $D'_k(c)$ simply equals $D_k(c)$.


One way to prove this is to follow these steps:

a. (5 points) Prove that $(G',E',D')$ is a valid public key encryption scheme in the sense that if $k$ and $(h,c_1,\ldots,c_m)$ are generated by $G'(1^n)$ then for every $b\in\zo$, $D'_k(E'_{h,c_1,\ldots,c_m}(b))=b$.

  We can show this directly. For all $b \in \zo$, we have:
  ~Math
   \begin{aligned}
     D'_k(E'_{h,c_1,\cdots,c_m}(b)) &= D_k(EVAL(f, c_1,\cdots,c_m)) \\
     &= f(h_1,\cdots,h_m) \\
     &= \sum_{i = 1}^m h_ix_i \\
     &= b
   \end{aligned} 
  ~
  Where we now explain each step. The first line is simply by construction of $(E',D)$. The second
  line follows from the fact that $(E,D)$ is a FHE. We then simply expand the definition of $f$, and
  note that $x_i$ was chosen (and hard-coded into $f$) such that the sum equaled $b$. Therefore,
  the above is a valid public encryption scheme. 

b.  (15 points) Prove that $(G',E',D')$ is  CPA secure. Namely, that the distributions $((h,c_1,\ldots,c_m),E'_{h,c_1,\ldots,c_m)}(0))$ and $((h,c_1,\ldots,c_m),E'_{h,c_1,\ldots,c_m)}(1))$ are computationally indistinguishable where $(h,c_1,\ldots,c_m)$ is the public key generated by $G'(1^n)$.
Hint: you can do this by showing that:

  1.  Both these distributions are indistinguishable from the case where we generate $(h,c_1,\ldots,c_m)$ from a modified algorithm $G''$ such that $G''(1^n)$ computes each $c_i$ to be an  encryption of $h'_i$ that is chosen at random in $\zo$ independently of everything else (and so in particular $h$ and $h'$ are independent from one another).
  2. If $(h,c_1,\ldots,c_n)$ is generated from $G''(1^n)$ then $E'_{(h,c_1,\ldots,c_n)}(0)$ is _statistically indistinguishable_ from $E'_{(h,c_1,\ldots,c_n)}(1)$. To prove that you want to use the leftover hash lemma (see statement below) and the fact that the length of the ciphertext is only $n$ bits.


## Appendix: definitions

For convenience, here are some definitions of the concepts mentioned in this exam:

A private or public  encryption scheme with encryption algorithm $E$ and decryption algorithm $D$ is _valid_ if for every message $m$, $D_d(E_e(m))=m$ where $d$ is the decryption key  and $e$ is the encryption key (in the private key case they are both the same and random strings, while in the public key case they are generated by a key generation algorithm).

__CPA secure private key encryption:__ A valid private-key encryption scheme $(E,D)$ with $n$-bit keys and $\ell$-bit messages
is _CPA_ secure if  every efficient adversary $A$ wins with probability at most $1/2+negl(n)$ in the following game:

1. The key $k$ is chosen randomly at uniform from $\zo^n$.
2. $A$ gets polynomial number of accesses to the function $m \mapsto E_k(m)$.
3. $A$ chooses $m_0,m_1 \in \zo^\ell$ and sends it to the challenger.
4. The challenger chooses $b'\getsr\zo$ and sends $c^* = E_k(m_b)$ to $A$.
5. $A$ gets another polynomial number of accesses to the function $m \mapsto E_k(m)$.
6. $A$ outputs $b'\in\zo$ and _wins_ if $b'=b$.


__CCA secure private key encryption:__ A valid  private-key encryption scheme $(E,D)$ with $n$-bit keys and $\ell$-bit messages
is _CCA_ secure if  every efficient adversary $A$ wins with probability at most $1/2+negl(n)$ in the following game:

1. The key $k$ is chosen randomly at uniform from $\zo^n$.
2. $A$ gets polynomial number of accesses to the functions $m \mapsto E_k(m)$ and $c \mapsto D_k(c)$.
3. $A$ chooses $m_0,m_1 \in \zo^\ell$ and sends it to the challenger.
4. The challenger chooses $b'\getsr\zo$ and sends $c^* = E_k(m_b)$ to $A$.
5. $A$ gets another polynomial number of accesses to the functions $m \mapsto E_k(m)$ $c \mapsto D_k(c)$ with the restriction that if $A$ queries $c^*$ to the second function then it recieves ```error``` as a response.
6. $A$ outputs $b'\in\zo$ and _wins_ if $b'=b$.

__CPA secure public key encryption:__
A valid public-key encryption scheme $(G,E,D)$ with  $\ell$-bit messages
is _CPA_ secure if  every efficient adversary $A$ wins with probability at most $1/2+negl(n)$ in the following game:

1. The keys $(e,d)$ are generated by $G(1^n)$. $A$ gets $e$.
2. $A$ chooses $m_0,m_1 \in \zo^\ell$ and sends it to the challenger. (If $\ell=1$ then without loss of generality $m_0=0$ and $m_1=1$)
3. The challenger chooses $b'\getsr\zo$ and sends $c^* = E_k(m_b)$ to $A$.
4. $A$ outputs $b'\in\zo$ and _wins_ if $b'=b$.



__Fully homomorphic encryption:__ A valid  private or public key encryption scheme with encryption and decryption algorithms $E,D$ is  _fully homomorphic_ if there exists
an efficient algorithm $EVAL$ such that for every function $f:\zo^m\rightarrow\zo$ (represented as a Boolean circuit) and $x_1,\ldots,x_m \in \zo$,
$c^* = EVAL(f,E_e(x_1),\ldots,E_e(x_m))$ satisfies $|c^*|\leq n$ and $D_d(c^*)=f(x_1,\ldots,x_m)$ here in the public key case $(e,d)=G(1^n)$ where $G$ is the key generation algorithm
and in the private key case $e=d$ is a uniformly random string in $\zo^n$.

We say that a fully homomorphic encryption is _strong_ if for every $x_1,\ldots,x_m,f$ as above and $c_1,\ldots,c_m$ such that $c_i$ has posivie probability of being output by $E_e(x_i)$, the distribution of $EVAL(f,c_1,\ldots,c_m)$ and $E_e(f(x_1,\ldots,x_m))$ are identical, where the randomness here is only over the internal coins used by the $EVAL$ and $E$ algorithms.


__CMA secure signature scheme:__ A signature scheme $(G,S,V)$ for length $\ell$ messages is _valid_ if for every $(s,v)$ generated by $G(1^n)$ and message $m\in\zo^\ell$,
$V_v(m,S_s(m))=1$. As valid signature scheme $(G,S,V)$ is _secure against existential forgery with respect to a chosen message attack_ (_CMA secure_ for short) if every efficient adversary $A$ wins the following game with probability at most $negl(n)$:

1. The keys $(s,v)$ are generated by $G(1^n)$. $A$ gets $v$.
2. $A$ gets polynomial number of queries to the function $m \mapsto S_s(m)$.
3. $A$ outputs some $m^*$ that it did not query in the previous step and some string $\sigma$, and _wins_ if $V_v(m^*,\sigma)=1$.

(In the random oracle model, $A$ gets also polynomial number of queries to the random oracles.)



__Collision resistant hash function:__
A collection of functions $\{ f_s \}$ is a _collision resistant hash function collection_ if there is an efficient algorithm that maps $(s,x)$ to $f_s(x)$ and for every efficient adversary $A$, the probability over $s \getsr\zo^n$ that  $A(s)$ outputs $x \neq x'$ such that $f_s(x)=f_{s}(x')$ is negligible.

__Pseudorandom functions:__
A collection of functions $\{ f_s \}$ is a _pseudorandom function collection_ if for every efficient adversary $A$, the probability that $A$ wins the following game is at most $1/2+negl(n)$:

1. We choose $b\getsr\zo$. If $b=1$ then we choose $s\getsr\zo^n$.
2. The adversary gets polynomial number of queries to the function $x \mapsto F(x)$ where if $b=0$ then this is a random function (i.e., for every distinct query $x$ returning an independent random string in the output range of $f_s$) and if $b=1$ then $F(x)=f_s(x)$.
3. The adversary outputs $b'$ and _wins_ if $b'=b$.


__Leftover Hash Lemma:__ For a distribution $X$ we define $H_\infty(X)$ to be the minimum over all $x$ in the support of $X$ of $1/\log(\Pr[X=x])$.
One version of the leftover hash lemma is that for every distribution $X$ over $\zo^m$ such that $H_{\infty}(X) \geq 10\ell$, with probability at least $1-2^{-\ell}$ over the choice
of a random $h\in \zo^m$, the random variable $Y= \sum_{i=1}^m h_i X_i \; (\mod \; 2)$ (where the randomness is solely over the choice of $X$) satisfies that $\Pr[ Y= 0 ] \in (1/2 - 2^{-\ell}, 1/2 + 2^{-\ell})$.
