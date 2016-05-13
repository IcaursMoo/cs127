Homework 7: Authenticated Key Exchange, CCA for public key, zero knowledge
~ MathDefs
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
~

### Total of 175 points


1. (30 points) An attractive way to perform a bidding is the following: the seller publishes a public key $e$. Each buyer sends through the net the encryption $E_e(x)$ of its bid, and then the seller will
decrypt all of these and award the product to the highest bidder. One aspect of security we need to achieve this is that given an encryption $E_e(x)$, it will be hard for someone not knowing $x$ to come up with $E_e(x+1)$ (otherwise bidder B could always take the bid of bidder A and make into a bid that is one dollar higher).

    a. (15 points)  Show a CPA-secure public key encryption $(G,E,D)$ on messages in $\zo^\ell$ (interpreted as numbers in $[0,2^\ell-1]$) such that there is an algorithm that given $e$ and a ciphertext $c=E_e(x)$ for $x < 2^\ell$, outputs a ciphertext $c'$ that decrypts to $x+1$. (If it makes your life easier, you can make the algorithm work only if $x$ is a multiple of $2^{10}$.)
~ Proof
We provide a generalized algorithm that works for all values of $x$ where $x$ is a multiple of $2$. 
The specific construction we have in mind of $(G,E,D)$ is as follows. Let $\{p_k\}$ be a 
secure family of trapdoor permutations. Then $G$ simply runs they key generations algorithm
of the TDP to obtain $(\tau,k)$. We let $\tau$ be the secret decryption key and $k$ be the public
encryption key. To encrypt a message $x \in \zo^l$ with key $k$ we chose a random $r$ and then output
$(p_k(r), H(r) \oplus x)$. To decrypt the message $(y,z)$, we use $\tau$ to calculate $H(p_k^{-1}(r)) \oplus z$. 
Note that the above construction is precisely that presented in lecture and in the lecture notes.
Therefore, the construction is CPA-secure given that $\{p_k\}$ is TDP secure and $H$ is a random oracle. 
However, we now construct an algorithm which given the encryption $E_e(x)$ will compute 
the encryption of $E_e(x+1)$. The algorithm works as follows. Given the input $(y,z)$ which is
the encryption of $E_e(x)$, we compute the encryption $c' = (y,z \oplus 1)$. Then note that this is a valid
encryption $E_e(x+1)$. We know this because given that $x$ is a multiple of $2$, we know that the
least significant bit is $0$. Therefore, the least significant bit of $z$ corresponds to the least
significant bit of $H(r)$ since $z = H(r) \oplus x \implies z_l = H(r)_l \oplus x_l = H(r)_l \oplus 0 = H(r)_l$.
Then by flipping the final bit, we flip the final bit of $x$. Since $x$ is even, this implies that
$x$ now becomes $x+1$. Formally, let use take a look at the encryption of $x+1$ where we assume that
we use the same randomness $r$ as before. This is given by $(p_k(r), H(r) \oplus (x + 1))$. Note that
everything is the same as $E_k(x)$ except for the final bit, which is now given by $\bar{H(r) \oplus x}$.
~
    b. (15 points) Show that if $(G,E,D)$ is CCA secure there is no such algorithm in the following sense: if $M$ is any polynomial time algorithm then the probability over $x\getsr [0,2^{\ell}-1]$ and $(e,d)=G(1^n)$ thet $D_d(M(E_e(x)))=x+1$ is negligible.
~ Proof
Suppose not this is not the case and such an algorithm exists. Then we can 
use $M$ to create adversary $\mathcal{A}$ which wins the CCA samge with non-negligible probability. 
On input $1^n$ and public encryption key $e$, choose a random $x \in_R [0,2^l - 2]$. Then submit
as challange texts the messages $m_0 = x$ and $m_1 = x + 1$. On receiving the challange ciphertext
$c^* = E_e(m_b)$ for some $b \in_R \zo$, compute $c' = M(c^*)$ and submit this as a query 
to the decryption oracle, obtaining $m' = D(c') = D(M(c^*))$. If $m' = x + 1$, output $0$, otherweise
output $1$. Then note that the adversary is correct with high probability. Formally, if $M$ exists,
then we have that $\Pr[D_d(M(E_e(m_b))) = m_b + 1] = 1 - negl$ (non negligible). We can condition $b$ and calculate
the probability of success by considering the probability that $\mathcal{A}(1^n,e) = b$:
~
~ Math
  \begin{aligned}
\Pr[\mathcal{A}(1^n,e) = b] &= \Pr[\mathcal{A}(1^n,e) = 1 \mid b = 0]\Pr[b = 0] + \Pr[\mathcal{A}(1^n,e) =1 \mid b = 1]\Pr[b = 0] \\
&= \frac{1}{2}[\Pr[D_d(M(E_e(x))) = x + 1] + \Pr[D_d(M(E_e(x+1))) \neq x + 1]] \\
&= \frac{1}{2}[1 - negl + 1 - negl] \\
&= 1 - negl \\
&> \frac{1}{2} + negl'
\end{aligned}
~
Therefore the $(G,E,D)$ is not $CCA$ secure, a contradiction. We therefore conclude that $M$ does not
exists for a $(G,E,D)$ that is $CCA$ secure.

2. (60 points) Consider a key exchange protocol where the client has the public keys of a server, chooses a key $k \getsr\zo^n$ for
a private key scheme, interacts with the server, and at the end decides whether or not to accept the key as valid. For
simplicity we restrict ourselves to two-message protocols (one message from client to server and one message from
server to client). Consider the following attack on such protocols: (In this attack the adversary completely controls
the network between the client and server, so that all messages transmitted between them go through the adversary.)

    1. Client sends the first message to the adversary.

    2. Adversary gets a polynomial number of interactions with the server, in each such interaction the adversary
    sends a message to the server. The server interprets the message as a first-message from some client, and it
    either accepts a key $k$ as a result of this message and outputs the second message of the protocol  or it
    outputs ```invalid``` . If the server accepted the key $k$, it also outputs $\Epriv_k(0^n)$. The adversary gets
    the outputs of the server.

    3. Adversary sends a message to the client.

    4. If the client accepts the message and obtained a key $k$, then it chooses $b \getsr \{0,1\}$, and does the
    following. If it accepted the key $k$  then the client outputs an encryption $\Epriv_k(0^n)$ if $b=0$, and
    $\Epriv_k(1^n)$ if $b=1$. Otherwise it outputs ```invalid```.

    5. The adversary outputs $b' \in \{0,1\}$. We say the adversary _wins_ if both (i) the client
    accepted the key and (ii) $b'=b$.

We say the protocol is secure  if the probability the
adversary succeeds in this attack is at most $1/2+negl(n)$


**Notation:** We denote by $(\Sign,\Ver)$ a secure signature scheme. We denote by $\Epubcca$ a CCA secure public
key encryption scheme, by $\Epubcpa$ a CPA secure public key encryption scheme, and by $\Epriv$ a CCA secure private
key encryption scheme. The protocol is secure if it is secure for _every_ suitable choice of the underlying
schemes. In all cases we denote by $e$ and by $v$ the public encryption key and verification key of the server, and
assume that the client knows them.

For each of the following protocols, either prove that it is secure (for _every_ suitable choice of the schemes)
or give an example showing it is insecure (for _some_ choice of the schemes).


>**Protocol 1:** (20 points)
>
>1. Client chooses $k \getsr \zo^n$ and $m \getsr \zo^n$ and sends to server $\Epubcpa_e(k \| m)$.
>
>2.  Server decrypts ciphertext to get $k,m$, accepts the key $k$, and sends to client $m,\Sign_s(m)$ (if
    ciphertext is invalid then server sends ```invalid```).
>
>3.  Client verifies $m$ is the same string it sent before, verifies signature and if it passes verification, it
    considers the key $k$ as valid.
~Proof
  Skipped.
~

>**Protocol 2:** (20 points) Same as Protocol 1 but with $\Epubcca$ instead of $\Epubcpa$.
~ Proof
Skipped.
~

>**Protocol 3:** (20 points)
>
>1.  Client chooses $k \getsr \zo^n$ and sends to server $y=\Epubcpa_e(k)$.
>
>2. Server decrypts ciphertext to get $k$, chooses $m\getsr \zo^n$ at random  and sends to client $y$,$m$ and $\Sign_s(y\| m)$ (if ciphertext is invalid then server sends ```invalid```).
>
>3. Client checks $y$ is the same message it sent before, verifies signature and if it passes verification, it   considers the key $k$ as valid.
~Proof
  Skipped.
~

3. (_zero knowledge is trivial for easy languages_, 15 points) We say that $L\subseteq \zo^*$ is decidable in probabilistic polynomial time (in complexity parlance $L\subseteq BPP$) if there is a probabilistic polynomial time algorithm $A$  such that  if $x\in L$ then $\Pr[A(x)=1]>2/3$ and if $x\not\in L$ then $\Pr[A(x)=1]<1/3$. Show that if $L$ is decidable in probabilistic polynomial time then it has a trivial zero knowledge proof system $(P,V)$ in which the prover sends no messages to the verifier.
~ Proof
The trivial system is as follows (call the system $\Pi$), where we use the definitions from lecture. We let $P$ be an
arbitrary polynomial time algorithm (we really don't care what it does) that does not interact
with $V$. For concreteness, however, we have $P$ be defined as always returning the constant $0$ 
(intuitively, this gives no information about the problem) on any query. We define $V$
to do as follows: on input $x$, it executes $A(x)$ $k$ times and outputs the majority answer (we
avoid ties by selecting odd $k$) where
$A_i(x)$ is the $i$-th execution of $A(x)$. We now show that this is a zero knowledge proof system.
We begin with completeness. Let $X_i$ be the indicator r.v for whether or not $A_i(x)$ output the 
correct result ($1$ if $x \in L$ and $0$ if $x \notin L$). Then note that we output $1$
when $\frac{1}{k}\sum_{i=1}^k X_i > \frac{1}{2}$ and $0$ when $\frac{1}{k} \sum_{i=1}^k X_i < \frac{1}{2}$.
Now Suppose $x \in L$. 
Then $V(x)$ will output 1 (Accept) with probability:
~
~ Math
  \begin{aligned}
\Pr[V(x) = 1] &= 1 - \Pr[V(x) = 0] \\
&= 1 - \Pr[\frac{1}{k} \sum_{i=1}^k X_i < \frac{1}{2}] \\
&= 1 - \Pr[\frac{2}{3} - \frac{1}{k} \sum_{i=1}^k X_i > \frac{1}{6}] \\
&> 1 - \Pr[E[X_i] - \frac{1}{k} \sum_{i=1}^k X_i > \frac{1}{6}] \\
&\geq 1 - \Pr[|E[X_i] - \frac{1}{k} \sum_{i=1}^k X_i| > \frac{1}{6}]  \\
&= 1 - 2^{-\frac{k}{144}}
\end{aligned}
~
where we can pick $k$ and therefore can make the above probability as close to $1$ as we wish.
The above follows from the fact that after $k$ repetitions, the majority vote becomes increasingly
likely to be correct. The last line follows from a direct application of the Chernoff bound. Therefore,
for concreteness, we can take $k > 144 \log 0.1$ which will establish the specific bound
discussed in lecture.  Now we show soundness. Take $x \notin L$. Then the probability 
that $V$ returns 1 where we allow the prover $P^*$ to be arbitrary is:
~ Math
  \begin{aligned}
\Pr[V(x) = 1] &= \Pr[\frac{1}{k}\sum_{i=1}^k X_i > \frac{1}{2}] \\
&= Pr[\frac{1}{k}\sum_{i=1}^k X_i - \frac{1}{3} > \frac{1}{6}] \\
&< Pr[\frac{1}{k}\sum_{i=1}^k X_i - E[X_i] > \frac{1}{6}] \\
&\leq Pr[|\frac{1}{k}\sum_{i=1}^k X_i - E[X_i]| > \frac{1}{6}] \\
&= 2^{-\frac{k}{144}} 
\end{aligned}
~
which follows immediately since $V$ doesn't access $P^*$ and therefore we can ignore what $P^*$
might or might not do. Again, the line of reasoning is similar to the previous section
exception we now have $E[X_i] < \frac{1}{3}$ as opposed to $E[X_i] > \frac{2}{3}$ as before, since
$x \notin L$. Note that the same value of $k > 144\log 0.1$ will give a concrete bound
satisfying the definition from lecture.  Finally, we show that the system is zero knowledge. To do this, we allow
an arbitrary efficient verifier $V^*$. Note that $V^*$ cannot interact with $P$ other than by
receiving the result $0$, which is completely uninformative. Therefore we can simply have the
simulation $S^*$ which on each query from $V^*$ simply returns the value $0$, as $P$ would normally
do. Then note that it is trivially true that for all $x \in L$, the output $V^*(x)$ after 
interactions with $P$ will be exactly the same as the output of $S^*(x)$ (since $S^*$ simulates
$P$ perfectly). Therefore, $(P,V)$ is a zero knowledge proof system. 

4. (_Interaction  is necessary for non-trivial zero knowledge_, 15 points)
   Let $L \subseteq \zo^*$. We say that $L$ has a _non interactive_ zero knowledge proof system if there it has a zero knowledge proof system $(P,V)$ where $P$ sends a single message to the verifier. Show that if there is a non-interactive zero knowledge proof system for $L$ then it is decidable in probabilistic polynomial time.[^NIZK]
~ Proof
Suppose we have a non-interactive zero-knowledge proof system $(P,V)$ for the language $L$. Then
we will use this system to construct a polynomial time algorithm $A$ such that if $x \in L$ then
$\Pr[A(x) = 1] > \frac{2}{3}$ and if $x \notin L$ then $\Pr[A(x) = 1 ] < \frac{1}{3}$. First note that
since the system is zero knowledge, there exists a simulator $S$ which on input $x$ produces the
same distribution as $V(x)$. Second, note that because this is a non-interactive system, we can
think of the verifier $V$ as consisting of a single algorithm $V_1(x,m_p,r)$ where $m_p$ is the
message received by the prover $P$ and $r$ is the internal randomness to be used by $V$ when
verifying. Then note that a simulation $S$ will require access to $V_1$. 
Then we define $A$ to do the following on inputs $x$: Run the simulation
$S(x)$ maintaining a count of the number of operations it has completed. If $S(x)$ runs too long
or rejects, then reject. Otherwise, if $S(x)$ accepts with some simulation of the prover's single 
message $m_p$ and some randomness $r$ used by the internal algorithm $V_1$, then chose a new random 
$r'$ and calculate $V_1(x,m_p, r')$. We first prove that if $x \notin L$, then $\Pr[A(x) = 1]< \frac{1}{3}$.
Suppose this is not the case, then we have that $\Pr[A(x) = 1] > \frac{1}{3}$ for $x \notin L$. However,
this implies that $V_1(x,m_p,r') = 1$ with non-negligible probability, which violates the soundness
of the proof system $(P,V)$. Therefore, we must have that $\Pr[A(x) = 1] > \frac{1}{3}$. We now prove
that if $x \in L$, then $\Pr[A(x) = 1] > \frac{2}{3}$. To see this, note that $A(x) = 1$ iff 
$V_1(x,m_p,r') = 1$, which by the completeness of $(P,V)$, we know occurs with probability at least
$0.9$, thereby proving our claim. Note that this naturally extend to computational indistinguishibility
using the fact that this distributions of $V_1(x,m_p,r')$ and $V(x)$ should be indistinguishable
by the fact that $(P,V)$ is zero-knowledge. With the above set, we have created an efficient 
algorithm $A$ such that for $x \in L$, $\Pr[A(x) = 1] > 0.9 > \frac{2}{3}$ and for $x \notin L$,
$\Pr[A(x) = 1] < 0.1 < \frac{1}{3}$, as desired. 
~
[^NIZK]: Because of this negative result, the standard definition in cryptography of a  _non interactive zero knowledge proof system (NIZK)_ is different and assumes some global trusted public parameters (known as a "common reference string"). This allows to bypass the impossiblity result.


5. (_Randomness is necessary for non-trivial zero knowledge_, 15 points)
   Show that if a language $L$ has a zero-knowledge proof system where the verifier is deterministic (i.e., uses no randomness) then $L$ is decidable by a  probabilistic polynomial time algorithm that may use some non uniform "hardwired constants" of $poly(n)$ size.
~ Lemma
We begin by showing that if a language $L$ has a zero-knowledge proof system where the verifier is deterministic,
then $L$ has a proof system with perfect soundness. This is to say that a proof system exists, $(P',V')$
such that for $x \notin L$, $\Pr[V'(x) = 1] = 0$ (we never accept even if $V'$ interacts with
arbitrary prover $P^*$). If the original prove system $(P,V)$ has this above property of perfect
soundness, then we are done. Therefore, let us assume that $(P,V)$ does not have this property.
Then there exists some subset $F$ of $x \notin L$ and some prover $P^*$ 
such that for every $x \in F$, $V(x)$ will return $1$ with some non-zero probability after
interacting with $P^*$ despite the fact that $x \notin L$. 
We consider two cases. In the first case, we have $|F| < \infty$. Then 
we construct a modified $V'$ such that on input $x$ it first checks if $x \in F$ (note that we 
can do this efficiently using a hash-table or by giving lexicographically ordering $F$). If $x \in F$,
then $V'(x) = 0$ immediately. Otherwise, $V'$ gives the input $x$ to $V$ and returns $V(x)$. Note
that this new system is zero-knowledge and complete (since for $x \in L$ it behaves identically
to $V$) and has perfect soundness (since for $x \notin L$, it always returns $0$). Therefore, in this
case, we are are done. Now consider the case where the cardinality of $F$ is infinite. Then we
construct an adversary $\hat{P}$ which causes $V$ to always accept on input $x \notin L$. We know 
that when $V$ interacts with $P^*$ on input $x \in F$ it accepts with non-zero probability. 
This means that for every $x \in F$, there exists a sequence of messages which causes $V$ to
accept with probability $1$ on input $x$. Given this, we can define $\hat{P}$ as an algorithm
that tries all possible sequences of messages until it finds the sequence which causes $V(x)$
to accept. We can check this because $V$ is completely deterministic, and given that $\hat{P}$
can take arbitrarily long, this is perfectly fine. However, this contradict the assumption that
the proof system is sound, and therefore the case where $F$ has infinite cardinality is not possible.
We can therefore conclude that if a language $L$ has a zero-knowledge proof system where
the verifier is deterministic, then we $L$ has a proof system with perfect soundness. 
~
~ Lemma
We now show that if $L$ is a language with a zero-knowledge proof system with perfect soundness, then $L \in BPP$.
We show this by explicitly constructing a probabilistic, polynomial time algorithm $A$ 
such that if $x \in L$, then $\Pr[A(x) = 1] > \frac{2}{3}$ and if $x \notin L$, then $\Pr[A(x) = 1] < \frac{1}{3}$.
Note that $L$ has a zero-knowledge proof system $(P,V)$, therefore for $V* = V$ we have
a simulator $S^*$ that has the same output distribution as $V$. Let $q(|x|)$ be an upper bound
on the steps that $S^*$ takes for $x \in L$. Then we define the algorithm $A$ as follows:
on input $x$, we run $S^*(x)$ and maintain a count of the number of operations that $S^*(x)$ is taking.
If $S^*(x)$ accepts, then $A$ accepts. If $S^*(x)$ rejects, then $A$ rejects. If the count of steps
exceeds $q(|x|)$, then $A$ also rejects. We prove that this is a probabilistic decider for 
the language $L$. Suppose $x \notin L$. Then note that $S^*(x)$ cannot possibly accept. For the
sake of contradiction, suppose it generates a simulation of the interaction between $V$ and $P$ 
which leads to $V$ accepting. Then this means that $V$ could accept $x \notin L$ with this 
exact sequence of interactions, contradicting the fact that our proof system is perfectly sound.
Therefore we have that $A(x) = 0$ with probability $1$ for $x \notin L$ which means
$\Pr[A(x) = 1] = 0 < \frac{1}{3}$. Next we show that 
if $x \in L$, then we have $\Pr[A(x) = 1] > 0.9 > \frac{2}{3}$. This is because the output 
distribution from $S^*$ for $x \in L$ and $V(x)$ after interacting with $P$ must be 
indistinguishable by the fact that the proof system is zero-knowledge. 
~
~ Proof
  We can now conclude our proof. The proof follows immediately from the two lemmas above. 
  If a language $L$ has zero-knowledge proof systems where the verifier is deterministic, then 
  by Lemma 1 the language $L$ has a zero-knowledge proof system with perfect soundness. Then 
  by Lemma 2, $L \in BPP$, which means it is decideable by a probabilistic polynomial time algorithm.
~

6. (40 points) For some prime $q=poly(n)$ and $m\times n$ matrix $A$ over $\Z_q$, we say that $A$ is "spread out" if for every $y\in\Z_q^n$, the probability over random $w\in\zo^m$ that  $w^\top A = y$ is at most $10\cdot q^{-n}$.

      a. (10 points) Prove that if $n$ is sufficiently large, $A$ is a random matrix with entries independently chosen from $\Z_q$ and $m> 1000 n \log q$ then with probability at least $0.99$ $A$ is "spread out". See footnote for hint[^LHL]
~ Proof
We just need to make us of the Leftover Hash Lemma (LHL). For simplicity, we now re-state it.
The LHL states that $\forall m,n,q,t$, if $m > n \log q + 10t$, then with probability
$1 - 2^{-t}$ over $A \in_R \Z_q^{n \times m}$, $\forall D$ (distinguishers), we have that
$|\E[D(A,y)] - \E[D(A,w^\dag A)]| < 2^{-t}$. What we will do is we will assume the problem statement
is not true, and use the result from that problem statement to create a distinguisher $\hat{D}$
between $(A,y)$ and $(A,w^T A)$ that can distinguish with probability at least $q^{-n}$. Letting
$t < 99.9 n \log q$ but still dependent on $n$, we note that the LHL with our definition of 
$m,n,q$ still holds since $m > 1000 n \log q >  n \log q + 999 n \log q = n \log q + 10 * 99.9 n \log q > n \log q + 10 t$,
and therefore we have that with probability $1 - 2^{-t} > 1 - q^{-99.9 n}$ (which is extremely
close to $1$ for large enough $n$), it must be the case that for all distinguishers $D$:
~
~ Math
|\E[D(A,y)] - \E[D(A,w^T A)]| < q^{-99.9 n} < 10 q^{-n}
~
again, for sufficiently large $n$. 
As discussed above, suppose that $\exists n$ such that for $A$ random
and $m > 1000n \log q$, $A$ is "spread out" with probability at most $0.99$, or in other words,
$A$ is not spread out with probability at least $0.1$. This means that $\exists \hat{y} \in \Z_q^n$
such that for $w \in_R \zo^m$ we have that $w^T A = \hat{y}$ with probability at least $10 q^{-n}$. Then
we can use this $\hat{y}$ to construct a distinguisher $\hat{D}$ between the distributions $(A,y)$ and
$(A,w^T A)$ where $y \in_R \Z_q^n$. On input $(A,z)$, the distinguisher checks to see if 
$z = \hat{y}$. If so, it returns $1$ (identifying the distribution as ($A,w^T A)$), and if not,
it returns $b \in_R \zo$. Then we have:
~ Math
|\E[\hat{D}(A,y)] - \E[\hat{D}(A,w^T A)]| > |q^{-n}(1) + (1-q^{-n})(\frac{1}{2}) - 10q^{-n}(1) - (1-10q^{-n})(\frac{1}{2})| > 10q^{-n}
~
for sufficiently large $n$. This is a contradiction on LHL, therefore it must be the case that 
$A$ is spread out. 
      b. (10 points) Prove that if $A$ has the form $(A'|y)$ where $A'$ is an $m\times n-1$ matrix and $y\in \Z^q_m$ has the property that $y = A'x+e$ for some $x\in\Z_q^{n-1}$ and $e$ satisfying $\sum_{i=1}^m |e_i| < q/(100n)$ then $A'$ is _not_ spread out.
~ Proof
We prove this directly by constructing a $\hat{y} \in \Z^n_q$ such that the probability over random
$w \in_r \zo^m$ that $w^T A = y$ is strictly greater than $10 q^{-n}$. Given that we know that 
$A$ is of the form $(A' | y)$ where $y = A'x + e$ for some fixed $x \in \Z_q^{n-1}$ and small
$e$, we let $\hat{y}$ be any vector of the form $(\hat{y}', \hat{y}_n)$ where $\hat{y}' \in\Z_q^{n-1}$
and $\hat{y}_n = \sum_{i=1}^{n-1} \hat{y}_i x_i$. Then note that the last element of $\hat{y}$ is
entirely determined by the first $n-1$ elements. Therefore, for random $w \in \zo^m$, we can
immediately see that by construction, $w^T A= \hat{y}$ with high probability given that for
the given $A = (A', y)$, the last element will be off only by an error which is less that
$q < 100 n$. Therefore we can have a distinguisher between $w^T A$  and $\hat{y}$ which will
check the first $n - 1$ entries, each which will match with probability $1/q$ and therefore all
entries will match with probability $q^{-(n-1)}$, and then checks to see if the last entries 
match each other. Note that this is guaranteed
to occur with very high probability given that the last entry is defined as $w^T A' x$ and where
$\hat{y}' = w^T A'$ and therefore we have that $(w^T A)_n = \sum_{i=1}^{n-1} (w^T A')_i x_i + \sum_{i=1}^{m} w^T_i e_i$
where the last sum has bounded absolute value of $q /(100 n)$. Therefore the last entry matches 
with probability at most $1 / (2q / (100 n)) = 50n/q$. Therefore the probability of a match
for random $w \in \zo^m$, is given by $50n q^{-n} > 10q^{-n}$ for $n > \frac{1}{5}$. We therefore
have the conclusion that $A$ is not spread out.   
~
      c. (20 points) Give a proof system $(P,V)$ (with a potentially inefficient prover) such that on input $A$, if $A$ is spread out then he can convince the verifier that it does _not_ have the form of the first item.
~ Proof
We give a proof system which follows a mechanism similar to that of proving HamCycle from the lecture
notes. I believe the proof system is close to zero-knowledge, though I don't proof this.
Regardless, we show that the proof system is sound and complete. 
~
  a. Common input to both $P$ and $V$ is the matrix $A$. 
  b. $P$'s private input are the $x$ and $e$ such that the last column of $A$ is given by $A'x + e$
  if $A$ is spread out.  
  c. $V$ chooses a random $w \in \zo^m$.
  d. $P$ chooses a completely random vector $x$ and then produces a new vector from this
  matrix which consists of each entry being the result of applying $G$ to each entry of $x$.
  It also choses a noise level $e$, and computes $\hat{y}$ for both $A$ and $A''$ (the new 
  matrix constructed using the choses $x$ and $e$). If $A$ is not of the form from part (b),
  then we let its corresponding $\hat{y}$ be completely random. This
  information is sent to $V$ so $V$ knows that $P$ has committed to this new system, and also has
  access to both $\hat{y}$'s for the original and for the old system. 
  e. $V$ chooses a bit $b \in_R \zo$ and sends it to $P$.
  f. If $P$ receives $0$, then it computes the original $w^T A$ and sends it to $V$.
  g. If $P$ receives $1$, then it computes the modified $w^T A''$ and sends it to $V$.
  h. In either case, $V$ checks to see that the property we know to be true for matrices 
  of the form given in $b$ when multiplied by the chosen $w$ holds. If it almost always holds
  with high probability, then $V$ gains confidence in the fact that $A$ is in fact of the form
  in (b), otherwise, it gains confidence that $A$ is spread out.
  
We can repeat the above process to increase the probability of obtaining a correct result and
decrease the probability of a false positive, so we omit these details. Now for the proof. Suppose
that $A$ is spread out, then note that the verifier will be able to discover this with 
high probability because when $b = 0$, it will compare two completely random vectors, which will
match with very low probability. Similarly, suppose $A$ has the property from part $b$. Then 
when $b = 0$, the verifier will, with high probability, obtain a match between the committed $\hat{y}$
and the original. Note that the prover can't trick $V$ because when $b = 1$ it must produce 
the permuted matrix, which $V$ can easily verify produces the correct results, and since $P$
does not know ahead of time whether $b = 0$ or $b=1$, then it must accurately compute both 
outputs or otherwise run the risk of the verifier losing trust in it by computing an arbitrary
$\hat{y}$. 

      d. (no points, don't write - just think about it) Can you come up with a _zero knowledge_ proof system by which a prover could prove to the verifier in zero knowledge that if $A$ has the form above then it is at least not spread out? One fact you might want to use is that if $e$ has small magnitude and we pick a random $e'$ of much larger (but still not too large) magnitude, then the distributions $\{ e' \}$ and $\{ e + e' \}$ are at least weakly indistinguishable from one another.
~ Proof
Tried to above :) Intuitively, it seems that the prove system is zero knowledge because it follows
the idea behing the zero-knowledge prove system for HamCycle in the sense that we force $P$
to commit to a particular matrix encoding (ie, a graph homomorphism), and then we ask it 
either for the result of the new system (the isomorphism) or for the 
~
   [^LHL]: You can use the Leftover Hash Lemma as was stated  in the lecture on lattices.
