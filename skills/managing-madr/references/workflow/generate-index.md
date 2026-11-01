# Generate index.md — Workflow

## Purpose

Generates `index.md` for the ADR directory by scanning all existing ADR files and extracting their metadata.

This workflow is invoked in two ways:

- **Explicitly:** The user requests to generate, create, or initialize `index.md`.
- **From another workflow:** A workflow (e.g., `create-adr.md`, `update-adr-status.md`) determines that `index.md` is needed but does not exist.

## Step 1: Determine the ADR storage directory

- If called from another workflow, use the directory already established there.
- Otherwise, use the default `docs/decisions/` relative to the user's working directory, or ask the user if no preference has been stated.
- If the directory does not exist, report to the user and stop.

## Step 2: Check for existing index.md

- If `index.md` **does not exist**: proceed to Step 3.
- If `index.md` **exists**:
  - If called **from another workflow**: proceed without prompting (the calling workflow controls confirmation).
  - If called **explicitly by the user**: ask for confirmation before overwriting.
    - Confirmed → proceed to Step 3.
    - Declined → stop and report.

## Step 3: Read the index template

Read `references/spec/index-template.md` to understand the required format.

## Step 4: Scan for ADR files

- List all files matching `NNNN-*.md` in the ADR directory, excluding `index.md` itself.
- Sort by sequence number ascending.
- If no ADR files are found, proceed to Step 6 with an empty entry list (generate a header-only index).

## Step 5: Extract metadata from each ADR file

For each ADR file (in sequence number order), read the file and extract:

| Field | Source | Fallback |
|---|---|---|
| Number | 4-digit prefix from filename | — |
| Title | H1 heading (`# …`) | Filename without prefix and extension |
| Status | `status` field in YAML front matter | `unknown` |
| Summary | See extraction rules below | `—` |

**Summary extraction rules (apply in order, stop at first match):**

1. Locate the `## Decision Outcome` section.
2. Find a line matching `Chosen option: "…", because …` — extract the clause after `because` up to the first `.` or end of line.
3. If not found, use the first non-empty sentence in the `## Context and Problem Statement` section.
4. If still not found, use `—`.

## Step 6: Write index.md

Using the format from `references/spec/index-template.md`, write `index.md` with:

- The header row.
- One row per ADR, sorted by sequence number ascending.
- Each `Number` cell must be a Markdown link: `[NNNN](NNNN-filename.md)`.

## Step 7: Confirm to the user

- If called **explicitly**: report the written file path and the number of ADR entries included.
- If called **from another workflow**: return control to the calling workflow without additional confirmation output (the calling workflow handles its own summary).
