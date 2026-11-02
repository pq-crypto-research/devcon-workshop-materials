# Workshop reference: Reed–Solomon codes and the Proximity Prize

This is a quick reference document for the workshop! We will cover everything, this is just so you have the definitions visually.

This workshop covers all the foundations required to understand the $1 million Ethereum Proximity Prize. 

This workshop assumes you have basic understanding of high schol algebra.

____

Wtf is coding theory? 

Coding theory is about protecting and checking information, not writing computer programs, using "code".

# Definitions:

## 1. Codes and basic objects

**Error-correcting code.** A method for encoding information with redundancy, so that some errors can be detected and, in suitable circumstances, corrected. The valid encoded messages form a set called a *code*, written $C$.

**Finite field ($\mathbb F$).** A finite set of elements where addition, subtraction, multiplication, and division by nonzero elements are defined and obey the usual field rules. Its size $|\mathbb F|$ is a prime or a power of a prime. For example, $\mathbb F_7=\{0,1,2,3,4,5,6\}$ uses arithmetic modulo 7.

**Vector.** An ordered sequence of elements. A length-$n$ vector over $\mathbb F$ is written $u=(u_1,\ldots,u_n)\in\mathbb F^n$.

**Word.** A vector in the ambient space $\mathbb F^n$. It may or may not belong to the code $C$. A received word is the vector a receiver gets, potentially with errors.

**Codeword.** A word that belongs to the code: $c\in C$. Every codeword is a word, but not every word is a codeword.

**Linear code.** A code closed under vector addition and scalar multiplication. If $c_0,c_1\in C$ and $a,b\in\mathbb F$, then $ac_0+bc_1\in C$.

## 2. Reed–Solomon codes

**Polynomial.** An expression such as $f(X)=a_0+a_1X+\cdots+a_{k-1}X^{k-1}$, with coefficients in $\mathbb F$. Here $\deg f<k$: there are at most $k$ coefficients.

**Evaluation domain ($L$).** A set of $n$ distinct field elements $L=\{x_1,\ldots,x_n\}\subseteq\mathbb F$ at which polynomials are evaluated.

**Reed–Solomon code ($C=\operatorname{RS}[\mathbb F,L,k]$).** The set of vectors obtained by evaluating every polynomial of degree less than $k$ at the points of $L$:

$$C=\{(f(x_1),\ldots,f(x_n)): f\in\mathbb F[X],\ \deg f<k\}.$$

The evaluation vector is the codeword. Not every length-$n$ vector comes from a polynomial of degree less than $k$, even though every such vector can be interpolated by a polynomial of degree less than $n$.

**Block length ($n$).** The number of coordinates in a word; for RS, $n=|L|$.

**Dimension ($k$).** The number of field elements used to specify an RS message polynomial's coefficients, assuming $k\le n$.

**Rate ($\rho$).** The fraction of coordinates that represent independent message information: $\rho=k/n$. Lower rate means more redundancy. The reciprocal $n/k=1/\rho$ is often called the *blowup factor*.

## 3. Distance and agreement

**Hamming distance (absolute distance).** The number of positions at which two equal-length words differ:

$$d_H(u,v)=|\{i:u_i\ne v_i\}|.$$

**Relative distance.** Hamming distance divided by word length: $\Delta(u,v)=d_H(u,v)/n$. It is a fraction between 0 and 1.

**Minimum distance of a code.** The smallest Hamming distance between two distinct codewords. The *relative minimum distance* is this value divided by $n$. For RS with degree $<k$, these are $n-k+1$ and $(n-k+1)/n$, respectively.

**Distance from a word to a code.** The distance to its nearest codeword: $\Delta(u,C)=\min_{c\in C}\Delta(u,c)$. A word is **$\delta$-close** to $C$ if $\Delta(u,C)\le\delta$; it is **$\delta$-far** if the distance is greater than $\delta$. Some sources use slightly different boundary conventions.

**Hamming ball.** The set of all words within a given distance of a center word. A relative-radius-$\delta$ ball around $u$ contains all $v$ with $\Delta(u,v)\le\delta$.

**Agreement set.** The positions where two words agree: $\operatorname{Agr}(u,v)=\{i:u_i=v_i\}$. Its agreement fraction is $|\operatorname{Agr}(u,v)|/n=1-\Delta(u,v)$.

**$S$ and restriction.** $S$ is a subset of coordinate positions (equivalently, a subset of $L$ when positions are identified with evaluation points). $u|_S$ means the entries of $u$ on $S$.

**$(S,\delta)$-close to $C$.** A word $u$ is $(S,\delta)$-close if $|S|\ge(1-\delta)n$ and there is a codeword $c\in C$ such that $u|_S=c|_S$. In words: $u$ agrees with a codeword on every position in the specified large set $S$. The set $S$ can be a subset of the full agreement set.

**Why specify $S$?** Ordinary $\delta$-closeness says a sufficiently large agreement set exists. $(S,\delta)$-closeness specifies *which* large set must work. If $u$ is $(S,\delta)$-close, it is $\delta$-close, but a particular $S$ need not witness an ordinarily close word.

## 4. Decoding and proximity

**Decoding.** Given a received word, recover one or more codewords close to it. In applications, a decoder may use this to recover the original message, provided the assumptions on errors hold.

**Unique decoding.** Recover the only codeword within a given radius, when such uniqueness is guaranteed. For relative minimum distance $\delta_{\min}$, uniqueness is guaranteed at radii strictly less than $\delta_{\min}/2$.

**List decoding.** Find all codewords within a specified radius, allowing multiple candidates. A word may be close to several codewords.

**Johnson radius.** A general list-decoding threshold; for RS codes it is approximately $\delta_J=1-\sqrt{\rho}$, ignoring finite-length corrections. It is a sufficient threshold, not a proof that larger radii are impossible.

**List-decoding capacity.** The asymptotic benchmark $1-\rho$: roughly the largest error fraction one can hope to approach at rate $\rho$ while maintaining suitably bounded lists. Precise statements depend on parameters and the bound desired.

**Proximity testing.** Instead of recovering a codeword, test whether a word is close to the code or far from it, usually by inspecting only a small number of positions. A probabilistic test can have a small soundness error.

## 5. Random combinations and MCA

**Random linear combination.** Given words $f,g\in\mathbb F^n$, sample $\alpha\in\mathbb F$ and form $h_\alpha=f+\alpha g$ coordinate by coordinate. Because RS is linear, if both inputs are codewords, $h_\alpha$ is always a codeword. If the inputs are not codewords, a particular combination can nevertheless be close to, or even equal, a codeword.

**Mutual correlated agreement (MCA), informally.** Ask whether a random combination $h_\alpha=f+\alpha g$ agreeing with a codeword on a large set $S$ implies that *each input* agrees with some codeword on that **same** set $S$. The input codewords may be different. A good MCA bound says that, except with small probability over $\alpha$, this implication holds for the sets and parameters covered by the theorem.

**MCA error.** An exceptional challenge $\alpha$ for which a large set $S$ witnesses $h_\alpha$ as close to a codeword, but at least one of $f$ or $g$ has no codeword agreeing with it on all of that same $S$. It is not enough merely to find different agreement sets for the inputs.

**Proximity gap.** Informally, the difference between the proximity parameters needed on two sides of a guarantee about random combinations and their inputs. The precise definition depends on the theorem or protocol; the Proximity Prize seeks stronger bounds relevant to efficient proximity testing.

## 6. Connection to STARKs

**Execution trace.** A table recording the steps or states of a computation. STARK provers represent trace columns using polynomial evaluations and use polynomial checks to show the trace obeys the computation's rules.

**Low-degree extension (LDE).** Evaluate a polynomial that represents data on a larger domain. The resulting evaluation vector has RS structure and adds redundancy useful for testing.

**FRI / WHIR.** Protocols for proving or checking low-degree structure efficiently. At a high level, they use random challenges, commitments, and queries; their soundness analyses relate to how proximity behaves under random combinations. Better proximity bounds can improve proof parameters, but the exact savings depend on the protocol and chosen security target.

**Soundness error.** The probability that a verifier accepts a false claim, under the relevant assumptions. A soundness error of about $2^{-\lambda}$ corresponds to roughly $\lambda$ bits of soundness security.

---

**Notation reminder:** $n=|L|$, $k$ is the number of message coefficients, $\rho=k/n$, and $\delta$ denotes a relative distance or proximity threshold. Distinguish the variable $\delta$ from a code's fixed minimum relative distance $\delta_{\min}$.
