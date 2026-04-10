# Session Notes — YouTube to Social Post

## Last updated: 2026-04-10

## What was accomplished

### Phase 1 (complete)
- Full pipeline working end-to-end: YouTube URL → transcript → Claude → image → X + LinkedIn
- LinkedIn image post working via UGC Posts API (HTTP Request node)
- Tweet posting working

### Phase 2 — Session 2026-04-09 to 2026-04-10
- Added second webhook trigger: `POST /article-to-post`
  - Accepts any website/article URL
  - Fetches clean content via Jina AI Reader (`https://r.jina.ai/{url}`) — free, no API key
  - Same Claude → image → X + LinkedIn pipeline as YouTube flow
- Added Slack approval step (Phase 2, item 1 — complete)
  - After Claude generates posts, workflow sends preview to `#all-n8n-post-approver`
  - Message includes Approve and Reject links using `$execution.resumeUrl`
  - Wait node pauses execution until link is clicked
  - IF node routes: approve → post to X + LinkedIn, reject → notify Slack + stop
  - Fixed: `unfurl_links: false` added to prevent Slack auto-fetching links
  - Fixed: `&action=approve` (not `?action=approve`) since resumeUrl already has `?signature=...`
  - Slack bot token stored as n8n Header Auth credential (not hardcoded)

## Final Node Order (updated)
1. Webhook (`/youtube-to-post`) OR Webhook - Article (`/article-to-post`)
2. extract_video_meta / Fetch Article Content (Jina AI)
3. HTTP Request — Supadata transcript / Code_Article_Claude
4. Code in JavaScript2 / Code_Article_Claude (Build Claude Request)
5. HTTP Request1 (Claude API)
6. ParseClaudeResponse
7. **Build Slack Approval Message** (Code node — new)
8. **Send to Slack** (HTTP Request — new)
9. **Wait for Approval** (Wait node — new)
10. **Check Approval** (IF node — new)
11. PollinationImageGenerate (only if approved)
12. Upload Image to X
13. Edit Fields → Bundle Outputs
14. Create Tweet → Merge → Respond to Webhook
15. upload_image_to_linkedin → Merge1 → Code in JavaScript
16. send_image_to_linkdin → Code in JavaScript1 → HTTP Request2 (LinkedIn UGC post)
17. Merge → Respond to Webhook

## Key fixes from Phase 2
- Slack unfurl auto-triggered reject: fixed with `unfurl_links: false` in message payload
- Double `?` in resume URL: fixed by using `&action=approve` instead of `?action=approve`
- IF node condition: `{{ $json.query.action }}` equals `approve` (string, fixed mode)

## Where we left off
- Phase 2, item 1 (Slack approval) complete and tested
- Ready to start Phase 2, item 2: duplicate checker

## Pending items
- Rotate Anthropic API key (was exposed in httpbin debug output — rotate ASAP)
- Rotate LinkedIn Bearer token (was exposed in httpbin debug output)
- X image posting (requires OAuth 1.0a — deferred)
- Phase 2, item 2: Duplicate checker (skip if same topic posted in last 30 days)
- Phase 2, item 3: Brand voice system prompt refinement

## Docker start command (required for X and LinkedIn OAuth2)
```bash
docker run -it --rm -p 5678:5678 -e N8N_EDITOR_BASE_URL=https://refusable-beneficially-gemma.ngrok-free.dev -e WEBHOOK_URL=https://refusable-beneficially-gemma.ngrok-free.dev -v n8n-docker_n8n_data:/home/node/.n8n n8nio/n8n
```
ngrok static domain — does not change on restart.
