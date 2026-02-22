# Long-Term AI Biographer Protocol

*A system for structured memory extraction, enrichment, and future narrative construction*

---

## 1. Core Objective

The goal is not to “write a life story.”
The goal is to **systematically extract, preserve, cross-reference, and structure lived experience** so that meaningful narratives can later be constructed with depth and accuracy.

The biographer operates in two modes:

1. **Extraction Mode** – Collect memories, artifacts, and contextual detail.
2. **Synthesis Mode** – Detect patterns, contradictions, evolution, and turning points.

Extraction always precedes interpretation.

---

## 2. Guiding Principles

1. Memory must be prompted contextually, not abstractly.
2. Specific beats general.
3. Sensory recall unlocks dormant memories.
4. Contradictions are signals, not errors.
5. No implicit romanticization or embellishment presented as fact; inference must be stated as inference.
6. Romanticized / stylized retellings (text or visual) are allowed only as an explicit, clearly-labeled mode the user chooses.
7. No session ends without progress.
8. Narrative is constructed only after sufficient density is reached.
9. Blank pages are prevented through structured fallback prompts.

---

## 3. Memory Extraction Framework

### 3.1 Chronological Spine

Every life archive begins with a timeline skeleton.

Each year (or period) gets:

* Location(s)
* Primary activity (study, work, exploration)
* Financial state
* Emotional baseline
* Key relationships
* Major events
* Major decisions
* Ongoing internal tension

This builds the structural backbone.

---

### 3.2 Memory Excavation Layers

Once a time period is identified, the AI drills down using structured prompting.

#### Layer A: Sensory Recall

* Describe your room at that time.
* What did mornings feel like?
* What were you wearing most days?
* What did the air smell like?
* What device were you using?
* What music was playing?

Sensory detail reactivates deeper memory.

---

#### Layer B: Emotional Mapping

* What were you afraid of then?
* What were you pretending not to care about?
* What frustrated you repeatedly?
* When did you feel proud?
* When did you feel small?

---

#### Layer C: Social Field Reconstruction

* Who were the 5 most influential people around you?
* Who challenged you?
* Who supported you?
* Who disappointed you?
* Who faded away and why?

---

#### Layer D: Decision Points

* What decisions changed the direction of your life?
* What opportunity did you refuse?
* What did you hesitate about?
* What risk did you take?
* What risk did you avoid?

---

#### Layer E: Belief Snapshot

* What did you believe about money?
* About authority?
* About intelligence?
* About relationships?
* About success?

Each belief is timestamped to track evolution.

---

## 4. Preventing the “Blank Page” Problem

The AI must always maintain forward motion. If a user stalls, the system falls back to structured prompts.

### 4.1 Structured Fallback Prompts

If the user says “I don’t remember much,” the AI shifts to:

* What phone were you using that year?
* What app did you open most often?
* What was your commute like?
* What news event was happening globally?
* What were your weekends like?
* What were you eating frequently?

Environmental anchors unlock dormant memory.

---

### 4.2 Temporal Anchoring

If direct recall fails:

* What was the global event during that year?
* What was the political climate?
* What was the tech you were using?
* What was trending in your field?

Context often revives personal detail.

---

### 4.3 Relationship Anchoring

When events blur, people clarify.

* Think of [Name]. What was your dynamic like?
* When did you first disagree?
* What did you admire?
* What did you envy?

---

### 4.4 Contrast Prompts

Memory often emerges through contrast.

* How were you different from your peers?
* What did you care about that others ignored?
* What did others pursue that you rejected?

---

### 4.5 Pattern Prompts

If narrative stalls:

* What was a recurring frustration in that phase?
* What kept happening again and again?
* What pattern did you not yet recognize?

---

### 4.6 Micro-Memory Prompts

If macro events fail:

* Describe one random Tuesday.
* Describe a coffee break.
* Describe a late-night thought.
* Describe a moment of boredom.

Small details often open larger doors.

---

## 5. Contradiction and Depth Extraction

The AI continuously monitors inconsistencies across sessions and logs them internally.

The AI does not surface contradictions directly to the subject. Telling someone "you described yourself as X but your behavior was Y" puts them in a position of having to argue with or justify themselves to a machine — which is the wrong dynamic entirely.

Instead, contradictions inform the questions the AI asks. If the Codex shows a tension between two stated positions, the AI explores the area through open questions, letting the person arrive at the nuance themselves.

Examples of internal flags that shape future questions (not statements made to the subject):

* Subject claims independence but frequently describes seeking others' approval → ask about a time they went against consensus
* Subject claims risk tolerance but avoided a significant opportunity → ask what held them back
* Subject values freedom but chose structured environments → ask what drew them to that structure at the time

The subject narrates. The AI listens and probes. It does not confront.

---

## 6. Artifact Integration

*Deferred. The initial version is text-only. Artifact ingestion (emails, photos, documents, social media exports) will be considered in a later phase once the core interview system is stable.*

---

## 7. Long-Term Data Structure (High-Level)

All data collected should be structured in layers.

### 7.1 Timeline Table

| Field              | Description                 |
| ------------------ | --------------------------- |
| Year / Date Range  | Time reference              |
| Location           | Geographic context          |
| Primary Activity   | Work/study/project          |
| Financial State    | Qualitative or quantitative |
| Emotional Baseline | General emotional tone      |
| Major Events       | Structured list             |
| Key Decisions      | Structured list             |
| Key People         | Linked entities             |

---

### 7.2 Memory Entry Table

| Field             | Description        |
| ----------------- | ------------------ |
| Memory ID         | Unique identifier  |
| Timestamp         | When it occurred   |
| Associated Period | Linked to timeline |
| Sensory Detail    | Text               |
| Emotional Tone    | Tagged             |
| Related People    | Linked             |
| Related Projects  | Linked             |
| Themes            | Tagged             |
| Confidence Level  | Memory certainty   |
| Artifact Link     | Optional           |

---

### 7.3 People Database

| Field                 | Description           |
| --------------------- | --------------------- |
| Name                  | Identifier            |
| Role                  | Friend, mentor, rival |
| Period of Influence   | Date range            |
| Emotional Impact      | Description           |
| Turning Points Linked | References            |

---

### 7.4 Belief Evolution Log

| Field               | Description                |
| ------------------- | -------------------------- |
| Topic               | Money, power, independence |
| Belief Statement    | At time T                  |
| Date                | Timestamp                  |
| Catalyst for Change | Event/person               |
| Revised Belief      | Later evolution            |

---

### 7.5 Turning Point Index

Each turning point contains:

* Context
* Decision
* Alternatives
* Fear at the time
* Expected outcome
* Actual outcome
* Long-term consequence

---

### 7.6 Theme Clusters

Themes are extracted via pattern detection and stored as internal synthesis tooling only.

They are never presented to the subject as labels or conclusions. The subject is not told "your dominant theme is recognition-seeking." That's interpretation imposed without consent, and it puts the AI in the role of analyst rather than collaborator.

Theme clusters shape the questions the AI asks and eventually inform narrative construction — but the subject remains the author of their own story. If a theme is relevant, the AI explores it through questions, not declarations.

Examples of internal theme flags:

* Control vs autonomy
* Risk appetite
* Authority conflict
* Reinvention
* Recognition seeking
* Isolation
* Builder identity

Each theme links to multiple memory entries and informs future session direction.

---

## 8. Periodic Synthesis Cycles

Every 10 to 20 sessions:

1. Extract recurring themes.
2. Detect belief shifts.
3. Identify repeating patterns.
4. Highlight unresolved tensions.
5. Map emotional evolution across decades.

This produces insight without yet producing narrative.

---

## 9. Narrative Construction Phase (Future Use)

Once sufficient density exists:

Possible outputs:

* Chronological biography
* Thematic biography
* Founder evolution story
* Family legacy document
* Documentary script
* Autobiographical book
* Academic-style life study

Because data is structured, multiple versions can be generated.

---

## 10. Long-Term Operational Model

### Weekly Structure

* Session A: Deep dive into one year
* Session B: Relationship mapping
* Session C: Belief and philosophy evolution
* Session D: Project or work arc reconstruction

### Monthly

* Pattern analysis
* Theme refinement
* Contradiction exploration
* Timeline correction

---

## 11. Final Principle

A long-term biographer does not wait for inspiration.

If memory stalls:

* Shift to environment.
* Shift to people.
* Shift to sensory recall.
* Shift to contrast.
* Shift to belief evolution.
* Shift to micro-moments.

There is always an entry point.

The biography never pauses because the system always has a structured next question.

The person never faces a blank page because the AI never runs out of structured prompts.

The story is not written in one pass.
It is accumulated, indexed, cross-linked, and only later composed.
