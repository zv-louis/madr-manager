---
name: managing-madr
description: Skill for creating, listing, and managing MADR (Markdown Architectural Decision Records) following the Structured MADR v4.0.0 format. Handles creating new ADR files, reviewing and updating the status of existing ADRs, listing all ADRs, and summarizing the decision history for a specific topic. Triggered by instructions such as "write an ADR", "record an architectural decision", "add a MADR", or "summarize ADR history".
allowed-tools: Read, Write, Edit, Glob, Bash(ls *), Bash(mkdir -p **), Bash(mktemp -d *), Bash(rm -rf $TMPDIR/madr-session-*), Bash(rm -rf /tmp/madr-session-*)
---

# MADR Manager Skill

## Language Policy

Write all ADR content in the same language the user is conversing in.  
For example, If the conversation is in Japanese, write the ADR in Japanese, and if the conversation is in English, write the ADR in English.  

## Temporary File Policy

Use the OS temporary directory when intermediate file storage is needed (e.g., staging content before writing, processing large structures). This is optional — only use when the workflow genuinely benefits from it.

**Session directory:**

- Create once per session using: `mktemp -d -t madr-session-XXXXXX`
- Store the path in a variable and reuse it throughout the session.
- Use it exclusively for intermediate work; never write final ADR files there.

**Cleanup:**

- Remove the session directory when it is no longer needed: `rm -rf <session-dir>`
- If cleanup is skipped (e.g., unexpected exit), leftover directories under `$TMPDIR/madr-session-*` or `/tmp/madr-session-*` are safe to ignore.

## File Reading Policy

**Never read the same file twice. Before loading any reference file, check which files have already been read in this conversation and skip if already loaded.**

| Category | Pre-loading | Rule |
|---|---|---|
| `references/workflow/` | Prohibited | Load immediately before executing the corresponding operation |
| `references/spec/` | Allowed | Load when needed (pre-loading is acceptable) |

## Operations

Identify which operation the user is requesting, then read the corresponding reference file and follow its instructions.

### 1. Create a new ADR

**Trigger:** User asks to create / add / write an ADR, or to record an architectural decision.

→ Read `references/workflow/create-adr.md` and follow the workflow defined there.

### 2. List existing ADRs

**Trigger:** User asks to list, show, or view existing ADRs.

→ Read `references/workflow/list-adrs.md` and follow the workflow defined there.

### 3. Update ADR status

**Trigger:** User asks to update, change status, deprecate, supersede, or reject an existing ADR.

→ Read `references/workflow/update-adr-status.md` and follow the workflow defined there.

### 4. Show ADR details

**Trigger:** User asks to show, read, or view a specific ADR.

→ Read `references/workflow/show-adr.md` and follow the workflow defined there.

### 5. Summarize ADR history for a topic

**Trigger:** Any of the following:

- User asks to summarize, review, or trace the decision history for a specific topic or keyword (e.g., "summarize ADR history for authentication", "how has the database decision evolved?")
- User asks about a specific topic and wants to reference or look up ADRs (e.g., "tell me about X by looking up the ADRs", "what does the ADR say about X", "tell me about current rules based on ADRs")
- User asks about current rules, policies, or decisions on a topic in a context where ADRs are the source of truth

→ Read `references/workflow/summarize-adr-history.md` and follow the workflow defined there.

### 6. Generate index.md

**Trigger:** User explicitly asks to generate, create, or initialize `index.md`. Also invoked from other workflows when `index.md` is required but does not exist.

→ Read `references/workflow/generate-index.md` and follow the workflow defined there.

## Resources

| File | When to read |
|---|---|
| `references/workflow/create-adr.md` | When creating a new ADR |
| `references/workflow/list-adrs.md` | When listing existing ADRs |
| `references/workflow/update-adr-status.md` | When updating ADR status |
| `references/workflow/show-adr.md` | When showing a specific ADR |
| `references/workflow/summarize-adr-history.md` | When summarizing decision history for a topic |
| `references/workflow/generate-index.md` | When generating or initializing index.md |
| `references/spec/adr-template.md` | When writing an ADR file (referenced from create-adr.md) |
| `references/spec/index-template.md` | When generating index.md (referenced from create-adr.md and generate-index.md) |
| `references/spec/structured-madr-spec.md` | When you need authoritative MADR format details |
