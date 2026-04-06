# YouTube to Social Post — Build Phases

## Overview
Automated pipeline: YouTube URL → Claude-generated post + image → auto-post to X and LinkedIn.

---

## Phase 1: Core Pipeline (Get it working end-to-end)
- [ ] Webhook trigger in n8n (accepts YouTube URL)
- [ ] Fetch video title, description via YouTube Data API v3
- [ ] Extract transcript via Supadata API or youtube-transcript API
- [ ] Call Claude API — returns X post, LinkedIn post, image prompt, hashtags as JSON
- [ ] Generate image via DALL-E 3 API
- [ ] Post to X (Twitter API v2 — upload image → post tweet)
- [ ] Post to LinkedIn (ugcPosts endpoint)
- [ ] Send Slack/email notification with post links

---

## Phase 2: Make it Smarter
- [ ] Slack approval step — Approve / Regenerate buttons before posting
- [ ] Brand voice — system prompt with your tone and preferred phrases
- [ ] Duplicate checker — skip if same topic posted in last 30 days (store in Google Sheets / Airtable)

---

## Phase 3: Enhancements
- [ ] X thread generator (5-10 tweet threads for long videos)
- [ ] LinkedIn carousel (slide-by-slide content + images)
- [ ] Hashtag optimizer
- [ ] Optimal posting time scheduler
- [ ] Multi-language post generation
- [ ] Blog post version (post to Medium / Hashnode)
- [ ] Email newsletter snippet (append to weekly digest)
- [ ] Reels/Shorts script (30-second hook + body + CTA)

---

## Phase 4: Full Automation (Zero-touch)
- [ ] Google Sheets trigger — polls for new YouTube URLs every hour
- [ ] Slack approval flow with one-click approve
- [ ] Auto-post at optimal time after approval
- [ ] Mark row as "Posted" in sheet with live post links

---

## Build Order
| Week | Focus |
|---|---|
| Week 1 | Transcript fetch + Claude post generation (manual post) |
| Week 2 | DALL-E image generation + auto-post to X |
| Week 3 | LinkedIn posting + Slack approval step |
| Week 4 | Google Sheets trigger + duplicate checker + brand voice |
| Later | Thread generator, carousel, blog repurposing |

---

## APIs Needed
| Service | Purpose | Notes |
|---|---|---|
| YouTube Data API v3 | Video metadata | Free, needs Google Cloud project |
| Supadata / youtube-transcript | Transcript extraction | Free tier available |
| Anthropic Claude API | Post generation | Pay per token |
| OpenAI DALL-E 3 | Image generation | Pay per image |
| Twitter API v2 | Post to X | Basic tier: $100/month or apply for free |
| LinkedIn API | Post to LinkedIn | Needs LinkedIn app approval |
| Slack API | Notifications + approval | Free |
