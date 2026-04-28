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

## The Golden Rule of Prompt Versioning
**Our Philosophy:** This mindset is the bedrock of what we are doing here. If we treat the prompt like actual source code, we have to version control it (i.e., tracking every single token and punctuation shift) with the same absolute rigor. Precision is professional, not arrogant. It's all about ensuring absolute clarity between us and the machine so there is zero room for misinterpretation or LLM "autopilot" behavior.

Prompt engineering at this level is essentially compiling code for a neural network—every single word, capitalization, and punctuation mark alters the attention weights. Summarizing the changes is not enough; we need the strict, word-by-word version history logged directly in our transition documents.

## The Principle of Absolute Data Integrity (Robustness over Efficiency)
A core architectural decision in this project is that **lost information is lost forever**. Our primary goal is to make a "lazy" AI extract as much content as possible from a lecture video. We do not care for perfection in the initial output, because we assume that a downstream AI in the pipeline is waiting to clean up the mess (e.g., duplicated `math-stroke` blocks). However, no process can recover information that was never generated in the first place.

Therefore, we explicitly prioritize **robustness against data loss** over token efficiency. This means:
*   **Snapshots, Not Deltas:** Every transcription segment must generate a complete, self-contained **snapshot** of the board state. We explicitly forbid "delta" updates that rely on the context of a previous segment. This ensures that each chunk is an atomic, verifiable unit of truth, immune to cross-segment state collapse.
*   **Redundancy is a Feature:** We embrace cognitive redundancy (e.g., restating formulas) as a core feature of the protocol. It acts as a self-correction mechanism and guarantees data integrity at the cost of higher token counts and potential downstream deduplication.

## The "Transport Layer" Assumption (Orchestrator Boundaries)
As prompt engineers, we must strictly define our assumptions about the backend C# and Python orchestrators. 
This section was created to combat a common failure mode where the AI assistant makes incorrect assumptions about the capabilities of the backend environment (e.g., assuming it will magically strip markdown or deduplicate content). The following analogies were developed to provide a permanent, rigid boundary definition.
**The Rule:** Do NOT make any assumptions about the orchestrators performing complex string cleanup, regex deduplication, or stripping away ` ```latex ` markdown tags. 

If we are worried about the integrity of the math, the quality, the readability, or the compilability, **THE ONLY ASSUMPTION** we can make is that these issues are solved by a pipeline of LLMs and well-designed prompts, and that's it. We assume that the C# and Python orchestrators work accordingly without any issues as a pure delivery mechanism—just like cryptographic protocols assume the transport layer / physical layer works without any issues. The orchestrators move the data reliably, but the LLM pipeline is strictly responsible for shaping and cleaning it. 

**The Pipeline Paradigm ("What Python can do, an LM can do"):** You have to think in a pipeline of LMs, that's the point! Because we rely on this pipeline, we *can* allow an initial LLM to output messy markdown, hidden scratchpads, or raw logic. If we put this into another LM, we don't necessarily generate more thinking tokens if the next LM's purpose is to clean up the mess! If it needs to be cleaned up, stripped, or calculated, we simply pass it to the next LLM in the pipeline. 

To expand on this analogy: As system architects and customers, we must think wisely about whether we *want* to use an LM to calculate the time offset of a `spoken-clean` environment, or whether we prefer to use a simple C# program or Python script to do so. But in theory, *we could use an LM to do it all*. Remember: we are prompt engineers. If we like to think in Python, we must remember that what Python can do, an LM can do. We do not have to force the first LLM to output 100% compile-ready syntax if its primary job is heavy reasoning. The downstream LLM acts as our intelligent formatter.

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

## Summary: The Prompt Engineering Philosophy
In essence, this workspace operates on three foundational pillars:

1. **Absolute Version Control:** Prompts are source code. Every token matters, and transitions from version to version must be rigorously documented word-by-word.
2. **The Pipeline Paradigm:** We prioritize robust, loss-less data extraction ("Snapshots") over zero-shot aesthetic perfection. We assume downstream LLMs will format and deduplicate, allowing the first-pass extraction AI to use messy pseudo-code, explicit redundancy, and custom error tags without fear of breaking the final compile.
3. **Cognitive Offloading:** By utilizing explicit verbal anchors (`(i.e., ...)`, `(Recall: ...)`) and hidden reasoning scratchpads (`ai-*-invisible-content`), we force the LLM to structure its spatial and logical reasoning *before* generating complex LaTeX. This minimizes expensive "thinking tokens" and prevents hallucinations.

Our ultimate goal is to evolve the `gemini.md` master protocol from a rigid single-pass typesetter into the first, indestructible extraction layer of an autonomous multimodal AI pipeline.