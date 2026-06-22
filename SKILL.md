---
name: meeting-prep
description: Convert bounded calendar event context into a focused, decision-ready pre-meeting brief.
runx:
  category: ops
---

# Meeting Prep

Turn bounded calendar event context into a focused pre-meeting brief: a
meeting summary, attendee briefs, an agenda, decisions to make, and
follow-up actions.

This skill reads only the calendar event, attendee notes, prior thread
snippets, and optional public links the operator has already hydrated and
redacted. It does not call into calendar systems, mailboxes, or any
provider to fetch additional context. Final approval, agenda lock-in, and
any side effects (sending invites, taking notes, creating follow-up tasks)
remain separate governed actions downstream of this brief.

## Quality Profile

- Purpose: help an operator walk into a meeting knowing the goal, the
  attendees, the open decisions, and the likely next actions.
- Audience: the operator who owns the meeting and any assistant reviewing
  the brief before the meeting.
- Artifact contract: meeting summary, attendee briefs, proposed agenda,
  decisions to make, key topics, risks, and follow-up actions.
- Evidence bar: cite the supplied calendar event, attendee note, or thread
  snippet for every claim. Do not invent facts the operator did not supply.
- Voice bar: concise operator brief, not a meeting transcript or agenda
  template. Lead with the decision or the goal.
- Strategic bar: reduce prep time while preserving the operator's final
  say on agenda and decisions.
- Stop conditions: return `needs_context` when the supplied event, notes,
  or snippets are too thin to brief against; return `manual_review` when
  the meeting is sensitive (legal, HR, compensation, medical, performance).

## Inputs

- `event` (required): redacted calendar event payload (title, time, organizer,
  attendees, location/link, description).
- `attendee_notes` (optional): per-attendee notes the operator has already
  gathered (role, recent work, open threads, prior asks).
- `prior_thread_snippets` (optional): redacted snippets of recent relevant
  conversations, decision logs, or docs the operator flagged.
- `public_links` (optional): redacted public links (URLs only, no private
  content) the operator wants the brief to consider.
- `objective` (optional): what the operator wants to achieve from this
  meeting (e.g., align on Q3 plan, unblock X, get sign-off on Y).

## Output

- `meeting_summary`: one-paragraph framing of what the meeting is, who owns
  it, and what the desired outcome is.
- `attendee_briefs`: per-attendee note of role, current focus, and any
  open thread the operator should remember.
- `agenda`: ordered list of proposed agenda items, each tied to a source.
- `decisions_to_make`: explicit list of decisions the meeting should reach,
  with the owner if known.
- `key_topics`: bullets of context the operator should be ready to discuss.
- `risks_and_open_questions`: things the brief flags as missing, contested,
  or sensitive.
- `follow_up_actions`: concrete next steps the operator can act on after
  the meeting (drafts, pings, doc updates), still pre-send.
- `verdict`: `ready` (operator can walk in), `needs_context` (operator must
  supply more), or `manual_review` (sensitive; operator must read first).
