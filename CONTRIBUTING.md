# Contributing to PRISM

PRISM welcomes technical contributions. This document explains how
to contribute and what the boundaries are, so there are no surprises.

---

## Before you start

Read [LICENSE](./LICENSE) and [IDENTITY.md](./IDENTITY.md) first.
They are short. They define the terms under which this project
accepts contributions and what falls outside the scope of
collaboration entirely.

If something in those documents is unclear, open an issue before
writing any code.

---

## What you can contribute

- Bug fixes and correctness improvements across any agent module
- Infrastructure and reliability enhancements — health monitoring,
  MQTT resilience, watchdog patterns, storage management
- Documentation improvements — clarity, accuracy, completeness
- Hardware integration additions aligned with PRISM's architecture
- Performance improvements to model routing, vision pipelines,
  or inter-agent communication
- New capabilities that fit cleanly within an existing agent's
  defined domain without crossing into another agent's scope

If you are proposing something significant — a new capability, a
change to how agents communicate, or anything that touches multiple
modules — open an issue first to discuss it. This avoids wasted
effort on both sides.

---

## What is outside scope

**PRISM's own codebase:** Contributions must not introduce
dependencies on cloud inference, external APIs for core
functionality, or new cross-agent memory write exceptions beyond
those already documented. PRISM's local-only, privacy-first
architecture is not a starting point for negotiation.

**Calista's character identity:** Her personality, speech design,
lore, name presentation, and emotional architecture are not
contribution targets. Technical improvements to modules she relies
on are welcome. Proposals that change who she is are not, and will
be closed without extended discussion. See [IDENTITY.md](./IDENTITY.md).

---

## How to submit

1. Open an issue to discuss any non-trivial change before writing code.
2. Fork the repository solely for the purpose of submitting a PR.
   The fork may not be used for any other purpose.
3. Keep PRs focused — one concern per PR where possible.
4. Write clearly in the PR description: what the change does, why
   it is needed, and what you tested.
5. Be prepared for the PR to be revised, held, or declined. This
   is a personal project with specific architectural opinions.

---

## Licence reminder

By submitting a pull request, you confirm that you have the right
to contribute the code and you grant the project owner a perpetual,
irrevocable licence to use and incorporate it. Submitting a PR
does not grant you any rights to this codebase. See
[LICENSE](./LICENSE) for the full terms.

---

## Contact

overpowered41.23@gmail.com
