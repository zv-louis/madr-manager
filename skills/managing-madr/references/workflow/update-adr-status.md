# Update ADR Status — Workflow

## Step 1: Identify the target ADR

Determine which ADR to update from the user's request:

- By number: find the file with the matching `NNNN-` prefix in `docs/decisions/`
- By name or partial title: search for a matching filename
- If ambiguous, list candidates and ask the user to confirm

## Step 2: Show the current state

Read the target file and display:

- Filename
- Current `status` value
- Current `date` value

## Step 3: Determine the new status

If the user has not specified the new status, ask. Valid values:

| Value | Meaning |
|---|---|
| `proposed` | Under discussion, not yet decided |
| `accepted` | Agreed and in effect |
| `rejected` | Evaluated and not adopted |
| `deprecated` | Was valid but is no longer recommended |
| `superseded by ADR-XXXX` | Replaced by another ADR (specify the number) |

## Step 4: Apply the update

Edit the `status` field in the YAML front matter of the target file.

```yaml
# Before
status: "proposed"

# After
status: "accepted"
```

If the new status is `superseded by ADR-XXXX`:

- Set the status using a markdown link to the superseding ADR:

  ```markdown
  superseded by [ADR-XXXX](XXXX-filename.md)
  ```

- Add a `More Information` section to the old ADR containing `Superseded by [ADR-XXXX](XXXX-filename.md)`. If the section already exists, append the entry.
- Locate the superseding ADR (XXXX) and add a `More Information` section containing `Supersedes [ADR-old](old-filename.md)`. If the section already exists, append the entry.

## Step 5: Update index.md

- If `index.md` **does not exist**, follow the `generate-index.md` workflow to create it first, then return here. The newly generated index will already reflect the correct status for all ADRs scanned — skip the edit below and proceed to Step 6.
- If `index.md` **exists**, read it and update the `Status` cell of the target ADR to reflect the new status.

## Step 6: Confirm to the user

Show the updated file path, the new status value, and any other files that were modified.

**Important:** Never delete or truncate any ADR. Status updates are the only change — all other content must be preserved.
