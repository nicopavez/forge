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
  table, table tr, table thead, table tbody, table tr:nth-child(even), table tr:nth-child(odd), table th, table td { background-color: #000 !important; color: #ccc !important; }
  table { font-size: 0.6em !important; width: auto; border-collapse: collapse; border: none !important; }
  table th { text-transform: uppercase !important; font-size: 0.85em !important; letter-spacing: 0.08em !important; text-align: left !important; padding: 6px 10px !important; border: none !important; border-bottom: 2px solid #333 !important; color: #999 !important; }
  table td { padding: 8px 10px !important; border: none !important; border-bottom: 1px solid #1a1a1a !important; vertical-align: top !important; }
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

### Technical implementation

# Three trust zones, one execution path

<div class="card" style="margin-top: 6px; padding: 8px;">
<svg viewBox="0 0 1160 380" style="width: 100%; height: auto; display: block; max-height: 380px;">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6.5" markerHeight="6.5" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z" fill="#666"/>
    </marker>
    <symbol id="icon-monitor" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="12" rx="1.5"/><line x1="8" y1="20" x2="16" y2="20"/><line x1="12" y1="16" x2="12" y2="20"/></g></symbol>
    <symbol id="icon-lock" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="5" y="11" width="14" height="9" rx="2"/><path d="M8 11V7a4 4 0 0 1 8 0v4"/></g></symbol>
    <symbol id="icon-db" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="8" ry="3"/><path d="M4 5v14c0 1.66 3.58 3 8 3s8-1.34 8-3V5"/><path d="M4 12c0 1.66 3.58 3 8 3s8-1.34 8-3"/></g></symbol>
    <symbol id="icon-gauge" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="8.5"/><line x1="12" y1="12" x2="15.5" y2="8"/><circle cx="12" cy="12" r="1.1" fill="currentColor" stroke="none"/></g></symbol>
    <symbol id="icon-server" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="6" rx="1.2"/><rect x="4" y="14" width="16" height="6" rx="1.2"/><circle cx="7.3" cy="7" r="0.9" fill="currentColor" stroke="none"/><circle cx="7.3" cy="17" r="0.9" fill="currentColor" stroke="none"/></g></symbol>
  </defs>

  <!-- Outside Front -->
  <rect x="20" y="65" width="190" height="230" rx="8" fill="none" stroke="#555" stroke-width="2"/>
  <text x="32" y="88" font-size="12" font-weight="600" letter-spacing="1" fill="#888">OUTSIDE FRONT</text>
  <use href="#icon-monitor" x="95" y="88" width="40" height="40" style="color:#999"/>
  <text x="115" y="148" font-size="13" font-weight="600" fill="#ddd" text-anchor="middle">External agents</text>
  <text x="115" y="172" font-size="11" fill="#999" text-anchor="middle">Customer-built copilot</text>
  <text x="115" y="192" font-size="11" fill="#999" text-anchor="middle">Partner integration</text>
  <text x="115" y="212" font-size="11" fill="#999" text-anchor="middle">Third-party MCP client</text>

  <!-- Network hop -->
  <line x1="215" y1="180" x2="378" y2="180" stroke="#666" stroke-width="2" marker-start="url(#arrow)" marker-end="url(#arrow)"/>
  <text x="296" y="164" font-size="12" fill="#888" text-anchor="middle">MCP over TLS</text>
  <text x="296" y="198" font-size="9.5" letter-spacing="1" fill="#555" text-anchor="middle">INTERNET</text>

  <!-- Front boundary -->
  <rect x="400" y="10" width="740" height="340" rx="8" fill="none" stroke="#4f8df9" stroke-width="2.2"/>
  <text x="414" y="32" font-size="14" font-weight="700" letter-spacing="1" fill="#4f8df9">FRONT</text>

  <!-- Public MCP edge -->
  <rect x="424" y="50" width="692" height="130" rx="6" fill="#4f8df9" fill-opacity="0.05" stroke="#4f8df9" stroke-width="1.5" stroke-dasharray="6,4"/>
  <text x="438" y="68" font-size="12" font-weight="600" letter-spacing="0.5" fill="#4f8df9">PUBLIC MCP EDGE &middot; NEW</text>

  <rect x="444" y="82" width="210" height="88" rx="6" fill="#0a0a0a" stroke="#333"/>
  <use href="#icon-lock" x="535" y="90" width="28" height="28" style="color:#4f8df9"/>
  <text x="549" y="136" font-size="12" font-weight="600" fill="#eee" text-anchor="middle">OAuth 2.1 + PKCE</text>
  <text x="549" y="154" font-size="9.5" fill="#888" text-anchor="middle">Short-lived, token exchange</text>

  <rect x="666" y="82" width="222" height="88" rx="6" fill="#0a0a0a" stroke="#333"/>
  <use href="#icon-db" x="763" y="90" width="28" height="28" style="color:#4f8df9"/>
  <text x="777" y="130" font-size="12" font-weight="600" fill="#eee" text-anchor="middle">Connection &amp;</text>
  <text x="777" y="145" font-size="12" font-weight="600" fill="#eee" text-anchor="middle">Scope Store</text>
  <text x="777" y="163" font-size="9.5" fill="#888" text-anchor="middle">Scope checked every call</text>

  <rect x="900" y="82" width="200" height="88" rx="6" fill="#0a0a0a" stroke="#333"/>
  <use href="#icon-gauge" x="986" y="90" width="28" height="28" style="color:#4f8df9"/>
  <text x="1000" y="136" font-size="12" font-weight="600" fill="#eee" text-anchor="middle">Rate Limiter</text>
  <text x="1000" y="154" font-size="9.5" fill="#888" text-anchor="middle">Per connection</text>

  <!-- Down arrow into internal platform -->
  <line x1="770" y1="180" x2="770" y2="196" stroke="#666" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="792" y="192" font-size="10" fill="#666">in-scope calls only</text>

  <!-- Internal platform -->
  <rect x="424" y="196" width="692" height="130" rx="6" fill="#22c55e" fill-opacity="0.05" stroke="#22c55e" stroke-width="1.5" stroke-dasharray="6,4"/>
  <text x="438" y="214" font-size="12" font-weight="600" letter-spacing="0.5" fill="#22c55e">INTERNAL PLATFORM &middot; EXISTING</text>

  <rect x="444" y="228" width="350" height="88" rx="6" fill="#0a0a0a" stroke="#333"/>
  <use href="#icon-server" x="605" y="236" width="28" height="28" style="color:#22c55e"/>
  <text x="619" y="282" font-size="12" font-weight="600" fill="#eee" text-anchor="middle">Conversation &amp; Workflow Services</text>
  <text x="619" y="300" font-size="9.5" fill="#888" text-anchor="middle">Same path Front's UI already uses</text>

  <rect x="814" y="228" width="286" height="88" rx="6" fill="#0a0a0a" stroke="#333"/>
  <use href="#icon-db" x="943" y="236" width="28" height="28" style="color:#22c55e"/>
  <text x="957" y="282" font-size="12" font-weight="600" fill="#eee" text-anchor="middle">Audit Log Store</text>
  <text x="957" y="300" font-size="9.5" fill="#888" text-anchor="middle">Connection, action, target, time</text>
</svg>
</div>

<div style="margin-top: 6px; font-size: 0.56em; color: var(--body);">The Connection &amp; Scope Store, rate limiter, and audit log are the genuinely new pieces. Everything inside the internal platform boundary already runs today, and <strong>SendReply</strong> there is issued by a human rep, never by an agent credential.</div>

---

### Access pattern

# Every scoped call, the same six steps

<table style="margin-top: 8px;">
<tr><th>Step</th><th>Control</th></tr>
<tr><td>Agent calls an MCP tool over TLS</td><td>Short-lived, token-exchange credential, not a long-lived key</td></tr>
<tr><td>Edge looks up the connection's stored scopes</td><td>Server-side only; the caller's claim is never trusted</td></tr>
<tr><td>Scope check gates the call</td><td>Zero tolerance for scope escapes; denials are logged too</td></tr>
<tr><td>In-scope call reaches Front's existing service</td><td>No second execution path to maintain or drift from</td></tr>
<tr><td>Service performs the action under its own rules</td><td>e.g. draft only, a workflow already approved</td></tr>
<tr><td>Every outcome writes one audit entry</td><td>Connection, action, target, time; retained 1+ year</td></tr>
</table>

<div style="margin-top: 14px; font-size: 0.62em; color: var(--body);">P95 latency: under 500ms read, under 800ms write. Rate limits by tier: roughly 120/30/20 calls per minute. Revocation reaches every future call in under a minute.</div>

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
