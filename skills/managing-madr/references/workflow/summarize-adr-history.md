# Summarize ADR History Workflow

Summarize the decision-making history for a specific TOPIC by starting from the index, then exploring linked ADRs.

## Steps

### Step 1: Confirm the TOPIC

If the user has not specified a TOPIC, ask:

> Which topic would you like to review the decision history for?

If specified, proceed with that topic.

### Step 2: Locate the ADR directory

Check in order:

1. `docs/decisions/`
2. If still not found, ask the user

### Step 3: Read index.md and select candidates

Read `index.md` in the ADR directory.

From the index table (Number / Title / Status / Summary), identify rows where the **Title** or **Summary** contains keywords related to the TOPIC. Use broad matching — partial matches and synonyms count.

Build an initial **candidate set** of ADR numbers (e.g., `{0003, 0007, 0012}`).

If `index.md` does not exist, fall back to listing all `NNNN-*.md` files and treating all of them as candidates.

If the candidate set is empty after scanning the index, report that no ADRs appear related to the TOPIC and suggest the user try broader keywords. Stop.

### Step 4: Read candidate ADR files and expand via links

Maintain two sets:

- **to-read**: ADR numbers queued for reading (start with the candidates from Step 3)
- **visited**: ADR numbers already read (start empty)

Process the **to-read** set iteratively:

1. Pick the next unvisited ADR number from **to-read**.
2. Read the ADR file.
3. Mark it as **visited**.
4. Extract the following for use in Step 5:
   - YAML front matter: `status`, `date`
   - H1 heading (Title)
   - Key sentence(s) from `## Decision Outcome`
   - Brief note from `### Consequences` (if present)
5. **Discover new links** by scanning the file for:
   - `## More Information` section — extract any ADR file links (e.g., `[ADR-0005](0005-*.md)`)
   - `Supersedes` references — in YAML front matter, section heading, or inline text (e.g., "Supersedes ADR-0002", `superseded by ADR-XXXX`)
   - `status` field containing "superseded by ADR-XXXX" — extract the referenced number
6. Add any newly discovered ADR numbers to **to-read** if not already in **visited**.

Repeat until **to-read** is empty.

### Step 5: Sort chronologically

Sort all **visited** ADRs by:

1. Primary: `date` field in YAML front matter (ascending)
2. Secondary: ADR number (ascending, as fallback when date is absent)

### Step 6: Present the summary

Output the following structure:

---

## Decision History: {TOPIC}

**Period:** {earliest date} → {latest date}
**Related ADRs:** {count}

---

### Timeline

#### {Date} — ADR-{NNNN}: {Title} `{status}`

**Decision:** {one-sentence summary of the decision outcome}

**Impact:** {brief note on consequences, or omit if none recorded}

> *Supersedes [ADR-XXXX](XXXX-filename.md)* — if applicable
> *Superseded by [ADR-XXXX](XXXX-filename.md)* — if applicable

---

<!-- repeat for each ADR in chronological order -->

---

### Summary

Write a 2–5 sentence narrative that:

- Describes how the decisions evolved over time
- Highlights key turning points (adoption, replacement, deprecation)
- Identifies the current accepted approach (most recent non-superseded ADR)

---

## Response Guidelines

Adapt the response depth and structure to the user's intent:

### When the user asks about current rules or policies

Lead with the **current accepted state** before presenting the full timeline:

1. **Current rule** — State the decision from the most recent non-superseded ADR upfront, using a summary table or bullet list for clarity.
2. **Decision history** — Follow with the timeline to show how the rule evolved.
3. **Narrative summary** — Close with a brief explanation of why the current rule was reached.

Example opening structure:

```markdown
## Current Rules: {TOPIC}

| ... | ... |
| ... | ... |

---

## Decision History: {TOPIC}
...
```

### When the user asks for full history or traceability

Use the standard Step 6 output format (timeline first, then narrative summary).

### General principles

- Match the language of the response to the user's language (Japanese query → Japanese response).
- Prefer tables and bullet lists over prose for rules and decisions.
- Keep the narrative summary concise (2–5 sentences).

## Notes

- Superseded and deprecated ADRs must be included — they are essential historical context.
- Do not fabricate content. Only summarize what is written in the ADR files.
- If an ADR was discovered via a link (not via the index), note it with a "(linked)" label.
- If a linked ADR number cannot be resolved to a file, note it as "(file not found)" and continue.
