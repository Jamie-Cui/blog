---
layout: default
---

## 1.1 加密 (General)

Roughly speaking, encryption is the problem of how two parties can communicate in secret in the presence of an eavesdropper. 

通俗来讲，加密是 Alice 和 Bob 在有第三方监听情况下可以实现安全通信的技巧。

### 1.1.1 Shannon cipher

Shannon cipher is a pair of $\mathcal{E}(E,D)$ functions, which holds the following properties:

- $E=(k,m)$
- $D=(k,c)$
- Correctness: $D(E(k,m))=m$

Then we present the *formal definition of encryption* 

> Assume we have key space $\mathcal{K}$, message space $\mathcal{M}$, cipher space $\mathcal{C}$. We can write:
> $$E:\mathcal{K}\times\mathcal{M}\to\mathcal{C}$$
> $$D:\mathcal{K}\times\mathcal{C}\to\mathcal{M}$$

### 1.1.2 One-time pad

> Fun fact, in mathematics, "$:=$" means a definition, not a proven/provable statement

One-time pad is shannon ciphers where keys, messages and ciphers have thte same length. $\mathcal{E}(E,D)$ is defined over $(\mathcal{K},\mathcal{M},\mathcal{C})$

$$\mathcal{K}:=\mathcal{M}:=\mathcal{C}:=\{0,1\}^L$$

$$E(k,m) := k \oplus m$$
$$D(k,c) := k \oplus c$$

**Variable length one-time pad**, again, $\mathcal{E}(E,D)$ is defined over $(\mathcal{K},\mathcal{M},\mathcal{C})$

$$\mathcal{K}:=\{0,1\}^L\quad \mathcal{M}:=\mathcal{C}:=\{0,1\}^l, \;\text{where}\;l\leq L$$

$$E(k, m):=k[0,...,l-1]\oplus m$$

$$D(k, c):=k[0,...,l-1]\oplus c$$

**Subsitution cipher**, again, $\mathcal{E}(E,D)$ is defined over $(\mathcal{K},\mathcal{M},\mathcal{C})$. Let $\mathcal{S}$ be symbol space $\{A,...,Z,[\quad],\}$ (consisting of A-Z and space), in this situation, message space and cipher space are both squence of symbols from $\mathcal{S}$ with length $L$.

$$\mathcal{M}:=\mathcal{C}:=\mathcal{S}^L$$

And the key space $\mathcal{K}$ consists all the permutations on $\mathcal{S}$, for instance, $k=\{[\quad],A, B,C...Z\}$, in that case, key space $\mathcal{K}$ is sufficiently large, $\mathcal{K}\approx 0.9\cdot 10^{28}$, we define $k^{-1}$ as the inverse permutation of $k$.

$$E(k, m) = (k(m[0]),k(m[1]),...,k(m[L-1]))$$

$$D(k, m) = (k^{-1}(m[0]),k^{-1}(m[1]),...,k^{-1}(m[L-1]))$$

**Additive one-time pad**, again, $\mathcal{E}(E,D)$ is defined over $(\mathcal{K},\mathcal{M}, \mathcal{C})$. We use modulus (mod) as a variation of one time pad.

$$\mathcal{K}:=\mathcal{M}:=\mathcal{C}:=\{0,1,...,n-1\}$$

$$E(k,m) = k+m\;\text{mod}\;n$$

$$D(k,c) = c-k\;\text{mod}\;n$$

### 1.1.3 Perfect Security

What is a “secure” cipher? Intuitively, the answer is that a secure cipher is one for which an encrypted message remains “well hidden,” even after seeing its encryption form. However, **turning this intuitive answer into one that is both mathematically meaningful and practically relevant is a real challenge**.

From that intutitive definition, we have
1) key $k$ should be hard to guess, therefore it should be chosen at random from a large key space. That is to prevent the "brute-force" atttack. 
首先从密钥的角度考虑，密钥需要很难被猜出来，需要耗费不合理的计算资源去进行暴力破解，因此需要密钥空间“大”，并且“均匀分布”。

2) $m$ is "well hidden", means it should be hard to determine $m$ from $c$ without any knowledger about $k$. 
接下来从 $\mathcal{E}(E,D)$ 的角度进行考虑，即在不知道 $k$ 的任何信息（除密钥空间$\mathcal{K}$）的条件下，不能从 $c$ 直接推导出来 $m$。

**The attacker many have prior knowledege about message space**, consider a very simple example, Alice sends an encryption form of "yes" or "no" to Bob, and an evesdropping advarsary Charlie wants to guess the messages (notice that we have skipped the one message case, coz it's trivial to prove security when the Chalie has already known message have only one message). The possibility of Chalie guessed out the correct message should be 50%.

但是呢，上面给出的安全定义并不能涵盖所有情况。在真实情况中，明文空间不会是无限大的，更极端的条件下 $\mathcal{M}:=\{m_0, m_1\}$，我们同样要保证在这种情况下的安全。因此就诞生了“完美安全”这个概念。

完美安全是加密安全最严格的定义。其定义了在最严格的条件下 $\mathcal{M}:=\{m_0, m_1\}$，并且密钥空间 $\mathcal{K}$ 足够大并且均匀分布时，对于一个加密后的密文 $c$ 来说，攻击者推断出正确答案的概率是 50%。

> **Definition of Perfect Security:**  Let $\mathcal{E}(E,D)$ be a Shannon cipher defined over $(\mathcal{K},\mathcal{M},\mathcal{C})$. Consider a probabilistic experiment in which the random variable $\mathbf{k}$ is uniformly distributed over $\mathcal{K}$. If for all $m_0, m_1\in\mathcal{M}$, and all $c\in\mathcal{C}$, we have
> $$Pr[E(\mathbf{k},m_0)=c]=Pr[E(\mathbf{k},m_1)=c]$$
> then we say that $\mathcal{E}$ is a perfectly secure Shannon cipher.

完美加密的概念也可以使用 "Structuring Security Proofs as Sequences of Games" 这个概念来证明。

---

**Theroem 1.1.3.1**: Let $\mathcal{E}(E,D)$ be a Shannon cipher defined over $(\mathcal{K},\mathcal{M},\mathcal{C})$. The following are equivalant:
1) $\mathcal{E}$ is perfectly secure
2) For every $c\in\mathcal{C}$, there exists $N_c$ (possibly depending on c) such that for all $m\in\mathcal{M}$, we have $|\{k\in\mathcal{K}:E(k,m)=c\}|=N_c$
3) If the random variable $\mathbf{k}$ is uniformly distributed over $\mathcal{K}$, then each of the random variables $E(\mathbf{k},m)$, for $m\in\mathcal{M}$, has the same distribution.

---

**Theroem 1.1.3.2**: The one-time pad is a perfectly secure Shannon Cipher.

*Proof*. Suppose that the Shannon cipher $\mathcal{E}(E,D)$ is a one-time pad, and it's defined over $(\mathcal{K},\mathcal{M},\mathcal{C})$, where $\mathcal{K}:=\mathcal{M}:=\mathcal{C}:=\{0,1\}^L$. For any fixed message $m\in \{0,1\}^L$ and ciphertext $c\in\{0,1\}^L$, there is a unique key $k\in\{0,1\}^L$ satisfying the equation

$$k\oplus m=c$$

therefore it satisfies condition (ii) in Theroem 1.1.3.1 (2).

但是对于 **Various-length one-time pad** 来证明其不满足“Perfect Secure”是很复杂的，实际上，我们依然可以通过 Theroem 1.1.3.1 中的 2. 来证明其不满足 Perfect Security。我们随意选取一个长度为1的明文信息 $m_0 \in\{1,0\}$，一个长度为2的明文信息 $m_1$；再假设有任意一个长度为1的明文信息 $c$，并选取 $\mathbf{k}$ 为密钥空间内随机选取的一个密钥，那么

$$Pr[E(\mathbf{k}, m_0)=c]=1/2 \quad Pr[E(\mathbf{k},m_1)=c]=0$$

首先因为

这样就违反了我们在 Theroem 1.1.3.1 中的定义。

For the **subsitution cipher**, it is not a perfectly secure Shannon cipher. Recall that

$$\mathcal{M}:=\mathcal{C}:=\mathcal{S}^L$$

Different from old-fashioned OTP, substitution ciphers holds a map for every char in $m$, where

---

### Computational ciphers and semantic security

For perfect security, the only way to achieve perfect security is to have keys that are as long as messages. However this is impractical. The way we shall od this is to considerr not all possible advarsaries, but only *computational feasible* adversaries, that is the "real world" adversaries must perform their calculations on read computers using a resonable amount of time and monery.

This leads to a weaker security model: **"semantic security"**
