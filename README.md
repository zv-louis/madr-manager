# madr-manager

English | [日本語](README_ja.md)

A Claude Code plugin for interactively creating and managing MADR (Markdown Architectural Decision Records) in the Structured MADR v4.0.0 format.

## Overview

This plugin helps you record design and operational decisions as ADR (Architectural Decision Records) files through a guided conversation.
You can create ADRs from meeting notes or conversation logs, list existing ADRs, update their statuses, and review their contents and the history behind a decision.

**Supported operations:**

- **Create**: Create a new ADR interactively. Each ADR records exactly one decision; meeting notes that contain several decisions are split into separate ADRs
- **List**: List all existing ADRs in the project
- **Update status**: Change the status of an existing ADR (proposed / accepted / rejected / deprecated / superseded)
- **View**: Display the full content of a specific ADR
- **Generate index.md**: Generate an index file (index.md) that summarizes all ADRs in the project
- **Summarize history**: Follow links from index.md to related ADRs and summarize the discussion and reasoning behind a decision

**Behavior rules:**

- ADRs are never deleted. Rejected and deprecated ADRs are kept as historical records.
- When a new ADR replaces an existing one, the old ADR's status is updated to `superseded by ADR-XXXX`, and both ADRs get a link to each other.
- index.md is updated whenever an ADR is created or its status changes. If index.md does not exist, it is generated.
- ADR content is written in the same language as the conversation.

## Project Structure

```txt
madr-manager/
├── .claude-plugin/
│   └── plugin.json                  # Plugin manifest
├── skills/
│   └── managing-madr/
│       ├── SKILL.md                 # Skill definition (operation routing and file-reading rules)
│       └── references/
│           ├── spec/
│           │   ├── adr-template.md      # ADR template
│           │   ├── index-template.md    # index.md template
│           │   └── structured-madr-spec.md  # Structured MADR v4.0.0 specification
│           └── workflow/
│               ├── create-adr.md        # Create ADR workflow
│               ├── generate-index.md    # Generate index.md workflow
│               ├── list-adrs.md         # List ADRs workflow
│               ├── show-adr.md          # Show ADR workflow
│               ├── summarize-adr-history.md  # Summarize ADR history workflow
│               └── update-adr-status.md # Update ADR status workflow
├── AGENTS.md                        # Basic information for agents (in Japanese)
├── LICENSE
├── README.md
└── README_ja.md
```

## Installation

Install madr-manager from the marketplace.

```bash
# Install the plugin
/plugin install madr-manager@zv-plugins
```

## Usage

Once installed, just ask Claude Code to manage your ADRs.

```markdown
# Create a new ADR
Create an ADR from {meeting notes, etc.}
I want to record an architectural decision
I want to document the reason for adopting PostgreSQL in an ADR

# List existing ADRs
List existing ADRs

# Update ADR status
Update ADR-0002 to deprecated
ADR-0003 has been superseded by ADR-0005

# View an ADR
Show ADR-0001

# Generate index.md
Generate the ADR index file (index.md)

# Summarize decision history
Tell me about the discussion and reasoning behind the current state of X
Based on the ADRs, explain how we got to this decision
```

ADR files are named `NNNN-title-with-dashes.md`, where NNNN is a 4-digit sequence number.
If you do not specify a storage location, you are asked where to store the ADR when creating one, and `{ProjectRoot}/docs/decisions/` is suggested as the default.
For listing ADRs and reviewing history, `docs/decisions/` is searched unless you say otherwise.
If you manage ADRs in a different directory, specify that location.
To avoid specifying it every time, we recommend writing the ADR storage location in CLAUDE.md, AGENTS.md, or a similar file.

## MADR Format

ADR files follow the [Structured MADR v4.0.0](https://adr.github.io/madr/) format.

## License

MIT
