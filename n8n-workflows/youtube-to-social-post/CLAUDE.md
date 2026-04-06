# YouTube to Social Post — Project Context

## What this project does
Automated pipeline: drop a YouTube URL → Claude extracts transcript → generates X and LinkedIn posts + image → auto-posts to both platforms.

## Current Stack
- **n8n** — workflow automation (running locally via Docker at localhost:5678)
- **Claude API** — post generation (claude-opus-4-6)
- **Supadata API** — YouTube transcript extraction (free tier)
- **Pollinations.ai** — free image generation (no API key needed)
- **Twitter API v2** — post to X (Phase 1, Week 2)
- **LinkedIn API** — post to LinkedIn (Phase 1, Week 3)

## Project Location
`/Users/prashant/ai-skills-portfolio/n8n-workflows/youtube-to-social-post/`

## Build Phases
See `PHASES.md` for full breakdown. Summary:
- **Phase 1:** Core pipeline (Webhook → Transcript → Claude → Image → Post)
- **Phase 2:** Smarter (approval step, brand voice, duplicate checker)
- **Phase 3:** Enhancements (threads, carousels, blog, newsletter)
- **Phase 4:** Full automation (Google Sheets trigger, zero-touch)

## Current Status
- Phase 1 in progress
- n8n workflow created: "YouTube to Social Post"
- Nodes to build: Webhook → Extract Video ID → Supadata Transcript → Claude API → Parse Response → Pollinations Image → Respond to Webhook
- YouTube Data API skipped for now (no key yet)
- Supadata API key: needs to be obtained from supadata.ai

## APIs & Keys
| Service | Status |
|---|---|
| Anthropic Claude API | Available (from existing workflows) |
| Supadata API | Pending — sign up at supadata.ai |
| Pollinations.ai | No key needed |
| YouTube Data API v3 | Skipped for now |
| Twitter API v2 | Phase 2 |
| LinkedIn API | Phase 2 |

## n8n Workflow Node Order (Phase 1)
1. Webhook (POST /youtube-to-post)
2. Code node — extract videoId from URL
3. HTTP Request — Supadata transcript
4. HTTP Request — Claude API (returns x_post, linkedin_post, image_prompt, hashtags as JSON)
5. Code node — parse Claude JSON response
6. HTTP Request — Pollinations image generation
7. Set node — bundle all outputs
8. Respond to Webhook — return result for verification

## How to continue in a new session
1. Read this file for full context
2. Read SESSION.md for where we left off
3. Read PHASES.md for the full build plan
