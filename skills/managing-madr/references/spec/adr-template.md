# ADR File Template

## Markdown template

```markdown
---
status: "{proposed | rejected | accepted | deprecated | superseded by ADR-XXXX}"
date: {YYYY-MM-DD}
decision-makers: {names, or omit entire line if not specified}
consulted: {names, or omit entire line if not specified}
informed: {names, or omit entire line if not specified}
---

# {Short title: describes both problem and solution}

## Context and Problem Statement

{Background and why this decision is needed. Use question form to invite discussion.}

## Decision Drivers

* {Driver 1}
* {Driver 2}

## Considered Options

* {Option 1 — chosen option first by convention}
* {Option 2}
* {Option 3}

## Decision Outcome

Chosen option: "{chosen option}", because {reason referencing Decision Drivers or Pros/Cons}.

### Consequences

* Good, because {positive impact}
* Bad, because {negative impact}
* Neutral, because {neutral impact}

### Confirmation

{How to verify the decision is implemented correctly. Automation (CI checks, ArchUnit) strongly recommended.}

## Pros and Cons of the Options

### {Option 1}

{Optional: description, example, or URL}

* Good, because {argument A}
* Neutral, because {argument B}
* Bad, because {argument C}

### {Option 2}

* Good, because {argument A}
* Bad, because {argument B}

## More Information

{Supplemental info, related ADR links, external resources, review schedule.}
```

## Template rules

- **YAML Front Matter:** Include only if at least one field has a value. Omit entire block otherwise.
- **Optional sections:** Omit entirely if no content — never leave placeholder text.
- **Considered Options order:** Put the chosen option first (project-wide convention).
- **Pros and Cons subsections:** Must match the order and names used in Considered Options.
- **Never delete ADRs:** Even rejected or deprecated ones must be kept as historical records.
