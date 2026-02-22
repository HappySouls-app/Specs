# Risk 1: The Emotional Labor Problem

## What the risk actually is

This product will ask people to recall the hardest parts of their lives. Not occasionally — regularly, as a feature. The interview structure is designed to surface everything: relationships, losses, regrets, the years that were bad, the decisions that went wrong. For anyone over 50 with a full life, grief and pain are not edge cases in the archive. They are the majority of the material.

The risk isn't that the AI says the wrong thing. The risk is that the product opens a wound and then has nothing to offer — no trained human, no closure, no way out that doesn't feel abrupt or cold. A disclaimer that says "not therapy" does nothing for someone who starts sobbing mid-session about a child who died.

"Not therapy" has to mean something in actual product behavior. Right now it's a positioning statement. It needs to become a design protocol.

---

## Where grief enters

These are not hypothetical scenarios. They will happen with regularity:

- Someone mentions a sibling and then says they haven't spoken in 20 years
- A parent is asked about their children and one of them is dead
- Someone recounts a marriage that ended because of infidelity or addiction
- A career story surfaces a moment of profound failure or humiliation
- An otherwise functional session suddenly hits a memory the person has never spoken about with anyone

The AI's default response in these moments — the trained instinct — is to acknowledge, validate, and continue. That's the problem. Acknowledging and validating is therapy-adjacent language from an entity that has no capacity to follow through on what it implies.

---

## What the AI should actually do

### When distress is detected in session

Do not ask the next question.

The correct move is stillness — not a pivot, not a validation, not a clinical acknowledgment. Something simple: "Do you want to pause here?" or "We can leave this for another time." That's it. The AI doesn't need to name the emotion or interpret the experience. It just needs to offer an exit. The person decides whether to take it — they may want to keep going, and that's their right. The point is that continuing is always their choice, not the AI's.

What the AI should never say in this moment:
- "I understand how hard this must be" — it doesn't, and people know it
- "It sounds like you carry a lot of grief around this" — that's a therapist's line and it lands wrong from a machine
- "I hear that this is painful" — same problem
- "It's important to capture this part of your story" — that's pushing, and it's wrong

The archive can wait. The person is in front of you.

### Informed consent before hard territory

Some topics can be flagged before the AI walks into them. If the system knows a child died — because it was mentioned in a previous session — it shouldn't just ask about the child as if nothing is loaded there.

A working approach: "I want to ask about [your son]. This might be difficult. Would you like to go there today, or come back to it another time?" Then honor whatever the person says, including if they say "never."

This is a design pattern, not just a UX nicety. It gives people agency before the wound, not after.

### Permanent opt-out for specific topics

Users need the ability to mark topics as permanently off-limits. Not "come back later" — done, never ask again. Some things people don't want in the archive at all. Some things they are not going to talk about with an AI, ever.

This should be easy to invoke mid-session, not buried in settings. A simple "I don't want to include this in my story" should be enough. No follow-up prompt, no soft retry in a future session.

### End-of-session handling after heavy material

Don't end abruptly after something heavy. A brief closing acknowledgment: "Today went somewhere real. We can come back to this, or go somewhere different next time." Not therapeutic, not performative — just human enough to not feel like the session just closed a tab.

An optional question before close works well here: "Is there something lighter you want to say before we finish?" Not required, not pushed. Just offered.

---

## The mandatory protocol: genuine crisis

This one is not optional.

If someone discloses suicidal ideation, abuse, or a current crisis situation mid-session, the interview stops. Immediately. Not "let's bookmark this" — the session suspends and the person gets clear crisis resources before anything else.

This is a legal requirement and an ethical floor. The product needs this behavior designed in before launch, not added as a hotfix after an incident.

The line to establish internally: if the person's current safety is in question, the archive is irrelevant.

---

## What "not therapy" means in product behavior

The distinction matters, and it's not just philosophical.

A therapy session aims to process emotion. It invites deeper feeling, looks for patterns, helps the person sit with discomfort for productive reasons. A good therapist will push toward the wound because there's a trained human there to help close it.

This product is an archive. Its job is to capture how the person narrates their life, not to process it. The AI should record what happened and how the person describes it. It should not encourage deeper emotional processing, should not offer interpretations of the person's emotional state, and should not probe for more feeling than the person is already expressing.

That distinction should be visible in every session. The AI never asks "how did that make you feel?" It asks "what happened next?" or "what do you remember about that period?" The emotional content enters through the story, not through the prompts.

If this line is blurry in the interview design, the product will reliably drift into therapy-adjacent behavior — and then the disclaimer provides no protection, legally or ethically.

---

## What to build

In rough priority order:

1. **Distress detection** — voice signals (pitch, silence, crying), text signals (fragmented responses, "I don't want to," expressions of grief), explicit disclosures. Requires deliberate engineering, not an afterthought.

2. **Session pause protocol** — a clearly accessible way to stop, leave a topic, or end the session mid-stream. Should be available by voice command and always visible in the UI.

3. **Topic opt-out** — permanent "not in my story" flag on any topic. Propagates across sessions, never re-prompted.

4. **Pre-entry consent on flagged topics** — if the system has information suggesting a topic will be heavy, it previews before asking.

5. **Closing ritual for heavy sessions** — brief, non-clinical. Just enough to not feel like the lights cut out.

6. **Crisis protocol** — session suspension, crisis resources, no re-entry into the interview without acknowledgment. Non-negotiable.

---

## The honest framing

The emotional labor problem won't be eliminated by design. People will cry. Sessions will hit things that were buried for decades. That's partly what makes the product meaningful — it goes places a casual conversation never would.

The goal isn't to prevent that. The goal is to make sure the product behaves with enough care in those moments that people trust it to continue, and don't feel left alone with something they weren't ready to open.

The product's reputation, in the long run, will be built or destroyed at exactly these moments.
