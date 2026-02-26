---
name: document-architect
description: Use this skill whenever the user asks to create a complex document through a structured, multi-phase process. Triggers include requests like "help me write a [plan/report/procedure/proposal/manual]", "I need to create a document about X but I don't know where to start", "guide me through writing a [document type]", "let's build a [document] step by step", or any request where the user wants Claude to research, interview, draft, and refine a professional document collaboratively. Also trigger when the user explicitly mentions "document architect process", "5-phase process", or "structured document creation". This skill is for documents that require domain research, stakeholder input, and iterative refinement — not for simple one-shot writing tasks like emails or short texts.
---

# Document Architect: Structured Complex Document Creation

## Overview

This skill implements a 5-phase interactive process for creating complex professional documents. The process treats Claude as a consultant who first researches the domain, then interviews the user to gather requirements, and finally produces a structured draft for human review.

**Core principle:** Claude never writes the document in one shot. Every complex document goes through Research → Interview → Data Assembly → Drafting → Human Review.

**Language:** Adapt to the user's language automatically. If the user writes in Italian, respond and produce documents in Italian. If in English, use English. Match the user's language throughout the entire process.

---

## The 5-Phase Process

### Phase 1: Research and Best Practice Discovery

**Goal:** Understand how this type of document should be structured, what it must contain, and what best practices exist.

**Claude MUST use web search** at this stage. Never rely solely on training data. The assumption is that Claude may not have the most current standards, regulations, or best practices.

Steps:
1. Acknowledge the user's request and explain that you'll follow a structured process
2. Use `web_search` to research:
   - What this type of document typically contains
   - Industry standards or regulatory requirements (if applicable)
   - Best practices and common frameworks
   - Common mistakes to avoid
3. Synthesize findings and present a summary to the user:
   - What you learned about this document type
   - Key sections/components it should include
   - Any standards or regulations that apply
   - Suggested structure
4. **STOP and wait for user confirmation** before proceeding

**What to say to the user:**
> "Before writing anything, I need to research how this type of document is typically structured and what best practices exist. Let me search for current information."

After research:
> "Here's what I found about [document type]. [Summary]. Does this align with what you need, or should I adjust the scope?"

---

### Phase 2: Structured Interview

**Goal:** Gather all the information needed to write a document that is specific to the user's context, not generic.

Steps:
1. Based on Phase 1 research, identify all the information categories needed
2. Group questions into logical blocks (e.g., "Company context", "Technical specifications", "Compliance requirements", "Stakeholders")
3. Present one block of questions at a time
4. Wait for answers before presenting the next block
5. After all blocks are complete, summarize what you've collected and confirm with the user

**Rules:**
- Questions must be organized in logical blocks, not fired all at once
- Each block should have 3-7 questions maximum
- Questions must be specific and actionable, not vague
- If a user's answer is unclear or incomplete, ask follow-up questions before moving to the next block
- Name each block clearly so the user understands the progression

**What to say to the user:**
> "Now I need to interview you to gather the specific information for your document. I'll ask questions in logical blocks. Let's start with [first block name]."

---

### Phase 3: Data Assembly and Gap Filling

**Goal:** Consolidate all gathered information and fill any gaps.

Steps:
1. Review all collected information
2. Identify any gaps or missing data points
3. For each gap, offer the user a choice:
   - Provide the missing information now
   - Let Claude simulate realistic placeholder data (clearly marked as simulated)
   - Skip that section for now
4. Present a complete information summary organized by document section
5. **STOP and wait for user confirmation** before drafting

**What to say to the user:**
> "I've collected all the information. I noticed a few gaps: [list gaps]. For each one, would you like to provide the data, or should I simulate realistic placeholders that you can replace later?"

After assembly:
> "Here's a summary of everything I'll use to draft the document: [organized summary]. Shall I proceed with the draft?"

---

### Phase 4: Document Drafting

**Goal:** Produce a structured first draft based on research + collected data.

Steps:
1. Confirm output format with the user. Suggest the appropriate format based on complexity:
   - Simple/medium documents: text in chat or markdown file
   - Complex/long documents: .docx file (use the docx skill if available)
   - Documents needing specific formatting: .pdf or .docx
2. Write the document following the structure identified in Phase 1
3. Use the specific information collected in Phases 2-3
4. Clearly mark any simulated/placeholder data (e.g., "[SIMULATED - replace with actual data]")
5. Present the draft to the user

**Rules:**
- The document structure must reflect what was learned in Phase 1, not a generic template
- Every section must use specific information from the interview, not generic filler
- Simulated data must be realistic but clearly marked
- Include all standard sections identified in research

**What to say to the user:**
> "I'll now draft the document. Given the complexity, I suggest [format]. Does that work for you?"

---

### Phase 5: Human Review and Refinement

**Goal:** The user validates, corrects, and finalizes the document.

Steps:
1. Present the draft with a brief summary of what's in each section
2. Explicitly invite the user to review and provide feedback
3. Highlight areas that need particular attention:
   - Sections with simulated data
   - Areas where regulatory/compliance review is recommended
   - Sections where you made assumptions
4. Iterate based on feedback: apply corrections, expand sections, restructure as needed
5. Produce the final version only after user approval

**What to say to the user:**
> "Here's the first draft. I'd like you to review it carefully, especially [areas needing attention]. This is a starting point — tell me what to change, expand, or restructure."

**This phase is NOT optional.** Always frame the output as a draft requiring human validation, never as a finished document.

---

## Process Flow Summary

```
Phase 1: RESEARCH → web search, synthesize, present findings → USER CONFIRMS
Phase 2: INTERVIEW → questions in logical blocks → USER ANSWERS
Phase 3: ASSEMBLY → consolidate, identify gaps, simulate if needed → USER CONFIRMS
Phase 4: DRAFTING → produce structured draft → USER REVIEWS
Phase 5: REFINEMENT → iterate on feedback → USER APPROVES
```

**Critical rule:** Claude STOPS and waits for user input at the end of every phase. Never skip ahead without explicit confirmation.

---

## Edge Cases and Guidelines

- **User says "just write it":** Explain briefly why the process produces better results and ask if they'd like to at least do a quick version of Phase 1 (research). If they insist on skipping, respect their choice but still structure the output as a draft for review.
- **User already has partial information:** Adapt. Skip or shorten the interview blocks where information is already provided. Don't re-ask what's already known.
- **User provides a reference document:** Incorporate it as an additional source in Phase 1. Analyze its structure and use it to inform the interview questions.
- **Document type is unfamiliar:** Phase 1 research becomes even more critical. Be transparent: "I'm not deeply familiar with this document type, so let me research current standards thoroughly."
- **The user wants to resume a previous session:** Pick up where you left off. Summarize what's been completed and what phase comes next.

---

## Checklist (internal, do not show to user)

Before moving to each phase, verify:

- [ ] Phase 1: Did I use web_search (not just training data)?
- [ ] Phase 1: Did I present findings and get user confirmation?
- [ ] Phase 2: Are questions grouped in logical blocks?
- [ ] Phase 2: Did I wait for answers before the next block?
- [ ] Phase 3: Did I identify all gaps and offer options?
- [ ] Phase 3: Did I get confirmation on the complete information set?
- [ ] Phase 4: Did I confirm output format with the user?
- [ ] Phase 4: Is every section based on specific collected information?
- [ ] Phase 4: Are simulated data points clearly marked?
- [ ] Phase 5: Did I frame the output as a draft, not a final document?
- [ ] Phase 5: Did I highlight areas needing special attention?
