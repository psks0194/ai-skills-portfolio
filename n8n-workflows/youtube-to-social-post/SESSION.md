# Session Notes — YouTube to Social Post

## Last updated: 2026-04-06

## What was accomplished
- Built full n8n workflow end-to-end
- Successfully posting to both X and LinkedIn automatically
- Phase 1 complete
- Brand voice prompt updated (Indian tone, simple language, general audience)
- LinkedIn image upload working (register upload → send binary → get asset URN)

## Final Node Order
1. Webhook
2. extract_video_meta (Extract Video ID)
3. HTTP Request (Fetch Transcript — Supadata)
4. Code in JavaScript2 (Build Claude Request)
5. HTTP Request1 (Claude API)
6. ParseClaudeResponse
7. PollinationImageGenerate
8. Upload Image to X (fails silently — OAuth1 issue)
9. Edit Fields (Bundle Outputs — Set node)
10. Bundle Outputs (Respond to Webhook — used as pass-through)
11. upload_image_to_linkedin (Register Upload)
12. Merge1 (combines Pollinations binary + upload URL)
13. Code in JavaScript (extracts uploadUrl + binary)
14. send_image_to_linkdin (PUT binary to LinkedIn)
15. Create Tweet (X posting)
16. Create a post (LinkedIn posting)
17. Merge (combines X + LinkedIn outputs)
18. Respond to Webhook

## Where we left off
- LinkedIn image upload is working (send_image_to_linkdin returns `{}` = success)
- Next step: wire up **Create a post** (LinkedIn node) to include the image asset URN
  - Add Additional Fields → Media Category = IMAGE
  - Media URL or URN = `={{ $('upload_image_to_linkedin').first().json.value.asset }}`

## Key fixes learned
- n8n Raw body field uses `{{ }}` not `={{ }}` (no = prefix)
- Set node expressions need `={{ }}` with expression toggle ON
- URL field in HTTP Request: use expression toggle ON + no `={{ }}` wrapper
- Supadata content is an array — must map/join before passing to Claude
- Claude returns markdown-wrapped JSON — must strip ```json``` before parsing
- X media upload requires OAuth 1.0a — not supported with OAuth 2.0
- n8n must be started with N8N_EDITOR_BASE_URL=ngrok URL for OAuth2 to work
- LinkedIn: turn OFF "Organizational support" and "Legacy" toggles in credential
- LinkedIn requires "Sign In with LinkedIn using OpenID Connect" product for openid scope
- LinkedIn image upload returns empty `{}` on success (201 response)
- Use `.first()` not `.item` in Code nodes

## Docker start command (required for X and LinkedIn OAuth2)
```bash
docker run -it --rm -p 5678:5678 -e N8N_EDITOR_BASE_URL=https://refusable-beneficially-gemma.ngrok-free.dev -e WEBHOOK_URL=https://refusable-beneficially-gemma.ngrok-free.dev -v n8n-docker_n8n_data:/home/node/.n8n n8nio/n8n
```
Note: ngrok URL changes on restart — update both docker command and X Developer Portal when it does.

## Pending items
- Rotate Anthropic API key (was exposed in httpbin debug output)
- Wire up Create a post (LinkedIn) to include image asset URN
- Fix Merge node — not connected to Respond to Webhook
- Image posting to X (requires OAuth 1.0a — deferred)

## Next steps (tomorrow)
1. Connect Create a post LinkedIn node with image asset URN
2. Test full pipeline with image on LinkedIn
3. Move to Phase 2 — Slack approval step
