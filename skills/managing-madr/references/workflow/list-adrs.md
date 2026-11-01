# List Existing ADRs — Workflow

## Step 1: Determine the ADR directory

- Default: `docs/decisions/` relative to the user's working directory.
- If the user specified a different directory, use that.

## Step 2: Find ADR files

List files matching `NNNN-*.md` in the target directory.

If no files are found, inform the user that no ADRs exist yet and suggest creating one.

## Step 3: Extract metadata from each file

For each ADR file, read the file and extract:

- **Number**: from the filename prefix (`NNNN`)
- **Filename**: the full filename
- **Title**: the H1 heading (`# ...`)
- **Status**: from YAML front matter (`status:` field); display `—` if not present
- **Date**: from YAML front matter (`date:` field); display `—` if not present

## Step 4: Display the result

Present a Markdown table sorted by number (ascending):

```
| # | File | Title | Status | Date |
|---|------|-------|--------|------|
| 1 | 0001-use-postgresql.md | Use PostgreSQL for relational data | accepted | 2025-01-15 |
| 2 | 0002-use-graphql-api.md | Use GraphQL API for client communication | proposed | 2025-02-01 |
```

If any file is missing YAML front matter, show `—` for missing fields without erroring.
