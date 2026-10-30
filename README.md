**Portfolio:** https://emmanuel-sunday-ai.netlify.app/
# AI Lead Conversion System (n8n)

A portfolio project: an automated system that qualifies and follows up on website quote requests for a **simulated** client, NorthBuild Renovations, an imagined renovation company in the Greater Toronto Area. All leads and business figures are test data or stated assumptions.

## What it does

1. A web form captures a quote request.
2. An AI model (Gemini) classifies it as hot, warm, cold or spam and returns a score, summary, service-area check and a draft reply as JSON.
3. Plain rules route the lead. Every lead except spam is saved to HubSpot with the AI fields attached, and repeat emails update the same contact.
4. Hot leads email the owner with the lead's details and a suggested reply to review.
5. Warm and cold leads get an automatic reply only when the AI's confidence is at least 0.8, the service area is known and a draft exists. Otherwise the owner is asked to review the lead.
6. If the AI call fails or returns unreadable output, the owner is emailed. A separate workflow emails an alert if any other step fails.

## Files

| File | Purpose |
|---|---|
| `northbuild-lead-intake.json` | The live workflow |
| `northbuild-accuracy-test.json` | Runs 30 labelled test leads through the same AI step and scores the answers |
| `northbuild-error-alert.json` | Emails an alert when another workflow fails |
| `accuracy-test-results.json` | Output of the 30-lead test |

## Results (30 labelled fake leads)

| Measure | Result |
|---|---|
| Category correct, all leads | 27 of 30 (90%) |
| Category correct, clear leads | 27 of 27 |
| Service area correct | 26 of 26 |
| Spam caught | 4 of 4 |
| Failed AI calls | 0 |

The three misses were all borderline-area leads (Barrie, Guelph, Niagara Falls). The model flagged each as "unknown" for service area, so the guard sent them to the owner for review.

## Tools (all free)

n8n Community Edition (self-hosted), Google Gemini 3.1 Flash Lite (free tier), HubSpot free CRM, Mailpit (local test inbox).

## Setup

1. In n8n, import each JSON file (workflow menu, Import from File).
2. Create your own credentials: a Gemini API key, a HubSpot service key and an SMTP connection. No keys are included in these files.
3. In HubSpot, create these contact properties: AI Category, AI Score, AI Summary, Project Type, Budget Range and Timeline.

## Limits

Simulated client. Thirty synthetic leads with labels written with AI assistance. One run on one model. Runs locally on free tiers and sends no real email. A production deployment would need always-on hosting, a paid or client-owned AI key, a real email service and a privacy review.

## Case study

https://github.com/Emmanuelsun/northbuild-ai-lead-conversion/blob/main/northbuild-case-study.pdf

