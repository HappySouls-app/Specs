# Docs index

A reference for what each document contains and when to use it.

---

## Foundation

### `vision-ethics.md`
The core product document. Defines what this product is, what it is not, the philosophical foundation, guiding principles, ethical boundaries, and strategic positioning. Read this first when onboarding anyone new to the project, before making any significant product or design decision, and whenever a debate arises about product direction. If something contradicts this document, it needs to be either reconciled or flagged as a deliberate change in direction.

### `initial-summary.md`
The first structured writeup of the project, covering vision, key features, UX concepts, target audience, and open strategic questions. Useful as a starting point for understanding how the idea was first articulated. Less authoritative than `vision-ethics.md` — where they conflict, defer to the latter.

---

## How the product works

### `howto-biography.md`
The operational protocol for the AI biographer. Covers the memory extraction framework, fallback prompts, contradiction handling, data structure, synthesis cycles, and narrative construction. This is the internal spec for how sessions actually run. Not customer-facing — the register is too technical for users. Read when designing prompts, session flow, or the Codex data model.

### `ai-biographer-characteristics.md`
Defines the personality, tone, and behavioral characteristics of the AI. What it sounds like, how it handles silence, what it never says, how it balances warmth with structure. Read when writing or reviewing prompts, evaluating session transcripts, or briefing anyone working on the conversational layer.

---

## Risks

### `risks.md`
The master list of identified blind spots and risks, with brief descriptions of each. Start here when doing a product review or when something feels off. Links conceptually to the individual risk documents below.

### `risks-emotional-labor.md`
Addresses the core emotional risk: the product will regularly surface grief, trauma, and regret, and needs concrete design responses — not just disclaimers. Covers distress detection, in-session protocols, the crisis protocol, what "not therapy" means in practice, and a prioritized build list. Read when designing session behavior around difficult topics, reviewing the crisis protocol, or thinking about legal/ethical exposure.

### `risks-family-or-subject.md`
Addresses the tension between the person who initiates the archive (often an adult child) and the person whose life is being recorded (the subject). Covers ownership, access control, in-session privacy, cognitive decline, and the risk of the product silently serving the family's interests over the subject's. Read when making decisions about account structure, access tiers, consent flows, or anything touching post-death archive custody.

### `risks-aspergers.md`
Addresses a specific blind spot: the warmth and felt experience of sessions is the hardest thing to evaluate from the inside, and the most important thing to get right. Covers how to build an external calibration system — live observation, a warmth collaborator, a prompt review checklist — to compensate for the difficulty of self-evaluating relational texture. Read before any user testing phase and when reviewing session transcripts or prompt quality.

---

## Decisions logged

The following decisions have been made and are reflected across the documents above:

- **Ego-driven high achievers** are a secondary segment. All core design decisions prioritize the elderly/family use case. (`initial-summary.md`, `risks.md`)
- **Artifact ingestion** (emails, photos, documents) is deferred. The initial version is text-only. (`howto-biography.md §6`)
- **Psychological profile** is not an output type. (`howto-biography.md §9`)
- **Contradiction surfacing** to the subject is off. Contradictions are logged internally and used to shape questions, never stated as confrontations. (`howto-biography.md §5`)
- **Theme clusters** are internal synthesis tooling only, never presented to the subject as labels. (`howto-biography.md §7.6`, `vision-ethics.md`)
