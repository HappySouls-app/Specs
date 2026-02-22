# Risk 3: The "family wants it" vs. "subject wants it" conflict

## What the risk actually is

The product is designed as if the subject is always the one who decided to do this. They want to preserve their story, they show up willing, they understand what they're building.

That's one real user. But a significant portion of the people sitting in front of this product will be there because someone else wanted them there. An adult daughter who is terrified of losing her father's stories before he forgets them. A son who wants something to show the grandchildren. A family who watched their mother get a diagnosis and panicked.

The subject may be reluctant. They may be passive — willing to do it but not invested. They may be cognitively declining and not entirely clear on what they're consenting to. The person who set up the account and is paying for it may be someone other than the person speaking into the microphone.

The documents as written do not distinguish between these two people at all. That's the gap.

---

## The specific problems this creates

### Who actually controls the archive?

Right now, the product has no answer to this. If an adult child sets up the account, they presumably have account credentials. Can they access all sessions? Can they edit them? Can they delete them? Can they change who else has access?

The subject is the one speaking. They have the most at stake in what gets preserved and who sees it. But if they didn't set up the account and can't navigate the interface, the functional controller of the archive is the adult child.

That's a real problem, and it needs a real structural answer before it becomes a support ticket nightmare or a legal dispute between siblings after a parent dies.

### The subject says something the family shouldn't see

This will happen. A parent will disclose an affair. They'll describe a child in ways that child would find devastating. They'll admit to something they've carried in private for decades — a financial failure, an estrangement they caused, a pregnancy that ended badly — because the AI asked and they'd never been asked before.

If the adult child who set up the account can read everything, the subject is essentially talking to their family without knowing it. The therapeutic weight that makes the product meaningful also makes uninstructed family access genuinely dangerous.

The subject needs to know, before they say a word, exactly who can read what. And they need the ability to mark things private in the moment — not in settings, not retroactively.

### Family and subject have different goals

The adult child wants a warm, complete account of their parent's life to share with the grandchildren someday. They want stories about the old country, about how the grandparents met, about the hard years that built the family.

The subject might want to talk about different things. The years before the family existed. Regrets that have nothing to do with the family. A version of their life that doesn't center their role as parent and grandparent.

The archive belongs to the subject. Their story shouldn't be shaped by what the family signed up hoping to get. If the product doesn't establish this clearly in onboarding, the implicit frame will drift toward "what the family wants" simply because that's often who's holding the phone.

### Cognitive decline

This is the hardest case. A parent with early Alzheimer's can still consent to this today. Can they consent to it in six months? Who decides when they can no longer meaningfully consent to what's being added to their archive?

The product doesn't need to make clinical determinations here — it's not a medical device. But it does need a thought-through protocol for what happens when the subject's capacity to understand and control their archive is visibly diminishing. Right now there's nothing.

---

## Design responses

### Subject-first onboarding, always

If an adult child sets up the account, the first session with the subject should be a direct confirmation — not a checkbox, a conversation. The AI talks to the subject: here's what this product is, here's what gets recorded, here's who can access it, here's what you can keep private, here's how to stop at any time.

The adult child doesn't consent on behalf of the subject. The subject consents for themselves, with full information, before any archive content is created.

This adds friction to the family-initiated setup. That friction is intentional and correct.

### Separate account ownership from archive ownership

The person who creates and pays for the account is not necessarily the person who controls the archive. These need to be distinct roles in the data model.

The account owner can set up billing, manage the subscription, invite the subject. The subject controls the archive: who can read it, what's private, who inherits access, when chapters are released.

If the subject is unable or unwilling to manage their own settings, they can explicitly delegate that control to someone — but the default should be that the archive is the subject's, full stop.

### Tiered access that the subject sets

The access model should not be binary ("family can see everything" or "family can see nothing"). A workable structure:

- Everything shared by default with the family the subject designates
- A private layer that only the subject can access, ever, including after death
- A sealed layer that releases under conditions the subject sets ("to be opened after I die," "available to my children but not my grandchildren," "shared with no one")

The subject sets these at the session level and can mark individual moments within a session as private in real time.

### The in-session "keep this between us" flag

During a session, the subject should be able to say — in plain language, by voice — "I don't want this part shared" and have that section locked out of family access immediately. The AI should confirm it's marked private and continue.

This isn't a privacy setting buried in a menu. It's a first-class conversational feature that the AI mentions at the start of sessions: "Anything you want to keep private, just let me know."

### No family editing of subject content

Family members should never be able to edit, delete, or annotate the subject's recorded sessions or transcripts, regardless of account ownership. They can add their own memories to a family section if that feature exists, but the subject's archive is the subject's record.

This matters legally as much as ethically. The product's entire value proposition depends on the archive being trustworthy — which means no one altered it.

### Reluctance is information, not an obstacle

If a subject who was brought in by a family member consistently gives short answers, redirects, or seems disengaged, the AI should not interpret this as a UX problem to solve with better prompts. It should treat reluctance as information: this person may not fully want to be here.

A graceful check-in after a few sessions of low engagement: "Some people find this valuable right away, others take longer to warm up, and some decide it's not for them. How are you feeling about continuing?" That gives the subject a dignified out that doesn't require them to confront their family directly.

### A protocol for cognitive decline

The product should allow a subject to designate a trusted person who can act on their behalf if they become unable to manage their own archive settings — not to add content, but to manage access and custody.

This designation should be made by the subject while they're still fully capable, and it should have a clear scope: managing access, not editing content.

What happens to archive sessions recorded after the point of significant cognitive decline is a harder question. At minimum, those sessions should be automatically timestamped, and the subject's designee should be able to flag a period as "recorded during cognitive decline" — not to delete it, but to provide context for future readers.

---

## The honest framing

The product sells itself as something families do together. That framing is warm and it's probably right commercially. But it obscures that the subject and the family may have genuinely different interests in the archive — different goals, different ideas of what should be in it, different ideas of who should see it.

The way to honor both is to make the subject's control over their own archive structurally unambiguous. Not in the terms of service. In the product behavior. The family initiated it, the family might read it someday, but it belongs to the person who spoke.

If that's clear from day one, most of the conflict dissolves. If it isn't, the conflict will surface after something irreversible has happened.
