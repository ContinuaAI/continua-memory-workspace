# Continua Memory & Workspace

Persistent memory and a shared, versioned workspace for **developers, data teams, and AI engineers**, built on Continua Project Rooms.

Chat windows forget. A Project Room keeps the record, the work, and the memory in one place: every file is an immutable version you can restore, documents compile to line-numbered Markdown so answers cite file and line, and research reads every ready file with up to **1B tokens of working context** instead of a few retrieved chunks. These skills turn that into memory Claude can recall across sessions, a workspace agents can hand work through, and grounded references for docs, data, and evals.

## Requirements

This plugin depends on the **Continua Project Room** MCP server and requires a Continua account. Access and usage follow your account permissions and plan.

```text
https://app.askcontinua.com/mcp?subset=mini
```

The plugin declares this server in [`.mcp.json`](.mcp.json) (Streamable HTTP with OAuth), so installing the plugin adds the connection:

1. Install the plugin. In Claude Code:

   ```text
   /plugin marketplace add ContinuaAI/continua-memory-workspace
   /plugin install continua-memory-workspace@continua-memory-workspace
   ```

2. Sign in to **Continua Project Room**: in Claude, **Settings → Connectors**; in Claude Code, run `/mcp` and select `continua-project-room`. Users who already added Continua Project Room from the [Claude directory](https://claude.ai/directory/continua) are connected to the same server.

The skills call these Continua Project Room tools: `list_project_rooms`, `get_project_room`, `create_project_room`, `update_project_room`, `upload_project_files`, `search_project_room`, `read_project_file`, `research_project_room`, `get_project_research`, `list_project_file_history`, `restore_project_file`.

## Skills

| Skill | For | What it does |
| --- | --- | --- |
| [`/continua-memory-workspace:project-memory`](skills/project-memory/SKILL.md) | Developers and teams using Claude across sessions | Recall and save decisions (ADRs), conventions, gotchas, and session summaries as versioned files shared by every agent and teammate |
| [`/continua-memory-workspace:agent-handoff`](skills/agent-handoff/SKILL.md) | Long-running and multi-agent work | Goal, plan, and append-only log in a Room; one step per round, deterministic task keys, conflict checks before writes, resume after interruption |
| [`/continua-memory-workspace:corpus-research`](skills/corpus-research/SKILL.md) | API docs, RFCs, specs, wikis, logs, papers | Fast search for single facts; full-corpus research with file and line citations for questions that need every document |
| [`/continua-memory-workspace:data-dictionary`](skills/data-dictionary/SKILL.md) | Data and analytics engineers, text-to-SQL agents | Cited table, column, and metric definitions from schemas, dbt, SQL, and docs, with conflicting definitions kept visible until an owner decides |
| [`/continua-memory-workspace:grounded-evals`](skills/grounded-evals/SKILL.md) | AI engineers testing RAG and agents | Golden Q&A with verbatim evidence and line citations, including multi-document, conflicting, and unanswerable cases, exported as JSONL |

## Example requests

- "Remember that we pin Node 22 and use pnpm workspaces for this repo."
- "What did we decide about rate limiting last month, and why?"
- "Split this migration into steps in a work Room so another agent can pick it up tomorrow."
- "Load the v2 and v3 API docs and list every breaking change with citations."
- "Build a data dictionary from our dbt project and flag metrics defined differently across dashboards."
- "Create a 50-question eval set for our support-docs RAG bot, including unanswerable questions."

## How it fits together

- **Memory** lives as small Markdown files updated in place; a same-name upload creates a new version, and history stays restorable.
- **Work** happens in a Room that agents read, log to, and write back to, so any agent or person can resume it.
- **Answers** come from search for quick lookups and from research when completeness matters; citations map to `findings.json` and can be opened at the exact lines.

## Layout

```text
continua-memory-workspace/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── .mcp.json              # Continua Project Room MCP (subset=mini)
├── skills/
│   ├── project-memory/SKILL.md
│   ├── agent-handoff/SKILL.md
│   ├── corpus-research/SKILL.md
│   ├── data-dictionary/SKILL.md
│   └── grounded-evals/SKILL.md
├── .gitignore
├── LICENSE
└── README.md
```

## Local development

```bash
claude --plugin-dir .
claude plugin validate .
```

## Support and policies

- Product and documentation: [Continua Project Room](https://askcontinua.com/project-room) · [Help Center](https://askcontinua.com/help#projects)
- Support: [askcontinua.com/help](https://askcontinua.com/help)
- [Privacy policy](https://app.askcontinua.com/privacy-policy) · [Terms of service](https://app.askcontinua.com/terms-of-service) · [Trust & Security](https://askcontinua.com/trust)

Do not store secrets, credentials, or personal data in Rooms. Review cited evidence before acting on results.

[Continua AI](https://askcontinua.com)
