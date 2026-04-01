# Company Research Agent

Takes a company name via chat input, calls Claude API, and returns a 3-line research summary.

## Nodes
Chat Trigger → HTTP Request (Claude API) → Code

## Setup
1. Import the JSON in n8n via Menu → Import from File
2. Update the `x-api-key` header in the HTTP Request node with your Anthropic API key
3. Activate the workflow

## Example
Input: "Razorpay"
Output: 3-line summary covering what they do, market position, and a recent development
