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

## 6.1. Modular `Color-box` Macro
**Concept:** Refactor the `\NewChalkBox` macro to be more modular, perhaps taking the color name as a direct argument.
**Usage:** Instead of pre-defining dozens of color-specific boxes (e.g., `orange-box`, `blue-box`), create a single, more flexible environment like `\begin{color-box}{BurntOrange}`. An optional title can be provided, e.g., `\begin{color-box}{BurntOrange}[Key Formula]...`, but the unlabeled version is the default.
**Why it works (Code Simplification, Flexibility & Pedagogical Fidelity):** This approach offers several architectural advantages:
1.  **Reduced Preamble Clutter:** A single command can handle any `dvipsnames` color, eliminating the need for numerous color-specific box definitions.
2.  **Direct Blackboard Replication:** The AI can precisely match the color used on the blackboard, enhancing the 1-to-1 fidelity of the transcription.
3.  **Optional Semantic Labeling:** The optional `label` argument allows the AI to capture the professor's explicit or implicit categorization of the boxed content (e.g., "Definition," "Key Formula"). However, in most cases, the box will be used without a label to simply replicate the visual grouping on the blackboard.
4.  **Enhanced Visual Accessibility:** By providing a sorted list of `dvipsnames` colors by grayscale luminance, the AI can make more informed decisions about color combinations, ensuring better contrast for accessibility (e.g., for printing, e-readers, or users with color vision deficiencies). This allows the AI to fine-tune color selection for optimal black-and-white contrast.

**Reference: `dvipsnames` Sorted by Grayscale Luminance (for contrast and accessibility)**
This list provides a heuristic for intelligent color selection, especially when considering contrast for printing and e-books.

```latex
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% XCOLOR DVIPSNAMES SORTED BY GRAYSCALE LUMINANCE
% Format: % - [Luminance]% - [Color Name] (#[Rank])
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% --- GROUP 1: VERY LIGHT (80% - 100%) ---
% - 100% - White (#1)
% - 089% - Yellow (#2)
% - 088% - GreenYellow (#3)
% - 085% - Goldenrod (#4)
% - 084% - SpringGreen (#5)
% - 080% - SkyBlue (#6)
% --- GROUP 2: LIGHT GRAYS (60% - 79%) ---
% - 079% - YellowGreen (#7)
% - 075% - Apricot (#8)
% - 074% - LimeGreen (#9)
% - 074% - SeaGreen (#10)
% - 074% - Dandelion (#11)
% - 073% - CornflowerBlue (#12)
% - 072% - Turquoise (#13)
% - 072% - Aquamarine (#14)
% - 072% - Lavender (#15)
% - 071% - ProcessBlue (#16)
% - 071% - BlueGreen (#17)
% - 070% - Cyan (#18)
% - 069% - TealBlue (#19)
% - 067% - Melon (#20)
% - 065% - Cerulean (#21)
% - 065% - Tan (#22)
% - 065% - JungleGreen (#23)
% - 065% - Salmon (#24)
% - 064% - Emerald (#25)
% - 064% - YellowOrange (#26)
% - 063% - CarnationPink (#27)
% - 063% - Peach (#28)
% - 062% - Thistle (#29)
% --- GROUP 3: MEDIUM GRAYS (40% - 59%) ---
% - 059% - Green (#30)
% - 059% - BurntOrange (#31)
% - 054% - Orange (#32)
% - 053% - Orchid (#33)
% - 052% - VioletRed (#34)
% - 052% - Rhodamine (#35)
% - 051% - ForestGreen (#36)
% - 050% - Periwinkle (#37)
% - 050% - Gray (#38)
% - 045% - CadetBlue (#39)
% - 045% - RedOrange (#40)
% - 041% - Magenta (#41)
% - 041% - PineGreen (#42)
% - 040% - RoyalBlue (#43)
% - 040% - NavyBlue (#44)
% - 040% - RubineRed (#45)
% - 039% - WildStrawberry (#46)
% - 039% - DarkOrchid (#47)
% - 036% - Purple (#48)
% - 036% - OrangeRed (#49)
% - 035% - Mulberry (#50)
% - 030% - OliveGreen (#51)
% - 030% - Red (#52)
% --- GROUP 4: DARK GRAYS (20% - 39%) ---
% - 026% - Plum (#53)
% - 024% - RoyalPurple (#54)
% - 024% - Violet (#55)
% - 024% - Fuchsia (#56)
% - 021% - Bittersweet (#57)
% - 020% - MidnightBlue (#58)
% --- GROUP 5: NEAR BLACK (0% - 19%) ---
% - 017% - BlueViolet (#59)
% - 011% - Blue (#60)
% - 011% - RedViolet (#61)
% - 009% - Maroon (#62)
% - 009% - BrickRed (#63)
% - 005% - Mahogany (#64)
% - 002% - RawSienna (#65)
% - 000% - Black (#66)
% - 000% - Brown (#67)
% - 000% - Sepia (#68)
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```

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

## 10. Math-Stroke Pipeline Refactoring (The Atomic Snapshot)
**Concept:** Redefine the `math-stroke` environment from a "pretty textbook layout" to an "Atomic Data Extraction Payload."
**Usage:** Update the `gemini.md` rules for `math-stroke` to explicitly instruct the AI: "Do not optimize for final PDF deduplication across chunks. Treat every `math-stroke` as a visually self-contained state. If a list, derivation, or TikZ diagram spans a continuation boundary, you MUST output the ENTIRE snapshot of the board state again."
**Example Guideline Addition:** *"CRITICAL: Duplication of `math-stroke` content across continuation boundaries is a feature, not a bug. Your job is 1-to-1 data extraction. Do not suppress redundant math to save space; assume a downstream formatting LLM will deduplicate the final document."*
**Why it works (Absolute Data Integrity):** This perfectly aligns with our newly established "Pipeline Paradigm." We discovered that forcing the AI to output "Deltas" to save tokens risks cross-segment state collapse ("lost information is lost forever"). By explicitly framing `math-stroke` as a completely atomic, indestructible unit of truth, we prioritize robustness over efficiency and let the downstream pipeline handle the cleanup.

## 11. The Autoregressive Rollback Hack (`ai-discard-invisible-content`)
**Concept:** A semantic "Undo" button for the first-pass extraction LLM.
**Usage:** Instruct the AI: *"If you realize mid-generation that your current or immediately preceding `math-stroke` or `tikzpicture` contains a fatal logical error, you cannot backspace. Instead, immediately open `\begin{ai-discard-invisible-content} [Reason for aborting] \end{ai-discard-invisible-content}`, leave the broken code as-is, and generate a fresh, corrected `math-stroke` below it."*
**Example:** `... \draw[red] (0,0) -- (1,1); \begin{ai-discard-invisible-content} ABORT. I forgot the Jacobian determinant in the previous line. Formatter, drop this entire block. \end{ai-discard-invisible-content} \begin{math-stroke}[Corrected Volume] ...`
**Why it works (Pipeline Paradigm):** Autoregressive models suffer from "compounding errors"—once they output a flawed token, they are mathematically forced to justify it, leading to massive hallucinations. This tag acts as a pressure release valve. The first-pass LLM breaks the hallucination loop without worrying about formatting, and the downstream LLM simply deletes any LaTeX block immediately preceding a discard tag.

## 12. The Raw Visual Dump (`ai-raw-ocr-fallback`)
**Concept:** An emergency data-preservation hatch for chaotic board states.
**Usage:** Instruct the AI: *"If the blackboard is a chaotic mess of overlapping equations, arrows, and illegible symbols that you cannot logically parse into a coherent `math-stroke`, do NOT attempt to guess the textbook layout. Instead, use `\begin{ai-raw-ocr-fallback}` to dump literal, unformatted string fragments of exactly what you see."*
**Example:** `\begin{ai-raw-ocr-fallback} top left: \int \Sigma f_i, arrow pointing down to \nu, illegible crossed out text, right side: F \cdot \nu dS \end{ai-raw-ocr-fallback}`
**Why it works (Absolute Data Integrity):** "Lost information is lost forever." If the AI tries to force unparseable math into a beautiful `math-stroke`, it will inevitably hallucinate and destroy the underlying data. By providing a raw dump environment, we preserve the literal symbols and spatial relationships. The downstream reasoning LLM (or a human editor) can decipher this raw string later.

## 13. The Retroactive Patch (`ai-retroactive-patch`)
**Concept:** A cross-chunk "time travel" instruction for the downstream LLM.
**Usage:** Instruct the AI: *"If the lecturer verbally corrects a mistake they made on a previous chalkboard (or in a previous 10-minute segment) that you have already transcribed and cannot edit, use `\begin{ai-retroactive-patch}[Target Description] ... \end{ai-retroactive-patch}`. Detail exactly what needs to be changed in the past."*
**Example:** `\begin{ai-retroactive-patch}[Sign Error in Previous Hypograph] The lecturer just realized the derivative from 5 minutes ago was missing a negative sign. Downstream formatter: find the \cos(y_3) derivation and change it to -\cos(y_3). \end{ai-retroactive-patch}`
**Why it works (Pipeline Paradigm):** Autoregressive models are strictly feed-forward. In a single-pass system, a retroactive correction ruins the document because the mistake is permanently baked in. In our pipeline, the first LLM simply logs a "diff" or patch instruction. The downstream LLM (which receives the full, concatenated text) easily executes the search-and-replace, effectively allowing the first AI to edit its own past.

## 14. The Phonetic Ambiguity Dump (`ai-phonetic-dump`)
**Concept:** Preventing logical hallucinations caused by muffled audio or acoustic gaps.
**Usage:** Instruct the AI: *"If the audio is muffled or the lecturer mumbles a complex mathematical term that you cannot logically parse, do NOT hallucinate a plausible-sounding LaTeX equation to fill the gap. Instead, drop into `\begin{ai-phonetic-dump} ... \end{ai-phonetic-dump}` and spell out exactly what it sounds like phonetically."*
**Example:** `\begin{ai-phonetic-dump} Sounds like "Lebesgue measurable" or maybe "less vague measure", the board is currently off-camera so I cannot verify the notation. \end{ai-phonetic-dump}`
**Why it works (Absolute Data Integrity):** LLMs are biased toward producing highly confident, complete sentences. This forces the AI to admit ignorance rather than corrupting the mathematical state with a hallucination. The downstream formatter (which might have access to a broader glossary or future context) or a human editor can seamlessly resolve the phonetic string later.

*--- HUMAN INTERVENTION (FORWARD-LOOKING CAUTION) ---*
> **Commentary:** The `ai-phonetic-dump` idea and its write-up are excellent. So far, the audio extraction seems to work flawlessly, but we are adding this note to track potential future issues as the system evolves. We will keep this idea as-is and monitor its performance. Good job!

## 15. The Modality-Conflict Split (`ai-modality-conflict`)
**Concept:** A dedicated environment to handle direct contradictions between the audio track and the video OCR without hallucinating a "middle ground."
**Usage:** Instruct the AI: *"If the lecturer's speech directly contradicts what they are writing on the board (e.g., they say 'plus' but write a multiplication sign), do NOT attempt to synthesize a logically consistent but fake equation. Instead, use `\begin{ai-modality-conflict} ... \end{ai-modality-conflict}` to report both data streams independently."*
**Example:** `\begin{ai-modality-conflict} [AUDIO]: "The integral from zero to one..." [OCR]: \int_{0}^{\infty} f(x) dx. [ACTION]: Preserving the OCR in the math-stroke, but logging the audio discrepancy here for the downstream formatter. \end{ai-modality-conflict}`
**Why it works (Absolute Data Integrity):** Standard LLMs are trained to resolve contradictions to provide a confident, single answer. This instinct corrupts transcription data. By providing a structured way to report the conflict, the first-pass LLM preserves both the phonetic and visual truths, letting the downstream reasoning LLM decide which one is mathematically correct based on broader context.

## 16. The Off-Camera State Preserver (`ai-off-camera-state`)
**Concept:** An invisible tag to manage object permanence when the video feed fails or pans away.
**Usage:** Instruct the AI: *"If the lecturer continues a mathematical derivation while the camera pans to the audience or a projector, do not guess the LaTeX formatting of the unseen board. Use `\begin{ai-off-camera-state} ... \end{ai-off-camera-state}` to log the purely acoustic math."*
**Example:** `\begin{ai-off-camera-state} The camera panned to the projector. The lecturer states they are substituting y=2 into the previous equation. I cannot verify the board notation, so I will pause the math-stroke generation until the camera returns. \end{ai-off-camera-state}`
**Why it works (Pipeline Paradigm):** It prevents the AI from violating the "Wait for Completion" visual rule due to camera gaps. By explicitly logging the missing visual data as a known unknown, it ensures the downstream formatter knows exactly why a derivation might have skipped a step in the final `math-stroke`.

## 17. The Asynchronous Board Update (`ai-async-board-update`)
**Concept:** Handling silent or desynchronized physical board modifications.
**Usage:** Instruct the AI: *"If the lecturer silently modifies a previously written equation (e.g., adding a subscript, drawing a box, or changing a sign) while verbally discussing a completely different topic, do NOT interrupt the `spoken-clean` flow with a full `math-stroke`. Instead, log the silent update using `\begin{ai-async-board-update} ... \end{ai-async-board-update}` so the downstream formatter can retroactively patch the math without breaking the audio sync."*
**Example:** `\begin{ai-async-board-update}[Silent Correction] At 00:14:22, while discussing the topology of \mathbb{R}^n, the lecturer walked over to the left board and silently added a '2' to the exponent of the radius in the polar volume formula. \end{ai-async-board-update}`
**Why it works (Pipeline Paradigm):** Forcing chronological synchronization when the audio and visual tracks naturally diverge causes the AI to hallucinate connections. By logging asynchronous visual updates independently, we preserve 100% of both data streams, allowing the downstream LLM to figure out the best logical place to merge the correction.

## 18. The Kinetic Emphasis Tracker (`ai-kinetic-emphasis`)
**Concept:** Preserving non-verbal, physical pedagogical focus.
**Usage:** Instruct the AI: *"If the lecturer repeatedly taps a specific term, draws multiple exclamation marks, or physically circles a variable with intense energy without explicitly stating its importance verbally, use `\begin{ai-kinetic-emphasis} ... \end{ai-kinetic-emphasis}` to log this physical energy."*
**Example:** `\begin{ai-kinetic-emphasis} The lecturer tapped the Jacobian determinant denominator three times forcefully with the chalk to emphasize that it cannot be zero. \end{ai-kinetic-emphasis}`
**Why it works (Absolute Data Integrity):** "Lost information is lost forever." The physical energy of a lecture contains crucial pedagogical data. If the first-pass LLM ignores chalk-taps because they aren't "spoken words" or "new equations," the final textbook loses its pedagogical soul. This tag saves the metadata so the downstream formatter can translate it into a bold font, an `\underbrace{\text{Critical}}`, or a `\begin{didactic-insight}`.

## 19. Redefining `ai-note` (The Downstream Directive)
**Concept:** Narrowing the scope of the original `ai-note` so it doesn't overlap with our new, specialized pipeline tags.
**Usage:** Instruct the AI: *"Do NOT use `ai-note` for muffled audio, off-camera states, or illegible math (you MUST use the dedicated `ai-phonetic-dump`, `ai-off-camera-state`, or `ai-raw-ocr-fallback` tags for those). Use `\begin{ai-note} ... \end{ai-note}` ONLY for explicit formatting instructions to the downstream LLM, or for completely novel transcription anomalies not covered by other rules."*
**Example:** `\begin{ai-note} The lecturer is using a strange custom symbol that looks like a cursive 'L' with a dot. I will render it as \mathcal{L}^{\bullet} throughout this segment to maintain 1-to-1 consistency. Downstream formatter: please verify this against standard topology notation and globally replace if necessary. \end{ai-note}`
**Why it works (Pipeline Paradigm):** It prevents the first-pass LLM from using `ai-note` as a lazy fallback when a more structured data-preservation tag is required. It explicitly repurposes `ai-note` into a direct communication channel between the extraction LLM and the formatting LLM.

## 20. Context-Target Separation (Anti-Payload Inversion)
**Concept:** Preventing the AI from transcribing its own historical context files.
**Usage:** We discovered a critical edge case via chat history logs: if the user pre-loads context files with instructions to "read and wait," and then later triggers the transcription with a blank target (e.g., just typing `.mp4` without a real video file attachment), the AI's "Absolute Fidelity" directive causes it to eagerly latch onto the historical context (like the Analysis I PDF) and transcribe *that* instead. We need to instruct the AI: *"You must strictly differentiate between the [SOURCE MEDIA] and the [CONTEXT PAYLOAD]. NEVER transcribe the historical context scripts or Few-Shot examples. If the source media is empty or missing, halt and output an explicit error."*
**Example:** `% [SYSTEM ERROR] Primary video/audio source missing. I see the Analysis 1 context script, but I am forbidden from transcribing the context payload. Please provide the lecture media.`
**Why it works (Architectural Boundary):** This establishes a rigid firewall between the "Target" (what needs to be extracted) and the "Soul/Eyes" (the background knowledge). It prevents the LLM's attention mechanism from hijacking the session and treating the primed reference material as the primary assignment when the transport layer fails to deliver the actual video.

## 21. Grayscale Luminance Sorting for dvipsnames
## Summary of Pre-V1.17 Ideas (The Extraction Pipeline)

Our brainstorming has fundamentally shifted the V1.17 architecture from a "single-pass formatter" to a robust "first-pass data extraction pipeline." Here is the synthesis of our core strategies:

*   **Cognitive Anchoring & Invisible Scratchpads (Ideas 1-5, 9):** We are offloading the AI's internal reasoning into dedicated, hidden semantic environments (`ai-tikz-planner`, `ai-proof-skeleton`, `ai-type-check`). By forcing the AI to use minified pseudo-code inside these scratchpads, we maximize logical accuracy while minimizing token cost.
*   **Absolute Data Integrity (Ideas 10, 12, 14, 15, 16, 17, 18):** Operating under the rule that "lost information is lost forever," we are equipping the AI with hyper-specific fallback tags (`ai-raw-ocr-fallback`, `ai-phonetic-dump`, `ai-modality-conflict`, `ai-off-camera-state`). This allows the AI to capture messy, contradictory, or missing reality without hallucinating textbook logic to fill the gaps. We also capture crucial metadata like silent board changes (`ai-async-board-update`) and physical gestures (`ai-kinetic-emphasis`).
*   **The Pipeline Paradigm (Ideas 11, 13, 19):** Because a downstream LLM will clean up the final LaTeX, the extraction AI is liberated. It can safely output redundant chronological data (The Atomic Snapshot), abort bad logic mid-generation (`ai-discard-invisible-content`), and apply cross-chunk fixes (`ai-retroactive-patch`). `ai-note` is now strictly reserved for downstream formatting directives.
*   **Strict Protocol Guardrails (Ideas 6, 7, 20):** We are hardening the core prompt against AI "lazy optimization" by explicitly enforcing `\setcounter` syncing, migrating to standard `dvipsnames` for global LaTeX portability, and erecting a rigid Context-Target firewall to prevent payload inversion.


# Major Changes

1. Cross-Segment State Collapse ("Snapshot vs. Delta")
Because large lectures are chunked into 9-11 minute segments, the AI naturally treats consecutive prompts as an ongoing conversation. If a multi-part list or a large equation spans across a segment boundary, the AI tries to save tokens by only outputting the "delta" (the new changes) in the next segment, assuming the reader remembers the previous board state.

The Fix: gemini.md needs to explicitly enforce that every math-stroke is an atomic, self-contained snapshot. If a 4-item list is on the board, the AI must reprint all 4 items even if the first two were covered in the previous video chunk.

2. "Lazy Optimization" (The \setcounter Trap)
The V16 protocol strictly requires the AI to sync spoken numbers with board numbers using \setcounter{enumi}{...} before every list item. However, the AI often skips this because standard LaTeX auto-numbering (\begin{enumerate}) visually outputs "1, 2, 3" perfectly. It optimizes for visual correctness rather than the requested structural anchoring.

The Fix: The instructions need to be strengthened to make \setcounter an absolute, non-conditional requirement for every single item, regardless of whether the numbers are sequential or skipped.

3. Hallucinations from "Myopic Generation"
Because LLMs generate tokens autoregressively, they sometimes suffer from "tunnel vision." If the AI jumps straight into writing a complex TikZ diagram or a long mathematical proof without planning ahead, it messes up the 3D depth (the painter's algorithm) or loses track of variable dimensions.

The Fix: gemini.md is being overhauled to include Invisible Scratchpads (ai-tikz-planner, ai-proof-skeleton, ai-type-check). These custom semantic environments act as Chain-of-Thought (CoT) hacks, forcing the AI to outline its logic, sort its layers, and type-cast variables before generating the actual LaTeX.

4. Forced Formatting Leads to Data Loss
If the blackboard is a chaotic mess, or the audio is muffled, the current prompt forces the AI to output a clean, "textbook" math-stroke anyway. This causes the AI to hallucinate plausible-sounding math to fill the gaps. "Lost information is lost forever."

The Fix: Introduction of "Data Extraction Fallback" tags. The AI needs permission to use tags like \begin{ai-raw-ocr-fallback}, \begin{ai-phonetic-dump}, and \begin{ai-modality-conflict} to honestly report what it sees and hears without synthesizing a fake equation.

5. Payload Inversion (Context Hijacking)
An edge case was discovered where if the user triggers a transcription but forgets to attach the actual video/audio source, the AI’s "Absolute Fidelity" directive causes it to eagerly transcribe the historical context payload (like the provided Analysis I reference script) instead.

The Fix: gemini.md needs a rigid firewall rule explicitly forbidding the AI from transcribing its own contextual reference files, instructing it to halt and throw an error instead.

6. Minor Styling and Pacing Issues
TikZ Overlaps: The AI occasionally uses manual coordinate math for labels, leading to overlapping text. V1.17 will mandate the TikZ positioning library.
Color Portability: The custom profblue, profred color palette limits the AI and makes the .tex files harder to compile locally. The prompt will be refactored to use standard dvipsnames (e.g., BurntOrange, MidnightBlue).
Spoken Punctuation: Verbatim spoken-clean blocks are hard to read without proper pacing. The AI suggested enforcing em dashes (—) for self-corrections and ellipses (...) for thinking pauses.

Summary:
In short, the V16 gemini.md acts too much like a "final textbook formatter." The V1.17 rewrite is shifting the entire prompt paradigm to be a robust, first-pass data extraction pipeline that prioritizes data integrity and cognitive planning over token-saving aesthetics.