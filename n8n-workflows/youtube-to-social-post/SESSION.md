# Session Notes — YouTube to Social Post

## Last updated: 2026-04-08

## What was accomplished
- Phase 1 fully complete and smoke tested
- LinkedIn image post working via UGC Posts API (HTTP Request node)
- Tweet posting working
- Full pipeline tested end-to-end: YouTube URL → transcript → Claude → image → X + LinkedIn

## Final Node Order
1. Webhook
2. extract_video_meta (Extract Video ID)
3. HTTP Request (Fetch Transcript — Supadata)
4. Code in JavaScript2 (Build Claude Request)
5. HTTP Request1 (Claude API)
6. ParseClaudeResponse
7. PollinationImageGenerate
8. Upload Image to X (fails silently — OAuth1 issue, deferred)
9. Edit Fields (Bundle Outputs — Set node)
10. Bundle Outputs (Respond to Webhook — used as pass-through)
11. Create Tweet → Merge → Respond to Webhook
12. upload_image_to_linkedin (Register Upload)
13. Merge1 (combines Pollinations binary + upload URL)
14. Code in JavaScript (extracts uploadUrl + binary)
15. send_image_to_linkdin (PUT binary to LinkedIn)
16. Build LinkedIn Body (Code node — builds UGC post object)
17. HTTP Request2 (POST to /v2/ugcPosts — LinkedIn post with image)
18. Merge → Respond to Webhook

## Key fix from this session
- HTTP Request2 body: use `={{ $json }}` NOT `={{ JSON.stringify($json) }}`
- When rawContentType is application/json, n8n serializes the object automatically
- Using JSON.stringify causes double-encoding — body gets sent as a JSON string instead of a JSON object

## Where we left off
- Phase 1 complete
- Ready to start Phase 2: Slack approval step before auto-posting

## Pending items
- Rotate Anthropic API key (was exposed in httpbin debug output — visible in this session too, rotate ASAP)
- Rotate LinkedIn Bearer token (was exposed in httpbin debug output this session)
- X image posting (requires OAuth 1.0a — deferred to later phase)

## Docker start command (required for X and LinkedIn OAuth2)
```bash
docker run -it --rm -p 5678:5678 -e N8N_EDITOR_BASE_URL=https://refusable-beneficially-gemma.ngrok-free.dev -e WEBHOOK_URL=https://refusable-beneficially-gemma.ngrok-free.dev -v n8n-docker_n8n_data:/home/node/.n8n n8nio/n8n
```
ngrok static domain — does not change on restart.

## Next steps (Phase 2)
1. Add Slack approval step — workflow pauses, sends preview to Slack, waits for approve/reject
2. On approve → post to X and LinkedIn
3. On reject → discard or allow editing
