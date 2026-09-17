# Forge: external agents on Front

[![PRD](https://img.shields.io/badge/deliverable-PRD-blue)](docs/02-prd.md)
[![Event Model](https://img.shields.io/badge/validated-event%20model-9cf)](diagrams/event-model.svg)
[![Deck](https://img.shields.io/badge/slides-12--slide%20deck-orange)](slides/deck.pdf)
[![License](https://img.shields.io/badge/license-all%20rights%20reserved-lightgrey)](#license)

A product direction for connecting third-party AI agents to Front, written as a
take-home response to Front's Developer Platform PM exercise: how should external
agents read context and take action on a customer's behalf, given Front already
shipped an early MCP server.

## The gap

Front's MCP server already does real work: OAuth 2.1 with PKCE, per-teammate
consent, and three genuine scope tiers at the tool level (read, draft-not-send,
send-with-confirmation). What it doesn't have is an identity for the agent itself.
Every connection authenticates as a specific human teammate and inherits that
person's exact permissions wholesale. An admin can't grant an agent its own
narrower, centrally revocable access, and any activity record shows the teammate,
not whether the agent or the person actually acted.

That's not "Front has no scopes" or "Front has no audit trail." It's a more precise
and more actionable claim: identity is borrowed, not assigned. The PRD's whole
thesis follows from that one gap, source-checked against Front's own public docs,
see [Assumptions](docs/02-prd.md#assumptions) for exactly what's verified vs.
genuinely unknown from outside the company.

## What's here

- [`docs/01-opportunity-brief.md`](docs/01-opportunity-brief.md): the problem, the
  personas, and why now, in about a page.
- [`docs/02-prd.md`](docs/02-prd.md): the PRD, following the brief's own three parts.
  Part 1 (Problem Formulation): abstract, problem statement, personas, a domain
  glossary, use cases (UC-01...) ranked by a stated leverage metric. Part 2 (Solution
  Definition): MVP vs. roadmap, phases with risk and mitigation per phase,
  dependencies, out of scope vs. non-goals, four success metrics. Part 3 (Technical
  Implications): a communication diagram, functional and non-functional requirements
  (FR-01...), technical trade-offs, and the hardest assumption. A decision log closes
  it out.
- [`docs/03-event-model.md`](docs/03-event-model.md) and
  [`diagrams/event-model.svg`](diagrams/event-model.svg): the mechanism behind the
  PRD's central claim, that an agent can read and draft but only a human can send,
  modeled as commands, events, and read models rather than asserted in prose.
- [`slides/deck.md`](slides/deck.md) (rendered: [`deck.html`](slides/deck.html),
  [`deck.pdf`](slides/deck.pdf)): a 12-slide walkthrough of the PRD, built for a live
  20-minute session with a tech lead and two PMs.
- [`mockups/index.html`](mockups/index.html): four B&W wireframes of the MVP flow,
  connecting an agent, the developer consent screen, an agent-drafted reply awaiting
  approval, and the audit log.

## Personas, with real examples

Matching the brief's own three nouns exactly, each checked against a live source
rather than assumed:

- **Customer** ([Instructure](https://front.com/customer-stories/instructure), a
  published Front customer story): wants an agent inside their own workspace, needs
  to grant only what it needs and revoke it without engineering help.
- **Partner** ([Aircall](https://front.com/partners/integration), named on Front's
  own partner page): has a formal relationship with Front, needs a scope model an
  admin grants directly, not one tied to whichever teammate happened to authorize it.
- **Third-party client** (Claude Desktop): needs zero partnership to connect to any
  MCP server, which is exactly the persona's point, the access pattern has to work
  identically with no relationship to Front at all.

## Repo structure

```
forge/
├── README.md
├── docs/
│   ├── 01-opportunity-brief.md   # problem, personas, why now
│   ├── 02-prd.md                 # the PRD, the main deliverable
│   └── 03-event-model.md         # narrates the event model diagram
├── diagrams/
│   ├── event-model.json          # validated source (Pydantic schema)
│   └── event-model.svg           # rendered diagram
├── slides/
│   ├── deck.md                   # Marp source
│   ├── deck.html
│   └── deck.pdf
├── mockups/
│   └── index.html                # 4 B&W wireframes: connect, consent, draft/approve, audit
└── .claude/skills/                # public Claude Skills used to build this, see NOTICE.md
```

## How this was built

Every deliverable used a real Claude Skill pulled from a public repo rather than a
blank prompt, copied into [`.claude/skills/`](.claude/skills/) and credited in
[`.claude/skills/NOTICE.md`](.claude/skills/NOTICE.md):

- The opportunity brief and PRD draw on [prd-development, opportunity-solution-tree,
  problem-framing-canvas, jobs-to-be-done, and proto-persona](https://github.com/deanpeters/Product-Manager-Skills).
- The event model uses [noahseger/agent-skills' event-modeling
  skill](https://github.com/noahseger/agent-skills), including its Pydantic validator
  and SVG renderer, which caught several information gaps in an earlier draft of the
  model before it ever got to a diagram.
- The deck uses [robonuggets/marp-slides](https://github.com/robonuggets/marp-slides),
  rendered with `@marp-team/marp-cli`.
- The mockups adapt the visual language of
  [Magdoub/claude-wireframe-skill](https://github.com/Magdoub/claude-wireframe-skill).
  Its default workflow scans an existing codebase for design context; this is a
  net-new concept with no existing app, so the four screens were built by hand in the
  same B&W wireframe style instead of through its automated pipeline.

## Quick start

**Render the slides:**
```bash
cd slides
npx @marp-team/marp-cli deck.md --html --allow-local-files -o deck.html
npx @marp-team/marp-cli deck.md --pdf --allow-local-files -o deck.pdf
```

**Re-validate the event model:**
```bash
python .claude/skills/event-modeling/event_model.py validate diagrams/event-model.json
python .claude/skills/event-modeling/event_model.py render diagrams/event-model.json -o diagrams/event-model.svg
```
On Windows, the render step needs `PYTHONUTF8=1` set first (the default console
encoding chokes on a unicode character the renderer emits).

## Known gaps

- The event model diagrams UC-01, UC-02, and UC-05 through UC-08 (connect, read,
  draft, send, revoke, audit). UC-03 (triage) and UC-04 (trigger workflow) are
  described in prose as following the identical command/event/read-model pattern but
  aren't separately modeled in the JSON/SVG.
- Real request volume behind the brief's "growing priority," whether an
  agent-identity fix is already planned internally at Front, and the extent of
  Front's internal activity logging beyond what's public: all stated as open in the
  PRD's Assumptions section rather than guessed at.

## License

All original content here (this README, `docs/`, `diagrams/`, `slides/`,
`mockups/`) is all rights reserved: public to view, not licensed for reuse. The
skills in [`.claude/skills/`](.claude/skills/) are third-party work under their
own original licenses (CC BY-NC-SA 4.0 for five of the folders, MIT for the rest),
unchanged from their source repos. See [`LICENSE`](LICENSE) and
[`.claude/skills/NOTICE.md`](.claude/skills/NOTICE.md) for the full breakdown.
