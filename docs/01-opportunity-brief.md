# Opportunity brief: external AI agents on Front

## Problem

Customers and partners want to connect external AI agents to Front to triage
conversations, draft replies, and trigger workflows. Front's MCP server already
supports OAuth-based, per-teammate access with real read, write, and send scopes at
the tool level. What's missing is an identity for the agent itself: every connection
authenticates as a specific human teammate and inherits that person's exact
permissions. There's no way for an admin to grant an agent its own narrower,
centrally revocable access, and any activity record shows the teammate, not whether
the agent or the person actually acted.

Customers feel this as a permissions problem: the only lever an admin has is which
teammate's OAuth grant an agent rides on, not what the agent itself is allowed to do.
Partners, companies with a formal relationship through Front's Integration Partner
Program, feel it as a fragility problem: their integration's access ceiling moves
whenever the authorizing teammate's role changes, and breaks outright if that person
leaves. Third-party clients, unaffiliated integration platforms with no formal
partnership with Front, feel it hardest: they need the same access pattern to work
identically everywhere they're installed, and a model built around one teammate's
identity can't give them that.

## Why now

MCP is becoming the default way agent frameworks expect to connect to external
systems, backed by OpenAI, Google, and Microsoft through the Linux Foundation.
Gartner projects task-specific AI agents in 40% of enterprise apps by 2026. The
identity gap won't
get easier to fix later: the more agents get wired up against individual teammates'
OAuth grants, the more that pattern hardens into the default, and the harder it
becomes to introduce a proper agent principal without breaking existing connections.

## Who this is for

Named directly in the brief's own context: agents connect to Front as "customer-built
copilots, partner integrations, third-party agent frameworks," and the PM is asked to
balance "customer and partner workflow needs" against "third-party client
constraints." Those three, customer, partner, third-party client, are the personas.

**Customer** (e.g. Instructure, a published Front customer story). Wants an agent
inside their own Front workspace, whether an admin governing access or an internal
engineer building the copilot itself. Needs to grant only the access it needs, see
what it did, and revoke it without engineering help.

**Partner** (e.g. Aircall, a named Front Integration Partner). Has a formal
relationship with Front, building a supported integration or agent. Needs a scope
model a customer's admin grants directly, not one tied to whichever of that
customer's teammates happened to authorize it.

**Third-party client** (e.g. Claude Desktop, which needs no partnership at all, a
user just points it at Front's MCP server). A general-purpose MCP client with no
formal relationship to Front. Needs the exact same access pattern to work identically
at every customer that installs it.

## The opportunity

Give the agent its own identity on top of the MCP server Front already has: a named,
admin-granted connection with its own scopes, distinct from any one teammate's OAuth
grant. Start narrow: read access and draft-not-send are the two use cases every one
of the personas above already wants, and neither requires solving the harder trust
problem of autonomous customer-facing action.

## Hypothesis

If admins can grant a small, fixed set of scopes to a named agent connection, see an
audit trail of what it did, and revoke it instantly, they'll say yes to connecting a
first agent. If a Support rep can accept an agent's draft in one click instead of
writing a reply from scratch, they'll actually use it. Both are testable within a closed beta
before committing to anything beyond draft-and-approve.
