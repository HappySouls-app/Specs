# HappySouls — System Architecture

*How context, memory, and agents are structured across sessions and time.*

---

## The Core Problem

A biographical archive grows indefinitely — months and years of sessions, thousands of memory entries, dozens of relationships, evolving beliefs. No single context window can hold all of it. The architecture has to be designed around this from the start.

The solution is not to fit more into one context. It is to design a system where each agent only holds what it actually needs.

---

## Two Agents, Not One

The system is built around two agents with completely different jobs.

### The Session Agent

Talks to the user. Handles the live conversation — listening, asking questions, reading energy, parking threads, flagging new entities. Warm, present, unhurried.

It does not hold the full Codex. It holds a briefing. It retrieves specific things on demand via tool calls when the conversation requires them.

It also manages **segments** — the topical units a session naturally falls into. When the conversation shifts to a meaningfully new subject or thread, the agent closes the current segment, compresses it into a summary, and starts a fresh one. At any point, its working context holds: the current segment in full, compressed summaries of all prior segments from this sitting, and the Tier 1 permanent load. When the session ends, the agent hands the archivist a pre-segmented record — not a monolithic transcript. This lets both agents scale cleanly to sessions of any length.

### The Archivist Workflow

Never talks to the user. Runs between sessions. Its job is compression and structure — extracting everything from the session transcript and updating all long-term stores.

The archivist is not a single monolithic agent running everything sequentially. It is a **workflow of specialized steps**, some sequential where there are dependencies, some parallel where there are not. A theme extraction step acts as a gate that determines which downstream update steps get triggered. Steps that don't depend on each other run simultaneously.

The full workflow is described in The Archivist Workflow section below.

These two roles are genuinely different and should not be merged. Keeping them separate means the session agent can stay fully in conversation mode without juggling extraction mechanics, and the archivist workflow can be exhaustive without time pressure.

---

## The Archivist's Completion Model

The archivist is not purely reactive. Beyond processing each session's transcript, it maintains a **completion model** — a definition of what a well-formed archive looks like — and continuously measures the current archive against it. The gap between current state and the completion model is the archivist's priority list. It feeds directly into the briefing generator: the most urgent gaps become the suggested threads for the next session.

The completion model has seven dimensions.

---

### 1. Timeline Density

Is the life covered evenly across time?

The archivist divides the person's life into periods (roughly by decade, or by recognizable life chapters) and tracks a coverage score for each. No major period should be empty without a declared reason. Periods with very few Codex entries are flagged as density gaps and promoted to the session agenda.

---

### 2. Fact Sheet Completeness

Is the predetermined set of atomic facts filled in?

The fact database has a fixed checklist of facts that should be present for any archive to be functional: full name, date of birth, birthplace, nationalities, languages spoken, family structure (parents, siblings, partner, children), places lived, education, professions, and hobbies. The archivist tracks which are `confirmed`, which are `stated` or `inferred`, and which are entirely missing. Missing facts are surfaced as low-friction thread targets — easy wins for sessions where energy is low.

---

### 3. Extraction Layer Completeness Per Period

Is each covered period explored in depth, not just breadth?

A period can have many Codex entries and still be thin. Twelve event entries for the 1990s with no sensory detail, no emotional layer, and no belief snapshot is wide coverage but shallow depth. The archivist tracks which of the five extraction layers have been touched for each period:

- Sensory recall
- Emotional mapping
- Social field (key people)
- Decision points
- Belief snapshot

A period is not considered well-formed until all five layers have at least some coverage.

---

### 4. People Depth

Are important people explored proportionally to their significance?

Named people in the graph accumulate an implicit importance score based on how frequently they appear across sessions and how many Codex entries reference them. The archivist flags people whose mention frequency is high but whose dedicated exploration is low — a person mentioned 15 times across 8 sessions who has never been the subject of a drilling session is a gap. These become candidate person threads on the Kanban.

---

### 5. Belief Coverage Across Time

Is the belief log populated across all life phases, not just recent years?

The belief evolution log should ideally have entries across the full timeline — not just from the person's current vantage point. If beliefs about authority, money, or relationships are only logged from age 40 onward, the earlier decades are missing a dimension. The archivist tracks belief topic × life period as a coverage matrix and flags empty cells.

---

### 6. Life Chapter Structure

Does the archive have recognizable narrative shape?

Beyond year-by-year density, the archivist tracks whether recognizable life chapters are represented: childhood, adolescence, education, early career, first relationships, family formation, mid-life, later life. A chapter that is entirely absent — not sparse, but absent — is a structural gap distinct from a density gap. It means something significant may never have been raised.

---

### 7. Thread Hygiene

Is the Kanban board staying useful?

Threads that have been `surfaced` for many sessions without being promoted are either worth scheduling or worth quietly closing. The archivist flags stagnant threads so a deliberate call can be made. A Kanban board full of threads no one has touched in months is noise, not inventory.

---

### The Scope Declaration

The completion model needs to know what this archive is trying to cover before it can assess gaps meaningfully. Someone archiving only their professional life should not have childhood coverage flagged as missing. Someone creating a family heritage document has different priorities than someone doing a full life archive.

At the start of an archive, a scope declaration is set:

- **Coverage scope** — full life / professional arc / specific period / family history
- **Depth priority** — which extraction layers matter most for this person's goals
- **Audience** — who will eventually access this archive (self / children / grandchildren / public)

The archivist's completion model is calibrated against the scope. Gaps are only flagged if they fall within the declared scope. This also means the minimum viable archive threshold — the point at which synthesis becomes possible — is scope-dependent.

---

### The Headline Metric

The archivist maintains a single summary score across all seven dimensions: an overall archive completeness percentage. This is not a vanity metric — it is what tells the person how far along their archive is, and what tells the briefing generator how urgently to prioritize gap-filling over depth-drilling. Early in the archive, the headline metric drives almost everything. Later, when the broad structure is in place, depth and belief coverage become the remaining work.

---

## Context Tiers

### Tier 1 — Always in context (permanent load)

- The system prompt — behavior, identity, ethical constraints
- The fact sheet — the full contents of the fact database (tiny, always relevant)
- The biography summary — a 400–700 token portrait of the person: life arc, key relationships, recurring themes, voice and tone notes. Updated every 24 hours by the archivist if new sessions have occurred.
- The biographer's notes from the last 2–3 sessions — recent observations about how this person has been showing up: what unlocked memory, what fell flat, what was circled but not landed on
- The session briefing — pre-generated by the archivist after the previous session
- The current session agenda — top 3–5 threads from the Kanban board

This is bounded and small. The fact sheet and biography summary cost very little. The biographer's notes are recent only — not the full history, just enough to carry the feel of recent sessions forward.

### Tier 1b — Current session working context

Within a session, the agent also holds:

- **Current segment** — the full live conversation since the last segment boundary. This is what gives the agent moment-to-moment continuity and lets it respond naturally.
- **Prior segment summaries** — compressed summaries of every completed segment from this sitting. Each is a few hundred tokens — enough to recall what ground was covered without holding the full exchange.

Together, Tier 1 and Tier 1b define the agent's total working context. Neither is static: the current segment grows until a boundary is detected, then compresses and moves to the prior-segment list. The Tier 1 permanent load never changes during a session.

### Tier 2 — Dynamic, loaded on demand during the session

The session agent retrieves specific things when the conversation requires them:

- `theme.get(name)` — when the conversation enters a theme's territory, load the full theme document
- `codex.search(query)` — when a specific memory or moment is needed, retrieve relevant entries
- `period.get(range)` — when a time period opens up, get the current coverage picture
- `person.get(name)` — when a relationship deepens, get the full profile
- `belief.get(topic)` — when a belief topic resurfaces, get the evolution log for it

Only load what the current conversation actually needs. Theme documents are the highest-level retrieval — they give synthesized thematic context rather than individual fragments. The agent calls `theme.get("career")` when the conversation enters career territory, not a battery of individual Codex searches.

### Tier 3 — Out of context, accessible only via tools

- The full Codex — all memory entries across all sessions
- The full people and relationships graph
- The full timeline with coverage data
- The full Kanban board — agent sees only the top queued items
- All archived session transcripts

### Tier 4 — Post-session batch processing

Not in any live context. Runs after the session ends:

- Full transcript analysis and entity extraction
- Codex writes
- Thread card creation and state updates
- Timeline density recalculation
- Briefing generation for the next session

---

## The Briefing

The briefing is the mechanism that makes continuity possible without infinite context. It is generated by the archivist after every session and loaded by the session agent at the start of the next one.

It is not the Codex. It is the archivist's editorial judgment about what the session agent needs to walk in prepared.

**Contents:**

- **Last session** — where it ended, what was left open, the mood it closed on.
- **Top open threads** — the 3–5 most relevant cards from the Kanban with their current states.
- **Timeline gaps** — which periods are currently sparse and worth targeting.
- **Relevant Codex entries** — 5–10 entries most likely to matter for today's agenda, selected by the archivist based on the queued threads.
- **Belief evolution snapshot** — current state of any belief topics recently active.

The identity summary that previously lived here has moved to the Biography Summary — a dedicated, slowly-updated artifact that does that job better than a paragraph inside a session-specific document.

Target size: 1,500–3,000 tokens. Tighter than before, since the biography summary and biographer's notes now carry the person-level context separately.

---

## The Biography Summary

A 400–700 token portrait of the person as they currently appear in the archive. Not a list of facts — the fact database handles those. Not session logistics — the briefing handles that. The biography summary is what gives the session agent a felt sense of who it is talking to.

**What it contains**

- **Life arc** — the shape of the person's life so far in compressed narrative form: key periods, major transitions, the through-lines that connect them
- **Key relationships** — the most significant people and places by name, with a phrase of context each
- **Recurring themes** — what keeps coming up across sessions, stated as observation not conclusion (*"career decisions consistently framed in terms of escape rather than pursuit"*)
- **Voice and tone notes** — how this person speaks, what kinds of prompts unlock them, where they tend to open up or deflect, whether they frame things narratively or analytically

**Update cycle**

Regenerated every 24 hours by the archivist, but only if at least one new session has occurred since the last update. No new sessions means nothing has changed. The trigger is: *24 hours elapsed AND new session content exists.*

Regeneration is incremental — the previous biography summary is the starting point, and only new session material changes it. The archivist does not rewrite from scratch; it evolves. This preserves continuity and keeps the update cost low regardless of archive age.

First generation — when no previous summary exists — reads all available session summaries, the full fact sheet, the top entity nodes from the graph, and the belief evolution log. Expensive once, then incremental forever.

**The length constraint is a hard rule**

Without it, biography summaries grow with each cycle. The archivist must be explicitly instructed to stay under 700 tokens and to cut rather than append when new material arrives.

**What it is not**

It is not a Wikipedia article about the person. It is not a neutral summary of events. It should read like notes a thoughtful biographer would write before walking into a session — warm, specific, attentive to texture, not just facts.

---

## The Biographer's Notes

A free-form artifact produced by the archivist after every session. Where the session summary captures what was said, the biographer's notes capture what was noticed.

These are the private working notes of the biographer — the qualitative, observational layer that no other artifact holds.

**What goes in them**

- **Voice observations** — particular phrases the person reaches for, how they frame things, the metaphors they use instinctively. Patterns in language that reveal how they think.
- **Energy map** — where they became animated and went into flow, where they became flat or evasive. What kind of prompt unlocked a particularly rich passage. What shut things down.
- **What wasn't said** — things implied but not stated, topics circled without being landed on. Gaps that felt deliberate rather than accidental.
- **Prompt effectiveness** — which questions worked well in this session, which fell flat. Accumulated across sessions, this becomes a calibration guide for interviewing this specific person.
- **Tone of the session** — was it reflective, nostalgic, guarded, expansive? Did the tone shift mid-session, and around what?
- **The biographer's hunches** — subjective interpretations, clearly labeled as such. *"Seems to use humor consistently as an entry point to painful topics."* Private working observations, not conclusions.

**Format and size**

Free-form prose, not structured. Actual notes, the way a human biographer would write in a private journal after a conversation. 200–400 tokens per session. The size constraint is important — without it, notes become summaries of summaries.

**How they are used**

The last 2–3 sessions' biographer's notes load into Tier 1 alongside the briefing. They answer a different question: not *"what is this person's life arc?"* but *"how has this person been showing up lately?"* A person who was guarded two sessions ago but opened up significantly in the last session is different to approach than one who has been consistently expansive.

The archivist also reads accumulated biographer's notes when regenerating the biography summary — specifically for the voice and tone section, which has no other good source.

**The ethics of this artifact**

These notes are never shown to the user as analysis of who they are. Their purpose is to make the agent a better interviewer, not to profile the subject. The tone must be warm and curious, not clinical. They are stored as interpretation and never promoted to fact.

**Per-session artifact summary**

| Artifact | Captures | Format | Size |
|---|---|---|---|
| Raw transcript | Everything said, verbatim | Unstructured | Full length |
| Session summary | What was said, compressed | Structured | 500–800 tokens |
| Biographer's notes | What was noticed | Free-form prose | 200–400 tokens |
| Fact sheet updates | New confirmed facts | DB writes | — |

---

## Theme Documents

Living documents that accumulate the archive's knowledge about specific domains of the person's life — career, family, childhood, love, parenthood, and others. They sit between the biography summary (the whole person, compressed) and raw Codex retrieval (individual memories, granular). Theme documents give the session agent synthesized thematic depth without loading the entire archive.

**What they are not**

Not a list of Codex entries. Not a period summary. Not a structured fact sheet. A theme document is a coherent narrative portrait of one dimension of a life — the arc, the pivots, the key people within that domain, the recurring patterns, the emotional texture. It should read the way a thoughtful biographer would brief a colleague before a conversation focused on that theme.

**The default set**

Some themes are universal enough to exist from the start, even if initially empty:

- Childhood & origins
- Family of origin
- Education
- Career & work
- Love & romantic relationships
- Friendship
- Parenthood
- Health & body
- Home & places
- Money & material life
- Loss & grief
- Creative life & passions
- Identity & self-perception

**Emergent themes**

Others are created by the archivist when enough material warrants it — immigration, faith, illness, reinvention, exile. These are not declared upfront. The archivist detects during theme extraction that a significant theme has accumulated critical mass and creates the document. The session agent treats emergent and default themes identically once they exist.

**How they are loaded**

Not always in context. Loaded on demand by the session agent when the conversation enters a theme's territory — `theme.get("career")`. The session agent decides which themes to pull based on what is being discussed. No cross-linking between theme documents is needed; that intelligence belongs in the agent, not in the documents themselves.

If the briefing flags a theme as central to today's planned session, it can be pre-loaded alongside the briefing rather than waiting for an in-session call.

**How they are updated**

The archivist's theme extraction step identifies which themes were touched in a given session. Only those theme documents are updated — sessions that don't touch a theme leave its document unchanged. Each update is incremental: the existing document is the base, relevant new material from the session summary is folded in. Never rewritten from scratch.

The archivist also logs a session↔theme junction entry each time a theme document is updated — making it queryable in both directions: which sessions contributed to the career theme, and which themes were active in session 14.

**Size and format**

400–800 tokens each. Narrative prose, not structured lists. The length constraint is a hard rule — without it, theme documents grow into unwieldy compilations. The archivist must cut rather than append when new material arrives.

---

## The Archivist Workflow

The archivist runs after every session as a structured workflow. Its input is not a monolithic transcript — it is the pre-segmented session record produced by the session agent: a list of segments, each with its compressed summary, entity flags, and thread flags, plus the final segment as a full transcript.

Steps that have dependencies run sequentially. Steps that are independent run in parallel. Per-segment processing and cross-session synthesis are handled as distinct phases.

```
Session (segmented)
  [Segment 1: summary + flags]
  [Segment 2: summary + flags]
  [Segment N: full transcript]
  │
  ├─ [PER SEGMENT, PARALLEL] ────────────────────────────────────────┐
  │   transcript.summarize()    full summary for final segment;       │
  │                             summaries already exist for prior ones │
  │   notes.write()             biographer's notes (across session)   │
  │   facts.set() / infer()     fact database updates                 │
  └───────────────────────────────────────────────────────────────────┘
  │
  ├─ theme_extraction()          ← runs on session-level summary (not raw)
  │     identifies which themes were touched; creates new theme docs if needed
  │     └── [themes: career, family, ...]
  │           │
  │           └─ [PARALLEL] ─────────────────────────────────────────┐
  │               theme.update("career")                             │
  │               theme.update("family")   one step per touched theme │
  │               theme.update(...)                                  │
  │             ──────────────────────────────────────────────────────┘
  │
  ├─ [PARALLEL, depends on summarize()] ──────────────────────────────┐
  │   codex.add_entries()        memory entries with entity links     │
  │   graph.update()             entity nodes + session↔entity links  │
  └───────────────────────────────────────────────────────────────────┘
  │
  ├─ [PARALLEL, depends on codex + graph] ────────────────────────────┐
  │   timeline.update_density()                                       │
  │   thread.process()                                                │
  │   completion.score()                                              │
  └───────────────────────────────────────────────────────────────────┘
  │
  ├─ biography.update()          ← conditional: only if 24h elapsed + new sessions
  │
  └─ briefing.generate()         ← depends on everything above
```

`theme_extraction()` runs on a session-level summary, not on individual segment summaries or the raw transcript. The archivist first stitches the per-segment summaries into a single session-level summary, then runs theme extraction on that. This prevents theme detection from being fragmented across segments and keeps it consistent with prior sessions.

Prior segments already arrive with compressed summaries. The archivist's `transcript.summarize()` step only needs to produce a full summary for the final segment (which arrives as a raw transcript) before stitching them together.

---

## The Fact Database

A small, structured database of discrete, atomic facts about the person. Not narrative, not memory — just things that are simply true and stable.

Its entire contents load into the session agent's context at the start of every session. At a few hundred tokens, it costs almost nothing. The agent never needs a tool call to retrieve a birth year or a parent's name — it already has it.

**Schema**

```
facts
├── fact_id       unique identifier
├── category      identity | family | geography | career
├── key           e.g. date_of_birth, father_name, birthplace
├── value         e.g. 1972-01-21, Frank, Lyon
├── confidence    confirmed | stated | inferred
└── session_id    source session where this was first recorded
```

**Confidence levels**

- `confirmed` — user stated it directly and unambiguously
- `stated` — user mentioned it in passing; recorded but not the focus of the session
- `inferred` — derived by the archivist from context (*"I was 20 when the wall fell"* → born ~1969). Flagged for the session agent to verify at a natural moment

`inferred` facts are included in the fact sheet but visually distinguished in the briefing so the agent knows to confirm them without making it feel like a quiz.

**Categories and example keys**

| Category | Example keys |
|---|---|
| `identity` | `full_name`, `date_of_birth`, `birthplace`, `nationalities`, `languages` |
| `family` | `father_name`, `mother_name`, `siblings`, `partner_name`, `children` |
| `geography` | `current_city`, `current_country` |
| `career` | `current_profession`, `primary_field` |

The category list is not fixed. The archivist adds new categories as the archive matures and new stable fact types emerge.

**What does not belong here**

Anything with nuance, evolution, or emotional weight belongs in the Codex or the graph. *"Father's name: Frank"* is a fact. *"My father was a complicated man"* is a Codex entry. *"Frank Hartwig, engineer, emotionally distant in Chris's childhood, reconciled in his 40s"* is a graph node with edges.

The fact database is not trying to be a summary of the person. It is a lookup table — fast, flat, and always available.

**Temporal resolution**

The archivist uses the fact database to anchor Codex entries in time. When a user says *"I was around 20 when that happened,"* the archivist computes the approximate year from `date_of_birth` and stores it as the entry's life timestamp. This makes the timeline density calculation accurate without the user needing to remember exact dates.

---

## Session Boundaries

A session ends on one of three triggers:

- **User-driven** — the user signals they are done, or naturally goes quiet.
- **Energy-driven** — the agent reads sustained low engagement and wraps up cleanly rather than pushing on.
- **Narrative-driven** — a thread reaches a natural close, and the agent offers to stop there.

Sessions are not time-bounded by the clock. Ending at 30 minutes because a timer ran out is mechanical and wrong. Sessions end when the conversation finds a natural pause point.

The session context itself is modest by design — the live transcript plus the briefing plus a few retrieved Codex entries. Somewhere in the range of 20,000–40,000 tokens is likely sufficient for a full session. The richness of years of data lives outside the context window and comes in only when queried.

---

## Segments

A session is a temporal unit — one sitting. A segment is a topical unit within that sitting — a stretch of conversation that belongs to a coherent subject or thread before the conversation meaningfully shifts.

The session agent creates segment boundaries. The archivist does not.

**Why segments exist**

Without segmentation, a session is a single undivided context block. For a 30-minute session this is fine. For a 2-hour session covering four different periods and six different people, a single block becomes unwieldy. And in extreme cases — a user who talks for four hours, or who does an intense weekend marathon session — a monolithic transcript would blow past any practical context limit for both the session agent and the archivist.

Segments solve this by keeping the live context bounded regardless of how long the session runs. Only the current segment lives in full detail. Everything prior is already compressed.

**How boundaries are detected**

The session agent monitors for topical shift signals:

- The subject of conversation changes significantly — from a person to a period, from a theme to a specific event, from one era of life to another
- A thread is explicitly closed (*"I think that's all I remember about that"*)
- Collection Mode ends and Drilling Mode begins on a new topic — the sweep is a natural boundary
- The agent explicitly transitions (*"Let's set that aside for now and come back to..."*)

Boundaries are judgment calls, not keyword triggers. The agent uses contextual understanding to decide when the conversation has genuinely moved on rather than momentarily digressed.

**What happens when a segment closes**

1. The agent calls `segment.compress(summary)` — producing a 200–400 token summary of the completed segment: who and what it covered, key memories surfaced, entities flagged, threads opened or closed
2. The compressed summary moves into the prior-segment list (Tier 1b)
3. The full detail of the closed segment is released from the live context
4. A new current segment begins

The agent carries forward continuity naturally. It knows what the previous segment was about from its compressed summary. It can refer back to earlier content in the sitting — *"you mentioned your father earlier — how does that connect to what you're describing now?"* — without holding the full transcript of that earlier segment.

**What the archivist receives**

At session end, the agent does not hand over a single raw transcript. It hands over a **segment list**:

```
Session
  └── Segment 1  (compressed summary + entity flags + thread flags)
  └── Segment 2  (compressed summary + entity flags + thread flags)
  └── Segment 3  (current — full transcript, not yet compressed)
  └── ...
```

The final segment (the current one at close) is passed as a full transcript. All prior segments are passed as their compressed summaries plus the structured flags the agent accumulated during them.

The archivist processes each segment independently where possible, then synthesizes across them for graph updates, thread processing, and briefing generation.

**Bulk ingestion**

For extreme cases — importing recorded conversations, intensive multi-hour sessions, retrospective diary entries — a dedicated bulk ingestion mode exists. The input is pre-chunked by the ingestion tool before any agent processes it. Bulk ingestion bypasses the live session agent entirely: chunks go directly to the archivist workflow, which treats each chunk as a segment. The biography summary and briefing are regenerated once at the end of the full batch, not after each chunk.

---

## The Codex

The Codex is the primary long-term store. Every memory entry has:

| Field | Description |
|---|---|
| Entry ID | Unique identifier |
| Life timestamp | When it occurred in the user's life (approximate is fine) |
| Session source | Which session it came from |
| Content | Free text, in the user's own words |
| Entity links | People, places, organizations mentioned — linked to their graph nodes |
| Themes | Emotional tone, recurring topics |
| Confidence | How certain the user was of this memory |
| Type | Memory / Interpretation / Belief / Event / Relationship note |

The entity links are what connect the Codex to the graph. Each memory entry points to the specific entity nodes involved, and each entity node accumulates those back-references over time. A Codex entry about a childhood home links to the place node for that home and to any people who were there — making it navigable from multiple directions.

Memory and interpretation are always stored separately and tagged accordingly. The Codex never presents interpretation as remembered fact.

Complexity and evolution live here naturally. If someone describes their father one way in session 3 and differently in session 14, both entries exist, both timestamped. There is no flag that marks one as contradicting the other. The record is richer for holding both. That is not a problem to resolve — it is an accurate portrait of a person across time.

---

## The People, Places and Relationships Graph

A graph where nodes are people, places, organizations, and objects with emotional weight — and edges carry relationship type, time period, and emotional valence.

Neither the graph nor the sessions own the link between them. A junction sits between them, created by the archivist at ingestion time:

```
Session ←→ [Junction] ←→ Entity (Person / Place / Organization / Object)
```

Each junction entry holds: session ID, entity ID, entity type, and a brief context note about how the entity appeared in that session. One archivist operation creates both sides of the link.

This makes the relationship queryable in any direction without either side owning the other:

- *"Every session where Marie appeared"* → filter junction by entity
- *"Every person and place mentioned in session 14"* → filter junction by session
- *"When did this place first appear, and in what context?"* → sort by session date, filter by entity
- *"Which people and places co-appear most often?"* → find sessions they share in the junction

Because Codex entries also link to entity nodes, the full navigable chain becomes:

```
Entity node
  → Junction entries (all sessions where it appeared)
  → Codex entries (all specific memories involving it)
  → Other entities linked through those same memories
```

A person node in the graph is therefore not just a profile — it is a live index of everywhere that person has appeared in the archive, with direct access to the sessions and memories involved.

The graph is updated by the archivist after each session, not during the live conversation.

---

## Transcript Summaries

Raw session transcripts are preserved in full but are not the primary resource for navigation or retrieval. People do not speak in optimal density — they circle back, repeat, hesitate, go on tangents, and sometimes spend twenty minutes reaching a point that could be stated in two sentences. The raw transcript is the faithful record. The summary is the usable one.

The archivist produces a session summary after every session as a distinct artifact. It is not a replacement for the transcript — both are kept. The summary is what gets used downstream: by the briefing generator, by the session agent when it needs to recall a past session, and by the person themselves when reviewing what they've covered.

**What a session summary preserves:**

- Every named entity that appeared — people, places, organizations — even if only mentioned in passing
- Every discrete memory or event described, in compressed form
- The emotional tone of the session and any notable shifts within it
- Open threads raised and whether they were parked or deferred
- Belief statements made, flagged for the belief evolution log
- The life timestamp range the session covered

**What it drops:**

- Conversational filler and hesitations
- The agent's questions (compressed to a single line at most, or omitted entirely)
- Repetitions of material already in the Codex
- Tangential asides that yielded no new content

**What it never does:**

It does not paraphrase the person into cleaner language. Key moments are quoted directly, or very lightly compressed while preserving the person's own words. The summary is a distillation, not a rewrite.

**Summary tiers**

Beyond the per-session summary, the archivist can generate higher-order summaries on demand:

- **Period summary** — a synthesis across all sessions that covered a specific era of the person's life. Useful when a period has been touched across multiple non-contiguous sessions and the agent needs a coherent picture before going deeper.
- **Person summary** — a synthesis of every mention and session involving a specific individual across the full archive. Generated on demand when a person thread is queued for deeper exploration.
- **Theme summary** — a compressed view of how a specific belief topic or emotional theme has appeared across sessions over time.

These are generated by the archivist on demand rather than after every session — they are expensive and only needed when the session agent or the briefing generator requires a cross-session synthesis of a specific entity or topic.

---

## The Kanban Thread Board

Open threads move through five states:

| State | Meaning |
|---|---|
| `surfaced` | In the backlog. Mentioned but not yet scheduled. |
| `queued` | In the next session agenda. |
| `parked` | Deferred — started or explicitly set aside, to be revisited. |
| `blocked` | User not ready. Agent does not raise it. |
| `done` | Complete enough for now. Can reopen if a new angle surfaces. |

Transitions:

```
surfaced → queued | blocked
queued   → parked | blocked | done
parked   → queued | blocked | done
blocked  → parked | done
done     → parked  (if a new angle surfaces)
```

`active` is not a state — it is implicit during a session. The agent knows what thread it is working on without the board needing to reflect it. When the session ends, the thread moves to `parked` or `done`.

`done` is not permanent. For a biographical archive that runs across years, almost nothing is definitively finished — person threads, period threads, and belief threads can always have more. If a new memory surfaces that opens a new angle on a completed thread, the archivist moves it back to `parked` and it re-enters the flow.

Thread types affect how the agent handles transitions:

- **Period threads** — a decade or era not yet covered. Moves cleanly through the states. Reaches `done` when coverage is sufficient across all extraction layers.
- **Person threads** — a named relationship. Frequently returns from `done` to `parked` as new context surfaces in unrelated sessions.
- **Belief threads** — a recurring topic (identity, risk, authority). Rarely reaches `done`. Each resurfacing adds a new dated entry to the belief evolution log rather than cycling through Kanban states.

The backlog is not a failure state. It is inventory. The agent is not racing to close threads — it is accumulating material for the long term.

---

## The Belief Evolution Log

A timestamped ledger of stated positions on recurring topics — identity, money, authority, risk, relationships, success, belonging. Separate from the Codex because beliefs are explicitly tracked across time, not just stored.

Each entry: the topic, the stated belief, the date, and the session it came from. The log makes it possible to ask, years later: *"Here is how your thinking about risk has changed since we started. Does that feel accurate?"*

This is different from contradiction tracking. It is not looking for inconsistencies to probe. It is preserving evolution as a feature of a real life — the record of a person thinking across time.

---

## Session Modes

Within a single session, the agent operates in two distinct modes and switches between them:

**Collection Mode** — the user is narrating freely, covering ground fast. The agent listens. It does not interrupt to follow up. It flags entities and events for the archivist to process later. Every new name, place, or period mentioned becomes a `surfaced` thread. The agent's only job here is to hold space and not break the flow.

**Drilling Mode** — the user has slowed or paused. The agent selects one thread from what has surfaced and goes deep. Not three things. One. It asks follow-up questions, pursues sensory detail, and stays with the thread until it reaches natural depth or the user's energy drops.

The transition from Collection to Drilling is the craft. After a sweep, the agent acknowledges the breadth and anchors to one thing:

*"You covered a lot of ground there. I've got it all. Before we move on — you mentioned [name]. Tell me about the day you met them."*

Everything else from the sweep enters the Kanban as `surfaced`. The agent resists the urge to chase all of it at once.

---

## Tool Classification

### Session agent tools — fast, read-heavy, in-conversation

| Tool | Purpose |
|---|---|
| `theme.get(name)` | Load a theme document when the conversation enters that domain |
| `codex.search(query)` | Retrieve relevant memory entries by semantic search |
| `person.get(name)` | Get a person's profile from the relationship graph |
| `period.get(range)` | Get coverage and known entries for a time period |
| `belief.get(topic)` | Get the evolution log for a belief topic |
| `thread.get_agenda()` | Get the current session's top queued threads |
| `thread.park(topic, note)` | Park an open thread with a note for the archivist |
| `entity.flag(name, type)` | Lightweight flag for post-session processing |
| `segment.compress(summary)` | Close the current segment, store its summary, start a new one |

### Archivist workflow steps

| Step | Runs when | Purpose |
|---|---|---|
| `transcript.summarize()` | Every session | Structured session summary from raw transcript |
| `notes.write()` | Every session | Biographer's notes for the session |
| `facts.set() / facts.infer()` | Every session | Fact database updates and inferences |
| `theme_extraction()` | Every session, on summary | Identify touched themes; create new theme docs if needed |
| `theme.update(name)` | Per touched theme | Incrementally update each theme document |
| `codex.add_entries()` | Every session | Memory entries with entity links |
| `graph.update()` | Every session | Entity nodes + session↔entity and session↔theme junctions |
| `timeline.update_density()` | Every session | Recalculate timeline coverage scores |
| `thread.process()` | Every session | Create and update Kanban thread cards |
| `completion.score()` | Every session | Recalculate headline metric and dimension scores |
| `belief.log()` | When belief statements found | Add entries to the belief evolution log |
| `summary.generate_period()` | On demand | Synthesize a period summary across relevant sessions |
| `summary.generate_person()` | On demand | Synthesize all appearances of a specific person |
| `biography.update()` | Conditional (24h + new sessions) | Regenerate biography summary |
| `briefing.generate()` | Every session, last step | Generate the briefing for the next session |

---

## What This Architecture Does Not Do

It does not maintain a contradiction register. Complexity and evolution are preserved naturally in the Codex — both versions of a memory live there, timestamped, without a flag marking one as conflicting with the other. The agent is not a covert analyst looking for inconsistencies to probe. It is a collaborator. The record is richer for holding the full picture, not cleaner for resolving it.

---

## The Flow

```
SESSION ENDS
  Agent names what will be picked up next time
  Segment list → archivist workflow
    [Segment 1: compressed summary + entity/thread flags]
    [Segment 2: compressed summary + entity/thread flags]
    [Segment N: full transcript + entity/thread flags]

ARCHIVIST WORKFLOW
  Pre-step — summarize final segment, stitch into session-level summary

  Step 1 — parallel, no dependencies
  ├── notes.write()
  └── facts.set() / infer()

  Step 2 — theme extraction (runs on session-level summary)
  └── theme_extraction()  →  [career, family, childhood, ...]
        └── parallel per touched theme
            ├── theme.update("career")
            ├── theme.update("family")
            └── theme.update(...)    ← creates new doc if theme is new

  Step 3 — parallel, depends on session summary
  ├── codex.add_entries()
  └── graph.update()        ← entity nodes + session↔entity + session↔theme junctions

  Step 4 — parallel, depends on codex + graph
  ├── timeline.update_density()
  ├── thread.process()
  └── completion.score()

  Step 5 — on demand only
  ├── summary.generate_person()
  └── summary.generate_period()

  Step 6 — conditional (24h elapsed + new sessions)
  └── biography.update()

  Step 7 — final, depends on everything
  └── briefing.generate()

SESSION STARTS
  Agent loads into context:
  ├── System prompt                       (Tier 1, permanent)
  ├── Fact sheet                          (Tier 1, always, tiny)
  ├── Biography summary                   (Tier 1, always, ~600 tokens)
  ├── Biographer's notes (last 2–3)       (Tier 1, always, recent feel)
  └── Briefing + session agenda           (Tier 1, always, tactical)

DURING SESSION
  Agent reads energy → Collection Mode or Drilling Mode

  Session context (Tier 1b, evolves during session):
  ├── Current segment          full detail, grows until boundary detected
  └── Prior segment summaries  compressed, 200–400 tokens each

  On boundary detected:
  └── segment.compress(summary)   close segment, start new one

  On demand via tool calls:
  ├── theme.get(name)       conversation enters a theme's territory
  ├── codex.search(query)   specific memory or moment needed
  ├── person.get(name)      relationship being explored
  └── period.get(range)     time period opening up

  As things surface:
  ├── entity.flag(name, type)    flag for archivist
  └── thread.park(topic, note)   conscious deferral
```
