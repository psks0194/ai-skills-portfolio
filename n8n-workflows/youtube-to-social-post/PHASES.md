# YouTube to Social Post — Build Phases

## Overview
Automated pipeline: YouTube URL → Claude-generated post + image → auto-post to X and LinkedIn.

---

## Phase 1: Core Pipeline (Get it working end-to-end)
- [x] Webhook trigger in n8n (accepts YouTube URL)
- [x] Extract transcript via Supadata API
- [x] Call Claude API — returns X post, LinkedIn post, image prompt, hashtags as JSON
- [x] Generate image via Pollinations.ai (free, no API key)
- [x] Post to X (Twitter API v2)
- [x] Post to LinkedIn (ugcPosts endpoint with image)
- [x] Second webhook trigger (accepts article/website URL via Jina AI Reader)

---

## Phase 2: Make it Smarter
- [x] Slack approval step — workflow pauses, sends preview to Slack, Approve/Reject links
- [ ] Duplicate checker — skip if same topic posted in last 30 days (Google Sheets / Airtable)
- [ ] Brand voice — refine system prompt with tone and preferred phrases

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
