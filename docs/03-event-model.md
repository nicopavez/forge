# Event model: external agent access to Front

This is the mechanism behind the PRD's core safety claim: an agent can read and draft,
but only a human sends. It also shows where an agent connection gets its scope, and
where every scoped action gets logged. Full diagram: [`../diagrams/event-model.svg`](../diagrams/event-model.svg),
source: [`../diagrams/event-model.json`](../diagrams/event-model.json).

## How to read it

Four chapters, left to right:

**Connect an Agent.** An admin creates a connection with a name and a fixed set of
scopes (`read_conversations`, `draft_reply`, etc). That's the only place scopes get
set. Revoking a connection is immediate and shows up in the same summary view the
admin used to grant it.

**Agent Reads Context.** The agent calls an MCP tool to read a conversation. This is
the lowest-risk surface and the one most agents will use most: it's how an agent
gets enough context to be useful before it drafts or acts on anything.

**Draft, Review, Send.** This is the slice that matters most. The agent's `draft_reply`
call produces a `ReplyDrafted` event, never a sent message. A rep sees the draft
through the normal conversation view and issues `SendReply` themselves, whether they
send it untouched or rewrite it. `SendReply` is the only command in the whole model
that can produce a message the customer sees, and only a human can issue it. That's
the human-in-the-loop control point the PRD relies on for v1, and it's a single,
auditable choke point rather than a UI convention that could be bypassed.

**Audit.** Every scoped action an agent takes produces an audit entry an admin can
see. The diagram models this for one action (a draft); the same pattern (automation
triggered by the action's event, logging connection, action type, target, and time)
applies to every other scoped action as they're added.

## What this rules out by construction

An agent's credentials never appear anywhere near the `SendReply` command. There's no
scope, no flag, no config value that lets an agent connection reach it. Adding
autonomous send later means adding a new command and a new scope the model doesn't
have today, not flipping a switch on this one. That's deliberate: it makes the v1
non-goal (no autonomous send) something the architecture enforces, not something a
policy document asks people to respect.

## Where this stays honest about what's unmodeled

`ConversationContext`, the read model an agent queries for conversation history, is
built by Front's existing conversation system, not by anything in this model. Scope
checks (does this connection's stored scopes include `draft_reply`?) happen before a
call reaches the slices shown here; they're an authorization concern that sits in
front of the model, not a domain event in it. Both are called out as notes on the
relevant slices rather than modeled as if they were new domain concepts, since they
aren't.
