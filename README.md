# Konoha

> A hidden village of 14 specialist agents and one Hokage, built from the best books in software engineering.

**Konoha** is a development team for [Claude Code](https://code.claude.com): product, requirements, architecture, data, UX, development, code review, QA, security, privacy, DevOps, SRE, performance and documentation. Each specialist is a subagent whose method comes from the classic literature of its field. The `konoha` skill is the Hokage — the tech lead who picks the right specialists for the task, runs them in order, and checks every quality gate before moving on.

A typo doesn't need fourteen ninjas. A new service does. Konoha scales the team to the mission.

## The team

| Agent | Role | Built on |
|---|---|---|
| `konoha` (skill) | **Hokage** — tech lead, orchestration, gates, retro | *Team Topologies*, *The Mythical Man-Month*, *Accelerate* |
| `konoha-product` | Problem, outcome, metrics, first release | *Inspired*, *Continuous Discovery Habits*, *User Story Mapping* |
| `konoha-requirements` | FR, NFR, business rules, acceptance criteria | *Software Requirements*, *Specification by Example*, ISO/IEC/IEEE 29148 |
| `konoha-architect` | Structure, boundaries, ADRs | *Fundamentals of Software Architecture*, *Domain-Driven Design*, *Clean Architecture* |
| `konoha-data` | Data model, normalization, indexes, migrations | *Designing Data-Intensive Applications*, *Fundamentals of Database Systems*, *SQL Antipatterns* |
| `konoha-ux` | Flows, screen states, accessibility | *Don't Make Me Think*, *The Design of Everyday Things*, WCAG 2.2 |
| `konoha-developer` | Test-first implementation | *The Pragmatic Programmer*, *A Philosophy of Software Design*, *Code Complete*, *Effective Java* |
| `konoha-reviewer` | Code review, smells, refactoring | *Refactoring*, *Working Effectively with Legacy Code*, *Clean Code* |
| `konoha-qa` | Test strategy, edge cases, exploratory testing | *Unit Testing Principles, Practices, and Patterns*, *Agile Testing*, *TDD by Example* |
| `konoha-security` | Threat modeling, OWASP review, dependencies | *Threat Modeling*, *Secure by Design*, OWASP Top 10 / ASVS |
| `konoha-privacy` | Personal data, legal basis, retention, rights | LGPD, *Privacy by Design*, GDPR |
| `konoha-devops` | CI/CD, build, environments, rollback | *Continuous Delivery*, *The DevOps Handbook* |
| `konoha-sre` | SLOs, observability, resilience | *Site Reliability Engineering*, *Release It!*, *Observability Engineering* |
| `konoha-performance` | Measure, find the bottleneck, verify NFRs | *Systems Performance* |
| `konoha-docs` | README, guides, reference (Diátaxis) | *Docs for Developers*, Diátaxis |

Full references in [BOOKS.md](BOOKS.md).

## How it works

The Hokage classifies each request into a track:

| Track | When | Team |
|---|---|---|
| **D — quick fix** | Typo, config, obvious one-function change | developer → reviewer |
| **C — bug fix** | Wrong behavior in existing code | developer (regression test first) → reviewer → qa |
| **B — feature** | New behavior in an existing system | requirements → architect\* → data\* · ux\* → security\* · privacy\* → developer → reviewer → qa → security → performance\* → sre\* → devops\* → docs |
| **A — new project** | Empty repo or new service | the full village |

\* only when the change needs it. Agents separated by `·` run in parallel.

Every specialist **delivers an artifact** to `docs/konoha/<slug>/` and must pass a **gate** before the next one starts:

| Agent | Artifact | Gate |
|---|---|---|
| product | `product.md` | problem in one sentence, metrics with targets |
| requirements | `srs.md` | every FR has an acceptance criterion, every NFR has a number |
| architect | `architecture.md` + `docs/adr/` | every top quality attribute addressed, real alternatives in each ADR |
| data | `data-model.md` + migrations | keys and constraints everywhere, expand/contract for destructive changes |
| ux | `ux.md` | five states per screen, WCAG 2.2 AA |
| developer | code + `dev-notes.md` | every AC has a passing test, suite and linter green |
| reviewer | `review.md` | no blocker or major finding open |
| qa | tests + `test-plan.md` | every AC automated, no high-severity bug open |
| security | `threat-model.md` | no high or critical issue without written risk acceptance |
| privacy | `privacy.md` | every personal field has purpose, legal basis and retention |
| devops | pipeline + `delivery.md` | pipeline green, rollback procedure exists |
| sre | instrumentation + `reliability.md` | timeouts everywhere, SLIs emitted, runbooks for alerts |
| performance | `performance.md` | every performance NFR measured against its threshold |
| docs | docs + `docs-report.md` | install and main use case work from the docs alone |

When review, QA or security fail a gate, the Hokage sends the findings back to the developer and checks again, up to three rounds. Then it asks you.

It stops for your approval after requirements (scope), after architecture on new projects, and before any security or privacy risk is accepted. Each mission ends with a `retro.md` that suggests improvements to the agents themselves.

## Design choices

- **Methods, not name-dropping.** Telling a model "follow Fowler" does little. Each agent carries the concrete checklist: the smells to look for, the STRIDE categories, the five screen states, the four golden signals.
- **Least privilege per role.** Reviewer, security and performance never touch code; they only write their reports. Only the developer edits production code; QA edits tests; DevOps edits the pipeline.
- **Draft cheap, verify expensive.** Drafting roles run on `medium` effort; verifying roles (architect, data, reviewer, QA, security, performance) run on `high`. Works well with [Kaioken](https://github.com/BernardoKrause/kaioken).
- **Artifacts over chat.** Decisions live in the repository as Markdown, reviewable and versioned with the code.

## Install

```bash
git clone https://github.com/Tetzdesen/konoha.git
mkdir -p ~/.claude/agents ~/.claude/skills
cp konoha/agents/*.md ~/.claude/agents/
cp -R konoha/skills/konoha ~/.claude/skills/
```

To install for a single project, copy into that project's `.claude/agents/` and `.claude/skills/` instead.

## Use

```text
konoha: add CSV export to the orders page
konoha: users get logged out after 5 minutes, fix it
konoha: start a REST API for the farmers' market, Java + PostgreSQL
konoha: run unattended — add password reset by e-mail
```

You can also call any specialist directly, e.g. "use konoha-security to threat-model the login flow".

## Limits

- Agents apply the principles of the books; they are not a substitute for reading them or for human judgment.
- Subagents cannot call other subagents, so the Hokage runs in your main conversation.
- The full village is expensive in tokens. Let the tracks do their job.
- The privacy agent is not a lawyer, and the security agent is not a penetration test.

## License

MIT — see [LICENSE](LICENSE).

*Konoha is a fan reference to the village in Naruto. This project is not affiliated with or endorsed by the Naruto rights holders.*
