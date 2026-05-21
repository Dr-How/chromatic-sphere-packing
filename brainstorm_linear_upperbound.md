# Brainstorm: Linear Upper Bound Conjecture M(n) = O(n)

## Conjecture
**M(n) ≤ C·n for some absolute constant C > 1, for all n ≥ 1.**

This would be a dramatic improvement over the current best M(n) ≤ O(τ(n)·√(log n/n)),
replacing the exponential τ(n) with a linear function. It is consistent with the
lower bound M(n) ≥ n + Ω(n^{2/3}), which already forces C ≥ 1.

---

## 1. Evidence For the Conjecture

### 1a. All known constructions give O(n)
| Family | Dimension | χ | χ/n |
|--------|-----------|---|-----|
| Regular n-simplex (trivial) | n | n+1 | →1 |
| A_n lattice packing | n | n+1 (exact) | →1 |
| GQ(q,q²) complement | d_q = q³−q²+q | q³+1 | →1 |
| FCC lattice (n=3) | 3 | ≤4 | ≤1.33 |
| Leech lattice (n=24) | 24 | ? | unknown but likely small |

The ratio χ(construction)/n is bounded above by a small constant in every known case.
Crucially, the GQ lower bound gives M(d_q)/d_q → 1 from above, so C ≥ 1 is necessary.

### 1b. Packing density forces sparsity
The optimal sphere packing density ρ_n satisfies ρ_n ≤ 2^{−0.599n} (Kabatiansky–Levenshtein).
This means that in any ball of radius R, the number of sphere centers is N ≤ C·(2R)^n/2^{0.599n}
 = C·(√2·R)^n · (1/√2)^n → decaying. As n grows, spheres in a region become
sparser, suggesting that the contact graph becomes sparser per unit volume, consistent
with a small chromatic number.

### 1c. Comparison with χ(R^n)
The chromatic number of Euclidean space χ(R^n) (color all of R^n with no monochromatic
distance-1 pair) is known to be exponential: 2^{0.2n} ≤ χ(R^n) ≤ 3^n.
But M(n) ≤ χ(R^n) is not tight: packing graphs are far sparser than R^n itself.
The gap M(n) vs χ(R^n) could in principle be exponential.

### 1d. The Hoffman framework gives O(n)
Every sphere packing contact graph arising from the Spectral Realization Theorem has
chromatic number bounded by the Hoffman bound H(G) = 1−k/s. For all strongly regular
sphere packing graphs in known finite geometry constructions, H(G) = O(n).
The barrier theorem (Section 6) confirms this is a ceiling, not a floor.

---

## 2. Evidence Against / Obstacles

### 2a. The Mycielski obstruction for K_3-free
For GENERAL K_{n+2}-free graphs, the fractional chromatic number χ_f can be
arbitrarily large (Mycielski graphs are K_3-free with χ_f → ∞).
So M(n) = O(n) cannot follow from K_{n+2}-free alone: the geometric constraint
(1-separated unit-distance graph) must be essential.

### 2b. No known proof even for χ_f = O(n)
Even the weaker claim χ_f(G) = O(n) for all sphere packing contact graphs G in R^n
is open. This is the correct fractional version of the conjecture.

### 2c. The gap is large
Current upper: M(n) ≤ O(τ(n)·√(log n/n)) = 2^{0.401n+o(n)}.
Conjectured upper: M(n) = O(n).
This is an improvement by a factor of 2^{0.4n}/n = 2^{Ω(n)}. Any proof must
exploit the sphere-packing geometry deeply—no purely graph-theoretic argument suffices.

---

## 3. The Six Main Approaches

---

### Approach A: Fractional Coloring via Independent Set Density

**Core claim**: For any sphere packing contact graph G in R^n,
every induced subgraph H ⊆ G satisfies α(H) ≥ |H|/(Cn)
for some absolute constant C.

This would give χ_f(G) ≤ Cn by the greedy bound on fractional chromatic number,
and (with a small gap) χ(G) ≤ Cn+1.

**Why it might be true**: An independent set I in G consists of sphere centers that
are pairwise NON-touching (pairwise distance > 1). Given any sphere packing X in R^n,
define a random sub-packing: retain each sphere independently with probability p.
The retained spheres form a sub-packing X'. The expected number of retained tangent
pairs = p²|E|. The expected size of X' is p|X|.
After "cleaning" by removing one sphere from each remaining tangent pair,
we get an independent set of expected size ≥ p|X| − p²|E| = p|X|(1 − p·τ(n)/2).
Setting p = 1/(Cn): expected IS size ≥ |X|/(Cn) · (1 − τ(n)/(2Cn)).
For C·n ≥ τ(n), this is non-negative—but τ(n) = 2^{0.4n} >> Cn, so this fails.

**The actual difficulty**: The tangency graph has max degree τ(n) = 2^{Ω(n)},
and a random coloring requires τ(n) colors to eliminate conflicts—much more than O(n).
The fractional IS bound α ≥ |X|/(Cn) requires showing that the ACTUAL graph has
much smaller effective degree from a coloring perspective.

**Key sub-question**: Is the "effective degree" (for independent set purposes) O(n)?
i.e., is there a weighting w: V → [0,∞) with Σ_{v~u} w(v) ≤ Cn for all u, and
Σ_v w(v) maximized? This is the LP dual of the fractional chromatic number.

**Status**: Open. The geometric constraint (sphere packing = 1-separated) could force
the "effective degree" to be O(n) even though the max degree is τ(n) = 2^{Ω(n)}.
Intuitively: although each sphere has up to τ(n) tangent neighbors, these neighbors
are constrained to lie on S^{n-1}(v), and from a COLORING perspective, many of them
can share colors (since they are themselves a sphere packing with chromatic number O(n)).

---

### Approach B: Recursive Dimension Reduction

**Claim**: M(n) ≤ M(n-1) + C for some absolute constant C ≥ 1.
This gives M(n) ≤ M(1) + C(n-1) = O(n).

**Mechanism**: For a sphere packing X in R^n, use a generic linear functional
ℓ: R^n → R (e.g., the last coordinate x_n). Partition X into "horizontal slabs":
S_k = {v ∈ X : ℓ(v) ∈ [k·s, (k+1)·s)} for spacing s ∈ (0,1).
Within each slab, project to R^{n-1}: π(v) = (v_1,...,v_{n-1}).

**Issue**: Two sphere centers in slab k with ||v − u|| = 1 project to ||π(v)−π(u)||
= √(1−(v_n−u_n)²) ≥ √(1−s²). For this to give a valid (n-1)-dimensional sphere
packing, we need √(1−s²) ≥ 1—impossible.

**Fix**: Use a "layered" rather than "slab" decomposition:
 - Classify sphere centers by ⌊v_n/s⌋ into layers.
 - Two tangent spheres (distance 1) in the SAME layer have |v_n−u_n| < s.
   Their projected distance is √(1−(v_n−u_n)²) > √(1−s²).
 - If s ≤ 1/2, projected distance ≥ √(3/4) = √3/2 ≈ 0.866.
 - Rescale by 1/√(1−s²): projected packing is a sphere packing in R^{n-1}
   with minimum distance 1 (rescaled) — a valid (n-1)-dimensional packing.
 - Color within each layer using M(n-1) colors.
 - Different layers may share the same M(n-1) colors IF we use ⌊v_n/s⌋ mod m
   for some m such that same-layer and same-mod spheres can't be tangent.

For m = 3 (spacing s ≤ 1/3): two spheres in the same-mod-3 layer but in layers
3 apart have |v_n−u_n| ≥ 3s ≥ 1 (if s=1/3), so they can't be tangent.
This uses C = 3 "layer color bands" each coloring with M(n-1) colors,
giving M(n) ≤ 3·M(n-1)?

**Problem**: This gives M(n) ≤ 3·M(n-1) → exponential M(n) = 3^n.

**Better approach**: The CHROMATIC NUMBER of the "inter-layer" conflict graph
(which pairs of layers can contain tangent spheres) is O(1) (since only layers
within distance 1 can contain tangent pairs, giving a "path" structure
with χ ≤ 3 for the layer conflict graph). This gives M(n) ≤ 3·M(n-1) still.

**The recursion must be additive, not multiplicative**:
M(n) ≤ M(n-1) + O(1) requires that inter-layer tangencies contribute only O(1)
NEW colors, while intra-layer uses M(n-1) colors.

For this to work: inter-layer tangent pairs (where v_n ≠ u_n in different layers)
form a graph that can be properly colored with O(1) additional colors COMPATIBLE
with the M(n-1)-coloring of intra-layer pairs. This requires the inter-layer
graph to have chromatic number O(1), which is NOT obviously true.

**Status**: Promising framework, but the recursion is M(n) ≤ 3M(n-1) (exponential)
unless a cleverer inter/intra-layer argument is found. This is a key open sub-problem.

---

### Approach C: Lovász Theta / Semidefinite Programming

**Setup**: For any sphere packing contact graph G in R^n, the Gram matrix of
the edge vectors forms an (|E| × n) matrix with PSD Gram matrix of rank ≤ n.
This constrains the eigenvectors of G's adjacency matrix in a specific way.

**The Lovász theta function** ϑ(Ḡ) satisfies χ_f(G) ≤ ϑ(Ḡ) ≤ χ(G).
If ϑ(Ḡ) = O(n), then χ_f(G) = O(n).

**Claim**: For any sphere packing contact graph G in R^n,
ϑ(Ḡ) ≤ n+1.

This would immediately give χ(G) ≥ ϑ(Ḡ) and also χ_f(G) ≤ ϑ(Ḡ) ≤ n+1,
hence M(n) ≤ n+1.

**Why ϑ(Ḡ) ≤ n+1 might be true**:
The Lovász theta function ϑ(Ḡ) is computed by an SDP:
ϑ(Ḡ) = max{Tr(JZ) : Z ≥ 0 PSD, Tr(Z) = 1, Z_{uv} = 0 for {u,v} ∈ E(G)}.
For sphere packing contact graphs, Z_{uv} = 0 whenever ||u−v|| = 1 (tangent pair).
The constraint "Z is PSD of rank ≤ n+1" from the geometric structure of R^n
might force ϑ(Ḡ) ≤ n+1 via the Delsarte LP bound for spherical codes.

**Connection to Delsarte / Cohn-Elkies LP**:
The Delsarte bound for kissing numbers in R^n uses an SDP over R^n:
it shows that the maximum number of unit vectors with pairwise angle ≥ 60° is τ(n).
The same SDP machinery might give ϑ(Ḡ) ≤ n+1 for packing contact graphs.

Specifically: For the contact graph G of a sphere packing X in R^n,
construct the "distance matrix" D_{uv} = ||u−v||. The matrix D is a negative
definite kernel on the unit sphere up to distance 1, which is exactly the
Delsarte admissible function regime.

**Key lemma to prove**: ∀ sphere packing X ⊂ R^n, ∀ PSD matrix Z with Z_{uv}=0
for tangent pairs: rank(Z) ≤ n+1.
[This is the "dimension bound" — Z lives in R^n, constraining its rank.]

If the optimal Z in the SDP has rank ≤ n+1, then ϑ(Ḡ) ≤ Tr(JZ)/Tr(Z)·Tr(Z)
= Tr(JZ) ≤ (n+1) (since J is the all-ones matrix and rank(Z)=n+1 gives n+1 nonzero eigenvalues summing to 1).

**Status**: This is the most mathematically promising approach. The dimensional
constraint (sphere packing lives in R^n) should bound the Lovász theta function.
Proving ϑ(Ḡ) ≤ n+1 exactly would give the sharp bound M(n) ≤ n+1, which is
slightly too optimistic (the GQ construction gives M(d_q) ≥ q³+1 > d_q+1).
A weaker claim ϑ(Ḡ) ≤ Cn for some constant C ≥ 2 might be achievable.

**Sub-problem**: Compute or bound ϑ(Ḡ) for the GQ(q,q²) complement graph.
Since χ_f = χ = q³+1, we have ϑ(Ḡ) ≥ q³+1. Is ϑ(Ḡ) = q³+1 exactly?
If so, this provides the correct O(n) bound AND shows the GQ construction is tight.

---

### Approach D: Sphere Packing Density → Independent Set

**Claim**: For any sphere packing X in R^n, the contact graph G = G_X satisfies:
α(G) ≥ |X| · Ω(1/n).
This gives χ(G) ≤ |X|/α(G) ≤ O(n), hence M(n) ≤ O(n).

**Why packing density helps**: The sphere packing density ρ_n → 0 exponentially
as n → ∞ (ρ_n ≤ 2^{−0.599n}). But the CONTACT density (number of tangencies
per sphere) is τ(n) = 2^{0.401n}, which grows. These together constrain how
"touching" the graph can be locally.

**A combinatorial argument**:
Fix a random ordering of X = {x_1, ..., x_N} (uniform random permutation).
Apply the greedy independent set algorithm: include x_i if x_i has no
earlier-included neighbor. The probability that x_i is included is:
Pr[x_i included] ≥ 1/(d(x_i)+1) where d(x_i) ≤ τ(n).

But we need this to give Ω(1/n) per vertex. The max degree is τ(n) = 2^{Ω(n)},
so 1/(d(x_i)+1) ≤ 1/τ(n) = 2^{-Ω(n)}. This bound is exponentially small.

**Better approach — the "local structure" argument**:
The neighborhood N(v) of any vertex v in G has ω(G[N(v)]) ≤ n (proved in paper).
More strongly: N(v) forms a spherical code on S^{n-1}(v), which itself has
chromatic number M^{sphere}(n-1) (the max chromatic number of sphere packings
on the unit sphere). If M^{sphere}(n-1) = O(n), then neighborhoods are O(n)-colorable.

**The key recursion**: Let M_s(n) = max chromatic number of sphere packing
contact graphs on S^n (the n-sphere). Then:
 - M_s(n) relates to M(n) via stereographic projection
 - M(n) ≤ 1 + M_s(n-1) (color v by "which hemisphere" + the color within the hemisphere)

This would give M(n) ≤ n+1 if M_s(n) = M(n) and M(1)=2. This recursion
is not obviously correct but suggests the right structure.

**Status**: The fractional "1/n density" claim requires a new geometric argument
exploiting the n-dimensional constraints on the contact graph. The greedy independent
set algorithm fails because τ(n) >> n. A cleverer selection procedure
(e.g., based on the dimension of the affine span of the neighborhood) might work.

---

### Approach E: Algebraic / Representation-Theoretic Bound

**Setup**: For LATTICE sphere packings, the contact graph is a Cayley graph
Cay(Λ, S) where S is the set of shortest vectors (the kissing vectors).
The chromatic number of a Cayley graph Cay(G, S) is the minimum index [G:H]
such that S ∩ H = ∅ (no generator lies in H), over all subgroups H ≤ G.

**For lattice packings**: A proper coloring corresponds to a lattice quotient map
Λ → Λ/mΛ (for some integer m) such that no two sphere centers in the same
coset are tangent. The minimum m is related to the "color number" of the lattice.

**Key result**: For the A_n lattice: χ(Cay(A_n, short vectors)) = n+1.
Proof: A_n has a quotient of order n+1 (the coset structure) that is a proper
coloring. No smaller quotient works (since the clique number is n+1).

**For non-lattice packings**: There is no group structure. But the coloring
might still be characterized by a "coset-like" partition using the n-dimensional
affine structure.

**A specific algebraic conjecture**:
For any sphere packing X in R^n, there exists a finite abelian group H of
order ≤ Cn and a map φ: X → H such that φ(u) ≠ φ(v) whenever ||u−v|| = 1.
This is exactly M(n) ≤ |H| ≤ Cn.

The existence of such H would follow if we could find a "nearly periodic" structure
in any sphere packing. By the "crystallization theorem" for dense sphere packings,
they often do have approximate periodicity — but for arbitrary (non-dense) packings,
this fails completely.

**Status**: Works perfectly for lattice packings (χ = n+1 in many cases)
but has no obvious extension to general packings.

---

### Approach F: The "Dimension-Reduction via Linear Programming" Approach

**The Delsarte / Cohn-Elkies LP bound** controls the maximum density of sphere packings
via auxiliary functions f: R^n → R with specific positivity conditions. We can adapt
this to bound χ instead of packing density.

**A "chromatic LP"**:
Define a positive semidefinite kernel K(u,v) = K(||u−v||) (a radial kernel) such that:
 1. K(0) = 1 (normalization)
 2. K(r) ≤ 0 for r = 1 (tangent pairs) [This makes same-color tangent pairs impossible]
 3. K̂(ξ) ≥ 0 for all ξ (positive definite → K is a valid kernel)

If such a kernel exists with K(0) = 1 and K(1) < 0, then for any sphere packing X:
Σ_{u,v ∈ X, same color} K(||u−v||) ≤ 0

But Σ_v K(0) = |X| (positive). So some colors have Σ_{u,v ∈ color class} K(||u−v||) ≥ |X|/χ.
This bounds χ from below (Delsarte approach to chromatic number).

To get an UPPER bound: construct a covering by sets where K restricted to each set
is positive, ensuring that O(n) such sets cover X.

**Key observation**: The function K(r) = (1 - r²/2) (the "cosine" kernel) satisfies:
 - K(0) = 1
 - K(1) = 1 - 1/2 = 1/2 > 0 (wrong sign for tangent constraint)
Try: K(r) = 1 - r (for small r):
 - K(0) = 1
 - K(1) = 0 (neutral at tangency)
 - Need K(r) < 0 for r slightly > 1 for the argument to work.

The "Delsarte polynomial" approach gives bounds on α(G)/|X|, not directly on χ(G).

**Status**: Standard LP/SDP methods give packing density bounds, not directly chromatic
number bounds. Adapting them to give χ = O(n) requires a new LP formulation specific
to the chromatic number problem.

---

## 4. Most Promising Sub-Problems

In order of mathematical tractability:

1. **Compute ϑ(Ḡ) for GQ(q,q²) complement** (Approach C):
   - Is ϑ(Ḡ_q) = q³+1 (equal to χ_f = χ)?
   - If yes: the GQ complement is a "theta-perfect" packing contact graph,
     and the SDP bound is tight.
   - Proof strategy: use the strongly-regular graph theory to compute ϑ
     via the formula ϑ(Ḡ) = v(-s)/(k-s) for SRG(v,k,λ,μ) with least eigenvalue s.
     For the GQ complement: ϑ(Ḡ_q) = v_q·q/(k_q+q) = (q+1)(q³+1)·q/(q⁴+q)
     = (q+1)(q³+1)·q/(q(q³+1)) = (q+1) = ω(G̃_q). Hmm, this gives n+1 = q+1 not q³+1.
     Need to recheck which graph is which.

2. **Prove M(n) ≤ n(n+1)/2 via the simplex coloring** (weak polynomial bound):
   - Partition the contact graph by "simplex membership": each sphere center belongs
     to one of at most n(n+1)/2 maximal simplices, and color by simplex membership.
   - This is not a proper coloring in general, but might be adaptable.

3. **Prove χ_f(G) ≤ (n+1)² for all sphere packing contact graphs** (Approach C, weaker):
   - Use the SDP formulation of ϑ with the constraint that the feasible matrices
     have rank ≤ n+1 (since they embed in R^{n+1} via the "lift").
   - A rank-r PSD matrix has Lovász theta ≤ r (roughly), giving ϑ ≤ n+1 → χ_f ≤ n+1.
   - This is the most concrete near-term direction.

4. **Find a sphere packing with M(n) ≥ 2n** (falsifying linear with small constant):
   - If M(n) ≥ 2n for some sequence of n, the conjecture M(n) ≤ Cn has C ≥ 2.
   - The GQ construction gives M(d_q)/d_q → 1, suggesting C=1 if the conjecture is true.
   - An algebraically constructed packing with χ ~ 2n would clarify the constant.

5. **Prove the additive recursion M(n) ≤ M(n-1) + O(1)** (Approach B):
   - The key is to show that "inter-dimensional" tangencies in the slab decomposition
     can be handled by O(1) extra colors.
   - Even M(n) ≤ M(n-1) + n would give M(n) = O(n²).
   - This approach is likely more tractable than the LP approaches.

---

## 5. Key Structural Observations

### Obs 1: The contact graph is "dimension-limited"
Every sphere packing contact graph G in R^n satisfies: G has an n-dimensional
"orthogonal representation" — we can assign a unit vector f(v) ∈ R^n to each
vertex such that ⟨f(v), f(u)⟩ = 1/2 for every tangent pair (edge) {u,v}.
(Proof: f(v) = (v - u)/||v - u|| for any fixed reference u, extended linearly.)

This n-dimensional representation constrains the Lovász theta function:
ϑ(G) ≤ n+1 would follow from the fact that all "column vectors" live in R^n.

### Obs 2: The independence number has an algebraic lower bound
For a K_{n+2}-free graph G with a fractional clique-cover of size ≤ C·n
(each vertex covered by ≤ Cn cliques of the cover), the fractional chromatic number
satisfies χ_f ≤ C·n. For sphere packing contact graphs, the relevant "clique cover"
is the set of all simplices (maximal cliques), and each vertex belongs to at most
S(n) simplices. If S(n) = O(n), then χ_f ≤ S(n) = O(n).

**Question**: How many maximal cliques (= regular simplices of n+1 spheres)
can a sphere packing center belong to simultaneously?
 - In R^2: a sphere center can belong to at most 6 triangular simplices (hexagonal packing)
 - In R^3: at most 12 tetrahedral simplices (FCC packing)? Likely O(n) in general.
 - If max simplex membership is O(n), then a "simplex cover" argument gives χ_f = O(n).

### Obs 3: The problem is "locally linear"
In every neighborhood N(v), the contact subgraph has ω ≤ n (one less than global),
max degree ≤ τ(n), and lives on S^{n-1}. The chromatic number of the neighborhood
graph satisfies χ(G[N(v)]) ≤ M_{S^{n-1}}(n-1) where M_S is the spherical version.
If χ(G[N(v)]) = O(n) for all v, then global χ(G) ≤ 1 + O(n) = O(n)? NOT directly,
but a "local-to-global" transfer lemma (à la Lovász's ε-nets) might give this.

---

## 6. The Strongest Form of the Conjecture

The GQ construction gives M(d_q) ≥ q³+1 ≥ d_q + d_q^{2/3}, so M(n) ≥ n+Ω(n^{2/3}).
The ratio M(d_q)/d_q = q³/(q³−q²+q) → 1 from above.

So the tightest plausible linear bound is: **M(n) ≤ n + O(n^{2/3})**, with the
GQ construction being asymptotically tight. This would mean:

**Strong Conjecture**: M(n) = n + Θ(n^{2/3}).

This is the cleanest form — it would mean the chromatic number of sphere packings
is always within o(n) of the embedding dimension. It is consistent with ALL known
constructions and all known upper bounds (since n + O(n^{2/3}) = O(n)).

The GQ construction already gives the lower bound n + Ω(n^{2/3}).
Proving the upper bound n + O(n^{2/3}) (for all n, not just along d_q) would be
a complete characterization of M(n).

**Sub-conjecture**: M(n) ≤ n + C·n^{2/3} for some absolute constant C ≥ 1.

---

## 7. Recommended First Steps

1. **Compute ϑ for GQ complements** — verify if ϑ(Ḡ) = q³+1 (which would confirm
   Approach C is the right framework).

2. **Prove the slab recursion bound M(n) ≤ M(n-1) + n** — gives M(n) = O(n²),
   a polynomial (if not linear) improvement over exponential.

3. **Check the simplex-membership count S(n)** — if S(n) = O(n), then a fractional
   coloring argument gives χ_f = O(n) directly.

4. **Literature search**: Does any paper bound χ(G) in terms of the "orthogonal rank"
   of G? The orthogonal rank of sphere packing contact graphs is exactly n, and
   there may be known results bounding χ from the orthogonal rank.

5. **Attempt to construct M(n) ≥ ωn packing** — if no such construction exists,
   this strongly supports the conjecture.
