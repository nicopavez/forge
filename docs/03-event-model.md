# Event model: external agent access to Front

This is the mechanism behind the PRD's core safety claim: an agent can read and draft,
but only a human sends. Full diagram: [`../diagrams/event-model.svg`](../diagrams/event-model.svg),
source: [`../diagrams/event-model.json`](../diagrams/event-model.json).

## How to read it

Three chapters, left to right:

**Connect & Scope.** An admin creates a connection with a name and a fixed set of
scopes. That's the only place scopes get set. Revoking a connection takes effect
immediately.

**Agent Reads & Drafts.** The agent reads conversation context, then can draft a
reply. Both require the matching scope. A draft is never a message the customer
sees, it's a `ReplyDrafted` event, nothing more.

**Human Sends, Everything Logs.** A Support rep reviews the draft and issues `SendReply`
themselves, whether they send it untouched or rewrite it first. That's the only
command in the whole model that can put a message in front of the customer, and no
agent connection can ever issue it. Every action on this timeline, connect, revoke,
draft, send, produces one audit entry an admin can see.

## What this rules out by construction

An agent's credentials never appear anywhere near `SendReply`. There's no scope, no
flag, no config value that lets an agent connection reach it. Adding autonomous send
later means adding a new command the model doesn't have today, not flipping a switch
on this one. That's what makes the v1 non-goal (no autonomous send) something the
architecture enforces, not something a policy document asks people to respect.

## What's simplified here on purpose

This diagram is built for a walkthrough with PMs and a tech lead, not as an
engineering spec. It drops the field-level detail, test cases, and read-model
projections a build-ready version would carry, keeping only the actors, the
commands and events, and the one control point the PRD's thesis depends on. Triage
and workflow-trigger (UC-03, UC-04) follow this same pattern and aren't modeled
separately here, see the PRD's [use cases](02-prd.md#use-cases) for both.
