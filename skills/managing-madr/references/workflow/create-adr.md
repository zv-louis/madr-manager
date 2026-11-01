# Create a New ADR — Workflow

## Premise: One ADR per Decision

Each ADR records exactly one decision. If a single conversation or meeting note contains multiple independent decisions, split them into separate ADRs — one per decision. Creation order does not matter; relationships between decisions are expressed through mutual links.

## Step 1: Read the MADR spec

Read `references/spec/structured-madr-spec.md` to understand the full format before proceeding.

## Step 2: Determine the ADR storage directory

- If the user has explicitly stated a directory in their request, use that.
- Otherwise, ask the user where to store the ADR before proceeding. Suggest `docs/decisions/` (relative to the working directory) as the default option.

## Step 3: Determine the next sequence number

- List existing files matching `NNNN-*.md` in the target directory.
- Use the next available 4-digit number (e.g., `0001`, `0002`, …).
- If no files exist, start from `0001`.

## Step 3.5: Read the index file

- Check if `index.md` exists in the ADR storage directory.
- If it exists, read it to understand existing decisions and their statuses.
- If it does not exist, skip this step (it will be created in Step 8).

See `references/spec/index-template.md` for the index format.

## Step 4: Gather required information

Collect the following via conversation or `AskUserQuestion`:

| Field | Required | Notes |
|---|---|---|
| Title | Yes | Short phrase describing problem + solution. Pattern: `Use [X] for [Y]` |
| Context and Problem Statement | Yes | Background and why this decision is needed |
| Considered Options | Yes | All options actually evaluated (≥2 items) |
| Decision Outcome (chosen option + reason) | Yes | Which option was chosen and why |
| Status | Yes | Default: `accepted`. Values: `proposed`, `accepted`, `rejected`, `deprecated`, `superseded by ADR-XXXX` |
| Date | Yes | Default: today's date (YYYY-MM-DD) — use without asking |
| Decision Makers | Optional | Who made the decision |
| Consulted | Optional | Who was consulted (two-way communication) |
| Informed | Optional | Who is informed (one-way communication) |
| Decision Drivers | Optional | Constraints and quality requirements |
| Consequences | Optional (recommended) | Good/Bad/Neutral impacts of the chosen option |
| Confirmation | Optional (recommended) | How to verify the decision is implemented correctly |
| Pros and Cons of the Options | Optional | Detailed comparison per option |
| More Information | Optional | Links, related ADRs, review dates |

**Gathering guidelines:**

- Ask for title and problem statement first.
- Then ask for considered options and the chosen outcome.
- Offer to fill in optional sections (Consequences, Confirmation, Pros and Cons) — suggest including them, as they improve record quality.
- If the user doesn't specify a date, use today's date without asking.

## Step 5: Generate the filename

Pattern: `NNNN-title-with-dashes.md`

- Convert the title to lowercase.
- Replace spaces with hyphens.
- Remove or replace special characters with hyphens.
- Example: "Use PostgreSQL for relational data" → `0001-use-postgresql-for-relational-data.md`

## Step 6: Create the directory if needed

Create the directory determined in Step 2 if it does not already exist:

```bash
mkdir -p <target-directory>
```

## Step 6.5: Identify supersede relationships

Using the index read in Step 3.5, determine if any existing ADR is superseded by the new one.

**Process:**

1. Compare the new ADR's decision content against the Summary of each `accepted` ADR in the index.
2. If a clear supersede candidate is found (one match, high confidence): proceed with automatic update in Step 8.
3. If multiple candidates or low confidence: ask the user to confirm which ADR(s) are superseded before proceeding.
4. If no candidates or index does not exist: skip supersede processing.

## Step 7: Write the ADR file

Read `references/spec/adr-template.md` to get the template and template rules, then write the file.

- Omit optional sections if no content was provided — never leave placeholder text.
- If a superseded ADR was identified in Step 6.5, add a `More Information` section containing: `Supersedes [ADR-XXXX](XXXX-filename.md)`

## Step 8: Update related files

### 8a. Update the superseded ADR (if applicable)

If a supersede relationship was identified in Step 6.5, apply the following changes to the old ADR:

1. Update the `Status` field:

   ```markdown
   superseded by [ADR-XXXX](XXXX-filename.md)
   ```

2. Add a `More Information` section containing `Superseded by [ADR-XXXX](XXXX-filename.md)`. If the section already exists, append the entry.

### 8b. Update index.md

Read `references/spec/index-template.md` for the format.

- If `index.md` **does not exist**, follow the `generate-index.md` workflow to create it. Because the new ADR file was already written in Step 7, it will be included automatically during the scan. Skip the remaining instructions in this step and return here after `generate-index.md` completes.
- If `index.md` **exists**:
  - Add a new row for the newly created ADR. The `Number` cell must be a link to the ADR file: `[NNNN](NNNN-filename.md)`. Generate the `Summary` as a concise one-sentence description of the decision made.
  - If an existing ADR was superseded, update its `Status` cell accordingly.

### 8c. Confirm to the user

Show the created file path, a brief content summary, and any files that were updated (superseded ADR, index.md).
