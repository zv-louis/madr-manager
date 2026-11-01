# index.md Template

## Format

```markdown
# ADR Index

| Number | Title | Status | Summary |
|--------|-------|--------|---------|
| [0001](0001-example-decision.md) | Example decision title | accepted | One-sentence summary of the decision made |
```

## Rules

- **Number:** 4-digit sequence number linked to the corresponding ADR file: `[NNNN](NNNN-filename.md)`
- **Title:** Short title matching the ADR's H1 heading (without `#`)
- **Status:** One of `proposed`, `accepted`, `rejected`, `deprecated`, `superseded by ADR-XXXX`
- **Summary:** Concise one-sentence description of the decision made

## Notes

- Create this file if it does not exist when writing the first ADR.
- Add one row per ADR in sequence number order.
- When an ADR is superseded, update its Status cell to `superseded by ADR-XXXX`.
