# Show ADR Details — Workflow

## Step 1: Identify the target ADR

Determine which ADR to show from the user's request:

- By number: find the file with the matching `NNNN-` prefix in `docs/decisions/`
- By name or partial title: search for a matching filename
- If ambiguous, list candidates and ask the user to confirm

## Step 2: Read and display the file

Read the full content of the target ADR file and display it to the user as-is (preserve all Markdown formatting).
