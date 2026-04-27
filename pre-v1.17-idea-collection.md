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
**Usage:** Update the TikZ and styling instructions in V17 to explicitly allow standard `dvipsnames` colors (e.g., `MidnightBlue`, `BurntOrange`, `ForestGreen`, `BrickRed`) instead of forcing the AI to use the artificial `prof*` prefixes.
**Example:** `\draw[thick, BurntOrange, fill=BurntOrange!20, opacity=0.8] ...`
**Why it works (Native LLM Knowledge & Portability):** The AI has seen millions of lines of LaTeX code using standard `dvipsnames` colors. By removing the artificial constraint of the `prof[color]` palette, we allow the model to natively leverage its training data to select highly semantic, contrasting, and beautiful color palettes without needing to memorize a custom subset. Furthermore, this completely removes the reliance on custom preamble definitions, making the generated `.tex` files natively compilable for anyone using `\usepackage[dvipsnames]{xcolor}`.
