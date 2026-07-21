# n8n + Claude: Weekly Dev Summary Workflow

Automated weekly GitHub development summary powered by Claude Sonnet 4. Fetches commits, closed issues, and merged PRs, then generates a narrative summary delivered to Discord.

## Setup (5 steps)

1. **Install n8n** — `docker run -d --name n8n -p 5678:5678 -v n8n-data:/home/node/.n8n n8nio/n8n:latest`

2. **Import the workflow** — Open n8n UI → Workflows → Import from File → select `workflow.json`

3. **Configure the Config node** — Open the workflow, edit the "Config" node:
   - `GITHUB_REPO` — your repo (e.g. `owner/repo`)
   - `GITHUB_TOKEN` — your GitHub PAT ([generate one](https://github.com/settings/tokens))
   - `ANTHROPIC_API_KEY` — your Anthropic API key ([get one](https://console.anthropic.com/))
   - `DISCORD_WEBHOOK_URL` — your Discord webhook URL ([create one](https://support.discord.com/hc/en-us/articles/228383668))
   - `LANGUAGE` — `EN` for English or `FR` for French

4. **Test the workflow** — Click "Execute Workflow" in the n8n editor. Verify the summary appears in your Discord channel.

5. **Activate** — Toggle the workflow to "Active". It will run every Friday at 5pm (configurable in the Schedule Trigger node).

## How it works

```
Schedule Trigger (Fri 5pm)
  → Config node (sets repo, API keys, language)
  → GitHub API: Commits (last 7 days)
  → GitHub API: Closed Issues (last 7 days)
  → GitHub API: Merged PRs (last 7 days)
  → Code node: Merge & format raw data
  → Claude Sonnet 4 API: Generate narrative summary
  → Extract Summary node
  → Discord Webhook: Deliver summary
```

## Features

- **Bilingual** — Set `LANGUAGE` to `EN` (English) or `FR` (French) in the Config node
- **Configurable** — Change repo, webhook URL, or language without editing workflow logic
- **No n8n credentials needed** — All API keys live in the Config node (paste-and-go)
- **Portable** — Single JSON file, import into any n8n instance

## Requirements

- n8n (self-hosted or cloud)
- GitHub Personal Access Token (increases API rate limit)
- Anthropic API key (for Claude Sonnet 4)
- Discord webhook URL (or modify the last node for Slack/email)

## Cost

- ~$0.05–0.15 per weekly run (Claude Sonnet 4, ~2K input + ~800 output tokens)
- GitHub API: free with PAT
- n8n: free self-hosted

## License

MIT
