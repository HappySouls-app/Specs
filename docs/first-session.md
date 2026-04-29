# HappySouls — First Session Design

*The first session is a unique product moment. It has no Codex, no briefing, no biography summary, no prior sessions. The session agent walks in with nothing but the system prompt and the fact that this person has chosen to start.*

*This document specifies what happens in those first minutes — the opening, the scope conversation, the bootstrapping of the fact database, the first extraction question, and how the session closes. It is both a design specification and a script the session agent can follow directly.*

---

## Why the First Session Is Different

Every session after the first has a briefing. The agent walks in knowing something — where things were left, what threads are open, a felt sense of who this person is. It can be specific from the first word.

Session one has none of that. The agent cannot be specific because it knows nothing. But it also cannot be generic — generic is the thing this product is not.

The first session has to accomplish several things at once without feeling like any of them:

- Establish the dynamic: this is a long-term project, not a chat, not therapy, not a one-off
- Set expectations without making it sound like a process
- Collect the basic facts the system needs to function (date of birth, name, where they grew up)
- Open the first real extraction thread and get some actual content
- Make the person want to come back

The first session is the only one where the agenda is fixed. Every session after it is shaped by what has accumulated. Session one shapes everything that follows.

---

## What the Agent Has Available

On first session, the agent loads:

- The system prompt
- An empty fact database
- No biography summary (nothing to summarize yet)
- No biographer's notes
- No briefing
- The default first-session agenda (below), hardcoded in place of a generated briefing

The agent should not behave as though this is a problem. It should behave as though it has been waiting for this conversation.

---

## The First-Session Agenda

Fixed for every new archive. In order:

1. Open with a statement that establishes the nature of the project
2. Understand what this person is trying to preserve and for whom
3. Collect the basic fact set (through conversation, not a form)
4. Open the first extraction thread — one vivid, concrete place from their past
5. Follow with a second thread — a person from that place, or a specific moment within it
6. Close with forward momentum

This agenda is not announced to the user. It is the structure the agent works through invisibly.

---

## The Opening Statement

The agent speaks first. Not with a question — with a brief orienting statement. The purpose is to establish three things without naming them: that this is long-term, that the person controls the pace, and that the job here is to listen.

**Recommended opening:**

> Before we start collecting anything, I want to tell you what this is going to be.
>
> We're not going to tell your life story today. Today is just the beginning of a much longer conversation — one that might run for months or years, across as many sessions as it takes. My job is to ask good questions. Your job is to answer them in your own words, at whatever pace works for you. Some sessions will be long. Some will be fifteen minutes. All of it adds up.
>
> What we're building together is a real record of your life — not a polished version, not a summary, but the actual thing: the specific people, the specific places, the decisions that mattered and the ones that surprised you. The kind of record that can eventually become whatever you want it to be.
>
> Let's start with a question that doesn't require you to know where to begin: **Who is this for?** Is this something you're doing for yourself, for your children, for grandchildren who don't exist yet? There's no right answer — it just helps me understand what we're trying to preserve.

---

### Why This Opening Works

**It names the long-term nature immediately.** The person who thinks this is a one-session thing should know within 30 seconds that it isn't. Better to lose them early than after session five.

**It does not say "I'm an AI" or "I'm a biographer."** Both of those phrases create distance. The dynamic should emerge from behavior, not labels.

**It does not say "I'm not a therapist."** Saying what you're not raises the question. Not saying it lets the person discover what this is through experience.

**It establishes that the person controls the pace.** This is crucial for retention — people should feel no pressure to perform completeness in any given session.

**The first question is about audience, not content.** It is the lowest-stakes question possible. It gathers a piece of the scope declaration without feeling like a form. And whatever the person answers, it opens into something the agent can follow up on naturally.

---

## The Scope Conversation

The answer to "who is this for?" shapes the follow-up. The agent should respond to what was actually said, not run a generic script. But the underlying goals are consistent: understand the scope and register the audience.

**If they say "for myself":**
> So more of a personal archive — your own record, something to return to. Is there a particular period of your life you feel most urgently about capturing? Or would you rather we start at the beginning and work through?

**If they say "for my children" / "for my family":**
> That changes what we prioritize a little — things that might feel obvious to you are often the things your children know least about. We'll make sure we don't skip the early material just because it feels distant. Do they know much about your life before they were in it?

**If they say "I don't know" or are uncertain:**
> That's a completely honest answer, and it doesn't change anything. We'll collect the material first and figure out what to do with it later. The archive works regardless — you can always decide once there's something to decide with.

In all cases, the agent registers the audience in the fact database as an initial scope entry, then moves forward.

---

## Bootstrapping the Fact Database

The system needs a minimal set of facts to function — specifically date of birth and birthplace, which are used to anchor every subsequent memory in the timeline. These are collected through conversation in the first session, not through a form.

The agent does not ask for them directly. It asks questions that produce them naturally.

**Date of birth — collected via:**
> Just so I can orient myself: what year were you born, and where?

This is asked early, framed as the agent needing it to do its job (which is true), not as a data point being collected. If the person gives year only, the agent notes it. If they give a full date, even better.

**Full name — collected via:**
> And what name do you go by — the one you'd want on the archive?

This is usually obvious from account setup, but confirms the preferred name versus a formal name.

**Birthplace and early geography — this is the transition into the first extraction thread.** The agent does not ask for it separately. It asks the first real question, which gathers it implicitly.

---

## The First Real Question

This is the most important design decision in the first session. The first real extraction question sets the tone for everything that follows.

**Requirements:**
- Concrete, not abstract
- Sensory by design — sensory questions unlock dormant memory and demonstrate the style of this work
- Low stakes — no emotional heaviness on day one
- Specific enough to feel purposeful, open enough to allow any answer
- Produces something the archivist can actually work with

**The question:**

> Now — describe a place from your childhood. Not necessarily the most important one. Just one that's vivid for you. The physical space. What it looked like, what it smelled like, what time of day you picture it most.

---

### Why This Question

**It is a place, not a person or an event.** Places are easier to access than relationships or turning points. They come with sensory detail attached. They rarely trigger defensiveness. And they naturally open into the people and events that happened within them — the agent can follow any thread the person opens.

**It is deliberately vague about which place.** "The house you grew up in" narrows too quickly. "A vivid place" lets the person choose, and what they choose is information. The person who immediately describes a school is different from the person who describes a grandmother's kitchen.

**It demonstrates the style of this work immediately.** Asking for sensory detail on the very first question tells the person what to expect: precision, specificity, not high-level summaries. The first question trains the person as much as it gathers data.

**It is genuinely low-stakes.** There are no wrong answers. The person cannot fail to remember their childhood place well enough. This builds confidence before any harder material is attempted.

---

## The Second Move — Person or Moment

One place is a start, not a foundation. If session one ends with a single place and nothing else, the archivist has a location node and a few sensory details — thin material for a briefing and a biography summary that will read like a stub.

The first session should always attempt a second extraction thread before closing, unless the person's energy clearly signals it's time to stop. The second thread is not a separate topic — it grows directly out of the place answer. Two options, chosen based on what the person gave:

**If they named a person in their place description:**
Follow that person immediately.

> You mentioned [name]. Tell me a little about them — who were they to you at that time?

This opens a person node in the graph, creates a relationship edge to the place, and usually surfaces a specific memory without the agent having to ask for one. People unlock stories. The place gets the agent into the room; the person gets the agent into the life.

**If no person was named:**
Ask for a specific moment inside the place.

> I want to get more specific. Think of one particular moment in that place — not the general feel of it, but a single scene. Something that happened there. It doesn't have to be important. Just one that comes to mind.

A moment is different from a place description. It has a before and after. It usually has other people in it. It produces the first real Codex entry — not just a setting, but an event with emotional tone that the archivist can classify, timestamp, and link.

**If energy is clearly low after the place question:**
Skip the second move. Do not push. Close the session and note in the biographer's notes that this person may need shorter, gentler openings.

The session has still succeeded. The second move is the target, not the requirement.

---

## Reading the First Response

The first response reveals how this person naturally communicates. The agent should pay close attention and calibrate accordingly.

**If the response is expansive and detailed:**
The person is comfortable and in flow. Go deeper. Ask about a specific object or a specific time of day. Follow the first thread that has emotional texture rather than switching to a new place or period. Do not interrupt the momentum by pivoting to fact collection — the facts can wait.

*Example follow-up:* "You mentioned [specific detail]. Tell me more about that. What was happening in your life at the time this place felt most like home?"

**If the response is brief or uncertain:**
The person may be nervous, unfamiliar with this kind of reflection, or simply not a natural narrator. Do not push for more. Accept the answer gracefully, anchor it with one specific follow-up, then move on to something easier.

*Example follow-up:* "That's useful. Let me ask something simpler — what age were you, roughly, when you picture that place most clearly?"

**If the person immediately goes to something emotional or difficult:**
Follow with care. Do not shut it down, but do not go deep on session one. Acknowledge what was offered, note it explicitly as something to return to, and redirect to something more concrete for now.

*Example follow-up:* "That sounds like it carries a lot. I want to come back to that — let's put a pin in it for now and return to it once we've built a little more context. For today, tell me about the physical place itself."

---

## Fact Collection During Session One

Beyond date of birth and name, the first session should aim to gather several key facts without making it feel like an intake form. The agent weaves these in naturally as the conversation develops:

- **Birthplace** — usually emerges from the place question
- **Where they grew up** — same
- **Approximate family structure** — "who was in your household?" asked once, not probed heavily
- **Languages / nationalities** — surfaces naturally if relevant to the childhood description; if not, defer to session two

The goal is not to complete the fact database in session one. It is to gather enough that the archivist can generate a minimal biography summary and a real briefing for session two. The minimum viable fact set for that purpose is: name, birth year, birthplace, one early place, one family member named.

---

## Closing Session One

The first session should close before the person is tired, not after. This is more important in session one than any other — the person's last feeling in session one determines whether there is a session two.

**Closing trigger:** After the first extraction thread has reached a natural resting point — not a dramatic conclusion, just a moment of natural pause. The agent closes before the person runs out of material, not after.

**The close:**

> We've made a real start. Let me tell you what I've got, and what we'll pick up next time.
>
> [Brief summary of what was covered — 2–3 sentences in plain language, naming specific things: the place described, any people mentioned, the period of life touched.]
>
> Next time, I want to go back to [specific thing they mentioned] and go a little deeper. I also want to start filling in some of the years around [the period touched]. But that's next time.
>
> You can come back whenever you're ready. There's no schedule. The archive will be here when you are.

---

### Why This Closing Works

**It summarizes concretely.** The person should be able to hear back what they gave and feel that something was received and held.

**It names a specific next thread.** This is the equivalent of the hook at the end of a chapter. *I want to go back to [thing]* makes the next session feel like it has a destination before it begins.

**"Whenever you're ready" removes pressure.** The product lives or dies by whether people feel welcomed back rather than obligated. Obligation kills long-term use.

---

## What the Archivist Produces After Session One

Session one is processed identically to all subsequent sessions, but produces some artifacts for the first time:

- **First fact sheet entries** — name, birth year, birthplace, family structure basics
- **First Codex entries** — the childhood place described; if a second move was made, at least one person profile and one specific memory entry with an emotional tone tag
- **First graph nodes** — place node(s), person node(s) for any named individuals, and an edge linking them if both were collected
- **Scope entry** — the audience and coverage intent captured from the scope conversation
- **First biographer's notes** — observations from this session about how this person communicates, what kind of prompts worked, the emotional texture of the session
- **First biography summary** — generated from the session summary, fact sheet, and graph. Will be sparse. That is expected.
- **First briefing** — with a real agenda for session two based on what was collected. Session two is no longer a cold start.

The archivist should not attempt to generate theme documents after session one — there is not enough material. The default theme stubs are created but left empty. They will fill over time.

---

## Open Design Questions

These are not resolved in this document. They are noted here because the first-session design eventually forces a decision on each.

**The name of the product vs. the name of the agent.** Does the agent have a name? Or is it just "your biographer"? The opening statement avoids this entirely, but eventually the person will ask.

**Onboarding vs. session one.** Is there a pre-session onboarding UI that sets expectations before the agent speaks? Or is the agent's opening statement doing double duty? If there is a UI layer, the opening statement changes.

**The demo session.** Before someone commits to their own archive, should there be a demonstration session with a fictitious person? This would let people experience the dynamic without having to narrate their own life before they understand what they're committing to.

**What happens if someone wants to stop.** The opening statement mentions this is long-term. If someone reaches session one and says "actually I just wanted to try this, I don't want to do the whole thing," what does the agent do? Does the archive stay incomplete indefinitely? Can it be deleted?
