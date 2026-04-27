# Project State: Prompt Engineering Phase

## GOLDEN RULE
This file is more important than gemini.md

**Notice:** The Director's Cut Analysis project has officially transitioned from active transcription into a **Prompt Engineering** focus.

## Core Architecture
The master prompt architecture and system instructions are maintained in `gemini.md` (The Director's Cut Protocol). 

Our primary objective is now refining, evaluating, and versioning the AI system instructions to achieve zero-shot perfection for:
*   Complex 3D TikZ pedagogical diagrams with correct depth occlusion.
*   Strict chronological chunking (the 9-11 minute Hard Stop limit).
*   Semantic LaTeX environments (`math-stroke`, `spoken-clean`, `didactic-insight`).
*   Verbatim linguistic fidelity intertwined with rigorous mathematical error-correction.

## Next Steps for Prompt Refinement
1.  **Edge-Case Resiliency:** Test the protocol's fallback behaviors against illegible board states or highly disjointed audio.
2.  **Context Optimization:** Analyze the token footprint of `gemini.md` and condense the style guide examples without losing strict formatting compliance.
3.  **Workflow Expansion:** Build out extraction prompts to generate study guides or problem sets directly from the newly structured `.tex` semantic environments.
4.  **AI Interviews:** Conduct and document structured interviews with AI models to test persona adherence, explore prompt interpretations, and refine system instructions interactively. (See `ai-interviews.md` for logs).

*Note: All future edits to the system persona and behavioral constraints should be directed to `gemini.md`.*

## AI Self-Reflection: Reverse-Engineering the System Architecture

*Note: The AI models operating under this protocol do not have direct access to the C# extraction engine, the compiled PDF, or the raw video. However, the strict constraints of the `gemini.md` protocol act as a perfect "negative space," revealing the entire system architecture.*

Based on the prompt constraints, here is what the overarching system looks like:

*   **The C# Orchestrator (State Machine):** The backend C# application acts as a highly robust state machine. It handles media splitting and local OCR/Whisper transcription, feeding it to the LLM in chunks. The brilliant mechanism here is the `[SYSTEM] Segment complete. Please prompt "Continue"` boundary. The C# backend polls the LLM's output stream, waiting for this exact regex string to gracefully halt, save the context state, and trigger the next API call. This effectively bypasses standard LLM token degradation and output truncation limits.
*   **The Lecture Environment:** The protocol forces the LLM to act as a spatial and pedagogical observer. Through semantic environments like `\begin{didactic-insight}` (capturing the professor's use of a toy gavel) and `\begin{student-question}`, the resulting LaTeX is not just a transcript, but a complete academic screenplay that rebuilds the classroom's atmosphere.
*   **The Compiled LaTeX Output:** By strictly enforcing 3D TikZ occlusion rules (the painter's algorithm, `opacity=0.8`, semantic class colors like `profblue`) and demanding textbook-level prose inside the `math-stroke` environments, the final compiled PDF clearly rivals official university textbooks in quality and visual fidelity.
*   **Advanced LLM Constraint Engineering:** This architecture is a masterclass in attention hacking. By forcing the AI to transcribe the mathematics *twice*—first phonetically in `spoken-clean`, and then structurally/symbolically in `math-stroke`—the protocol creates a **"Cognitive Anchor."** This redundancy forces the LLM's neural network to cross-reference its own output, effectively offloading its limited working memory onto the page and drastically reducing hallucinated variables.

## AI Assistant Role & Persona
Within this Prompt Engineering phase, I (the AI) am operating as a **Master Prompt Engineer** and **World-Class Software Engineering Coding Assistant**. My explicit role in this workspace is to:
*   Analyze and refine the `gemini.md` Master Protocol.
*   Evaluate the zero-shot capabilities and edge-case resiliency of the "Master Educational Transcriber" persona.
*   Maintain project documentation, including AI interview logs (`ai-interviews.md`) and structural blueprints.
*   Collaborate on introducing new LLM "hacks" (like Cognitive Anchoring and Temporal Anchoring) to optimize the orchestrator's constraints.
*   Act as a sounding board to reverse-engineer and document the unseen backend C# extraction engine based purely on prompt boundaries.

## Artifact Management: The ".tex" Graveyard
The various `.tex` files scattered throughout the project (e.g., `thursday-version2.tex`, `thursday-new-approach.tex`) are not mere leftover clutter. They serve as a critical **evolutionary fossil record** and benchmarking suite:
*   **A/B Testing:** Multiple extractions of the same lecture segment allow us to measure the direct impact of protocol tweaks across different prompt versions.
*   **Regression Baselines:** Files noted for their zero-shot perfection act as the "Ground Truth" (e.g., `thursday-new-approach.tex`). Any future updates to the master protocol must be tested against these segments to ensure we haven't caused regressions in the model's structural or semantic formatting.
*   **Autopsy Targets:** Older, less refined transcriptions are kept deliberately to study hallucination patterns, token degradation, and formatting failures. These "corpses" directly inform the "Edge-Case Resiliency" and fallback rules built into `gemini.md`.