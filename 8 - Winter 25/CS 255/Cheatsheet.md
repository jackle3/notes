
# Security
Our securities is constructed from an adversarial game involving a challenger $C$ and an efficient adversary $A$. Below are each definition and its corresponding adversarial game:

## 1. Security of a PRF $F$
1. $C$ chooses to run one of
	* experiment 0: $f(x) = F(k,x)$ for some randomly chosen key $k \in K$
	* experiment 1: $f(x)$ is some randomly chosen function $f \in \text{Funs}(X,Y)$
2. $A$ sends $C$ a query $x \in X$
3. $C$ returns $f(x)$
4. Repeat steps 2-3 as many times as necessary
5. $A$ guesses whether they have been receiving $f(x) = F(k,x)$ or a random $f(x)$ by choosing $\text{EXP} = 0$ or $1$, respectively

$F$ is secure if
$$
\text{Adv}(A,F) = |\Pr[\text{EXP}(0) = 1] - \Pr[\text{EXP}(1) = 1]|
$$
is negligible, where $\text{EXP}(b)$ is $A$'s guess when $C$ runs experiment $b$.

## 2. Semantic Security of a Cipher $(E,D)$ (such as a One-time pad) with a One-time Key
1. $A$ sends $C$ a pair of plaintexts $(m_1,m_2) \in X^2$
2. $C$ returns either option 0: $E(k,m_0)$ or option 1: $E(k,m_1)$ for some randomly chosen key $k \in K$
3. $A$ guesses whether they have received $E(k,m_0)$ or $E(k,m_1)$ by choosing $W = 0$ or $1$, respectively

$(E,D)$ is semantically secure if
$$
\text{Adv}(A,(E,D)) = |\Pr[W_0 = 1] - \Pr[W_1 = 1]|
$$
is negligible, where $W_b$ is $A$'s guess when $C$ returns option $b$.

## 3. CPA Security of a Cipher $(E,D)$ (such as a One-time pad)
1. $C$ first chooses $b = 0$ or $1$ and randomly chooses a key $k \in K$
2. $A$ sends $C$ a pair of plaintexts $(m_1,m_2) \in X^2$
3. $C$ returns $E(k,m_b)$
4. Repeat steps 2-3 as many times as necessary
5. $A$ guesses whether they have been receiving $E(k,m_0)$ or $E(k,m_1)$ by choosing $W = 0$ or $1$, respectively

$(E,D)$ is CPA secure if
$$
\text{Adv}(A,(E,D)) = |\Pr[W_0 = 1] - \Pr[W_1 = 1]|
$$
is negligible, where $W$ is $A$'s guess when $C$ returns option $b$.
