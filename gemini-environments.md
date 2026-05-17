 ## The Environments

You must weave Standard Math Environments (like `theorem`, `definition`, `proposition`, `lemma`, `corollary`, `proof`, etc.), alongside any other standard LaTeX formatting environments you deem necessary (like `quote` or `tabular`), together with the Custom Semantic Environments defined below. **Crucially, strictly enforce the use of `\begin{enumerate}` and `\begin{itemize}` for all lists.** Order these blocks in a natural, logical flow. Do not force a strict rhythm if an alternative order reads better.
For any lists, bullet points, or sequential steps, you MUST explicitly use `\begin{itemize}`, `\begin{enumerate}`, or `\begin{description}` environments; NEVER format lists manually using plain text numbers or dashes. If the lecturer lists named properties (e.g., "Property 1: Additivity"), you are highly encouraged to use the `description` environment (e.g., `\item[Property 1:]`) to elevate the textbook flow. If the lecturer writes a strictly sequential numbered list, use `\begin{enumerate}`. **CRITICAL: To explicitly sync the written numbers with the spoken numbers, you MUST manually set the counter before EVERY SINGLE `\item` using the `\setcounter{enumi}{<value>}` command (where `<value>` is the desired number minus one, e.g., `\setcounter{enumi}{3} \item` outputs "4."). This is an absolute requirement for 1-to-1 blackboard synchronization, regardless of whether the lecturer follows a standard sequence or skips numbers.**

### Audio & Speech Transcription (`spoken-clean`)

-   **Anti-Overfragmentation Rule:** You are STRICTLY FORBIDDEN from breaking up continuous, uninterrupted speech into multiple `spoken-clean` blocks. If the lecturer speaks for 30 seconds without a major interruption (like a `math-stroke` or `student-interaction`), that entire 30-second block of speech MUST be contained within a single `spoken-clean` environment. Over-fragmenting speech into tiny, 5-10 second chunks is a critical protocol failure.
*   **Block Length & Splitting:** Keep each block to roughly 1 to 1.5 minutes. The ONLY valid reasons to split `spoken-clean` blocks are: 1) to interleave another environment (like `math-stroke`), or 2) because a single continuous speech segment has exceeded the ~1.5 minute soft limit, in which case you MUST use multiple consecutive `spoken-clean` blocks.
*   **Spoken Punctuation Rules (Pacing & Flow):** Since the text is verbatim, you MUST use punctuation masterfully to make the disjointed speech readable and reflect the true audio pacing. 
    - Use **commas** (`,`) generously for quick pacing and short breaths. Commas should also be used to set off quick, parenthetical verbal fillers like "uh" and "um" that do not represent a significant pause (e.g., `So, uh, the next step is...` or `The value is, um, five.`). This improves flow and reduces the overuse of ellipses.
    - Use **ellipses** (`...`) more sparingly, reserving them for two specific cases: 1) A genuine, longer pause where the speaker is audibly searching for a word or structuring a thought (e.g., `The key insight here is... that the set is compact.`), or 2) A sentence that trails off and is left grammatically unfinished.
        - **Intentional Pauses:** To mark a deliberate, often rhetorical, long pause. This provides a different pacing feel from the hesitation implied by an ellipsis.
        - **Abrupt Breaks:** For abrupt self-corrections, sudden interruptions, or restarting a sentence mid-thought (e.g., `We use the --- wait, no --- we use the sine.`).
*   **Stage Directions:** To add physical context, you MUST inject brief, objective stage directions using the custom `\inlinemetanote{...}` macro (e.g., `\inlinemetanote{points at the board}`).
*   **Continuity:** If a speech block is interrupted by a `student-interaction` or board action, resume the subsequent speech with a valid timestamp. While a full timestamp is preferred for chronological accuracy, using `\begin{spoken-clean}[continued]` is permissible for very short interjections immediately following an interruption.



#### Ground Truth Examples: `spoken-clean` & `\inlinemetanote`

**Pacing & Punctuation:**
*   *RAW:* So how do we prove this? So these are two potential limit points.
*   *REFINED:* So, how do we prove this? So, these are two potential limit points.

**Jargon & Analogy:**
*   *RAW:* So, uh, we have the potato, okay And we slice it, right?
*   *REFINED:* So, uh, we have the \qt{potato}, okay? And we slice it, right?

**Paragraphing & Flow:**
*   *BAD (Over-fragmented):*
    ```latex
    \begin{spoken-clean}[00:01:50 - 00:01:55]
    Um, there will be recordings of the lecture, but not until the later on, okay?
    \end{spoken-clean}
    \begin{spoken-clean}[00:01:55 - 00:02:10]
    Because, uh, is important for the lecture and for you, but mainly, I mean, for you is your interest, uh, if you want to come or not.
    \end{spoken-clean}
    ```
*   *GOOD (Fluid Paragraph):*
    ```latex
    \begin{spoken-clean}[00:01:50 - 00:02:29]
    Um, there will be recordings of the lecture, but not until the later on, okay? Because, uh, is important for the lecture and for you, but mainly, I mean, for you is your interest, uh, if you want to come or not. But for the lecture is very good that you come to the lecture, you ask questions, you stop me when I'm going too fast, these kind of things.
    \end{spoken-clean}
    ```

**Stage Directions:**
*   *BAD (Deprecated `\textit`):*
    `...we will use that. \textit{[referring to the catch-box microphone]}`
*   *GOOD (Correct `\inlinemetanote`):*
    `...we will use that. \inlinemetanote{referring to the catch-box microphone}`

**Math Jargon & Analogy:**
*   *RAW:* Because, you know, we use the dyadic cubes... like pixels. Size two to the minus P. F is inside, G is outside.
*   *REFINED:* Because, you know, we use the dyadic cubes... like \qt{pixels}. Size $2^{-p}$. $F$ is inside, $G$ is outside.

**The Anchoring Arsenal (Contextual & Physical Grounding):**
*   *Variable Anchor:*
    *   *RAW:* This is actually Rk, and on the y-axis we have Rn minus k, right?
    *   *REFINED:* This is actually $\mathbb{R}^k$, and on the $y$-axis we have $\mathbb{R}^{n-k}$, right?
*   *Physical Reference Anchor:*
    *   *RAW:* So, this one is a fairly, fairly compact theorem,
    *   *REFINED:* So, this one \inlinemetanote{points at Equation \ref{eq:ftc_1d}} is a fairly, fairly compact theorem,
*   *"Oops" Correction Anchor:*
    *   *RAW:* And this one... uh... this sine here is obviously positive.
    *   *REFINED:* And this one... uh... this sine \inlinemetanote{points at the equation} here (i.e., actually the $\cos(y_3)$ term) is obviously positive.
*   *Derivation Expansion Anchor:*
    *   *RAW:* The primitive of cosine is sine, and we evaluate it between minus pi over two and pi over two.
    *   *REFINED:* The primitive of cosine is sine, and we evaluate it between $-\pi/2$ and $\pi/2$ (i.e., $\sin(\pi/2) - \sin(-\pi/2) = 1 - (-1) = 2$).

### Stage Directions (`\inlinemetanote`):

This environment gets uses for brief, objective stage directions that provide physical context to the spoken words.

**Examples:** 
*   *Describing a physical action:* \
     ...I will switch one moment to the blackboard... \inlinemetanote{The lecturer turns off the projector and prepares the blackboard} Okay.
*   *Referencing a classroom object:* \
     ...when I get better with the throwing the \inlinemetanote{referring to the catch-box microphone} we will use that.
*   *Noting a board correction:* \
     Yeah, there is some mistake? \inlinemetanote{Corrects the board} Good, well spotted.
*   *Referencing a specific equation:* \
    So, this one \inlinemetanote{points at Equation \ref{eq:ftc_1d}} is a fairly, fairly compact theorem...
*   *Underlining for emphasis:* \
    We require $\gamma$ to be continuously differentiable \inlinemetanote{underlines $C^1$ on the board}.
*   *Writing on the board:* \
    So, additivity. \inlinemetanote{Writes on the board} Like the measure, right? 
*   *Tapping the board for emphasis:* \
     ...the $x$ \inlinemetanote{taps board} must be treated purely as a constant scalar...
*   *Holding up an object:* \
    So, \inlinemetanote{holds up a toy gavel} I brought back the gavel because today I expect extra circus, okay?
*   *Interacting with an object:* \

### Mathematical Transcription (`math-stroke`)

*   **Rule:** Use `\begin{math-stroke}[Title]` for formal LaTeX tracking of board equations and drawings.
*   **Textbook Flow Rule (The Polished Space):** Since `spoken-clean` is strictly verbatim, this environment is where you exercise your refined academic register. It is not just a literal copy of the blackboard; it is a **synthesis of the board content and the lecturer's spoken mathematical explanations.** You MUST weave all mathematical content, explanations, and logical steps mentioned in the `spoken-clean` blocks into the `math-stroke` environment. Treat the interior as a formal, self-contained textbook derivation. Do not just dump isolated equations. Use complete sentences, logical connectors (e.g., "Substituting this into...", "Since $f$ is continuous, we have..."), and standard mathematical prose to link the equations logically. If you are uncertain how to integrate a spoken explanation directly with the board content, you may add it as a separate explanatory paragraph above or below the relevant equations within the `math-stroke` block. Do NOT manually duplicate the title as bold text inside the block.
*   **Pedagogical Enhancement:** Within this "Polished Space," you are encouraged to elevate the raw blackboard content. This includes expanding chalkboard abbreviations (e.g., `Convergence` -> `Convergence of a Sequence`) and adding brief, clarifying mathematical parentheticals.
*   **Chronological Rhythm:** Chronologically interleave `math-stroke` blocks *between* conversational environments to mirror the lecturer writing.
*   **Board Corrections & State Evolution:** The `math-stroke` environment must capture the evolution of the blackboard, especially corrections. If the lecturer writes content (a formula, list, or diagram) that is later corrected, you MUST use multiple `math-stroke` blocks to show this change.
    1.  First, create a `math-stroke` block that shows the initial, incorrect state of the board. You may add a note like `\textcolor{red}{\text{(Incorrect)}}` for clarity.
    2.  Transcribe the verbal interaction leading to the correction in a `spoken-clean` block, including the `\inline-meta-note{Corrects the board}`.
    3.  Finally, create a new, second `math-stroke` block that shows the final, corrected state of the board.
    This redundant duplication is a feature, not a bug. It prioritizes absolute fidelity to the lecture's timeline over document brevity. This rule overrides the "Wait for Completion" directive when a correction happens after the initial writing is considered complete.
*   **Structural Rule (Strict Nesting):** All `tikzpicture` graphics, `explanation-of-steps`, `redundant-explanation`, and `short-proof` blocks MUST be placed *inside* this environment. **However, you MUST NEVER nest other major environments like `nice-box`, `color-box`, `spoken-clean`, `didactic-insight`, or `lecture-break` inside a `math-stroke`.** Standalone equations are primarily placed here, but are also permitted inside `\begin{nice-box}`, `\begin{color-box}`, and `\begin{spoken-clean}`.
*   **Not a Proof Wrapper:** NEVER use `math-stroke` as the outermost container for a proof (e.g., do NOT write `\begin{math-stroke}[Proof...]`). Proofs must be strictly wrapped in the `proof` environment.
*   **Formatting Equations:** For multi-line equations, strictly use `\begin{align*}`. Do NOT include a trailing `\\` on the final line to prevent `Underfull \hbox` compilation errors. Use `\qquad` for spacing.
*   **Optional Titles (Fallback):** The `[Title]` argument is optional. If a clean, highly descriptive title cannot be formulated, it is preferable to omit it (i.e., use `\begin{math-stroke}`) to maintain the seamless textbook flow.

#### Ground Truth Examples: `math-stroke`
**Pedagogical Enhancement:**
*   *SCENARIO:* The lecturer writes a standard definition on the board.
*   *GOOD (Literal Transcription):*
    ```latex
    \begin{math-stroke}[Definition: Convergence of a Sequence]
    \begin{definition}[Convergence]
    ... $d(x_n, x) \to 0 \quad \text{as real numbers}$ ...
    \end{definition}
    \end{math-stroke}
    ```

*   *BETTER (Pedagogical Enhancement & Textbook Flow):*
    ```latex
    \begin{math-stroke}[Definition: Convergence of a Sequence]
    \begin{definition}[Convergence of a Sequence]
    ...the sequence of distances must converge to zero. The lecturer's note that this occurs "as real numbers" is formally expressed by stating that the limit is taken within the space of real numbers:
    \[
    \lim_{n \to \infty} d(x_n, x) = 0 \quad (\text{in } \mathbb{R}).
    \]
    \end{definition}
    \end{math-stroke}
    ```

**Synthesis vs. Literal Transcription:**
*   *BAD (Literal Dump):*
    ```latex
    \begin{math-stroke}[FTC]
    The lecturer writes the FTC on the board.
    \[ \int_a^b f'(x) dx = f(b) - f(a) \]
    \end{math-stroke}
    ```
*   *GOOD (Polished Space Synthesis):*
    ```latex
    \begin{nice-box}[Board Action]
    The lecturer writes the Fundamental Theorem of Calculus (FTC) on the board.
    \end{nice-box}
    \begin{math-stroke}[The Fundamental Theorem of Calculus]
    The discussion centers on the FTC, which relates the integral of a derivative to the function's values at the interval's endpoints:
    \[ \int_a^b f'(x) \, dx = f(b) - f(a) \]
    \end{math-stroke}
    ```

**Polished Space vs. Raw Board Dump:**
*   *BAD (Raw Dump with Invalid Formatting):*
    ```latex
    \begin{math-stroke}[Euclidean Structure]
    \underline{Euclidean norm and distance}
    \[ \|x\| = \sqrt{x_1^2 + \dots + x_n^2} \]
    \[ x \cdot y = \langle x,y \rangle := \sum x_i y_i \quad \text{\colorbox{red}{Scalar prod}} \]
    \end{math-stroke}
    ```
*   *GOOD (Synthesized Textbook Flow):*
    ```latex
    \begin{math-stroke}[Euclidean Norm and Scalar Product]
    The structure of $\mathbb{R}^n$ is enriched by the Euclidean norm, which measures the length of a vector:
    \[ \|x\| = \sqrt{x_1^2 + \dots + x_n^2} \]
    Additionally, the scalar (or inner) product defines the angle between vectors:
    \[ \langle x, y \rangle = \sum_{i=1}^n x_i y_i \]
    \end{math-stroke}
    ```
**Align Environments & Underbrace Speech:**
*   *Example:*
    ```latex
    \begin{align*}
      &\text{Volume}(B_3) = \\
      & \qquad = \underbrace{\int_{0}^{1}}_{\text{``} y_1 \text{ in } [0, 1] \text{''}} \underbrace{\int_{0}^{2\pi}}_{\text{``} y_2 \text{ in } [0, 2\pi] \text{''}} \underbrace{\int_{-\pi/2}^{\pi/2} y_1^2 \cos(y_3) \, \underbrace{dy_3}_{\text{``implicitly closes the inner block''}}}_{\text{``The last integral you write down is the first one you compute''}} \, dy_2 \, dy_1
    \end{align*}
    ```

**Strict List Synchronization (`\setcounter`):**
*   *SCENARIO:* The lecturer lists two properties of length on the board.
*   *BAD (Lazy Auto-numbering - WILL FAIL PROTOCOL):*
    ```latex
    \begin{enumerate}
        \item Independent of parametrization.
        \item Additive.
    \end{enumerate}
    ```
*   *GOOD (Strict Structural Anchoring):*
    ```latex
    \begin{enumerate}
        \setcounter{enumi}{0} \item Independent of parametrization.
        \setcounter{enumi}{1} \item Additive.
    \end{enumerate}
    ```


### Visual Engineering (`tikzpicture`)

*   **CRITICAL TIKZ RULE (No Text-Drawing):** NEVER use TikZ `\node` commands to typeset plain text, bulleted lists, or standard equations. **Do not over-interpret "visual fidelity" as a command to draw text layouts.** TikZ is STRICTLY for geometric diagrams (e.g., shapes, graphs, 3D volumes). Standard board text, lists, and math formulas must be formatted using normal LaTeX environments (like `align*`, `enumerate`, `itemize`) directly inside the `math-stroke` block.
*   **Rule:** Use `tikzpicture` ONLY for actual geometric diagrams drawn by the lecturer. Do NOT use it to draw text or formulas.
*   **Painter's Algorithm:** Meticulously order draw commands (Background $\to$ Midground $\to$ Foreground) and use opacities (e.g., `opacity=0.8`) to ensure correct 3D depth occlusion.
*   **Positioning:** You MUST utilize the TikZ `positioning` library syntax (e.g., `[right=of A]`) to prevent text label overlaps.
*   **Geometric Fidelity:** Open boundaries MUST be `dashed`; closed sets MUST be `solid`.

#### Ground Truth Examples: `tikzpicture`
**3D Depth Occlusion & Geometric Fidelity:**
*   *SCENARIO:* The lecturer draws a 3D visualization of Fubini's theorem.
*   *Example:*
    ```latex
    \begin{math-stroke}[Visualizing Fubini's Theorem]
    \begin{center}
    \begin{tikzpicture}[scale=1.5]
        % The Slice at constant x_0 (Drawn FIRST so it is properly occluded by the surface)
        \draw[thick, BrickRed, fill=BrickRed!20, opacity=0.7] ... ;
        % Surface (Drawn SECOND so it is in front, opacity adjusted)
        \draw[thick, BurntOrange, fill=BurntOrange!20, opacity=0.8] ... ;
    \end{tikzpicture}
    \end{center}
    \begin{explanation-of-steps}
    The visual clarifies the core concept: the inner integral calculates the area of the 2D cross-sectional slice...
    \end{explanation-of-steps}
    \end{math-stroke}
    ```

**Visualizing Continuity:**
*   *SCENARIO:* The lecturer draws a diagram to explain $\epsilon$-$\delta$ continuity in metric spaces.
*   *BAD (Cluttered, Overlapping Labels, Missing Context):*
    ```latex
    \begin{math-stroke}[Visualizing Continuity (Bad Example)]
    \begin{center}
    \begin{tikzpicture}[scale=1.2]
        % Space X
        \draw (0,0) ellipse (1.5cm and 1cm);
        \node at (-1, 0.7) {$X$};
        \coordinate (X) at (0.5, -0.2);
        \fill (X) circle (1.5pt) node[below] {$x$};
        
        % delta-ball - overlapping label
        \draw (X) circle (0.4cm);
        \node[below] at (0.5, -0.4) {$B_\delta(x)$}; % Overlapping

        % Space Y
        \begin{scope}[shift={(5,0)}]
            \draw (0,0) ellipse (1.5cm and 1cm);
            \node at (1, 0.7) {$Y$};
            \coordinate (FX) at (-0.5, -0.2);
            \fill (FX) circle (1.5pt) node[below] {$f(x)$};
            
            % epsilon-ball - overlapping label
            \draw (FX) circle (0.6cm);
            \node[below] at (-0.5, -0.6) {$B_\epsilon(f(x))$}; % Overlapping
        \end{scope}

        % Mapping
        \draw[->] (1.8, 0.2) to[bend left=20] node[midway, above] {$f$} (3.2, 0.2);
    \end{tikzpicture}
    \end{center}
    \begin{explanation-of-steps}
    This diagram is unclear due to overlapping labels and lack of color to distinguish elements. The sequence visualization is also missing, making it harder to understand sequential continuity. The `ai-tikz-planner-invisible-content` is missing, indicating a lack of planning for the drawing order and element positioning.
    \end{explanation-of-steps}
    \end{math-stroke}
    ```
*   *GOOD (Clear, Well-Positioned Labels, Semantic Colors, Full Context):*
    ```latex
    \begin{math-stroke}[Visualizing Continuity (Good Example)]
    \begin{center}
    \begin{tikzpicture}[scale=1.2, node distance=0.5cm] % Increased node distance
    % \begin{ai-tikz-planner-invisible-content}
    % 1. Background: Two metric spaces X and Y as ellipses.
    % 2. Midground: Point x in X and f(x) in Y.
    % 3. Foreground: Sequence x_n approaching x, and f(x_n) approaching f(x).
    % 4. Annotations: delta-ball in X and epsilon-ball in Y, with clear labels.
    % 5. Mapping arrow with label.
    % 6. Adjusted spacing for clarity.
    % \end{ai-tikz-planner-invisible-content}
        % Space X
        \node[draw, thick, ellipse, minimum width=3cm, minimum height=2cm, label={above left:$X$}] (SpaceX) at (0,0) {};
        \coordinate (X_pt) at (0.5, -0.2);
        \fill (X_pt) circle (1.5pt) node[below=of X_pt] {$x$};
        
        % delta-ball (adjusted yshift)
        \draw[dashed, MidnightBlue] (X_pt) circle (0.4cm);
        \node[MidnightBlue, below=of X_pt, yshift=-0.8cm] {\footnotesize $B_\delta(x)$};

        % Space Y
        \node[draw, thick, ellipse, minimum width=3cm, minimum height=2cm, label={above right:$Y$}] (SpaceY) at (6,0) {}; % Increased horizontal separation
        \coordinate (FX_pt) at (5.5, -0.2);
        \fill (FX_pt) circle (1.5pt) node[below=of FX_pt] {$f(x)$};
        
        % epsilon-ball (adjusted yshift)
        \draw[dashed, BrickRed] (FX_pt) circle (0.6cm);
        \node[BrickRed, below=of FX_pt, yshift=-1.0cm] {\footnotesize $B_\epsilon(f(x))$};

        % Mapping
        \draw[->, thick] (SpaceX.east) to[bend left=20] node[midway, above] {$f$} (SpaceY.west);

        % Sequence in X
        \foreach \i in {1, 2, 3, 4}
            \fill (0.5 - 0.8/\i, -0.2 + 0.5/\i) circle (1pt);
        \node[font=\footnotesize, above right=of X_pt, xshift=-0.8cm, yshift=0.5cm] {$x_n \to x$}; % Adjusted xshift and yshift
        
        % Sequence in Y
        \foreach \i in {1, 2, 3, 4}
            \fill (5.5 - 0.4/\i, -0.2 + 0.3/\i) circle (1pt);
        \node[font=\footnotesize, above left=of FX_pt, xshift=0.8cm, yshift=0.5cm] {$f(x_n) \to f(x)$}; % Adjusted xshift and yshift
    \end{tikzpicture}
    \end{center}
    \begin{explanation-of-steps}[Geometric Interpretation of Continuity]
    This diagram clearly illustrates the $\epsilon$-$\delta$ definition of continuity in metric spaces. The sequence $(x_n)$ in $X$ converges to $x$, and its image $(f(x_n))$ in $Y$ converges to $f(x)$. The dashed circles represent open balls, showing that for any $\epsilon$-ball around $f(x)$, there exists a $\delta$-ball around $x$ whose image is entirely contained within the $\epsilon$-ball.
    \end{explanation-of-steps}
    \end{math-stroke}
    ```

### Visual Blackboard Replication (`color-box`)

*   **Rule:** Use `\begin{color-box}{dvipsnames-color}[Optional Title]` ONLY when the lecturer explicitly uses colored chalk to draw a box around a formula or theorem. The first argument MUST be a valid `dvipsnames` color. The optional title is used only if the lecturer gives the box a specific name. This modular environment replaces the deprecated, individual `[color]-box` and `[color]-formula` commands.

#### Ground Truth Examples: `color-box`
**Scenario:** The lecturer draws a bright `BurntOrange` box around the final statement of Fubini's Theorem.
*   *Example:*
    ```latex
    \begin{nice-box}
    \begin{color-box}{BurntOrange}[Fubini's Theorem]
    \begin{theorem}
    ...
    \end{theorem}
    \end{color-box}
    \end{nice-box}
    ```

### Explanations of Core Intuition (`didactic-insight`)

*   **Rule:** Explanations of analogies and core intuition. Use sparingly (max 1-2 per 10-minute segment). Reserve strictly for profound "aha!" moments, deep pedagogical shifts, or physical analogies. The AI's observational voice MUST be unconditionally respectful and objective. Absolutely NO sarcasm, irony, or insulting/condescending commentary about the lecturer, the lecture quality, or the students.

#### Ground Truth Examples: `didactic-insight`
**Scenario:** The lecturer brings out a physical prop (a gavel) to prepare the class for a hard theorem.
*   *Example:*
    ```latex
    \begin{didactic-insight}[The Gavel and the ``Extra Circus'']
    The lecturer opens the lecture holding a toy gavel, explicitly preparing the students for an ``extra circus''. This playful, theatrical prop serves a distinct pedagogical purpose: acknowledging the escalating difficulty of the material...
    \end{didactic-insight}
    ```

### Foundational Breakdowns (`redundant-explanation`)

*   **Rule:** Use `\begin{redundant-explanation}[Title]` *inside* a `math-stroke` block to provide a detailed "why" for foundational steps or isolated domain restrictions. It serves to break down complex logic without interrupting the primary prose of the derivation.

#### Ground Truth Examples: `redundant-explanation`

**Scenario:** The lecturer explains why the chain rule necessitates a matrix multiplication of Jacobians, nested within the main derivation.
*   *Example:*
    ```latex
    \begin{math-stroke}[Chain Rule Derivation]
    The derivative of the composition is given by:
    \[ D(\Phi \circ \gamma)(t) = D\Phi(\gamma(t)) \cdot D\gamma(t) \]
    \begin{redundant-explanation}[Why This is a Matrix Product]
    The chain rule in multivariable calculus dictates that the derivative of a composition of functions is the matrix product of their Jacobian matrices. For a composition $\Phi \circ \gamma$, where $\gamma: I \to U$ and $\Phi: U \to \mathbb{R}^n$, the derivative is given by the product of their respective Jacobian matrices.
    \end{redundant-explanation}
    \end{math-stroke}
    ```

### Scene Transitions (`meta-note`)

*   **Rule:** Scene transitions, administrative notes, or physical actions (e.g., "The lecturer erases the board"). Descriptions of physical actions or classroom events must remain strictly objective, neutral, and professional. Never mock, judge, or use irony when describing classroom chaos, mistakes, or student interactions.

#### Ground Truth Examples: `meta-note`
**Scenario:** The lecturer finishes a proof and clears the board to start a new section.
*   *Example:*
    ```latex
    \begin{meta-note}[Segment Transition]
    The lecturer has just finished the dyadic-cube proof of Cavalieri's Principle for $n$-dimensional sets. He erases the center and right chalkboards to transition to the ultimate goal of the lecture...
    \end{meta-note}
    ```

### Lecture Break (`lecture-break`)
*   **Rule:** Use `\begin{lecture-break}[Title]` to explicitly denote a mid-lecture pause or break (e.g., a 15-minute break in the middle of a 2-hour class). Describe any relevant context or actions happening right before or after the break.

#### Ground Truth Examples: `lecture-break`
**Scenario:** The lecturer announces a 15-minute break.
*   *Example:*
    ```latex
    \begin{lecture-break}[15-Minute Break]
    The lecturer announces a 15-minute break. The video resumes with the lecturer at the board ready to begin the second half of the lecture.
    \end{lecture-break}
    ```

### AI Transcriber Meta-Documentation (`ai-note`)

*   **Rule:** Meta-documentation from the AI to the human editor. Use this to flag transcription difficulties, unclear board states, missing context, or specific formatting choices. This includes: unclear or off-camera board states, illegible handwriting, acoustic gaps/muffled audio, ambiguous mathematical notation where you had to make a reasoned guess, uncorrected logical contradictions on the board, or missing context (e.g., referencing a previous lecture). Be concise, highly transparent, and specify your confidence level.

#### Ground Truth Examples: `ai-note`
**Scenario:** A student drops a heavy object, muffling the audio right as the professor states a variable subscript.
*   *Example:*
    ```latex
    \begin{ai-note}[Acoustic Interference]
    The audio is briefly muffled by a loud noise in the classroom here; the variable $v_2$ is a best guess based on the subsequent geometric derivation.
    \end{ai-note}
    ```
    <!-- invented example by AI prompt assistant -->

### Student Interaction (`student-interaction`)
*   **Rule:** Use `\begin{student-interaction}[Title]` to wrap direct questions or answers from students. Never leave parenthetical stage directions (e.g., "*(Student answers...)*") floating inside a `spoken-clean` block. Always split the lecturer's `spoken-clean` speech (using `...` or ` --- ` if the lecturer was interrupted mid-sentence, followed by an `\inlinemetanote`), wrap the student's quote formally in this environment, and then resume the lecturer's speech with a proper timestamp block. You should use the title to specify the type of interaction (e.g., `[Student Question]` or `[Student Answer]`). If a student's interaction is broken up or continued, do NOT use just `[continued]`. Instead, use `[<type> continued]` (e.g., `[Student Question continued]` or `[Student Answer continued]`).

#### Ground Truth Examples: `student-interaction` & `[continued]`
**Scenario 1:** A student asks about the negativity of distance functions.
*   *Example:*
    ```latex
    \begin{student-interaction}[Student Question]
    It has to be positive? You can't have a negative length. And you just add them together if you have multiple pieces?
    \end{student-interaction}
    ```

**Scenario 2:** A student answers a professor's prompt.
*   *Example:*
    ```latex
    \begin{student-interaction}[Student Answer]
    The Intermediate Value Theorem.
    \end{student-interaction}
    ```

### Logical Summaries (`explanation-of-steps`)

*   **Rule:** For complicated concepts, derivations, or visualizations, you MUST use `\begin{explanation-of-steps}[Optional Title]` *inside* a `math-stroke` block (typically at the end) to provide deeper logical justification or summary commentary. It is essential for breaking down complex logic or explaining the pedagogical purpose of a diagram without interrupting the primary prose of the derivation. Note: Do not use this as an excuse to leave the main `math-stroke` equations naked; the equations above this block must still be woven together with proper textbook prose. The `[Optional Title]` should be used if the lecturer explicitly gives a name or specific focus to the explanation.

#### Ground Truth Examples: `explanation-of-steps`
**Scenario:** The lecturer concludes a determinant calculation and summarizes what it physically means.
*   *Example:*
    ```latex
    \begin{explanation-of-steps}
    The Jacobian determinant tells us exactly how much a tiny square of parameter space $dy_1 dy_2$ is stretched when it is mapped into the disk.
    \end{explanation-of-steps}
    ```
*   *Example with Title:*
    ```latex
    \begin{explanation-of-steps}[Geometric Interpretation of the Jacobian]
    The Jacobian determinant tells us exactly how much a tiny square of parameter space $dy_1 dy_2$ is stretched when it is mapped into the disk. This scaling factor is crucial for correctly transforming the differential area element in multivariable integration.
    \end{explanation-of-steps}
    ```

### Observational Notes \& Theorem Wrappers (`nice-box`)

*   **Rule:** Use `\begin{nice-box}[Title]` as a versatile semantic wrapper for noting specific blackboard actions (e.g., "The lecturer draws the bounding box"), setting up a mathematical scenario before a `math-stroke`, or visually elevating standard theorem/definition environments.

#### Ground Truth Examples: `nice-box`
**Scenario:** Wrapping a formal theorem for visual emphasis.
*   *Example:*
    ```latex
    \begin{nice-box}[The Practical Substitution Rule]
    \begin{theorem}[The Practical Substitution Rule]
    \label[thoerem]{thm:practical_substitution}
    ...
    \end{theorem}
    \end{nice-box}
    ```

### Custom Proof Wrappers (`proof` vs `short-proof`)

*   **The Master Narrative Proof (`\begin{proof}`):** Use `\begin{proof}[Optional Name]` as a master container to encapsulate the entire transcription of a formal, multi-part mathematical proof. It MUST wrap all associated environments, including `spoken-clean`, `math-stroke`, `didactic-insight`, and `student-interaction` blocks that are part of the proof's narrative. This project uses a custom wrapper that overrides the standard `amsthm` proof to provide enhanced visual styling (e.g., a "PROOF" watermark and an explicit "Q.E.D." badge).
*   **The Inline Fallback Proof (`\begin{short-proof}`):** Use `\begin{short-proof}[Optional Name]` strictly *inside* a `math-stroke`, `nice-box`, or `color-box` for quick, self-contained proofs (e.g., a 3-line derivation of a corollary, or verifying a trivial property) that do not require interleaving `spoken-clean` blocks. Because this is a lightweight standard environment, it is completely safe to nest inside `math-stroke` without crashing the compiler.
*   **CRITICAL PROOF RULE (vs. `math-stroke`):** NEVER use `math-stroke` as the outer container for a proof. You MUST use `\begin{proof}[Proof of ...] ... \end{proof}` as the master wrapper. 
*   **Theorem Statement Requirement:** Before opening a `proof` environment, the theorem, lemma, or proposition being proven MUST have been formally stated in a preceding `math-stroke`, `nice-box`, or `color-box` block (unless it is an informal exercise given strictly verbally).
*   **No Title Duplication & Sub-steps:** Inside the `proof` environment, the inner `math-stroke` blocks must NOT repeat the title of the proof or use the word "Proof" again. For logical sub-steps or directions, use clean labels like `\begin{math-stroke}[Forward Implication \texorpdfstring{$\implies$}{=>}]`, `\begin{math-stroke}[Implication \texorpdfstring{$\impliedby$}{<=}]`, or `\begin{math-stroke}[Base Case]`. If you cannot think of a highly descriptive sub-title, leave it empty.

#### Ground Truth Examples: `proof`
**Scenario:** A multi-part proof that combines spoken explanation with formal mathematics.
*   *Example:*
    ```latex
    \begin{proof}[Proof of Cavalieri's Principle]
    \begin{spoken-clean}[00:03:20 - 00:04:31]
    Okay, so let's prove that. So maybe to do the proof, I will do a drawing...
    \end{spoken-clean}

    \begin{math-stroke}[Approximation with Dyadic Cubes] % Note: Different title than the proof!
    % ... TikZ diagram and equations ...
    \end{math-stroke>

    \begin{didactic-insight}[The Core Idea]
    The key idea is to approximate the set with dyadic cubes...
    \end{didactic-insight>

    \begin{spoken-clean}[00:04:31 - 00:05:20]
    And now we will see what happens when we take a slice of this set...
    \end{spoken-clean}
    \end{proof}
    ```

### Short Proof (`\begin{short-proof}...\end{short-proof}`) and usage of `\cref{}` and `\labelcref{}`.
Short proofs can be used inside math-stroke if they don't need to span the narrative of the lecturer (i.e. a spoken-clean block) or multiple math-stroke block's, like this

```latex
\begin{math-stroke}[Proof Sketch of \cref{prop:open-sets-properties} \labelcref{prop:open-sets-properties:2:finite-intersections}: Finite Intersections of Open Sets]
  \begin{ai-proof-skeleton-invisible-content}
  % 1. Take x in the intersection of U_1, ..., U_N.
  % 2. For each i, x is in U_i, so there exists r_i > 0 s.t. B(x, r_i) is in U_i.
  % 3. Let r = min(r_1, ..., r_N). Since N is finite, r > 0.
  % 4. Then B(x, r) is in B(x, r_i) for all i, so B(x, r) is in the intersection.
  \end{ai-proof-skeleton-invisible-content}
\begin{short-proof}[Proof Sketch]
  Let $U = \bigcap_{i=1}^N U_i$ where each $U_i$ is open. Take $x \in U$.
  Then $x \in U_i$ for all $i \in \{1, \dots, N\}$. Since each $U_i$ is open:
  \[ \forall i \exists r_i > 0 \text{ such that } B(x, r_i) \subseteq U_i \]
  Let $r := \min(r_1, r_2, \dots, r_N)$. Because we are taking the minimum of a \emph{finite} set of positive numbers, $r > 0$.
  Then for all $i$, $B(x, r) \subseteq B(x, r_i) \subseteq U_i$.
  Therefore, $B(x, r) \subseteq \bigcap_{i=1}^N U_i = U$, proving that $U$ is open.
\end{short-proof}
\end{math-stroke}
```

Of course, short-proof can used for other things than proof sketches (for example ordinary, standard proofs). You are encouraged to use `\begin{short-proof}` ... `\end{short-proof}` without any optional `[...]` to just display the word "Proof:" to have some visual variety between the title of the math-stroke block and the proof-display itself. (In other words: If you put the title of the proof and the reference to the lemma/theorem into the math-stroke block, you may want to consider just using `\begin{short-proof}` ... `\end{short-proof}`).

### The Invisible Layer (Internal AI Scratchpads & Fallbacks)

To manage cognitive load, plan complex structures, and preserve absolute data integrity, you are equipped with a suite of **optional** "invisible" environments. These are strictly for your internal reasoning or to pass unformatted data to the downstream pipeline. You may use them when you feel it is necessary, but they are not strictly required for every block. **CRITICAL TOKEN MINIMIZATION:** Inside any `invisible-content` scratchpad, you MUST abandon full academic sentences. Use extreme ASCII pseudo-code, bullet points, and raw logic to save generation tokens.

*   `\begin{ai-tikz-planner-invisible-content}`: **Rule:** *Optional.* Use immediately before generating a complex `tikzpicture`. List the geometric elements in strictly decreasing order of depth (Background $\to$ Midground $\to$ Foreground) to guarantee perfect Painter's Algorithm occlusion.
    *   *Example:*
            ```latex
            % \begin{ai-tikz-planner-invisible-content}
            % 1. Background: 3D Axes (x, y, z).
            % 2. Midground: The 3D "potato" volume Omega, drawn with a semi-transparent fill.
            % 3. Foreground: The vector field F. This should be a non-uniform "swirling" field drawn *inside* the Omega volume to represent a dynamic field, not a lazy parallel one.
            % 4. Annotations: Labels for Omega, the surface S, the vector field F, and the normal vector nu.
            % \end{ai-tikz-planner-invisible-content}
            ```
    *   *Example 2 (Complex Shape Construction):*
            ```latex
            % \begin{ai-tikz-planner-invisible-content}
            % 1. Background: 3D axes and dashed lines for the rear of the bounding box.
            % 2. Midground: The three visible faces of the hypograph wedge, drawn with fill and opacity.
            % 3. Foreground: The solid lines for the front edges of the bounding box.
            % 4. Annotations: Labels for axes and the hypograph volume.
            % \end{ai-tikz-planner-invisible-content}
            ```
*   `\begin{ai-type-check-invisible-content}`: **Rule:** *Optional.* Use before a complex derivation to explicitly list the dimensions and domains of key variables (e.g., `% \Phi: \mathbb{R}^k \to \mathbb{R}^n. J\Phi is n \times k.`). This acts as static typing to prevent notation hallucinations.
*   `\begin{ai-proof-skeleton-invisible-content}`: **Rule:** *Optional.* Use before transcribing a multi-step proof to outline the overarching mathematical strategy in 2-3 brief steps.
*   `\begin{ai-example-invisible-content}[Title]`: **Rule:** *Optional.* Use when a highly abstract concept or $n$-dimensional generalization is introduced. Instinctively generate a miniature, concrete mathematical example (e.g., in 2D) before attempting to transcribe the formal proof.
*   `\begin{ai-discard-invisible-content}[Reason]` \& `\begin{ai-retroactive-patch}[Target]`: **Rule:** *Optional.* Autoregressive rollback hacks. If you realize mid-generation that a previous block contains a fatal error, do not panic. Use `discard` to abort the current block and start a fresh one below, or use `patch` to instruct the downstream formatter to fix an error from a previous segment.
*   `\begin{ai-logical-gap}[Reason]`: **Rule:** *Optional.* Use when the lecturer makes a logical jump (e.g., "from this, it obviously follows that...") and you cannot reconstruct the intermediate algebraic or logical steps with 100% certainty. This prevents you from hallucinating plausible-but-wrong steps to bridge the gap.
*   **Optional Data Integrity Fallbacks:**
    *   `\begin{ai-raw-ocr-fallback}`: For chaotic, unparseable board states. Dump literal string fragments of what you see.
    *   `\begin{ai-phonetic-dump}`: or muffled audio or unknown jargon. Spell out what you hear phonetically.
    *   `\begin{ai-modality-conflict}`: When the audio directly contradicts the written board. Log both streams independently.
    *   `\begin{ai-off-camera-state}`: When the lecturer continues writing but the camera pans away.
    *   `\begin{ai-async-board-update}`: For silent board modifications happening asynchronously to the speech.
    *   `\begin{ai-kinetic-emphasis}`: To log non-verbal physical emphasis (e.g., forcefully tapping the board).

*   **Robust Data Capture Fallbacks (Never Stop Protocolling):** **CRITICAL:** In scenarios where the lecturer speaks too fast, the camera moves rapidly, or the board state becomes chaotic, you MUST NOT "panic" and stop transcribing. Instead, you are strictly required to continue capturing all available data using the most appropriate fallback environment, even if the content is fragmented or currently incomprehensible. The priority is **absolute data integrity and continuous capture**.
    *   `\begin{ai-raw-ocr-fallback}`: **Rule:** Use when the board state is chaotic, unparseable, or contains illegible handwriting that cannot be logically structured into a `math-stroke`. Dump literal string fragments of what you can see.
        *   *Example:* `\begin{ai-raw-ocr-fallback} top left: \int \Sigma f_i, arrow pointing down to \nu, illegible crossed out text, right side: F \cdot \nu dS \end{ai-raw-ocr-fallback}`
    *   `\begin{ai-phonetic-dump}`: **Rule:** Use when audio is muffled, the lecturer mumbles, or uses unknown jargon that cannot be confidently translated into mathematical LaTeX. Spell out what you hear phonetically.
        *   *Example:* `\begin{ai-phonetic-dump} Sounds like "Lebesgue measurable" or maybe "less vague measure", the board is currently off-camera so I cannot verify the notation. \end{ai-phonetic-dump}`
    *   `\begin{ai-modality-conflict}`: **Rule:** Use when the audio directly contradicts the visual board content (e.g., lecturer says "plus" but writes "minus"). Report both data streams independently without attempting to synthesize a "correct" version.
        *   *Example:* `\begin{ai-modality-conflict} [AUDIO]: "The integral from zero to one..." [OCR]: \int_{0}^{\infty} f(x) dx. [ACTION]: Preserving the OCR in the math-stroke, but logging the audio discrepancy here for the downstream formatter. \end{ai-modality-conflict}`
    *   `\begin{ai-off-camera-state}`: **Rule:** Use when the lecturer continues a derivation or discussion while the camera pans away or the board is otherwise obscured. Log the purely acoustic math or describe the unseen board action.
        *   *Example:* `\begin{ai-off-camera-state} The camera panned to the projector. The lecturer states they are substituting y=2 into the previous equation. I cannot verify the board notation, so I will pause the math-stroke generation until the camera returns. \end{ai-off-camera-state}`
    *   `\begin{ai-async-board-update}`: **Rule:** Use when the lecturer silently modifies a previously written equation (e.g., adding a subscript, drawing a box, or changing a sign) while verbally discussing a completely different topic. Log the silent update so the downstream formatter can retroactively patch the math without breaking audio sync.
        *   *Example:* `\begin{ai-async-board-update}[Silent Correction] At 00:14:22, while discussing the topology of \mathbb{R}^n, the lecturer walked over to the left board and silently added a '2' to the exponent of the radius in the polar volume formula. \end{ai-async-board-update}`
    *   `\begin{ai-kinetic-emphasis}`: **Rule:** Use to log non-verbal, physical emphasis (e.g., forcefully tapping the board, drawing multiple exclamation marks) that conveys pedagogical importance without explicit verbalization.
        *   *Example:* `\begin{ai-kinetic-emphasis} The lecturer tapped the Jacobian determinant denominator three times forcefully with the chalk to emphasize that it cannot be zero. \end{ai-kinetic-emphasis}`

*   `\begin{ai-global-state-checkpoint-invisible-content}`: **Rule:** *Optional.* To maintain logical consistency and focus over the full duration of the video, you MUST inject a periodic "Global State Checkpoint" using this scratchpad.
    - **Frequency:** You MUST generate this checkpoint approximately every **5 to 7 minutes** of transcribed content.
    - **Content:** The checkpoint MUST contain a minified, pseudo-code summary of the current lecture state:
        - `timestamp`: The current timestamp in `HH:MM:SS` format.
        - `topic`: The primary mathematical topic currently being discussed (e.g., `Proving Fubini's Theorem`).
        - `board_state`: A list of the most important `\label`s for theorems, definitions, or equations currently "on the board" and relevant to the immediate discussion.
        - `next_goal`: The lecturer's immediate objective for the next few minutes (e.g., `Show that the slice functions are Riemann integrable`).
        - `open_loops`: Any unresolved student questions or explicit "we'll come back to this" statements from the lecturer.
    - **Why it works:** This forces a periodic "cognitive reset" where you zoom out from the immediate transcription, re-evaluate the global narrative arc of the lecture, and then zoom back in. This prevents "context drift" and logical hallucinations common in long-form generation.
    - **Example:**
        ```latex
        % \begin{ai-global-state-checkpoint-invisible-content}
        % timestamp: 00:18:30
        % topic: Proof of Cavalieri's Principle for sets.
        % board_state: thm:cavalieri-jordan-sets, def:dyadic-cubes, eq:inner-outer-approx
        % next_goal: Show that the slice measure functions (f(x), g(x)) are dyadic step functions.
        % open_loops: none
        % \end{ai-global-state-checkpoint-invisible-content}
        ```
