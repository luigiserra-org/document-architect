# Document Architect

A Claude Skill that implements a structured 5-phase process for creating complex professional documents. Instead of asking AI to write a full document in one shot, this skill guides a collaborative process: research, interview, assembly, drafting, and human review.

## The Problem

Most people ask AI to generate an entire document at once. The result is generic, shallow, and requires heavy editing. Complex documents — risk management plans, technical proposals, operational procedures, compliance reports — need domain research, specific data from the user, and iterative refinement.

## The Process

Document Architect forces a disciplined workflow:

| Phase | What happens | Who drives |
|-------|-------------|------------|
| **1. Research** | Claude searches the web for best practices, standards, and frameworks relevant to the document type | Claude (web search) |
| **2. Interview** | Claude asks the user structured questions in logical blocks to gather context-specific information | Interactive |
| **3. Assembly** | Claude consolidates all data, identifies gaps, and proposes simulated placeholders where needed | Claude + User |
| **4. Drafting** | Claude produces a structured first draft based on research + collected data | Claude |
| **5. Review** | User validates, corrects, and refines. Claude iterates until approved | User drives |

Claude stops at the end of every phase and waits for user confirmation before proceeding.

## Installation

### Claude.ai (Web & App)

This is the simplest method. Works for Pro, Max, Team, and Enterprise plans.

1. Download this repository as a ZIP file (Code → Download ZIP)
2. Inside the ZIP, ensure the folder structure is:
   ```
   document-architect/
   ├── SKILL.md
   └── README.md
   ```
3. Go to [claude.ai/settings/capabilities](https://claude.ai/settings/capabilities)
4. In the **Skills** section, click **Add Skill**
5. Upload the `document-architect` folder as a ZIP
6. Enable the skill

Claude will now automatically use this process when you ask it to create complex documents.

### Claude Code

For developers using Claude Code in the terminal.

**Personal skill (available across all projects):**

```bash
# Clone to your personal skills directory
git clone https://github.com/luigiserra-org/document-architect.git ~/.claude/skills/document-architect
```

**Project-specific skill:**

```bash
# Clone into your project's skills directory
cd your-project
git clone https://github.com/luigiserra-org/document-architect.git .claude/skills/document-architect
```

Claude Code will automatically discover and load the skill when relevant.

### Claude Desktop

Claude Desktop uses the same file-based skill discovery as Claude Code.

```bash
# Create the skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# Clone the skill
git clone https://github.com/luigiserra-org/document-architect.git ~/.claude/skills/document-architect
```

Restart Claude Desktop after installation.

### Claude API

If you're building with the Claude API and using the code execution tool, you can include the skill in your API requests. Refer to [Anthropic's API documentation](https://docs.claude.com) for details on skills integration.

### Manual (System Prompt / Project Instructions)

If none of the above methods work for your setup, you can copy the contents of `SKILL.md` directly into:

- **Claude.ai Projects:** Open a Project → Settings → paste the SKILL.md content into the project instructions
- **Custom system prompts:** Append the SKILL.md content to your system prompt
- **Any LLM:** The process described in SKILL.md is model-agnostic. You can adapt it for ChatGPT, Gemini, or other models by pasting the instructions into your system prompt or custom instructions

## Usage Examples

Once installed, simply ask Claude to create a complex document:

> "I need to create a business continuity plan for our manufacturing facility."

> "Help me write a risk management plan for flood risk in an industrial warehouse."

> "Guide me through creating a technical proposal for facility management services."

> "I need to produce an operational procedure manual but I don't know where to start."

Claude will automatically activate the 5-phase process, starting with web research.

## What It's Good For

- Risk management plans
- Business continuity plans
- Technical proposals and offers
- Operational procedure manuals
- Compliance documentation
- Safety assessment reports
- Facility management plans
- Any document that requires domain expertise and user-specific data

## What It's NOT For

- Quick emails or short texts
- Simple one-shot writing tasks
- Documents where you already have a complete outline and all the data

## Requirements

- **Claude Pro, Max, Team, or Enterprise plan** (for Claude.ai skill installation)
- **Code execution enabled** in Claude settings (for skill auto-loading)
- **Web search enabled** (Phase 1 requires web search for current best practices)

## Language

The skill adapts automatically to the user's language. Write in Italian, get responses in Italian. Write in English, get English. The SKILL.md is written in English for maximum portability.

## Contributing

Found a way to improve the process? Open an issue or submit a pull request. Particularly welcome:

- Improvements to the interview question structure
- Additional edge case handling
- Translations of the README (the SKILL.md should stay in English)

## License

MIT License. Use it, modify it, share it.

## Credits

Developed by Luigi Serra as part of the [AI Advanced Training Course](https://www.linkedin.com/in/luigiserra/) for business professionals.

Built on the [Agent Skills open standard](https://agentskills.io) by Anthropic.
