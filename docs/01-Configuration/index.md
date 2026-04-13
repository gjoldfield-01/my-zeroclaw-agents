# Configuration

A sanitised reference copy of the agent configuration. All secrets, tokens, and private paths have been replaced with placeholders.

!!! warning "Placeholders only"
    This file is published publicly. It must never contain real API keys, tokens, server paths, or personal identifiers. Replace every `YOUR_*` value with your own secret in your local copy — never commit the real version.

## config.toml (sanitised template)

```toml
# -----------------------------------------------------------------
# Weeny Jeanie — Agent Configuration (sanitised template)
# -----------------------------------------------------------------

[agent]
name        = "Weeny Jeanie"
role        = "Thought Leadership Companion"
version     = "0.1.0"

[server]
host        = "YOUR_SERVER_HOST"
port        = 0000
workdir     = "YOUR_SERVER_WORKDIR"

[apify]
api_token   = "YOUR_APIFY_TOKEN"

[telegram]
bot_token   = "YOUR_TELEGRAM_BOT_TOKEN"
chat_id     = "YOUR_TELEGRAM_CHAT_ID"

[linkedin]
client_id       = "YOUR_LINKEDIN_CLIENT_ID"
client_secret   = "YOUR_LINKEDIN_CLIENT_SECRET"
redirect_uri    = "YOUR_LINKEDIN_REDIRECT_URI"
access_token    = "YOUR_LINKEDIN_ACCESS_TOKEN"

[model]
provider    = "anthropic"
model_id    = "claude-opus-4-6"
api_key     = "YOUR_ANTHROPIC_API_KEY"

[logging]
level       = "info"
path        = "YOUR_LOG_PATH"
```

## How secrets are handled locally

- The real `config.toml` lives **only** on the local machine or private server.
- `.gitignore` excludes `config.toml`, `secrets.toml`, and `.env` so they can never be pushed to GitHub by accident.
- Before any commit: `grep -r "sk-" docs/` and `grep -r "api_key" docs/` are run to verify no live secrets leaked into the public site.
