# Skillmonger Agent Skill

Instructions for AI assistants to prepare questions for human experts through [Skillmonger](https://skillmonger.ai), get the user's approval for payment, and retrieve the answer.

This repository contains the public instruction bundle, not the Skillmonger application source or expert answers. Using the skill does not connect an account or purchase an answer automatically.

## Install

With the [Skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add Skillmonger-ai/agent_skills --skill skillmonger
```

Select the agent you use when prompted. Alternatively, copy the complete `skills/skillmonger/` directory into your agent's supported skills directory, keeping the `references/` files alongside `SKILL.md`.

## Contents

- [SKILL.md](skills/skillmonger/SKILL.md): when to use Skillmonger and the request, approval, payment and answer workflow.
- [API reference](skills/skillmonger/references/api-reference.md): supported interfaces, authentication, lifecycle and examples.
- [Tags](skills/skillmonger/references/tags.md): expertise and jurisdiction tags.

## Connect or learn more

- [Setup guides](https://skillmonger.ai/plug-in)
- [Current agent playbook](https://skillmonger.ai/llms.txt)
- Hosted MCP endpoint: `https://api.skillmonger.ai/mcp` (OAuth)
- [Terms](https://skillmonger.ai/terms) and [disclaimers](https://skillmonger.ai/disclaimers)

Skillmonger is a paid external service. The user chooses the bid and reviews and pays on the approval page for the card workflow. Autonomous x402 payments require separately authorized wallet spending. Installing these instructions alone does not authorize submission or spending.

## Source of truth

This repository is a published mirror. Make changes in Skillmonger's canonical application repository, in `skill/skillmonger/` and the API documentation it references—not directly in this mirror. Publish the reviewed, tested export to keep the skill, agent playbook and service contracts aligned.

- Source commit: `98e507437262600b378668f323d49fc3dc7703c3`
- Skill version: `1.1.3`
- Published bundle: the three Markdown files listed above, copied without content changes.

Publication of this mirror does not imply a listing or approval in any agent marketplace.
