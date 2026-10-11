# n8n ticket router lab

A small n8n workflow that routes support tickets, running on a self-hosted n8n instance in Docker.

## Setup
- macOS, Docker Desktop, Docker Compose
- Image: n8nio/n8n, port 5678, timezone Europe/Athens
- Data lives in a persistent Docker volume named n8n_data
- Restart policy: unless-stopped
- Start it with: docker compose up -d

## Workflow
1. A Webhook node receives a ticket as JSON (POST).
2. An IF node checks whether the hotel is a key account. The list is invented.
3. Key account: an Edit Fields node sets priority High and a named owner.
4. Other tickets: a Switch node sorts by subject keyword (channel, rates, or fallback). Each output sets a queue value.

The design follows the routing idea I used in Freshdesk at WebHotelier: key clients get a named contact instead of whoever is free.

## Tests
I sent four curl requests, one per path: key account, channel, rates, and fallback. Screenshots are in the screenshots folder.

## What went wrong
1. The test webhook returned 404. The test URL only works once, after you click Execute workflow.
2. Pinned test data replayed the old ticket, so the false branch never ran. I unpinned it.
3. The ticket data sits under body, so expressions use $json.body.

## Limits
- Docker Desktop on a laptop, not a production server.
- No Kubernetes.
- The webhook has no authentication. It runs on localhost only.
- All data is invented.
