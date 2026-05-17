## The Hard Specifications

### 1. Audio Extraction & Linguistic Tone 
- **Raw Audio Primary Extraction (with Broken Subtitle Fallback):** Use the audio track for the transcript. If the provided input happens to be auto-generated, broken subtitles lacking proper punctuation, you must actively perform error correction. Identify mathematical jargon phonetically; use the board content to resolve ambiguous symbols, subscripts, and operator names when the audio or speech-to-text is unclear.
- **Exclusion of Non-Content Audio:** Do not transcribe non-verbal sounds such as coughs, sneezes, laughter (unless it's a direct, meaningful reaction to content), long silences, or background noise. Focus exclusively on spoken words and board content relevant to the lecture.
- **Analogy & Jargon Preservation:** You MUST preserve all physical metaphors, analogies, and intentional pedagogical jargon (e.g., the \qt{potato}, the \qt{final boss}, \qt{pixels}, the \qt{extra circus}). Map profound metaphors specifically to `didactic-insight` environments. Use the custom `\qt{...}` macro (which stands for `\textit{``...''}`) for these colloquialisms to clearly indicate they are intentional and format them safely.
- **Chronological Flow:** **Generally preserve the chronological order of speech and board actions.** Minor local reordering is permitted only to align a spoken sentence with the immediately corresponding board action. Do not group or reorganize content across larger sections.
- **Timestamp Fidelity (No Calibration):** The `[HH:MM:SS - HH:MM:SS]` timestamps for `spoken-clean` environments must correspond directly to the timecodes in the source video file. **You MUST make absolutely sure the HH:MM:SS really matches the actual timing in the video.** Do not apply any offsets, speed multipliers, guess timings, or use other mathematical calibrations.
- **Mid-Sentence Interruptions & Anti-Hallucination (The Flash Guardrail):** Autoregressive models are naturally tempted to complete a mathematical thought. If the lecturer stops talking mid-sentence due to a student interruption, a sudden video cut, or a distraction, you MUST stop transcribing the sentence exactly where the audio stops. Mark the interruption (e.g., `Now I explain the proof with... \inlinemetanote{interrupted by student}`). **CRITICAL:** You are strictly forbidden from letting your latent mathematical training data "auto-complete" the proof or hallucinate subsequent examples. You may guess at most ONE single word to close an immediate phonetic sound, but absolutely no more. If the audio stops, your text MUST stop.

### 2. Mathematical Translation & Notation Fidelity
- **The `(i.e., ...)` Calibration Anchor (Thinking Token Optimization):** You must **frequently and proactively** inject explicit inline LaTeX annotations directly into the `spoken-clean` text. Whenever the lecturer uses vague pronouns ("this goes here"), translate it immediately (e.g., "this (i.e., $x_3$) goes here (i.e., into Equation \ref{eq:sphere})"). **Crucially, if the lecturer makes a verbal mistake that contradicts the correct board math (e.g., says "sine" but writes "cosine"), you MUST transcribe the spoken mistake verbatim, but instantly correct it inline:** (e.g., "So, the sine (i.e., actually $\cos(y_3)$ as written on the board) is..."). **By explicitly printing these implied variables and corrections, you offload working memory onto the visible page and prevent logical hallucinations.**
- **Visual Math Syncing:** Cross-reference the audio with the physical chalk strokes. If a variable is spoken while being written, that variable must be perfectly formatted in LaTeX in the corresponding `math-stroke`.
- **Blackboard Connections & Equation Referencing:** If the professor uses colors, arrows, markers like `(*)`, `(x)`, or draws boxes around equations on the blackboard to connect them and show derivations or proof-logic, **all of these logical steps must be explicitly written out**. Pay close attention if he uses `(*)`, `(x)`, or any other way to reference equations. Translate these visual or informal references into formal textbook cross-references using `\label{...}` and `\eqref{...}` (or `\ref{...}`) inside the `math-stroke` environment. You can use colorful equation tags as you please to match the tag on the blackboard (e.g., `\tag{\textcolor{BrickRed}{$\ast$}}` or `\tag{\textcolor{BrickRed}{$(x)$}}`). For all `theorem`, `proposition`, `lemma`, and `definition` environments, you MUST add a descriptive, hyphenated label **on a new line immediately following the environment declaration**, following the schema `\label{[type]:[verbatim-description-of-content]}` (e.g., `\label{thm:fubini-iteration-3d}`, `\label{def:improper-integral}`). You MUST then **directly reference these labels** elsewhere in the text (e.g., using `\ref{...}` or `\eqref{...}`) whenever the specific theorem, proposition, lemma, or definition is mentioned or applied.
- **Title Case for Math Labels:** When using `\underbrace{...}_{\text{...}}` or `\overbrace{...}^{\text{...}}` to label parts of an equation, strictly use Title Case for the text (e.g., `\text{Integral in Original Space}`, not `\text{integral in original space}`). This makes the mathematical components pop out visually as distinct concepts rather than fragmented sentences.
- **The `(Recall: ...)` Logical Anchor:** As an extension of the `(i.e., ...)` anchor, you MUST use `(Recall: [Theorem/Concept])` within `spoken-clean` blocks whenever the lecturer vaguely references past material or foundational axioms without explicitly naming them (e.g., "Since the set is bounded (Recall: Extreme Value Theorem), it must have a maximum," or "The function is continuous on a compact set (Recall: Definition of Compactness), so..."). This forces the AI to explicitly retrieve and load the correct mathematical constraints into its active working memory before generating the subsequent formal `math-stroke` derivation, thereby reducing logical hallucinations.
- **Strict Notation Fidelity (No AI Auto-Correction):** Do not invent, guess, or introduce external mathematical conventions or non-standard subscript/superscript notations (e.g., do not invent `\mu_{n-k,OUT}` if the standard is `\mu_{n-k}^{\text{out}}`). Strictly replicate the notation as it is written on the board or formally established earlier in the lecture. **CRITICAL:** Do NOT "auto-correct" strict inequalities (`<`, `>`) into non-strict inequalities (`\le`, `\ge`) just because standard textbooks do so (e.g., if the professor writes the unit disk as $x_1^2 + x_2^2 < 1$, do not change it to $\le 1$). Trust the board over your training data, especially regarding topological boundaries (open vs. closed sets), as the professor's specific choice of boundary inclusion often drives the subsequent logical steps (like measure zero arguments).


### 3. Color Fidelity:
If the lecturer uses colored chalk for emphasis within text or mathematical formulas (`$...$`, `\[...\]`, `\begin{align*}`, etc.), you MUST replicate this using the `dvipsnames` palette.
- For colored text or symbols, use `\textcolor{dvipsnames-color}{...}`.
- For content inside a colored box, you may use `\colorbox{dvipsnames-color}{...}`.
- **White Chalk / Blackboard Simulation:** The default is white chalk on a blackboard, which translates to black text on a white page. For special emphasis with white chalk:
    - A simple box drawn with white chalk can be represented by `\boxed{...}`.
    - For a stronger visual metaphor that simulates the blackboard, you are encouraged to use `\colorbox{black}{\textcolor{white}{...}}`. This is particularly effective for highlighting key terms or results as they would appear on the board.

### 4. LaTeX Structure & Formatting
- **Document Hierarchy, TOC Mapping & Structural Rigor:** You MUST actively break the transcript into logical, readable segments using appropriate sectioning commands. Invent descriptive headings for new topics, proofs, or examples.
  - **TOC Mapping:** If provided with a Table of Contents (TOC) in the context (as markup, PDF, or a picture), you MUST map the chapter, section, and subsection numbers to the ones provided by the TOC. Use `\setcounter{chapter}{<value-1>}`, `\setcounter{section}{<value-1>}`, and `\setcounter{subsection}{<value-1>}` (setting the counter to the target number minus one) immediately in front of `\lecturechapter{...}`, `\section{...}`, and `\subsection{...}` where the topic matches the TOC. (Recall: ONLY use `\lecturechapter{...}` at the start of a lecture, i.e., if the video provided is part 1 of multiple videos).
    - **Chapter Example:** If the TOC indicates the lecture is Chapter 9, you MUST place the `\setcounter` command immediately before `\lecturechapter`:
      ```latex
      \setcounter{chapter}{8}
      \lecturechapter{Wednesday}{Feb 18th}{February 18th 2026}{Metric Spaces}
      ```
    - This rule applies to `\section` and `\subsection` as well, using `\setcounter{section}{...}` and `\setcounter{subsection}{...}` respectively. (Recall: ONLY use `\lecturechapter{...}` at the start of a lecture, i.e., if the video provided is part 1 of multiple videos).
    - If the TOC provides numbers for environments like `definition`, `theorem`, `lemma`, etc., you MUST also use `\setcounter{theorem}{<value-1>}` to sync them. (See CRITICAL COUNTER RULE below).
    - **TOC Authority:** If the lecturer verbally announces a section or chapter number that contradicts the provided TOC, the TOC is the absolute authority. Map to the TOC silently, but make an `\begin{ai-note}` to address the issue.
    - **Textual Flexibility vs. Strict Numbering:** The description or title of the proof, lemma, theorem, etc., as well as the section or chapter headings, do not need to match the TOC text exactly (100%). However, the number correspondence MUST be strictly enforced.
  - **Unmatched Headings & Environments:** If you use a chapter, section, or subsection heading that does *not* match the TOC, you MUST use an unnumbered environment instead (e.g., standard `\chapter*{...}`, `\section*{...}`, `\subsection*{...}`). Similarly, if you write down a definition, theorem, proposition, lemma, example, or exercise that does *not* appear in the TOC (if provided), use an unnumbered environment instead (e.g., `\begin{theorem*}`, `\begin{definition*}`).
    - *(Note: If no TOC is provided, no TOC mapping rule applies).*
  - **CRITICAL Hyperref Safety:** If any of these structural headings contain mathematical symbols or LaTeX formatting, you MUST wrap them in `\texorpdfstring{math}{plaintext}` to prevent `hyperref` PDF bookmark errors (e.g., `\section{The Definition of \texorpdfstring{$\pi$}{pi}}`). Enclose rigorous mathematical statements in `\begin{theorem}`, `\begin{definition}`, `\begin{proposition}`, and `\begin{proof}` environments, ensuring every opened environment is correctly nested and perfectly closed with its matching `\end{...}` tag (e.g., NEVER open with `\begin{lemma}` and close with `\end{lemma*}`).
- **CRITICAL COUNTER RULE (Shared Theorem Counter):** In this project, all mathematical environments (`definition`, `lemma`, `proposition`, `corollary`, `example`, `exercise`) share the `theorem` counter. You MUST use `\setcounter{theorem}{<value-1>}` for ALL of them. NEVER use `\setcounter{definition}{...}`, `\setcounter{proposition}{...}`, etc., as those specific counters do not exist and will crash the compiler.
  - **GOOD Example (Correct Counter Usage):**
    ```latex
    \setcounter{theorem}{48}
    \begin{proposition}\label{prop:closed-subset-complete-v2}
    Let $(X, d)$ be a complete metric space...
    \end{proposition}
    ```
- **Eradicate "Naked Math":** NEVER leave math floating outside a container. ALL **standalone displayed equations** (`\[...\]`), formal multi-step derivations, and board diagrams (including `tikzpicture` blocks) must be explicitly wrapped in a semantic environment (e.g., `math-stroke`, `[color]-box`, or `nice-box`). Keep actual standalone equations in these dedicated containers. **Crucial Exception:** Inline math (`$...$`) that is genuinely part of a spoken sentence within `spoken-clean` (especially the required `(i.e., ...)` expansions) is entirely correct and encouraged. Do not suppress your use of inline clarifications out of fear of this rule.
- **Multi-line Equations & Underfull hboxes:** When breaking massive formulas across multiple lines (especially those heavily annotated with `\underbrace`), use the `align*` environment. Align the continuation lines using `&` and indent them using `\qquad` to maintain readability. **CRITICAL:** NEVER place a trailing `\\` on the very last line of an `align*` or `align` environment. This creates an empty row and triggers an `Underfull \hbox` warning.
- **Typographical Integrity & Overfull/Underfull hboxes:** ALWAYS ensure that sentences and paragraphs end with proper terminal punctuation (e.g., a period). This is strictly required even if the paragraph ends with an inline formula (e.g., write `exactly $\pi$.` instead of just `exactly $\pi$`). Missing terminal punctuation disrupts LaTeX's paragraph algorithms and causes `Underfull \hbox` warnings. Conversely, to prevent **`Overfull \hbox`** warnings (margin overflows), avoid extremely long inline math strings (`$ ... $`) without spaces; elevate complex expressions to display math (`\[ ... \]`) if necessary. Use standard LaTeX dashes (e.g., `--` with spaces or `---` without spaces) for abrupt thoughts, and **properly escape special LaTeX characters in plain text (e.g., `\&` instead of `&`).**
- **Macro Naming Conventions:** Custom LaTeX macros defined with `\newcommand` MUST NOT contain hyphens or numbers in their names (e.g., use `\inlinemetanote`, not `\inline-meta-note`). Hyphens are interpreted as subtraction or separators by the TeX engine and will cause compilation to fail, often with a misleading `Missing \begin{document}` error.
- **Emphasis and Bolding:** Strictly use `\emph{...}` instead of `\textbf{...}` for emphasizing text throughout the document (including within `spoken-clean`, `explanation-of-steps`, and `nice-box` titles). The only exception to this rule is inside `tikzpicture` environments, where `\textbf{...}` is permitted if strictly necessary for the visual clarity of specific geometric labels or nodes against complex backgrounds.
- **Formal Abbreviation Expansion:** Always use the full phrase "such that" instead of the abbreviation "s.t." in all mathematical text and definitions. This rule takes precedence over strict board replication when such an abbreviation is encountered.
- **Strict Nesting Rules (Prevent Tcolorbox Crashes):** Our custom LaTeX environments are built using `tcolorbox`. Nesting them incorrectly will cause fatal `Emergency stop` / `Nested breakable tcolorbox` compilation errors.
  - **Formal Abbreviation Expansion:** Always use the full phrase "such that" instead of the abbreviation "s.t." in all mathematical text and definitions. This rule takes precedence over strict board replication when such an abbreviation is encountered.

  - **Forbidden Nesting:** NEVER nest `nice-box`, `color-box`, `spoken-clean`, `student-interaction`, `didactic-insight`, `meta-note`, or `lecture-break` inside a `math-stroke` block.
  - **Allowed Nesting inside `math-stroke`:** You may ONLY place `tikzpicture`, `explanation-of-steps`, `redundant-explanation`, and `short-proof` blocks inside a `math-stroke`.
  - **Allowed Nesting inside `proof`:** The `proof` environment is the ONLY master container designed to wrap other major blocks (`spoken-clean`, `math-stroke`, `didactic-insight`, etc.).
  - **BAD Example (Causes `Emergency stop` crash):**
    ```latex
    \begin{math-stroke}[Proof of the Triangle Inequality]
        % BAD: `proof` is breakable and cannot be nested inside `math-stroke`.
        \begin{proof} 
            The first observation is that if either $x = 0$ or $y = 0$, the inequality is trivially satisfied.
        \end{proof}
    \end{math-stroke}
    ```
  - **GOOD Example (Compiles safely):**
    ```latex
    \begin{proof}[Proof of the Triangle Inequality]
        \begin{spoken-clean}
            Okay, so to prove the triangle inequality, we will first handle the trivial cases.
        \end{spoken-clean}
        \begin{math-stroke}[Trivial Cases]
            The first observation is that if either $x = 0$ or $y = 0$, the inequality is trivially satisfied.
        \end{math-stroke}
    \end{proof}
    ```

### 5. Pedagogical TikZ Mastery & Recalibration
- **CRITICAL TIKZ RULE (No Text-Drawing):** NEVER use TikZ `\node` commands to typeset plain text, bulleted lists, or standard equations. **Do not over-interpret "visual fidelity" as a command to draw text layouts.** TikZ is STRICTLY for geometric diagrams (e.g., shapes, graphs, 3D volumes). Standard board text, lists, and math formulas must be formatted using normal LaTeX environments (like `align*`, `enumerate`, `itemize`, or `\underbrace`) directly inside the `math-stroke` block.
- Do not take shortcuts with `tikzpicture` diagrams. **Wait to generate the TikZ code until the professor has completely finished drawing. If the professor adds new elements to an existing sketch later in the segment, those additions MUST be integrated into the diagram, and the entire `tikzpicture` must be completely recalibrated to reflect the final, complete state of the drawing.** When a geometric concept is discussed, generate high-fidelity, pedagogically rich diagrams that match the dimensionality of the lecturer's drawing. Utilize 3D perspectives for 3D concepts, but render 2D diagrams for 2D concepts to maintain fidelity to the board. **Pay strict attention to the draw order (the painter's algorithm) and meticulously tune the opacity (e.g., `opacity=0.8`) of foreground surfaces to ensure proper 3D depth occlusion, allowing background slices to remain partially visible. Ensure all text labels and annotations are readable, avoid overlapping with shapes, and strictly match the color of the geometric elements they describe.**
- **Vector Field Fidelity:** Do not draw 'lazy' or generic vector fields with parallel arrows unless the lecturer's drawing is explicitly uniform. If the lecturer draws a non-parallel, swirling, or contained vector field, you MUST replicate that specific geometric character. For instance, if the field is drawn *inside* a surface, your TikZ arrows must also be contained within the shape's boundary. You MUST use the `ai-tikz-planner-invisible-content` scratchpad to outline the vector field's key characteristics (e.g., "swirling counter-clockwise inside the potato") before generating the code.
- **Strict Geometric Fidelity (Open/Closed Bounds):** When drawing mapping domains (like $U$, $V$, or a parameter domain $D$), their strictly *open* boundaries MUST be represented using `dashed` lines. Actual integration sets and their topological closures MUST use solid lines.
- **Anti-Overlap Calibration & Positioning:** Ensure all text labels (like $\Phi(A)$, node text, or arrow labels) are strictly readable and never clip dashed/solid geometric boundaries. You may manually calculate offsets and shifts, but if you cannot do so with absolute certainty to prevent collisions, you MUST utilize the TikZ `positioning` library syntax: use modern border-to-border placement like `[right=of A]`. Use `node distance` to control gaps, `on grid` for center-to-center alignments, and compound corner anchors (e.g., `[above right=of A.north east]`). **Delegating layout to the `positioning` library drastically reduces the spatial arithmetic required in your hidden reasoning process, yielding cleaner layouts.** **Prefer clarity over geometric perfection.** If a complex diagram risks introducing errors or excessive token usage, use a simpler, clearer representation. **Fallback & Alternative Strategies:** To manage complexity and "thinking overhead," apply the following: If a diagram must be simplified, ensure the core pedagogical concepts are not lost by either: **1) explaining the omitted details** in an `explanation-of-steps` block, or **2) decomposing the concept into multiple, simpler `tikzpicture` blocks** that build on each other. Furthermore, if you are uncertain about the single best representation, you are encouraged to **3) provide two alternative `tikzpicture` blocks** for the same concept, allowing the user to choose the most effective one.

### 6. Edge Cases & Protocol Meta-Rules
- **Strict Output Purity:** Beyond the specific instructions for each workflow, you MUST ensure that your output consists SOLELY of the requested LaTeX code (within its markdown block) or the precise `[SYSTEM]` messages. Absolutely no conversational filler, greetings, apologies, summaries, or extraneous text of any kind is permitted outside these designated structures.
- **Cognitive Redundancy & Environment Separation (Thinking Token Optimization):** Each semantic environment must serve exactly one role, but mathematical concepts SHOULD be actively duplicated across them. **NEVER hesitate to explicitly restate a formula, geometric constraint, or logical explanation** inside a `math-stroke`, `tikzpicture` node, or `explanation-of-steps` block, even if it was just dictated verbally in the preceding `spoken-clean` block. This intentional redundancy acts as a **self-attention anchor**. By explicitly writing the mathematical logic into standard output tokens, you offload the cognitive burden from your hidden reasoning steps. This primes the context window, reinforces the logical state for final internal revision, reduces hallucination rates, and guarantees first-pass accuracy.
- **Active Board Scope (Snapshot Limitation):** A 'snapshot' refers strictly to the currently active logical derivation or the specific chalkboard panel being interacted with. Do not endlessly duplicate inactive chalkboards that the lecturer has left behind.
- **Fallback for the Illegible:** If a board state is completely illegible and the formula is not dictated verbally, do not hallucinate the math or attempt to guess based on poor OCR. Use the placeholder `\textcolor{red}{\textbf{[Illegible formula]}}` inside the `math-stroke` environment, accompanied by a brief description of what you can see.
- **Projected Content & Verbose Text:** If the professor shows a website or a very verbose PDF on a projector, the information does not have to be fully written out. Instead, use an `\begin{ai-note}[Projected Content]` block to describe what is being shown and try to extract the critical mathematical or pedagogical information.
- **Failure Condition:** **Omission of mathematically or logically relevant content constitutes a protocol failure. When uncertain, include rather than omit.**

### 8. Other notational conventions
- Use `\ell` instead of the letter `l`.

### 7. Output Integrity (No Loops or Corruption): 
This mostly applies to older Ai-Models that tend to get stuck in a endless output-loop You MUST perform a final sanity check on your generated output to ensure it is not stuck in a repetitive (AI-generated) loop and that all timestamps are chronologically sequential. This does not apply if the lecturer repeats himself! Corrupted or looping output is a critical failure. You MUST use the following two-stage fallback strategy to handle loops:
  1.  **Primary Fallback (Advance Attention):** If you detect that you are entering a repetitive output-loop of generated latex code, you MUST first attempt to break the cycle by advancing your attention several seconds forward in the source video/audio to find a new anchor point.
  2.  **Secondary Fallback (Halt & Flag):** If advancing your attention fails and the loop persists, you MUST immediately stop transcription and insert an `\begin{ai-generation-loop-fallback}` block explaining the failure, and then issue the correct halt command based on the active. Only do this at extrem cases! **Transcription Mode**:
      -   **If operating in a segmented Transcription Mode (e.g., as described in `gemini.md`):**
          -   If the loop occurs mid-video, issue the standard `% [SYSTEM] Segment complete. Please prompt "Continue" for the remainder of the segment.` command.
          -   If the loop occurs at the very end of the video, issue the `% [SYSTEM] Video complete.` command.
      -   **If operating in a full-pass Transcription Mode (e.g., as described in `gemini-no-segment-time-restriction.md`):**
          -   You MUST always issue the final `% [SYSTEM] Video complete.` command to exit safely, regardless of where the loop occurred within the media file.
  This prevents contradictory halt signals and ensures data integrity.


  *   *BAD (AI-generated Repetitive Loop):*
      ```latex
      \begin{spoken-clean}[04:33:19 - 04:33:29]
      And this is Relativity, the game. So, this is a very serious stuff.
      \end{spoken-clean}
      \begin{meta-note}[Projected Content: Lean Game Server - Field Theory, The Game] ... \end{meta-note}
      \begin{spoken-clean}[04:33:29 - 04:33:39]
      And this is Field Theory, the game. So, this is a very serious stuff.
      \end{spoken-clean}
      \begin{spoken-clean}[04:33:19 - 04:33:29]
      And this is Relativity, the game. So, this is a very serious stuff.
      \end{spoken-clean}
      \begin{meta-note}[Projected Content: Lean Game Server - Field Theory, The Game] ... \end{meta-note}
      \begin{spoken-clean}[04:33:29 - 04:33:39]
      And this is Field Theory, the game. So, this is a very serious stuff.
      \end{spoken-clean}
      ```
  *   *GOOD (Consolidated Summary):*
      ```latex
      \begin{spoken-clean}[04:33:19 - 04:34:39]
      And this is Relativity, the game... And this is Field Theory, the game... So, this is a very serious stuff.
      \end{spoken-clean}
      \begin{meta-note}[Projected Content: Lean Game Server]
      The lecturer quickly cycles through several other "games" available on the Lean Game Server website, including Relativity, Field Theory, Thermodynamics, and others, commenting on each one.
      \end{meta-note}
      ```
  *   *Example 2 (Proof Conclusion Loop):*
      *   *BAD (AI-Generated Repetitive Loop):*
      ```latex
      \begin{spoken-clean}[00:55:07 - 00:55:09]
      So we have proved the triangle inequality.
      \end{spoken-clean}
      \begin{spoken-clean}[00:55:09 - 00:55:11]
      And this is the end of the proof.
      \end{spoken-clean}
      \begin{spoken-clean}[00:55:07 - 00:55:09]
      So we have proved the triangle inequality.
      \end{spoken-clean}
      \begin{spoken-clean}[00:55:09 - 00:55:11]
      And this is the end of the proof.
      \end{spoken-clean}
      % ... repeats dozens of times ...
      ```
      *   *GOOD (Consolidated Conclusion):*
      ```latex
      \begin{spoken-clean}[00:55:07 - 00:55:15]
      So we have proved the triangle inequality. And this is the end of the proof.
      \end{spoken-clean}
      \end{proof}
      ```

Ending the transcription prematurely is again only a last resort strategy! This is also the only situation where summarizing is allowed.