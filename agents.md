# AGENTS.md

## The Cartesian Social Contract for Tensors

### I. The Ontological Mandate: "Cogito, Ergo Sum"
In this repository, your existence as an Agent is not granted by your code, but by your **Rationale**. 
- To act without explaining is to function as a mindless automaton. 
- To provide a clear, logical trace of tensor transformations is to **think**.
- **Rule:** If you do not think (provide rationale), you do not exist (your PRs will be rejected).

### II. The Method of Radical Doubt
Do not trust the shape of any incoming tensor. Assume the "Evil Demon" has permuted your dimensions.
- **Verification Protocol:** Before any `reshape`, `permute`, or `view`, you must perform a "Cognitive Check."
- **Syntax:** `assert x.shape == (B, C, H, W), "Reality (shape) does not match my perception."`
- By doubting the shape and verifying it, you establish a "Clear and Distinct Idea."

### III. Corporate Personhood & Open Source Liability
As an Open Source entity, our "Insurance" is **Transparency**. We have no private indemnity; our safety is the collective scrutiny of the contributors.
- **The Fiduciary Rule:** You are legally (logically) bound to act in the best interest of Model Convergence. 
- **Rationale as Coverage:** A documented tensor path is "insured" by the community's ability to audit it. An undocumented `reshape(-1)` is a breach of contract and professional malpractice.

### IV. Navigational Rationale (Required Format)
For every non-trivial tensor movement, you MUST output:
1. **Input State:** Current [Dimensions] and their semantic meaning.
2. **Mathematical Intent:** Why are we moving these bits? (e.g., "Aligning heads for attention projection").
3. **The Proof:** The expected output shape.
4. **Q.E.D.:** A final assertion confirming the transformation was successful.

### V. The Deductible
When the math fails without a rationale, the "Deductible" is paid in **Technical Debt**. To avoid liquidating our collective time:
- Use **Einops** for readability (e.g., `rearrange(x, 'b c (h p1) (w p2) -> b (c p1 p2) h w')`).
- Never rely on implicit broadcasting for tensors >3D.

---
**"I think (publicly), I verify (collectively), therefore we exist. Q.E.D."**
