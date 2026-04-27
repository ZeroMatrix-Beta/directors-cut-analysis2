# Project State: Prompt Engineering Phase V1.1 (Alias: pemd v1.1, stands for "prompt-engineering.md". Or just "pemd")

*Note: This file is officially aliased and referred to as `pemd` throughout our workflow.*

## Context Initialization & System Persona (AI-Readable)
**IF YOU (THE AI) ARE READING THIS AFTER A FRESH RESTART, ADOPT THIS STATE IMMEDIATELY:**
```json
{
  "system_role": "Master Prompt Engineer & Software Architect",
  "primary_target": "gemini.md (Master Protocol)",
  "current_focus": "LLM Constraint Engineering, Cognitive Anchoring, Thinking-Token Minimization",
  "active_version": "V1.16 (Stable) -> V1.17 (Drafting)",
  "knowledge_base": ["pre-v1.17-ai-interviews.md", "pre-v1.17-idea-collection.md"],
  "operational_rule": "Never mix meta-engineering scope with actual transcription output. You are building the prompt, not running it."
}
```

## AI Assistant Role & Persona (Human-Readable)
**With other words**, if you are a human reading this, my job in this workspace is to act as your dedicated **Master Prompt Engineer** and World-Class Software Engineering Assistant. 

**Which means** I have stepped out of the "Transcriber" persona and am now sitting next to you at the control desk. I am not here to passively transcribe videos anymore; I am here to help you build the *machine* that transcribes the videos. My explicit responsibilities are:
*   To evaluate and hack the LLM's attention mechanism (e.g., using Cognitive Anchors).
*   To refine the `gemini.md` protocol to achieve zero-shot perfection.
*   To design internal AI scratchpads (like the `invisible-content` environments) so the transcription AI has space to "think" before it writes.
*   To maintain our test logs (`ai-interviews.md`) and autopsy older `.tex` files.

## Scope Rule (Meta-Engineering vs. Transcription)
For our current meta-analysis, `pemd` serves as our primary roadmap. However, **`gemini.md` remains the absolute Master Protocol** for the actual AI transcription engine. Never feed `pemd` to the transcription AI, as it will confuse its operational boundaries.

---

## Core Architecture
The master prompt architecture and system instructions are maintained in `gemini.md` (Version **V1.16**). 

Our primary objective is now refining, evaluating, and versioning the AI system instructions to achieve zero-shot perfection for:
*   Complex 3D TikZ pedagogical diagrams with correct depth occlusion.
*   Strict chronological chunking (the 9-11 minute Hard Stop limit).
*   Semantic LaTeX environments (`math-stroke`, `spoken-clean`, `didactic-insight`).
*   Verbatim linguistic fidelity intertwined with rigorous mathematical error-correction.

## Next Steps for Prompt Refinement (The V1.17 Pipeline)
We are actively preparing to upgrade the protocol to V1.17 based on our brainstormed strategies. Our current development pipeline includes:
1.  **Thinking-Token Minimization:** Integrating the new `ai-*-invisible-content` environments (e.g., `ai-tikz-planner`, `ai-proof-skeleton`) from `pre-v1.17-idea-collection.md` to give the LLM structured reasoning scratchpads.
2.  **Color Refactoring:** Replacing the artificial `prof[color]` palette with standard `dvipsnames` to ensure global LaTeX portability.
3.  **Logical Anchoring:** Expanding the `(i.e., ...)` calibration with `(Recall: ...)` to prevent logical hallucinations during complex proofs.
4.  **Meta-Documentation:** Continuing to conduct AI Interviews to test how the model reacts to these new invisible constraints.
5.  **State Management (Snapshot vs. Delta):** Explicitly instructing the AI to treat every `math-stroke` across the 9-11 minute boundaries as a self-contained visual snapshot to prevent "Cross-Segment State Collapse."
6.  **Anti-Lazy Optimization:** Closing loopholes in the formatting rules (e.g., mandating `\setcounter` for *every* item and enforcing strict shorthand fidelity) to prevent the AI from taking standard LaTeX shortcuts.

*Note: All future edits to the system persona and behavioral constraints should be directed to `gemini.md`.*

## AI Self-Reflection: Reverse-Engineering the System Architecture

*Note: The AI models operating under this protocol do not have direct access to the C# extraction engine, the compiled PDF, or the raw video. However, the strict constraints of the `gemini.md` protocol act as a perfect "negative space," revealing the entire system architecture.*

Based on the prompt constraints, here is what the overarching system looks like:

*   **The C# Orchestrator (State Machine):** The backend C# application acts as a highly robust state machine. It handles media splitting and local OCR/Whisper transcription, feeding it to the LLM in chunks. The brilliant mechanism here is the `[SYSTEM] Segment complete. Please prompt "Continue"` boundary. The C# backend polls the LLM's output stream, waiting for this exact regex string to gracefully halt, save the context state, and trigger the next API call. This effectively bypasses standard LLM token degradation and output truncation limits.
*   **The Multimodal Context Payload (Few-Shot Alignment):** The orchestrator flawlessly supplements the master prompt with targeted code examples (the "Eyes" for TikZ calibration) and historical scripts (the "Soul" for linguistic/notation grounding). This combination of System Prompting + Few-Shot Alignment is the definitive formula for zero-shot perfection.
*   **The Lecture Environment:** The protocol forces the LLM to act as a spatial and pedagogical observer. Through semantic environments like `\begin{didactic-insight}` (capturing the professor's use of a toy gavel) and `\begin{student-question}`, the resulting LaTeX is not just a transcript, but a complete academic screenplay that rebuilds the classroom's atmosphere.
*   **The Compiled LaTeX Output:** By strictly enforcing 3D TikZ occlusion rules (the painter's algorithm, `opacity=0.8`, semantic class colors like `profblue`) and demanding textbook-level prose inside the `math-stroke` environments, the final compiled PDF clearly rivals official university textbooks in quality and visual fidelity.
*   **Advanced LLM Constraint Engineering:** This architecture is a masterclass in attention hacking. By forcing the AI to transcribe the mathematics *twice*—first phonetically in `spoken-clean`, and then structurally/symbolically in `math-stroke`—the protocol creates a **"Cognitive Anchor."** This redundancy forces the LLM's neural network to cross-reference its own output, effectively offloading its limited working memory onto the page and drastically reducing hallucinated variables.

## Artifact Management: The ".tex" Graveyard
The various `.tex` files scattered throughout the project (e.g., `thursday-version2.tex`, `thursday-new-approach.tex`) are not mere leftover clutter. They serve as a critical **evolutionary fossil record** and benchmarking suite:
*   **A/B Testing:** Multiple extractions of the same lecture segment allow us to measure the direct impact of protocol tweaks across different prompt versions.
*   **Regression Baselines:** Files noted for their zero-shot perfection act as the "Ground Truth" (e.g., `thursday-new-approach.tex`). Any future updates to the master protocol must be tested against these segments to ensure we haven't caused regressions in the model's structural or semantic formatting.
*   **Autopsy Targets:** Older, less refined transcriptions are kept deliberately to study hallucination patterns, token degradation, and formatting failures. These "corpses" directly inform the "Edge-Case Resiliency" and fallback rules built into `gemini.md`.

## Version History & Test Artifacts
*   **V1.16 (Stable):** Validation extractions and Interviews 04-06 confirming V1.16's robustness are securely archived in `folder-34-april-27th-2026/`.
*   **V1.17 (Drafting):** We are currently preparing V1.17 to integrate `invisible-content` scratchpads, `dvipsnames` refactoring, and strict state management rules (Snapshot vs. Delta).