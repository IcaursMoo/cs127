% Homework 9: Multiparty secure computation
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

### Total of 115 points

1. (25 points) Let $F$ be the two party functionality such that $F(H\|C,H')$ outputs $(1,1)$ if the graph $H$ equals the graph $H'$ and $C$ is a Hamiltonian cycle and otherwise outputs $(0,0)$. Prove that a protocol for computing $F$ is a zero knowledge proof (w.r.t. an _efficient_ prover) system for the language of Hamiltonicity.
~
We let $F$ be the two party functionality as described. We then show that the protocol for _securely_
computing $F$ is a zero knowledge argument system for the language of Hamiltonicity. The protocol
for computing $F$ consists of two parties, Alice and Bob, interacting with each other. Alice
on input $H\|C$ and Bob on input $H'$. Let Alice be the prover, $P$, with secret $C$ (which is fed
as part of Alice's input), and let Bob be the verifier, $V$. Let the public input to both be 
be $H$, where we feed $H$ as part of the input to Alice and we feed $H' = H$ as the input to Bob.
We first proof that the protocol is an argument system by showing soundness and completeness. Let
$L$ be the language consisting of graphs with Hamiltonian cycles.
~
*Completeness*: If $x \in L$, then on public input $x$ if $P$ and $V$ are given input $x$ and interact, then at the
end of the interaction $V$ will output $1$ with probability $1-negl$. 
~Proof
  Note that $x = H$, a graph with a Hamiltonian Cycle $C$. Then $x$ is fed to both $V$ and $P$,
  which means Alice is fed $H \| C$ and Bob is fed $H' = H$. Therefore, by the definition of $F$,
  since $H = H'$ and $C$ is a Hamiltonian cycle, $F(H\| C) = (1,1)$ which means Bob (the verifier)
  outputs $1$.
  [&box;]{float=right}
~
*Soundness*: If $x \notin L$ then for any efficient algorithm $P^*$, if $P^*$ and $V$ are given 
input $x$ and interact then at the end $V$ will output $1$ with $negl$ probability. 
~ Proof
Let $T$ (the subset controlled by the adversary) consist of Alice. Alice acting under the control
of the adversary is equivalent to $P^*$. Then by the definition of a secure protocol, the output
under this protocol is computationally indistinguishable from the output under a protocol with an
ideal simulator $S$ that choses the input to honest Alice to be $H$, where $H$ is a simple
cycle (and therefore contains a Hamiltonian cycle). Then note that $H \neq x$ (since $x\ notin L$),
and therefore the output of the protocol will be $(0,0)$ in the scenario where $P^*$ does not
abort, and will be $(_,\bot)$, which implies that Bob (the
verifier) outputs $1$ with probability $0$ (since it either outputs $0$ or aborts).
[&box;]{float=right}
~
*Zero Knowledge*: A proof system $(P,V)$ is zero-knowledge if for every efficient verifier strategy
$V^*$ and every $x \in L$, there exists an efficient probabilistic algorithm $S^*$ (known as the
simulator) such that the following are computationally indistinguishable:
  - The output of $V^*$ after interacting with $P$ on input $x$.
  - The output of $S^*$ on input $x$.
  
~ Proof
The proof follows almost immediately from the definition of a _secure_ protocol for computing
the two-party functionality of $F$. Take $T$ to be the subset consisting of Bob (the verifier). 
Bob acting under the control of the adversary is $V^*$. Then by definition, there exists 
an efficient ideal adversary $S$ such that on inputs $x$ for Alice, the following distributions
are indistinguishable:
  
  - The tuple $(y_1,y_2)$ where this is the output of the Alice and adversary Bob interacting.
  - The tuple $(y_1',y_2')$ where $S$ choses the input to Bob to be $x$ (same as Alice) 
  and then runs the protocol honestly if Bob does not abort. Otherwise, then alice computes
  $y_1$ and $y_2 = \bot$ (or something chosen by $S$).
  
  Then simply let $S^* = S$ where $S^*$ truncates the output of $S$ to simply be the second tuple
(Bob's output). Then note that by the definition above, $\{y_2\} \approx \{y_2'\}$. Therefore the
output of $V^*$ is indistinguishable from the output of $S^*$ with input $x$.
[&box;]{float=right}
~
The above concludes the proof that the system is a zero knowledge argument system.
2. (25 points) Let $F$ be the $k$-party functionality that on inputs $x_1,\ldots,x_k \in \zo$ outputs to all parties the majority value of the $x_i$'s.  Prove that in any protocol that securely computes $F$,  for any adversary that controls less than half of the parties, if at least $k/2+1$ of the other parties' inputs equal $0$, then the adversary will not be able to cause an honest party to output $1$.
~ Proof
Let us have a protocol the securely computes $F$, and an adversary $A$ that controls less than half
of the parties ($|T| < \frac{k}{2}$). Suppose for the sake of contradiction that at least $k/2 + 1$
of the other parties ($[k]\setminus T$) inputs equal $0$, yet the adversary is able to cause
an honest party to output $1$. Let the honest party influenced by the actions of $A$ 
be $H \in [k]\setminus T$, so that on input $x_H$, the output is $y_H = 1$. 
Let the output for the entire function be $(y_1,y_2,\cdots,y_H,\cdots,y_k)$. Consider any 
ideal adversary $S$. In the case where the adversary's last non-abort state is
some $k' < H$, because $H \notin T$, we must have that $y_H = \bot$. However, this is immediately
distinguishable from the previous distribution. In the case where the adverary's last non-abort
state is some $k' \geq H$, we must have that $y_H = y_H'$ where $y_H'$ is computed by letting
$S$ chose the inputs $\{x_i\}_{i \in T}$ and the running the honest protocol. However, since 
$|T| < k/2$, by definition of $F$ and the fact that $k/2 + 1$ other parties received input $0$,
we have that $y_H = 0$. This is again immediately distinguishable from the previous distribution.
Therefore, no such ideal adversary $S$ exists. This, however, contradicts the assumption that
the protocol securely computes $F$, and therefore our assumption that the adversary $A$ could
influence an honest party to output $1$ is wrong. 
[&box;]{float=right}
~
3. (25 points) For two distributions $X,Y$ over some set $\Omega$, we define their _total variation distance_, denoted as $\Delta(X,Y)$ as $\sum_{\omega\in\Omega}\left| \Pr[X=\omega]-\Pr[Y=\omega]\right|$. If $X$ is a distribution over $\Omega$ then we denote by $X^m$ the distribution over $\Omega^m$ where every entry of $X^m$ is sampled independently from $X$ (i.e., $\Pr[ X^m = (\omega_1,\ldots,\omega_m)]=\Pr[X=\omega_1]\cdots\Pr[X=\omega_m]$). Prove that if two distributions  $X$ and $Y$ satisfy $\Delta(X,T)<\delta$ then  $\Delta(X^m,Y^m) \leq m\delta$.
~ Proof
We proof by induction, by looking at a single coordinate in the distributions of $X^m$ and $Y^m$ at
a time. We show that for all $m$, if $\Delta(X^m,Y^m) < \delta$, then:
~
~ Math
  \Delta(X^m,Y^m) \leq m\delta
~
For the base case, we see it is immediate that the statement holds for $m=1$. Assume that the statement
holds for $m = i$. We show that it also holds for $i + 1$. 
~ Math
\begin{aligned}
  \Delta(X^{i+1},Y^{i+1}) &= \sum_{(w_1,\cdots,w_{i+1}) \in \Omega^{i+1}} |\Pr[X^{i+1} = (w_1,\cdots,w_{i+1})] - \Pr[Y^{i+1} = (w_1,\cdots,w_{i+1})]| \\
  &=  \sum_{(w_1,\cdots,w_{i+1}) \in \Omega^{i+1}} |\Pr[X = w_1]\cdots \Pr[X = w_{i+1}] - \Pr[Y =w_1]\cdots \Pr[Y = w_{i+1}]| \\
  &\leq  \sum_{w_{i+1} \in \Omega} |\Pr[X = w_{i+1}] - \Pr[Y = w_{i+1}|  \\
  &+\sum_{(w_1,\cdots,w_{i}) \in \Omega^{i}} |\Pr[X = w_1]\cdots \Pr[X = w_{i}] - \Pr[Y =w_1]\cdots \Pr[Y = w_{i}]| \\
  &= \sum_{w_{i+1} \in \Omega} |\Pr[X_{i+1} = w_{i+1}] - \Pr[Y_{i+1} = w_{i+1}| \\
  &+ \sum_{(w_1,\cdots,w_{i}) \in \Omega^{i}} |\Pr[X^{i} = ((w_1,\cdots,w_{i}))] - \Pr[Y^{i} = ((w_1,\cdots,w_{i}))]|\\
  &\leq \delta + i\delta \\
  &= (i+1)\delta
\end{aligned}
~
The key step occurs at the inequalities above. In the first inequality, we have that the total 
variational distance between two distributions is at most the variational distances between 
the distribution where we've fixed one coordinate to be constant plus the variational distance
in that single coordinate. This follows from the fact that variational distance is a valid
distance metric, and therefore must satisfy the triangle inequality. The last inequality
follows from our inductive hypothesis. Therefore, by the above, we've proven the statement.
[&box;]{float=right}

4.  (40 points) For a prime $p>5$, suppose that we select a random degree $2$ polynomial $S(x) = s_0 + s_1x+ s_2x^2$ moudlo $p$ by selecting $s_0,s_1,s_2$ independently and uniformly from $\Z_p$, and consider the random variable $(S(1),S(2),S(3),S(4),S(5)) \in \Z_p^5$.

     a. (10 points) Prove that for every distinct $i,j,k \in \{1,\ldots, 5 \}$, there is an algorithm to recover $S(0)$ from $S(i),S(j),S(k)$.
     ~Proof
     We prove this by considering the following matrix system. Note that we can encode the information
     in the problem statement into the matrix:
     ~
     ~Math
      \begin{pmatrix} 
      1 & i & i^2 \\
      1 & j & j^2 \\
      1 & k & k^2 
      \end{pmatrix}
      \begin{pmatrix}
      s_0 \\ s_1 \\ s_2
      \end{pmatrix}
      =
      \begin{pmatrix}
      S(i)\\S(j)\\ S(k)
      \end{pmatrix} \pmod{p}
     ~
     Note that if the above matrix is invertible, then we have a unique solution for the constants
     $s_0,s_1,s_1$. Then we've solved for $S(0) = s_0$, and in fact, have solved the entire polynomial.
     Recall that a matrix is invertible if and only if its columns are linearly independent. Given
     that $i \neq j \neq k$, we immediately have the independence of the first two columns. We
     futher claim that the third column is linearly indendent of the first two. For contradiction,
     suppose this is not the case. Then this means that there exists $c_0,c_1$ such that the following
     equations hold:
     ~Math
     \begin{aligned}
      c_0 + c_1 i &= i^2 \pmod{p} \\
      c_0 + c_1 j &= j^2 \pmod{p} \\
      c_0 + c_1 k &= k^2 \pmod{p} \\
     \end{aligned}
     ~
     However, combining 1 and 2 and 1 and 3, we have:
     ~Math
     \begin{aligned}
      c_1 (i-j) &= i^2 - j^2 = (i-j)(i+j) \pmod{p}\\
      c_1 (i-k) &= i^2 - k^2 = (i-k)(i+k)\pmod{p}\\
     \end{aligned}
     ~
     We know that $i \neq j \pmod p$ and $i \neq k \pmod p$, 
     therefore we can multiply by the inverse of $(i-j)$ and $(i-k)$. This gives us:
     ~Math
     \begin{aligned}
     c_1 &= (i+j) \pmod{p}\\
     c_1 &= (i+k) \pmod{p}
     \end{aligned}
     ~
     However, the above implies that $0 = i+j = i + k  \pmod{p} \implies j = k \pmod{p}$, a contradction on the fact 
     that $i,j,k$ are distinct. Therefore, $c_0,c_1$ such that the above holds cannot exist, and
     the last column is linearly independent of the first two. Therefore, the matrix consists 
     of linearly indendent columns, which implies it is invertible. We then have:
     ~Math
      \begin{pmatrix}
      s_0 \\ s_1 \\ s_2
      \end{pmatrix} = \begin{pmatrix} 
      1 & i & i^2 \\
      1 & j & j^2 \\
      1 & k & k^2 
      \end{pmatrix}^{-1}
       \begin{pmatrix}
      S(i)\\S(j)\\ S(k)
      \end{pmatrix} \pmod{p}
     ~
     so we can immediately solve for $S(0)$ simply be runnign the above process (note that 
     this is efficient).
     
     b. (10 points) Prove that for every $i,j \in \{1,\ldots,5\}$, the distribution of $S(0),S(i),S(j)$ is the uniform distribution over $\Z_p^3$. Conclude that there is no algorithm to recover $S(0)$ from $S(i)$ and $S(j)$.
     ~Proof
     Note that $S(0) = s_0$, which is already selected uniformly at random. Note that this
     gives us no information about $s_1$ and $s_2$, therefore the distribution $(S(0), S(i))$ for
     random $i$ is also uniformly random, since $S(i) = s_0 + s_1i + s_2 i^2$ can be treated as a 
     random number proportional to $i(s_1 + s_2 i)$ where $s_2 i$ is random by the randomness
     of $s_2$, and $s_1 + s_2 i$ is random by the randomness of $s_1$ and $s_2i$, and finally,
     the entire result is uniform by the uniformity of $(s_1 + s_2 i)$. Does knowing
     $S(0) = s_0$ and $S(i) = s_0 + s_1 i  + s_2i^2$ give us any information about
     $S(j) = s_0 +s_1 j + s_2j^2$ where $i \neq j$? Note that we can substract $S(0)$ from
     all polynomials, and we have $S(j) - S(0) = s_1j + s_2j^2$ and $S(i) - S(0) = s_1i + s_2i^2$. 
     Then it's immediately clear that knowing because $i \neq j$, the distribution $(S(i), S(j))$ is
     uniform, which implies that the distributions $(S(0),S(i),S(j))$ is uniform. From this,
     we can immediately see that no algorithm can exist to recover $S(0)$ from $S(i)$ and $S(j)$.
     For contradiction, suppose such an algorithm $A$ exists, which on input $S(i)$ and $S(j)$ 
     recovers $S(0)$. Then we would be able to use the algorithm to distinguish between the
     distributions $\{U^3\}$ and $\{S(0),S(i),S(j) \}$ by simply checking if the output of $A$ 
     in the second two inputs is the same as the first input to the distinguisher, contradicting
     our previous results. Therefore, no such algorithm can exist. 
     [&box;]{float=right}
     ~
     c. (20 points) The "pretty good privacy (PGP)" software used to have (essentially) the following mechanism for key recovery. To hide a key $K\in\zo^n$, we pick a prime $p>2^n$ (and so can think of $K$ as a member of $\Z_p$). The user would record $5$ question answer pairs $(q_1,a_1),\ldots,(q_5,a_5)$ (each encoded as a string). We let $H$ be a hash function that maps $\zo^*$ to $\Z_p$ and model it as a random oracle. Then we pick a random salt $salt\in\zo^n$, random degree $2$ polynomial $S$ as above subject to $S(0)=K$ and store the data block $D=(q_1,\ldots,q_5,salt,z_1,\ldots,z_5)$ where $z_i = H(a_i\|salt)+S(i) (\mod q)$ on the user's machine.

         i.  (10 points) Prove that given this information, a user who remembers at least three of the answers to the questions can recover the key $K$.
         ~Proof
         Let $(q_i, a_i), (q_j,a_j), (q_k,a_k)$ be the three distinct question-answer pairs the user
         remembers, implying that $i \neq j \neq k$ where $i,j,k \in \{1,\cdots,5\}$. Note
         that we can determine these indexes because we stored the questions $q_i$ for $i \in \{1,\cdots 5\}$.
         and are given the answers by the user. Then by the determinism of $H$, we can determine
         $S(i) = z_i - H(a_i \| salt)$ (similarly for $j$ and $k$). Therefore, by the results from
         part (a) above, we can use $S(i),S(j),S(k)$ to recover $S(0) = K$. 
         ~
         ii. (10 points) Prove that in the random oracle model, one can transform a time $T$ adversary $A$ that succeeds in recovering a random key $K$ from $D$ with probability at least $1/2$ into an adversary $A'$ that outputs three of the answers in $\{ a_1,\ldots, a_5 \}$ with probability at least $1/4$.
         ~Proof
         We provide the transformation from the adversary $A$ that recovers key $K$ to the adversary
         $A'$ that outputs three answers with probability $\frac{1}{4}$.
         The adversary does the following on input $D$ (the data block):
          
          - We feed $D$ to $A$ and recover the key $K$ with probability $\frac{1}{2}$. 
          - We monitor the queries that $A$ makes to the oracle $H$, recording the input $(a_i \| salt)$ and the output ($z_i'$).
          - Now that we know $K$, we can determine the indexes that correspond to each 
         Did not finish. The idea would be to somehow submit the same question multiple times? I'm
         just not sure whether the new $A$ has access to the queries $A$ makes to $H$? If it does,
         then why not just record the inputs $a_i \| salt$ and then pick randomly three of those?
         We would need to prove that at least three queries are required and any above $5$ are
         redundant, but doesn't this just follow from the part above? It seems too straightforward,
         I'd like more time to think about it. 
         ~