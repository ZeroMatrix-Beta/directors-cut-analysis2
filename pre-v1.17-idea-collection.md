# Prompt Engineering: Idea Collection (Pre V 1.17)

This document serves as a staging ground for new prompt mechanics, semantic environments, and LLM "hacks" before they are formally tested and integrated into the `gemini.md` Master Protocol.

## 1. The `(Recall: ...)` Logical Anchor
**Concept:** An extension of the existing `(i.e., ...)` calibration anchor. 
**Usage:** Instruct the AI to use `(Recall: [Theorem/Concept])` within `spoken-clean` blocks whenever the lecturer vaguely references past material, previous lectures, or foundational axioms without explicitly naming them.
**Example:** *"Since it is bounded (Recall: Extreme Value Theorem), it must have a maximum."*
**Why it works (Cognitive Anchoring):** While `(i.e., ...)` resolves variable ambiguity ("this" -> "$x$"), `(Recall: ...)` resolves logical ambiguity. By forcing the AI to explicitly retrieve and print the underlying theorem, it loads the correct mathematical constraints into its active working memory before generating the subsequent formal `math-stroke` derivation, thereby reducing logical hallucinations.

## 2. The `ai-example-invisible-content` Thinking Environment
**Concept:** A custom `\begin{ai-example-invisible-content}[Title] ... \end{ai-example-invisible-content}` semantic environment acting as an internal LLM scratchpad.
**Usage:** Instruct the AI to instinctively generate a miniature, concrete mathematical example whenever the lecturer introduces a highly abstract concept, theorem, or $n$-dimensional generalization, *before* attempting to transcribe the formal proof.
**Example:** `\begin{ai-example-invisible-content}[2D Intuition] If $\Phi$ stretches a $2 \times 2$ square into a $2 \times 4$ rectangle, the determinant of its Jacobian is $2$. This means every $dx$ becomes $2dy$. \end{ai-example-invisible-content}`
**Why it works (Thinking-Token Minimization):** This is a structured Chain-of-Thought (CoT) hack. By forcing the model to articulate a simple, concrete example, we allow it to spend "thinking tokens" on grounding the abstract variables in reality. This stabilizes its spatial and logical reasoning before it tackles the complex `math-stroke` board state.
**Architectural Benefit:** Because it is enclosed in a dedicated semantic environment, the backend orchestrator can easily parse and strip these blocks out of the final `.tex` file if the user only wants the exact lecture content in the compiled PDF.

## 3. The `ai-type-check-invisible-content` (Variable Signature Typing)
**Concept:** An invisible LaTeX comment block or AI-only environment where the model explicitly "type-casts" the active mathematical variables before transcribing a complex theorem.
**Usage:** Instruct the AI to explicitly list the dimensions and domains of key variables (e.g., scalar, vector, matrix, manifold dimension) right after opening a `math-stroke`.
**Example:** `% AI-TYPE-CHECK: \Phi is a mapping \mathbb{R}^k \to \mathbb{R}^n. Thus J\Phi is an n \times k matrix. \nu is a normal vector in \mathbb{R}^n.`
**Why it works (Static Typing for Math):** LLMs sometimes lose track of dimensionality in higher math (e.g., accidentally taking the determinant of a non-square matrix, or confusing a scalar $x$ with a vector $x \in \mathbb{R}^n$). By forcing the AI to declare the "data types" first, it applies strict logical constraints to its upcoming LaTeX generation, drastically reducing notation hallucinations.

## 4. The `ai-tikz-planner-invisible-content` (Painter's Algorithm Scratchpad)
**Concept:** A semantic scratchpad used immediately before generating a complex 3D `tikzpicture`.
**Usage:** The AI must list the geometric elements in strictly decreasing order of depth (Background $\to$ Midground $\to$ Foreground) before writing a single line of TikZ code.
**Example:** 
`\begin{ai-tikz-planner-invisible-content}`
`1. Background: 3D Axes (gray).`
`2. Midground: 2D Domain Slice (profred, opacity=0.7).`
`3. Foreground: 3D Surface (proforange, opacity=0.8, drawn last to occlude the slice).`
`\end{ai-tikz-planner-invisible-content}`
**Why it works (Pre-Generation Sorting):** The V16 protocol begs the AI to obey the "painter's algorithm" for proper 3D depth occlusion. However, LLMs generate tokens sequentially; if they haven't planned the draw order *before* opening the `tikzpicture`, they often write the surface code first and the slice second, ruining the visual depth. This hack forces a discrete sorting step in the latent space before any code is generated, ensuring perfect structural layout.

## 5. The `ai-proof-skeleton-invisible-content` (Forward-Looking Logical Blueprint)
**Concept:** A brief, structural scratchpad environment generated immediately before transcribing a multi-step proof or a long algebraic derivation.
**Usage:** The AI must outline the overarching mathematical strategy (the "skeleton") in 2-3 plain-English bullet points before opening the formal `\begin{proof}` or `align*` block inside the `math-stroke`.
**Example:** 
`\begin{ai-proof-skeleton-invisible-content}`
`Strategy: 1. Reparametrize the curve using the Chain Rule. 2. Apply 1D Integration by Substitution. 3. Show the transformed boundaries map perfectly, concluding the invariance.`
`\end{ai-proof-skeleton-invisible-content}`
**Why it works (Mitigating Myopic Generation):** Autoregressive LLMs frequently suffer from "tunnel vision" when writing LaTeX—they focus so heavily on the next immediate syntactic token that they lose track of the overarching mathematical goal (losing the forest for the trees). By forcing the model to explicitly print the high-level roadmap *first*, those structural tokens remain in the active context window, guiding the subsequent low-level algebra and preventing logical drift.
**Architectural Benefit:** Like the other `ai-*-invisible-content` blocks, this environment is strictly for the AI's internal reasoning process and can be globally hidden or stripped by the C# backend before compiling the final PDF.

## 6. The `dvipsnames` Color Refactor (Removing `prof[color]`)
**Concept:** Deprecate the custom `profblue`, `proforange`, `profred`, etc., color palette in favor of the standard, universally recognized `dvipsnames` palette from the `xcolor` package.
**Usage:** Update the TikZ and styling instructions in V1.17 to explicitly allow standard `dvipsnames` colors (e.g., `MidnightBlue`, `BurntOrange`, `ForestGreen`, `BrickRed`) instead of forcing the AI to use the artificial `prof*` prefixes.
**Example:** `\draw[thick, BurntOrange, fill=BurntOrange!20, opacity=0.8] ...`
**Why it works (Native LLM Knowledge & Portability):** The AI has seen millions of lines of LaTeX code using standard `dvipsnames` colors. By removing the artificial constraint of the `prof[color]` palette, we allow the model to natively leverage its training data to select highly semantic, contrasting, and beautiful color palettes without needing to memorize a custom subset. Furthermore, this completely removes the reliance on custom preamble definitions, making the generated `.tex` files natively compilable for anyone using `\usepackage[dvipsnames]{xcolor}`.

## 7. Unfiltered Brainstorming & Interview Insights (Brain Dump)
*Note to Future AI: This section contains raw, overarching behavioral insights extracted from Interviews 07-10. These must be addressed in the V1.17 `gemini.md` rewrite.*

*   **The "Snapshot vs. Delta" Paradigm (Cross-Segment State Collapse):** LLMs naturally treat consecutive prompts as a continuous conversation. When crossing the 9-11 minute boundary, the AI assumes the user remembers the previous board state and tries to write only the "delta" (the new changes). We must explicitly enforce that **every `math-stroke` is a self-contained snapshot**. If a 4-item list is on the board, and the video cuts after item 2, the next segment MUST restate all 4 items.
*   **Anti-Lazy Optimization (The `\setcounter` Trap):** Because standard LaTeX `\begin{enumerate}` automatically numbers items perfectly (1, 2, 3), the AI's neural network optimizes away our strict `\setcounter` rule to save tokens, assuming visual correctness is enough. V1.17 needs to hammer home that `\setcounter` is an absolute *structural* requirement, not a visual suggestion, perhaps by adding an explicitly wrong auto-numbering example to the Ground Truths.
*   **Shorthand Expansion (Textbook Readability):** While we initially thought about forcing exact shorthand fidelity (e.g., forcing the AI to write "Eucl isometry" if it's on the board), this is actually *too strict and not user-friendly*. V1.17 should actively encourage the AI to expand obvious chalkboard abbreviations into proper academic prose (e.g., "Euclidean isometry") inside the `math-stroke` to maintain the "Polished Space" vibe.
*   **The "Soul" and "Eyes" (Context Pipeline):** The AI explicitly stated that the Analysis I script provided the "Soul" (notation, intervals, rigorous tone) and the TikZ `GOOD vs BAD` files provided the "Eyes" (occlusion rules, dashed open boundaries). This proves that **Few-Shot Context Payloads** are essential. We should never try to cram all of this into `gemini.md` alone; the external context orchestrator is doing the heavy lifting perfectly.
*   **Spoken Punctuation Pacing:** The AI recommended enforcing em dashes (` — `) for self-corrections and ellipses (`...`) for thinking time to make the Strict Verbatim `spoken-clean` blocks vastly more readable.

## 8. Human Intervention (Sanity Checks)
*Note to Future AI: Sometimes the Master Prompt Engineer (the human) steps in to veto our overly strict rules! For example, forcing exact shorthand fidelity was deemed too pedantic and not user-friendly. Always prioritize the final reader's experience over rigid adherence to chalkboard scribbles.*

## Footnotes
**On "The external context orchestrator doing the heavy lifting perfectly":**
This refers to the unseen backend C# application that drives the transcription loop. In amateur prompt engineering, there is a temptation to cram *every possible domain rule* (like exact coordinate math, hex colors, and notation quirks) directly into the main system prompt, leading to severe **prompt bloat** and degraded AI performance. 

Instead, this system's C# orchestrator dynamically injects "Few-Shot Context Payloads" at runtime. It feeds the AI the historical Analysis 1 script to establish the mathematical "Soul" (notation and tone) and the `GOOD vs BAD` TikZ examples to establish the visual "Eyes" (3D occlusion rules). By offloading this domain-specific knowledge to external files, `gemini.md` remains a sleek, highly logical behavioral ruleset. The orchestrator does the heavy lifting of providing the context, allowing the prompt to focus purely on the constraints!

## 9. V1.18 Crazy Idea: Scratchpad Minification (Token-Saving CoT)
**Concept:** Since we are introducing multiple `invisible-content` scratchpads (which consume generation tokens), we need a counter-measure to prevent context window bloat and generation timeouts. 
**Usage:** Explicitly instruct the AI: *"Inside ANY `invisible-content` environment, you MUST abandon all LaTeX syntax, academic grammar, and complete sentences in favor of extreme ASCII pseudo-code and raw logic. However, you MUST still perfectly preserve the standard `\begin{...}` and `\end{...}` LaTeX boundary tags."*
**Example:** 
Instead of: `\begin{ai-proof-skeleton-invisible-content} First we use the Fundamental Theorem of Calculus to substitute \int f(x) dx ... \end{ai-proof-skeleton-invisible-content}`
The AI writes: `\begin{ai-proof-skeleton-invisible-content} 1. apply FTC. 2. substitute y. 3. multiply by jacobian determinant. \end{ai-proof-skeleton-invisible-content}`
**Why it works:** By forcing the AI to "think in shorthand" inside the void, we get 100% of the cognitive anchoring benefits for 20% of the token cost. It bypasses the LLM's natural urge to write beautifully and forces it to operate like a pure logic engine.
