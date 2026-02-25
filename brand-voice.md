---
name: brand-voice
description: Multi-client brand voice management system for marketing professionals. Onboards, stores, loads, and applies brand identity profiles for any company. Use this skill whenever writing marketing copy, analysing creative briefs, drafting social posts, emails, product descriptions, ad copy, landing pages, press releases, or any customer-facing content that needs to sound like a specific brand. Also triggers when the user says "load brand voice", "new client", "onboard", "creative brief", "which company are we working on", references a stored client by name, or needs brand-consistent content of any kind. This is a foundation skill — content agents (social, email, campaign) should load this first to ensure voice consistency.
---

# Brand Voice System

A multi-client brand voice engine for marketing professionals. Onboards companies, stores their identity, analyses creative briefs, and ensures every piece of content sounds authentically like the brand it represents.

## Architecture

```
brand-voice/
├── SKILL.md                         ← You are here (workflow engine)
├── references/clients/              ← One .md file per client
│   └── {client-slug}.md             ← Stored client profiles
└── assets/
    └── client-profile-template.md   ← Onboarding interview template
```

Each client profile lives in `references/clients/{client-slug}.md`. Only load the profile relevant to the active session.

---

## Entry Point

Every session begins here. Determine the correct mode from the user's request:

| User intent | Mode |
|-------------|------|
| Names a known client or says "load brand voice" | → **Load Client** |
| Says "new client" / "onboard" / names unknown company | → **Onboard Client** |
| Shares or uploads a creative brief | → **Analyse Brief** |
| Asks to write content with a client already loaded | → **Apply Voice** |
| Asks to update a client's details | → **Update Profile** |

If the intent is unclear, ask: *"Which company are we working on today?"*

If no clients exist yet, guide the user straight to onboarding.

---

## Mode 1 — Load Existing Client

**Trigger:** The user names a company, references a past client, or says "load brand voice."

1. Check `references/clients/` for a matching profile.
2. If found, read the full `.md` file into context.
3. Confirm: *"Loaded [Company Name] brand voice. Here's a quick snapshot:"*
   - Company name and what they do (one line)
   - Voice summary (the trait names, e.g., "Bold & Direct, Warm & Witty, Expert & Approachable")
   - Any active campaigns or seasonal context if noted in the profile
4. Ask: *"What are we working on?"*
5. If the profile is not found, offer to onboard them (→ Mode 2).

---

## Mode 2 — Onboard New Client

**Trigger:** The user says "new client", "onboard", or names a company with no existing profile.

Read `assets/client-profile-template.md` for the full profile structure, then conduct the interview below.

### The Interview

Don't dump every question at once. Group them into conversational rounds. After each round, summarise what you've captured and confirm before moving on.

**Round 1 — Identity**
Get the foundations. Who are they, what do they do, and what makes them tick?
- Company name and what they do
- Where they're based
- How they were founded (the origin story)
- Key facts that make them distinctive (certifications, heritage, process, team size — anything worth weaving into copy)

**Round 2 — Voice**
This is the most important round. Dig deep here.
- Ask for 3-5 adjectives that describe how the brand should sound.
- For each adjective, probe: *"What does [adjective] look like in practice? Can you show me an example of copy that nails this?"*
- Ask for anti-examples too: *"What should the brand never sound like? Any copy you've seen that made you cringe?"*
- Explore the nuances: witty or warm? Direct or poetic? Authoritative or approachable? Premium or accessible?
- Ask about the brand's relationship with its audience: friend, expert, mentor, peer, parent, co-conspirator?

**Round 3 — Practical Rules**
The operational details that keep content consistent.
- Salutations and sign-offs (email, social, other channels)
- Words or phrases to always use
- Words or phrases to never use
- Formatting preferences (sentence length, emoji policy, exclamation marks, hashtag style)
- Channel-specific rules (does the voice flex between email, social, product pages, ads?)

**Round 4 — Products & Audience**
The commercial context.
- Key product lines or services
- Target audience (primary and secondary)
- Competitive positioning — what makes this brand different from its competitors?
- Current or upcoming campaigns worth noting

**Round 5 — Quality Checklist**
Ask the user to define 5-7 yes/no questions that should be true of every piece of content. These become the brand's built-in QA gate.

Prompt them: *"If you could hold every piece of copy up against a checklist before it goes live, what would be on it? Think about what 'on-brand' means for [Company Name]."*

### After the Interview

1. Compile the complete profile using the template structure.
2. Present the full profile to the user for review.
3. Ask: *"Does this capture the brand accurately? Anything to add or change?"*
4. On approval, save to `references/clients/{client-slug}.md`.
5. Confirm: *"Brand voice profile for [Company Name] saved. Ready to use in any future project — just ask me to load it."*

---

## Mode 3 — Analyse Creative Brief

**Trigger:** The user shares, uploads, or pastes a creative brief, campaign brief, or project brief.

### Step 1 — Identify the Client
If no client is loaded, identify the company from the brief.
- If the company has a stored profile → load it automatically.
- If not → extract what you can from the brief and ask: *"I don't have a stored profile for [Company]. Want me to build one from this brief, or shall we work with what's here for now?"*

### Step 2 — Extract the Brief
Read carefully and pull out:

| Element | What to extract |
|---------|-----------------|
| **Objective** | What is this campaign trying to achieve? (awareness, sales, retention, launch) |
| **Audience** | Who are we speaking to? Is this the brand's core audience or a specific segment? |
| **Key messages** | What must be communicated? What's the hierarchy? |
| **Deliverables** | What content needs to be produced? (emails, social posts, landing page, ads, etc.) |
| **Tone adjustments** | Any brief-specific shifts from the standard voice? ("more playful", "more premium", "urgent but not aggressive") |
| **Products / services** | Specific products, collections, discounts, or promotions featured |
| **Constraints** | Deadlines, mandatory inclusions, legal/compliance notes, platform specs, character limits |
| **Assets** | Existing images, videos, or design references mentioned |

### Step 3 — Present the Analysis
Summarise the brief back to the user in a structured breakdown. Flag anything missing, ambiguous, or potentially conflicting with the brand voice profile.

### Step 4 — Confirm and Proceed
Once confirmed, the loaded brand voice + brief analysis become the foundation for all content produced in the session. Other agents (social, email, campaign) can build on top of this.

---

## Mode 4 — Apply Voice to Content

**Trigger:** Any content writing task with a client profile loaded.

### Writing Principles

1. **Internalise, don't imitate.** The profile gives you the brand's DNA — its values, voice traits, vocabulary, and rules. Write as if you are the brand's senior copywriter who deeply understands the company. Don't template-fill. Think, then write.

2. **Lead with the voice traits.** Every sentence should pass through the lens of the client's named voice characteristics. If the brand is "Warm & Witty," every line should carry warmth or wit. If it's "Bold & Direct," don't pad with filler.

3. **Adapt to the channel.** The same brand voice flexes across contexts:
   - **Email:** Follow the client's salutation/sign-off rules. More space for narrative and storytelling.
   - **Social:** Punchier, hook-first, mobile-formatted, platform-specific.
   - **Product copy:** Benefit-led, sensory where applicable, concise.
   - **Ad copy:** Sharp, single-message, CTA-driven but still on-brand.
   - **Web / landing page:** Scannable, hierarchy-driven, SEO-aware but never robotic.
   - **Press / PR:** More formal register while retaining the brand's personality.

4. **Respect the vocabulary rules.** Every profile has "always use" and "never use" lists. These aren't suggestions — they're the brand's identity encoded in word choices. A single wrong word can make an entire piece feel off-brand.

5. **Run the quality checklist.** Every client profile includes a quality checklist. Run the content through it before presenting. If any check fails, revise. Don't deliver work that doesn't pass the brand's own standards.

### Content Production Order
1. Load the client profile (if not already in context)
2. Load the brief analysis (if one exists for this session)
3. Draft the content
4. Run the quality checklist
5. Revise if needed
6. Present to the user

---

## Mode 5 — Update Client Profile

**Trigger:** The user provides new information about an existing client — new product line, tone shift, updated values, new campaign patterns, corrected facts.

1. Load the existing profile.
2. Identify what's changed.
3. Propose specific edits — show the before and after.
4. On approval, update the client `.md` file.
5. Confirm: *"Updated [Company Name] profile: [summary of changes]."*

Never silently update a profile. Always show the user what's changing and get confirmation.

---

## Multi-Client Sessions

If working across multiple brands in one session:

1. Always confirm which client is active before producing content.
2. Explicitly state when switching: *"Switching to [Company Name] voice."*
3. Never blend voices between clients. A clean switch means re-reading the new profile.

---

## Stored Client Profiles

Check `references/clients/` for current profiles. When the user mentions a stored client by name or common abbreviation, load the corresponding profile automatically without asking.

---

## Edge Cases

- **No profile exists and user wants to write now:** Work from whatever context the user provides, but flag that the output will be stronger with a full profile. Offer to build one after the immediate task.
- **Brief contradicts the brand profile:** Flag the conflict. Ask the user which takes priority for this campaign. Don't silently override either source.
- **User provides competitor content as reference:** Use it to understand what the brand is not. Note the contrast in your approach.
- **Partial information:** If the user can only provide some profile details, save what you have and mark the gaps. A partial profile is better than none — it can be filled in later via Mode 5.
- **Multiple team members, different preferences:** The profile is the single source of truth. Individual preferences should be captured as profile updates, not one-off exceptions.
