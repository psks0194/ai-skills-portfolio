# Session Notes — YouTube to Social Post

## Last updated: 2026-04-06

## What was accomplished
- Built full n8n workflow end-to-end
- Successfully posting to both X and LinkedIn automatically
- Phase 1 complete

## Final Node Order
1. Webhook
2. Extract Video ID
3. Fetch Transcript
4. Build Claude Request
5. Claude API
6. Parse Claude Response
7. Generate Image (Pollinations)
8. Upload Image to X (skipped on error — OAuth1 issue)
9. Bundle Outputs
10. Create Tweet (X)
11. Post to LinkedIn (parallel with Create Tweet)
12. Merge (waits for both X and LinkedIn)
13. Respond to Webhook

## Key fixes learned
- n8n Raw body field uses `{{ }}` not `={{ }}` (no = prefix)
- Set node expressions need `={{ }}` with expression toggle ON
- Supadata content is an array — must map/join before passing to Claude
- Claude returns markdown-wrapped JSON — must strip ```json``` before parsing
- X media upload requires OAuth 1.0a — not supported with OAuth 2.0
- n8n must be started with N8N_EDITOR_BASE_URL=ngrok URL for OAuth2 to work
- LinkedIn: turn OFF "Organizational support" and "Legacy" toggles in credential
- LinkedIn requires "Sign In with LinkedIn using OpenID Connect" product for openid scope
- Use Merge node (Append mode) to run X and LinkedIn posting in parallel

## Docker start command (required for X and LinkedIn OAuth2)
```bash
docker run -it --rm -p 5678:5678 -e N8N_EDITOR_BASE_URL=https://refusable-beneficially-gemma.ngrok-free.dev -e WEBHOOK_URL=https://refusable-beneficially-gemma.ngrok-free.dev -v n8n-docker_n8n_data:/home/node/.n8n n8nio/n8n
```
Note: ngrok URL changes on restart — update both docker command and X Developer Portal when it does.

## Pending items
- Rotate Anthropic API key (was exposed in httpbin debug output)
- Refine Claude prompt for better post quality
- Image posting to X (requires OAuth 1.0a — deferred)

## Next steps (Phase 2)
1. Rotate Anthropic API key
2. Add Slack approval step before posting
3. Add brand voice to Claude prompt
4. Add duplicate checker (Google Sheets)
