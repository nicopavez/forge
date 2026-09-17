---
marp: true
theme: default
paginate: true
style: |
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;700;800&family=Raleway:wght@100;200;300;400&display=swap');
  :root {
    --accent: #4f8df9; --accent-hover: #79aaff;
    --dark: #000; --card: #0a0a0a; --border: #141414;
    --body: #999; --label: #666; --muted: #555; --light: #fff;
    --green: #22c55e; --red: #ef4444; --yellow: #f5a623;
  }
  section { background: var(--dark); color: var(--light); font-family: 'Raleway', sans-serif; font-weight: 200; padding: 56px 72px; line-height: 1.5; }
  h1 { font-family: 'Outfit'; font-weight: 800; font-size: 2.6em; color: var(--light); letter-spacing: -0.02em; margin: 0 0 4px; }
  h2 { font-family: 'Raleway'; font-weight: 100; font-size: 1.15em; color: #888; margin: 0 0 20px; }
  h3 { font-family: 'Outfit'; font-weight: 600; font-size: 0.6em; color: var(--muted); text-transform: uppercase; letter-spacing: 0.2em; margin: 0 0 8px; }
  strong { color: var(--accent); font-weight: 400; }
  section.lead { display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; }
  .tag { font-family: 'Outfit'; font-weight: 600; font-size: 0.55em; letter-spacing: 0.1em; text-transform: uppercase; padding: 3px 10px; border-radius: 4px; }
  .card { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 18px 20px; }
  .row { display: flex; gap: 16px; }
  .col { flex: 1; }
  table { font-size: 0.6em; width: 100%; border-collapse: collapse; }
  table th { color: var(--muted); text-transform: uppercase; font-size: 0.85em; letter-spacing: 0.08em; text-align: left; padding: 6px 10px; border-bottom: 1px solid var(--border); }
  table td { color: var(--body); padding: 8px 10px; border-bottom: 1px solid #111; vertical-align: top; }
footer: ''
---

<!-- _class: lead -->

# External agents on Front

## A scoped, auditable way for third-party AI to read and act on Front

<div style="margin-top: 24px; font-size: 0.6em; color: var(--muted);">Developer Platform &middot; PRD walkthrough</div>

---

### The problem

# Agents borrow a teammate's identity, not their own

<div class="row" style="margin-top: 24px;">
  <div class="col card">
    <div style="color: var(--accent); font-weight: 600; font-size: 0.85em;">What exists</div>
    <div style="font-size: 0.75em; margin-top: 8px; color: var(--body);">OAuth-based access with real read, write, and send scopes at the tool level.</div>
  </div>
  <div class="col card">
    <div style="color: var(--red); font-weight: 600; font-size: 0.85em;">What's missing</div>
    <div style="font-size: 0.75em; margin-top: 8px; color: var(--body);">An identity for the agent itself, distinct from the teammate it's impersonating.</div>
  </div>
</div>

<div style="margin-top: 20px; font-size: 0.75em; color: var(--body);">
Every connection today authenticates as a specific teammate and inherits that person's exact permissions. There's no admin-managed way to grant an agent its own narrower, revocable access, and any activity record shows the teammate, not whether the agent or the person actually acted.
</div>

---

### Who this is for

# Three jobs, one platform decision

<div class="row" style="margin-top: 20px;">
  <div class="col card">
    <div style="font-weight: 600; font-size: 0.8em;">Customer <span style="color: var(--muted); font-weight: 200;">(Instructure)</span></div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 10px;">Grant only the access an agent needs. See what it did. Revoke it without engineering help.</div>
  </div>
  <div class="col card">
    <div style="font-weight: 600; font-size: 0.8em;">Partner <span style="color: var(--muted); font-weight: 200;">(Aircall)</span></div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 10px;">One scope model a customer's admin grants directly, not one tied to whichever teammate authorized it.</div>
  </div>
  <div class="col card">
    <div style="font-weight: 600; font-size: 0.8em;">Third-party client <span style="color: var(--muted); font-weight: 200;">(Claude Desktop)</span></div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 10px;">The same access pattern, working identically at every customer that installs it.</div>
  </div>
</div>

<div style="margin-top: 16px; font-size: 0.55em; color: var(--muted);">Real, public examples: a Front customer story, a named Front Integration Partner, and a generic MCP client that needs no partnership to connect at all.</div>

---

### Highest-leverage use cases

# Read, draft, triage, trigger

<table style="margin-top: 12px;">
<tr><th>Use case</th><th>Risk</th><th>Why it matters</th></tr>
<tr><td><strong>Read context</strong></td><td>Low</td><td>The on-ramp for everything else the agent does</td></tr>
<tr><td><strong>Draft a reply</strong></td><td>Low, human sends</td><td>Highest leverage per unit of risk</td></tr>
<tr><td><strong>Triage / classify</strong></td><td>Medium, recoverable</td><td>Value compounds at volume</td></tr>
<tr><td><strong>Trigger a defined workflow</strong></td><td>Medium, scoped</td><td>Reuses automation Front already trusts</td></tr>
</table>

---

### Out of scope, and non-goals

# What's deferred, and what's permanent

<div class="row" style="margin-top: 16px;">
  <div class="col card">
    <div style="color: var(--yellow); font-weight: 600; font-size: 0.8em;">Not now (deferred)</div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 10px; line-height: 1.8;">
    Autonomous send<br>
    Partner marketplace or directory<br>
    SDKs beyond raw MCP tool calls
    </div>
  </div>
  <div class="col card">
    <div style="color: var(--red); font-weight: 600; font-size: 0.8em;">Not ever (non-goals)</div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 10px; line-height: 1.8;">
    Self-expanding scopes<br>
    Cross-workspace or cross-tenant access<br>
    A non-MCP transport
    </div>
  </div>
</div>

<div style="margin-top: 16px; font-size: 0.6em; color: var(--muted);">Each deferred item has a real revisit trigger tied to beta data, not a fixed date. Non-goals would undermine the safety model itself, not just add scope to it.</div>

---

### MVP

# What ships first

<div class="row" style="margin-top: 16px;">
  <div class="col card">
    <div style="font-size: 0.7em; color: var(--body); line-height: 1.9;">
    <strong>Agent connection</strong> as a named, scoped principal, not a generic token<br>
    <strong>MCP tools</strong> gated by connection scope, not an impersonated role<br>
    <strong>Human-in-the-loop send</strong>, a structural gate, not a UI convention
    </div>
  </div>
  <div class="col card">
    <div style="font-size: 0.7em; color: var(--body); line-height: 1.9;">
    <strong>Audit log</strong> of every scoped action: connection, scope, target, time<br>
    <strong>One-click revocation</strong>, effective immediately<br>
    <span style="color: var(--muted);">No autonomous send, no self-expanding scopes, no marketplace, in v1</span>
    </div>
  </div>
</div>

---

### Roadmap

# Draft-and-approve first, autonomy earned later

<table style="margin-top: 16px;">
<tr><th>Phase</th><th>Duration</th><th>Scope</th></tr>
<tr><td>0</td><td>4-6 wks</td><td>Scope taxonomy and data model with security and infra</td></tr>
<tr><td>1</td><td>8-10 wks</td><td>Connect/consent UI, audit log, read + draft tools. Closed beta.</td></tr>
<tr><td>2</td><td>6-8 wks</td><td>Tag/assign and workflow-trigger tools. Open beta, self-serve.</td></tr>
<tr><td>3</td><td>GA</td><td>Decide v1.1 auto-send opt-in on measured trust, not a date</td></tr>
</table>

---

### Risks

# What could go wrong, and the mitigation

<table style="margin-top: 12px;">
<tr><th>Risk</th><th>Mitigation</th></tr>
<tr><td>A scoped agent is still a meaningful blast radius</td><td>Team/inbox-level default scopes, per-connection rate limits</td></tr>
<tr><td>Reps ignore drafts, adoption stalls</td><td>Measure edit-distance and send-rate in beta; make accept faster than writing</td></tr>
<tr><td>MCP auth patterns still stabilizing industry-wide</td><td>Version the server interface from day one, track the spec's auth working group</td></tr>
<tr><td>DPAs may not contemplate a third-party agent</td><td>Legal and compliance in Phase 0; developer data-handling attestation</td></tr>
</table>

---

### Success metrics

# Four, matching the brief's own categories

<div class="row" style="margin-top: 16px;">
  <div class="col card">
    <div style="color: var(--accent); font-size: 0.6em; font-weight: 600; text-transform: uppercase;">Adoption</div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 8px;">30% of API-plan workspaces create a connection within 90 days of GA.</div>
  </div>
  <div class="col card">
    <div style="color: var(--accent); font-size: 0.6em; font-weight: 600; text-transform: uppercase;">Time to first integration</div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 8px;">Median under 30 minutes, credentials to first sandbox tool call.</div>
  </div>
</div>
<div class="row" style="margin-top: 16px;">
  <div class="col card">
    <div style="color: var(--accent); font-size: 0.6em; font-weight: 600; text-transform: uppercase;">Developer satisfaction</div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 8px;">8/10+ on scope-model clarity in the post-beta survey.</div>
  </div>
  <div class="col card">
    <div style="color: var(--accent); font-size: 0.6em; font-weight: 600; text-transform: uppercase;">Reliability</div>
    <div style="font-size: 0.68em; color: var(--body); margin-top: 8px;">99.9% MCP server availability, P95 tool-call latency under 500ms.</div>
  </div>
</div>

---

### How it communicates

# MCP as the door, not a new house

<div class="card" style="margin-top: 16px; padding: 28px;">
  <div style="display: flex; align-items: center; justify-content: space-between; font-size: 0.7em;">
    <div style="text-align: center;">External agent</div>
    <div style="color: var(--muted);">&rarr; MCP tool call &rarr;</div>
    <div style="text-align: center; color: var(--accent);">Front MCP server<br><span style="font-size: 0.8em; color: var(--muted);">scope check</span></div>
    <div style="color: var(--muted);">&rarr;</div>
    <div style="text-align: center;">Front's internal services<br><span style="font-size: 0.8em; color: var(--muted);">same path the UI uses</span></div>
  </div>
</div>

<div style="margin-top: 20px; font-size: 0.68em; color: var(--body);">
Writes go through the same command/event pipeline Front already uses for its own automation. Nothing new at the domain level, only a new, scoped entry point into it. <strong>SendReply</strong> is the one command no agent credential can ever reach, see the event model.
</div>

---

### Technical trade-offs

# What I'd work through with engineering

<div style="font-size: 0.68em; color: var(--body); line-height: 2.0; margin-top: 12px;">
<strong>Coupling vs. gateway</strong>: fast to ship vs. safe to evolve independently<br>
<strong>Sync vs. async tool calls</strong>: simple today vs. what long-running actions need<br>
<strong>Fine-grained vs. bundled scopes</strong>: least privilege vs. easy to reason about<br>
<strong>Per-connection vs. per-workspace rate limits</strong>: predictable vs. infra-safe
</div>

---

<!-- _class: lead -->

### The hardest assumption

# Human review is enough to de-risk launch

<div style="max-width: 720px; font-size: 0.7em; color: var(--body); margin-top: 20px; text-align: left;">
If customers push for auto-send in week one because draft-and-approve doesn't save enough time, the fix is that the permission and audit layer, the actual safety mechanism, was already built robust enough to support it. Extending to v1.1 becomes a scope change, not a re-architecture.
</div>
