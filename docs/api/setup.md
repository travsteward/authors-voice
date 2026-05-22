# Setup — One-Time Configuration

## API Key (Email OTP — Primary Method)

The agent handles signup directly. No website visit needed.

1. Ask the user for their email address
2. Send `POST https://api.authors-voice.com/auth/request-code` with `{ "email": "<user's email>" }`
3. Tell user: "Check your email for a 6-digit verification code from Author's Voice."
4. User provides the code
5. Send `POST https://api.authors-voice.com/auth/verify-code` with `{ "email": "<user's email>", "code": "<6 digits>" }`
6. Response: `{ "apiKey": "av_live_...", "tenantId": "email|..." }`
7. Save the API key to `~/.claude/skills/authors-voice/local/config.md` and set `AV_API_KEY`

**Rate limits**: 5 requests/min per IP, 60s cooldown between sends, max 3 attempts per code, code expires in 10 minutes.

**If email OTP fails**: Ask the user to get a key manually at [authors-voice.com/voice?tab=api-keys](https://authors-voice.com/voice?tab=api-keys).

## OpenWriter Plugin

Author's Voice also works inside [OpenWriter](https://openwriter.io). The plugin auto-resolves the API key from `~/.openwriter/config.json` if configured.

## Base URL

Defaults to production. Override with `AV_BASE_URL` env var.

```bash
AV_BASE_URL="https://api.authors-voice.com/api/voice/mcp"
```

## Connections

Connect Google Drive or Notion for importing writing samples:
> Visit **https://authors-voice.com/voice?tab=connections** to connect your accounts.

After connecting, call `list_google_drive_docs` or `list_notion_pages` to verify.
