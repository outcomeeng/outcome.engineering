# Outcome Engineering Docs

The Mintlify documentation site published at docs.outcome.engineering. Its source lives in this directory; the Next.js landing page and blog live at the repository root.

## Governing instructions

The repository root guides carry the managed Spec Tree router and every blocking workflow requirement — context loading, base sync, verification, and the merge lifecycle. Read the guide for the active harness before working here: `../CLAUDE.md` for Claude Code, `../AGENTS.md` for Codex. This file supplements them with directory-specific facts and overrides nothing.

## Identity

- **Domain**: docs.outcome.engineering
- **Platform**: Mintlify
- **Repository**: [outcomeeng/outcome.engineering](https://github.com/outcomeeng/outcome.engineering)

## Content structure

Two tabs, configured in `docs.json`: Guide for the conceptual narrative, Reference for technical specification.

```
mintlify/
├── docs.json          # Mintlify configuration and navigation
├── index.mdx          # Homepage
├── guide/             # overview, spec-tree, nodes, building,
│                      # deterministic-context, lock-files,
│                      # operational-loop, guidelines, testing,
│                      # agent-skills
└── reference/         # node-types, index-numbering, node-states,
                       # filesystem, lock-file, assertions,
                       # spx-cli, context-injection
```

`docs.json` is the authority on navigation; the tree above orients a reader and is not a second source of truth.

## Methodology content

The methodology this site documents is authored in a separate repository and is under active revision. `docs/publication/site-architecture.md` and `docs/publication/release-plan.md` at the repository root govern which methodology content is current, how versioned content is published, and which existing pages are superseded. Read both before changing any guide or reference page, and do not treat a page's current wording as evidence that its concepts are still current.

## Development

```bash
npm i -g mint
mint dev
```

Preview at http://localhost:3000.
