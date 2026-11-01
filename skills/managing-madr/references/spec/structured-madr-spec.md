# Structured MADR Specification

> MADR = **M**arkdown **A**rchitectural **D**ecision **R**ecords (v4.0.0)

---

## Overview

**MADR** is a lightweight format for recording architectural decisions in Markdown. The name is a play on "decisions that matter" (pronunciation: [ˈmæɾɚ]).

**Structured MADR** refers specifically to the full template (`adr-template.md`), which includes all sections — both required and optional — among the multiple template variants provided by MADR.

### Template Variants

| Filename | Sections | Guidance text |
|---|---|---|
| `adr-template.md` | All sections (required + optional) | Included |
| `adr-template-minimal.md` | Required sections only | Included |
| `adr-template-bare.md` | All sections | Not included |
| `adr-template-bare-minimal.md` | Required sections only | Not included |

---

## File Naming and Location

```
docs/decisions/
├── 0001-use-postgresql.md
├── 0002-use-graphql-api.md
└── 0003-...
```

- Pattern: `NNNN-title-with-dashes.md` (4-digit sequential number)
- Location: `docs/decisions/` (stored in the same repository as the code)
- For large projects, subdirectories can be used to organize by domain

---

## Status Values and Lifecycle

```
proposed --> accepted --> deprecated --> (retained as historical record)
         --> rejected --> (retained as historical record)
accepted --> superseded by ADR-XXXX --> (retained as historical record)
```

| Status | Meaning |
|---|---|
| `proposed` | Under discussion, not yet decided |
| `accepted` | Agreed and in effect |
| `rejected` | Evaluated and not adopted |
| `deprecated` | Was valid but is no longer recommended |
| `superseded by ADR-XXXX` | Replaced by another ADR |

> **Note:** Never delete ADRs. Even rejected or deprecated ones must be retained as historical records.

---

## Full Template Structure

```markdown
---
status: "{proposed | rejected | accepted | deprecated | superseded by ADR-XXXX}"
date: {YYYY-MM-DD}
decision-makers: {everyone involved in the decision}
consulted: {those consulted (two-way communication)}
informed: {those kept informed (one-way communication)}
---

# {Short title: describes both the problem and the solution}

## Context and Problem Statement

{Describe the background and why this decision is necessary}

## Decision Drivers

* {Driver 1}
* {Driver 2}

## Considered Options

* {Option 1}
* {Option 2}
* {Option 3}

## Decision Outcome

Chosen option: "{chosen option}", because {reason}.

### Consequences

* Good, because {positive impact}
* Bad, because {negative impact}

### Confirmation

{How to verify that the decision is implemented correctly}

## Pros and Cons of the Options

### {Option 1}

{Description, example, or reference link (optional)}

* Good, because {argument A}
* Neutral, because {argument B}
* Bad, because {argument C}

### {Option 2}

* Good, because {argument A}
* Bad, because {argument B}

## More Information

{Supplemental information, related ADR links, external resources, review schedule, etc.}
```

---

## Section Details

### YAML Front Matter (optional)

```yaml
---
status: "proposed"
date: 2025-01-15
decision-makers: Alice, Bob
consulted: Charlie (architect)
informed: entire team
---
```

- **Purpose:** Machine-readable metadata. Maps to the RACI model:
  - `decision-makers` → Responsible / Accountable
  - `consulted` → Consulted (two-way)
  - `informed` → Informed (one-way)
- **Omittable:** If not needed, remove the entire front matter block

---

### H1: Title (required)

```markdown
# Use PostgreSQL for relational data storage
```

- A concise phrase that represents both the problem and the solution
- Recommended pattern: `Use [Solution] for [Problem]` / `Do not use [X] for [Y]`

---

### H2: Context and Problem Statement (required)

```markdown
## Context and Problem Statement

Which database should we use for storing relational data?
We need a solution that supports ACID transactions and complex queries.
```

- Describe the background and why this decision is needed
- Writing in question form encourages discussion
- May include links to an issue tracker

---

### H2: Decision Drivers (optional)

```markdown
## Decision Drivers

* ACID transactions required
* Team is familiar with SQL
* Must be open source
* Horizontal scaling not needed in the near term
```

- List the forces, constraints, and quality requirements that drive the decision
- Serves as the evaluation criteria for the Pros/Cons section

---

### H2: Considered Options (required)

```markdown
## Considered Options

* PostgreSQL
* MySQL
* MongoDB
```

- List all options actually evaluated
- Recommended convention: put the chosen option first (apply consistently across the project)
- Keep options at the same level of abstraction (do not mix technology choices with design patterns)

---

### H2: Decision Outcome (required)

```markdown
## Decision Outcome

Chosen option: "PostgreSQL", because it satisfies our ACID requirements and the team already has deep expertise with it (see "Pros and Cons" below).
```

- Clearly state the decision
- Provide justification by referencing Decision Drivers or Pros/Cons
- **Intentionally placed before Pros/Cons** so readers can grasp the decision without scrolling

---

### H3: Consequences (optional, subsection of Decision Outcome)

```markdown
### Consequences

* Good, because complex queries are easy to write
* Good, because low learning cost for the team
* Bad, because horizontal scaling is difficult
* Neutral, because a managed service (e.g., RDS) is required
```

- Record the impacts of the chosen option
- Use the consistent format: `Good, because` / `Bad, because` / `Neutral, because`
- **Always record both positive and negative impacts**

---

### H3: Confirmation (optional, subsection of Decision Outcome)

```markdown
### Confirmation

* Verify that `package.json` dependencies include only `pg`
* Confirm via code review that the ORM uses only PostgreSQL-specific features
* Automate dependency checks with ArchUnit or static analysis tools
```

- Describes how to verify that the decision is correctly implemented
- Renamed from "Validation" to "Confirmation" in v4.0.0
- Automation (fitness functions, CI checks) is strongly recommended
- Optional by spec, but **effectively recommended as it appears in most ADRs**

---

### H2: Pros and Cons of the Options (optional)

```markdown
## Pros and Cons of the Options

### PostgreSQL

https://www.postgresql.org/

* Good, because fully ACID-compliant
* Good, because rich ecosystem of extensions
* Good, because team has deep expertise
* Neutral, because vertical scaling only
* Bad, because horizontal scaling is difficult

### MySQL

* Good, because widely adopted
* Good, because horizontal scaling options available
* Bad, because ACID compliance is less strict than PostgreSQL
* Bad, because team has less experience

### MongoDB

* Good, because schema-less and flexible
* Bad, because ACID transactions are complex
* Bad, because unsuitable for relational data
```

- **The core of Structured MADR**: makes the comparison process fully transparent
- Create an H3 subsection for each option (matching the order in Considered Options)
- Each argument uses `Good, because` / `Neutral, because` / `Bad, because`
- An optional description, example, or URL can be added at the top of each option

---

### H2: More Information (optional)

```markdown
## More Information

* [PostgreSQL vs MySQL comparison article](https://example.com/comparison)
* Related to ADR-0005 (data model design)
* Re-evaluate horizontal scaling requirements in 6 months (July 2025)
* Team agreed via Slack #arch-decisions
```

- Store supplemental information, evidence, and external links
- Cross-reference related ADRs
- Record review schedule or conditions
- Document the team's consensus process

---

## Required vs Optional Sections — Quick Reference

| Section | Required | Notes |
|---|---|---|
| YAML Front Matter | Optional | Omit if not needed |
| Title (H1) | **Required** | |
| Context and Problem Statement | **Required** | |
| Decision Drivers | Optional | Omit if clear from context |
| Considered Options | **Required** | |
| Decision Outcome | **Required** | |
| └ Consequences | Optional | Strongly recommended |
| └ Confirmation | Optional | Strongly recommended (appears in most ADRs) |
| Pros and Cons of the Options | Optional | Core of Structured MADR |
| More Information | Optional | For supplemental info and cross-references |

---

## Best Practices

### Writing Quality

- Write the problem as a question ("Which framework should we use?")
- Keep options at the same level of abstraction
- Record both positive and negative impacts
- Use `Neutral, because` to show complete analysis
- Put the chosen option first in Considered Options (apply consistently within the project)

### Process

- ADRs should be co-authored by the team, not written by an individual
- Review ADRs the same way you review code
- Agree on a template variant at project start (changing later is costly)
- **Never delete ADRs** (even rejected or deprecated ones must be kept as historical records)
- When superseding, explicitly link with `superseded by ADR-XXXX`

### Tooling

```bash
# Install via npm
npm install madr
mkdir -p docs/decisions
cp node_modules/madr/template/* docs/decisions/
```

- Use `markdownlint` with MADR-provided config to enforce consistent formatting
- Automate Confirmation with static analysis tools such as ArchUnit

---

## Complete Example

```markdown
---
status: "accepted"
date: 2025-01-15
decision-makers: Alice, Bob
consulted: Charlie (senior architect)
informed: entire development team
---

# Use Plain JUnit5 for Advanced Test Assertions

## Context and Problem Statement

How should we write readable assertions in tests?
Which library is best for maintaining readability in complex test cases?

## Decision Drivers

* Readable even for developers unfamiliar with the codebase
* Minimize external dependencies
* Widely known as "common knowledge" (reduces onboarding cost)

## Considered Options

* Plain JUnit5
* Hamcrest
* AssertJ

## Decision Outcome

Chosen option: "Plain JUnit5", because comes out best (see "Pros and Cons of the Options" below).

### Consequences

* Good, because tests are easy to read
* Good, because tests are easy to write
* Bad, because complex assertions can become harder to read

### Confirmation

* Verify that project dependencies include only JUnit5
* Collect feedback on JUnit5 usability during sprint reviews
* Next re-evaluation: at the next major version upgrade

## Pros and Cons of the Options

### Plain JUnit5

https://junit.org/junit5/docs/current/user-guide/

* Good, because widely known as "common knowledge" among Java engineers
* Bad, because complex assertions can become hard to read
* Bad, because no Fluent API

### Hamcrest

https://github.com/hamcrest/JavaHamcrest

* Good, because advanced matchers available (e.g., `contains`)
* Bad, because not a fully Fluent API
* Bad, because high learning curve

### AssertJ

https://joel-costigliola.github.io/assertj/

* Good, because Fluent API enables chained writing
* Good, because flexible verification such as substring matching
* Bad, because uncommon, so high learning cost for newcomers
* Bad, because test case style tends to vary

## More Information

* [Hamcrest vs AssertJ comparison (German)](https://www.sigs-datacom.de/uploads/tx_dmjournals/philipp_JS_06_15_gRfN.pdf)
* Related to ADR-0004 (test strategy)
```

---

## Reference Resources

- [MADR official site](https://adr.github.io/madr/)
- [GitHub repository (adr/madr)](https://github.com/adr/madr)
- [MADR template primer (ozimmer.ch)](https://ozimmer.ch/practices/2022/11/22/MADRTemplatePrimer.html)
- [Paper: Markdown Architectural Decision Records (CEUR-WS)](https://ceur-ws.org/Vol-2072/paper9.pdf)
