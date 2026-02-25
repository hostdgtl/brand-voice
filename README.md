# Brand Voice

A Claude Code skill that manages brand voice profiles for marketing teams. Onboards new clients through a structured interview, stores their identity as portable `.md` files, and ensures every piece of content — social posts, emails, ads, product copy — sounds authentically like the brand it represents.

## What It Does

- **Onboards clients** — Guided 5-round interview covering identity, voice traits, practical rules, products/audience, and a quality checklist
- **Stores profiles** — One `.md` file per client in `references/clients/`, portable and human-readable
- **Loads on demand** — Say a client name and the profile is pulled into context instantly
- **Applies voice** — Every content skill (social, email, campaign) loads this first for tone consistency
- **Analyses briefs** — Reads creative briefs against the stored profile, flags conflicts, extracts deliverables

## Requirements

- [Claude Code](https://claude.ai/claude-code) (this is a Claude Code skill)

No dependencies. Profiles are plain Markdown files — no database, no API keys.

## Setup

Install as a Claude Code command:

```bash
# Copy brand-voice.md to your commands directory
cp brand-voice.md ~/.claude/commands/brand-voice.md
```

Or use directly by referencing the skill file in your project.

## Usage

### Inside Claude Code

Trigger with natural language:
- "Load brand voice for [company name]"
- "New client" / "Onboard [company name]"
- "Which company are we working on?"
- Reference any stored client by name

Or use the slash command: `/brand-voice`

### Modes

| User Intent | Mode |
|-------------|------|
| Names a known client or says "load brand voice" | **Load Client** |
| Says "new client" / "onboard" / names unknown company | **Onboard Client** |
| Shares a creative brief | **Analyse Brief** |
| Asks to write content with a client loaded | **Apply Voice** |
| Provides updated info about a client | **Update Profile** |

## Onboarding Interview

New clients are onboarded through 5 conversational rounds:

| Round | Focus | What's Captured |
|-------|-------|-----------------|
| 1 — Identity | Who they are | Company name, location, origin story, distinctive facts |
| 2 — Voice | How they sound | 3-5 voice traits with examples, anti-voice, brand-audience relationship |
| 3 — Practical Rules | Operational details | Salutations, always/never use words, formatting, channel-specific rules |
| 4 — Products & Audience | Commercial context | Key products, target audience, competitive positioning, campaigns |
| 5 — Quality Checklist | QA gate | 5-7 yes/no checks every piece of content must pass |

Each round is summarised and confirmed before moving on.

## Client Profile Format

Profiles are saved as `references/clients/{client-slug}.md` using the template in `assets/client-profile-template.md`:

```
# Client Profile: [Company Name]

## Identity
## Voice (traits, anti-voice, brand relationship)
## Practical Rules (salutations, vocabulary, formatting, channel rules)
## Products & Audience (products, target audience, positioning, campaigns)
## Quality Checklist (5-7 yes/no checks)
```

## Architecture

```
brand-voice/
├── brand-voice.md                   ← Skill definition (Claude Code command)
├── assets/
│   └── client-profile-template.md   ← Onboarding interview template
├── references/
│   └── clients/                     ← One .md file per client
│       └── {client-slug}.md
└── output/                          ← Working drafts
```

## How It Works

- Profiles are plain Markdown — human-readable, version-controllable, portable
- The skill acts as a foundation layer: social content, email campaigns, and other agents load it first
- Voice traits aren't just adjectives — each is probed for practical examples and anti-examples
- The quality checklist is the client's own QA gate, built during onboarding
- Multi-client sessions are supported with explicit voice switching
- Brief analysis cross-references the stored profile and flags conflicts

## Companion Skills

This skill is designed as the foundation for:
- [Social Content Generator](https://github.com/hostdgtl/social-content-generator) — social media copy + AI images
- [Email Campaign Writer](https://github.com/hostdgtl/email-campaign-writer) — email copy + Figma design

Both load brand-voice first to ensure tone consistency.

## License

MIT