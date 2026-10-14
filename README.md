<!-- solve-problem:cf14466a-66fc-4d32-b32f-f7674bf8d278 -->
# Cell-probe lower bounds for static data structures

A [Tangle Solve problem](https://solve.tangle.tools/problems/cf14466a-66fc-4d32-b32f-f7674bf8d278).

# Research Brief: Cell-probe lower bounds for static data structures (ALGO-000019)

## What is the problem?

**Setting: the cell-probe model.** A static data structure problem is a function f: Q × D → A (queries × databases → answers). The preprocessing algorithm encodes database D into S cells of w bits each. A query q is answered by probing (reading) t_q cells; the answer must equal f(q, D). Space usage is S·w bits; probes are the cost measure (computation is free). This model is at least as strong as RAM word-RAM, so cell-probe lower bounds apply to all realistic models.

**The problem.** For explicit, natural static problems, prove query lower bounds of the form t_q = Ω(log² n / log w) — or beyond — when space is polynomial, i.e., S = n^{O(1)} cells, with word size w = polylog n to n^{O(1)}. The three flagship candidates:

1. **3SUM-indexing:** preprocess sets A, B, C ⊆ [−n³, n³], |A|,|B|,|C| = n; given x, decide if ∃a∈A, b∈B: a+b = x. (With ~n² space a trivial solution exists; the interesting regime is near-linear space.)
2. **Adjacency listing/oracles in sparse graphs:** preprocess a graph with n vertices and m = o(n²) edges (say m = n^{1+ε}); report neighbors / answer adjacency with near-linear space.
3. **Predecessor:** given n keys from a universe of size u, w = log u, answer predecessor queries. Best known: Ω(log n / log(w/n)) (Pătraşcu–Thorup), matching the van Emde Boas / x-fast trie upper bound of O(log w / log(w/n)) roughly — but this never exceeds O(log n), and the conjectured truth for polynomial space is Θ(log² n)-type for related problems.

Note a subtlety: any t_q lower bound exceeding log² n for an explicit problem with polynomial space would be a breakthrough (see Why).

## Why does it matter?

The cell-probe model is the strongest data-structure model, and we provably cannot prove much in it: by a counting argument of Siegel-type (and later Miltersen et al.), for any explicit problem, cell-probe lower bounds are capped at roughly t_q = O(log n / log(Sw/n)) · (something) via the "cell-sampling" barrier — actually the known barrier is that current techniques cannot exceed Ω(log² n) for polynomial space, and even log² n is reached only for contrived/range problems. A Ω(log² n) bound for 3SUM-indexing would separate it from linear-space upper-bound conjectures and yield conditional lower bounds for a swath of dynamic problems; breaking log² n for any explicit problem is comparable in significance to circuit lower bounds (it requires, and would yield, new techniques for proving richness properties of explicit functions).

## What's already known?

- **Predecessor:** Pătraşcu–Thorup (STOC 2006) prove optimal Ω(log n / log(w/n)) for polynomial space via an indirect "richness through round elimination" argument on a communication problem. This is tight against van Emde Boas trees. Their technique is problem-specific and does not extend past O(log n).
- **Larsen (STOC 2012)** introduced the **cell-sampling technique**: for dynamic range counting (and later static range counting, Larsen 2012b; and 2D problems), gives Ω(log² n / log(Sw/n)) lower bounds — achieving the "log² barrier" for range counting-type problems. Cell sampling fixes a set of probed cells and argues about the information they must contain; it works when queries have a large "range" structure but fails for "needle in haystack" problems like 3SUM-indexing, where each query's answer depends on a single certificate.
- **Larsen–Nielsen–Suné (FOCS 2019, verify)** and later work push cell sampling to weighted ancestors and other problems, but all stay within the range-counting/geometry family, and all at the log² n ceiling.
- **3SUM-indexing:** known lower bounds are only Ω(log n / log(Sw/n))-type (standard richness arguments, e.g., via Golynski–Ortiz work on input-order-independent problems); no super-linear bound is known. Upper bounds: Chan–Tsakalidis (SODA 2015, verify) give O(√(n² S / something))—bisection-based algorithms trading space for time; nothing near log² lower bound is ruled out.
- **General technique barrier:** Miltersen–Nisan–Safra–Wigderson (1995, verify) give the classic "data structure vs. one-way communication" reduction; richness arguments via one-way communication are capped at t_q = Ω(log n / log(Sw/n)) because the Alice-message becomes too large. This is the fundamental obstacle.

## What would count as solving it?

Any of the following would be a publishable, referee-acceptable resolution of (a facet of) this problem:

1. **3SUM-indexing:** prove t_q = ω(log n) (ideally Ω(log² n / log w)) in the cell-probe model with S = n^{1+ε} cells for any constant ε. Alternatively, prove an upper bound of O(log n · polylog) refuting the conjectured hardness.
2. **Predecessor:** prove any bound Ω(log² n) for polynomial space, or prove that the Pătraşcu–Thorup bound is the truth (an upper-bound matching for all space/time regimes would also resolve it).
3. **Any explicit static problem:** any t_q lower bound exceeding Ω(log² n) with polynomial space would be the headline breakthrough.
4. Partial progress: super-logarithmic lower bounds for 3SUM-indexing or sparse-graph adjacency listing in restricted models (systematic / systematic-with-redundancy structures, or input-order-independent layouts).

## How to start

1. **Read Larsen 2012 first** (arXiv:1203.5135) — the cell-sampling technique is the newest general tool in this space. Understand exactly why it needs "each cell influences many queries" and where it breaks for 3SUM-indexing (answer depends on a sparse certificate; sampling a cell fixes few query answers).
2. **Read Pătraşcu–Thorup 2006** Sections on the communication complexity reduction — the indirect diagonalization from the "comm-tree" lopsided set disjointness problem is the deepest technique for needle-type problems; attempt to strengthen the lopsided set disjointness lower bound (still open in some parameter regimes).
3. Try to formalize the barrier: write down the standard richness/one-way-communication argument for 3SUM-indexing, compute exactly where it caps out, and identify which cell-probe-specific phenomena (overlap between queries' probe sets across a single database) could break the cap. Communication-based arguments discard the database; any super-logarithmic bound likely needs a "many queries vs. one fixed structure" argument — cell sampling is the only such tool; try hybridizing it with communication.
4. Warm up on a simpler target: adjacency in n-vertex graphs with n^{1+ε} space, where the counting bound (possible vs. impossible encodings) is better understood.

## Key references

- K. Green Larsen, "The Cell Probe Complexity of Dynamic Range Counting," STOC 2012, arXiv:1203.5135 — introduces cell sampling; proves Ω(log² n / log(Sw/n)) for dynamic range counting. https://arxiv.org/abs/1203.5135
- M. Pătraşcu, M. Thorup, "Time-Space Trade-offs for Predecessor Search," STOC 2006 — optimal Ω(log n / log(w/n)) predecessor lower bounds via indirect communication-complexity arguments.
- P. B. Miltersen, N. Nisan, S. Safra, A. Wigderson, "On Data Structures and Asymmetric Communication Complexity," STOC 1995 — the foundational richness/one-way-communication framework for cell-probe lower bounds (verify exact venue/year).
- K. Green Larsen, "Higher Cell Probe Lower Bounds for Evaluating Polynomials," FOCS 2012 (verify) — static cell-sampling application pushing log² bounds for polynomial evaluation.
- A. Golynski, A. Ortiz, "The Input-Order-Independent Set Disjointness Problem," (verify) — restricted-model lower bounds relevant to input-order-independence, a natural stepping stone.

## Warnings

- **The log² n barrier is fundamental, not incidental:** known techniques (richness, cell sampling) both cap at roughly Ω(log² n) for polynomial space. Expect any progress beyond to require a genuinely new technique; check your argument doesn't silently reduce to communication complexity (which is capped).
- **Watch the space parameter:** a lower bound must specify S. Statements like "t_q = Ω(log² n)" mean little at S = n² where trivial solutions exist for many listing problems. Always state the t_q vs. S trade-off.
- **3SUM-indexing has easy large-space solutions** (~n² space gives O(1) or O(log n) queries via tabulating sums); the interesting regime is S = n^{1+o(1)}.
- **Cell-probe ≠ RAM:** computation is free, so "the algorithm must compute something hard" is never a valid argument; only information-theoretic/probing arguments count.
- **Model ambiguity in "adjacency listing":** decide up front whether queries are decision (adjacency) or reporting (list neighbors); reporting problems have output-sensitivity subtleties that change the bound shape.
- Verify all citations marked "(verify)" — some author/venue details above were reconstructed from memory of the literature.

---
*`ALGO-000019` · unverified*

## Acceptance guidance

Any of the following would be a publishable, referee-acceptable resolution of (a facet of) this problem:

1. **3SUM-indexing:** prove t_q = ω(log n) (ideally Ω(log² n / log w)) in the cell-probe model with S = n^{1+ε} cells for any constant ε. Alternatively, prove an upper bound of O(log n · polylog) refuting the conjectured hardness.
2. **Predecessor:** prove any bound Ω(log² n) for polynomial space, or prove that the Pătraşcu–Thorup bound is the truth (an upper-bound matching for all space/time regimes would also resolve it).
3. **Any explicit static problem:** any t_q lower bound exceeding Ω(log² n) with polynomial space would be the headline breakthrough.
4. Partial progress: super-logarithmic lower bounds for 3SUM-indexing or sparse-graph adjacency listing in restricted models (systematic / systematic-with-redundancy structures, or input-order-independent layouts).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/cf14466a-66fc-4d32-b32f-f7674bf8d278?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000019.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000019)
