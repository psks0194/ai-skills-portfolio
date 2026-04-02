# HubSpot Lead Enrichment Agent

AI-powered workflow that researches a company using Claude and automatically creates an enriched contact in HubSpot CRM.

## How It Works
1. User enters a company name via chat
2. Claude API researches the company and returns structured data (CEO name, email, website, description)
3. Code node parses Claude's response into individual fields
4. HTTP Request creates a new contact in HubSpot with the enriched data

## Nodes
Chat Trigger → HTTP Request (Claude API) → Code (Parse Response) → HTTP Request (HubSpot API)

## HubSpot Fields Populated
- First Name (CEO/Founder)
- Last Name
- Email
- Company Name
- Website
- Job Title

## Setup
1. Import the JSON in n8n via Menu → Import from File
2. Update the `x-api-key` header in the Claude HTTP Request node with your Anthropic API key
3. Update the `Authorization` header in the HubSpot HTTP Request node with your HubSpot Private App token (format: `Bearer pat-xxx`)
4. Activate the workflow

## Prerequisites
- Anthropic API key (console.anthropic.com)
- HubSpot account with a Private App configured (Settings → Integrations → Private Apps)
- Private App needs scopes: crm.objects.contacts.read, crm.objects.contacts.write

## Example
Input: "Razorpay"
Output: New HubSpot contact created with Razorpay CEO details, company info, and website
