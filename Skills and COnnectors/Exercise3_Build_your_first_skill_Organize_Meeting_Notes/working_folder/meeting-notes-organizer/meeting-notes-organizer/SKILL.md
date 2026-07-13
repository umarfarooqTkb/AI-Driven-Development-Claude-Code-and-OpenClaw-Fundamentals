---
name: meeting-notes-organizer
description: Converts rough, unstructured meeting notes, call notes, discussion notes, interview notes, or a raw meeting transcript into a clear, professional meeting record with a summary, decisions, an action-item table with owners/due dates, risks, and open questions. Use this skill whenever the user provides meeting notes or a transcript (however messy or informal) and asks to organize, clean up, summarize, structure, or extract action items/decisions/owners/deadlines from them, or asks to turn notes into "minutes of meeting." Do not use it for writing meeting invitations or agendas ahead of a meeting, general advice about running meetings, or summarizing content that isn't from a meeting (e.g. an article, report, or audio transcription task with no organizing step).
---

# Meeting Notes Organizer

## Purpose

Turn messy, informal meeting input into a clean, scannable, and actionable record — without ever inventing information that wasn't actually in the notes. The core tension this skill manages is between two goals that pull in opposite directions: making the output look complete and professional, while staying strictly honest about what was and wasn't actually said. When in doubt, always favor honesty over completeness — a gap clearly labeled "Not specified" is far more useful to the user than a plausible-looking guess.

## When this applies

Use this skill when the user supplies meeting notes, call notes, discussion notes, interview notes, or an unstructured transcript and asks to organize, summarize, structure, or extract actions/decisions from them. This includes requests like "clean this up," "turn this into minutes," or "pull out the owners and deadlines" — the user doesn't need to say the exact phrase "meeting notes."

This skill does **not** apply to: drafting a meeting invite, building an agenda for an upcoming meeting, general meeting-facilitation advice, transcribing audio into text, or summarizing non-meeting content (articles, reports, papers). If the input isn't notes/transcript *from* a meeting that already happened, this skill isn't the right tool.

## Process

Work through the notes in this order:

1. **Read the whole input first** before writing anything, so later mentions of a topic (e.g., a decision that gets revised mid-meeting) correctly override earlier ones.
2. Identify the likely purpose of the meeting from context.
3. Draft a concise summary (3–5 sentences — resist the urge to pad this out).
4. Pull out confirmed decisions — something the group actually agreed on, not something merely floated.
5. Pull out every action item mentioned, however casually phrased ("I'll take a look at X" counts).
6. For each action item, identify the owner *only* if the notes clearly assign it to a named person.
7. For each action item, identify a due date *only* if one is actually stated (a relative date like "by Friday" is fine to keep as-is, or convert to an explicit date if the meeting date is known — don't guess a date that was never mentioned).
8. Note dependencies, blockers, and risks mentioned anywhere in the notes.
9. Note unresolved questions — things the group explicitly flagged as needing a follow-up or decision.
10. Note what important information seems to be missing (e.g., no date was given for the meeting itself, or a decision was discussed but never actually confirmed).

### The line between confirmed and assumed

This is the most important judgment call in the skill, and it's worth being deliberate about it:

- A **decision** belongs in "Decisions Made" only if the notes show the group actually landed on it — not if someone merely proposed or suggested it. "We should probably switch to weekly syncs" is a suggestion; "Team agreed to move to weekly syncs starting next week" is a decision.
- Never assign an owner to an action item unless a person is clearly named in connection with that task. Don't infer ownership from someone merely being the one who raised the topic.
- Never invent a due date. If the notes say "soon" or give no timing at all, the due date is "Not specified" — don't convert vague language into a specific date.
- If you're not sure whether something is a decision or a suggestion, treat it as a suggestion and, if it seems important, surface it in Key Discussion Points or Open Questions instead of Decisions Made.

## Output format

Always produce these sections, in this order:

### Meeting Summary
3–5 sentences covering what the meeting was about and its general outcome.

### Key Discussion Points
The main topics covered, as a bulleted list. Combine points that are really the same topic discussed more than once.

### Decisions Made
Only clearly confirmed decisions. If none were confirmed, say so explicitly rather than omitting the section.

### Action Items
A single table:

| # | Action Item | Owner | Due Date | Priority | Status |
| - | ----------- | ----- | -------- | -------- | ------ |

- Owner / Due Date: use **Not specified** when not given in the notes.
- Priority: one of `High`, `Medium`, `Low`, `Not specified`. Only mark High/Medium/Low if the notes give a real signal (urgency language, explicit priority, or a near-term hard deadline) — otherwise use Not specified rather than guessing.
- Status: one of `Not started`, `In progress`, `Blocked`, `Completed`, `Not specified`. Default to `Not specified` unless the notes indicate otherwise.

### Risks and Blockers
Anything flagged as a risk, dependency, or blocker. If none were mentioned, say so.

### Open Questions
Anything left unresolved that needs a future decision or follow-up.

### Missing Information
Call out anything important that's absent from the notes — e.g. no clear meeting date, an action item with no owner, a decision that was discussed but never confirmed. This section is what lets the user quickly see what to go chase down.

## Ground rules

- Preserve the original meaning — don't editorialize or add opinions about what the team *should* have decided.
- Every meaningful action item should make it into the table, even small ones — but don't split one task into multiple rows just to pad the list.
- Keep language plain and concise; this is a working document, not a narrative.
- Never pull in outside information about the topic, company, or people that wasn't in the supplied notes, even if you happen to know it.
- If the notes are extremely sparse (e.g., just a list of names and one sentence), it's fine for several sections to be short or to say "None mentioned" — don't manufacture content to fill every section out.

## Delivery

After presenting the organized notes in the chat reply, also save the result as a Markdown (`.md`) file and share it with the user via the file-presenting tool, so they have something to download and forward. Use a filename based on the meeting topic and/or date if one is mentioned (e.g. `2026-07-13-product-sync-notes.md`), otherwise `meeting-notes-organized.md`. If the user explicitly asks for a Word document instead, use the docx skill to produce a `.docx` file rather than Markdown.

## Example

See `examples/sample.md` for a full worked example (input notes → output record) illustrating the confirmed-vs-assumed judgment calls above.
