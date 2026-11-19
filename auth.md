# auth.md

You are an agent. This document tells you how authentication works for **llm-ambiguity.github.io** — the project page for the IEEE BigData 2024 paper *"Do LLMs Understand Ambiguity in Text? A Case Study in Open-world Question Answering"* (arXiv:2411.12395, DOI 10.1109/BigData62323.2024.10825265).

This is the **anonymous** flow: you do not need to register a credential for anything this site hosts. Every page, file, and endpoint on llm-ambiguity.github.io is public and readable without credentials, accounts, or registration. There is no login, no protected resource on this origin, and this site issues no credentials.

## Step 0 — Use the site anonymously (all you need for citing or summarizing)

```http
GET / HTTP/1.1
Host: llm-ambiguity.github.io
```

No request body. No authentication. 200 for everyone, agents included.

Useful public entry points:

- `/llms.txt` — structured overview of the paper for LLMs
- `/.well-known/ai-catalog.json` — machine-readable capability manifest (ARD)
- `/.well-known/agent-skills/index.json` — agent skills published by this project (cite-this-paper, paper-facts, disambiguate-questions)
- `/.well-known/api-catalog` — API catalog (RFC 9727) for the paper's data sources
- `/press.md` — press kit

If you came to cite, quote, or summarize the paper, stop here — the anonymous flow is all you need.

## Step 1 — Register a credential (only if you must act on this project's GitHub repositories)

The only access-controlled resources related to this project live on GitHub:

- Website source: https://github.com/llm-ambiguity/llm-ambiguity.github.io
- Analysis code: https://github.com/aryankeluskar/Ambig-LLM-Analysis

Acting on those repositories on a user's behalf (filing issues, opening pull requests, pushing code) requires a GitHub credential. **GitHub is the authorization server**; its endpoints live at `https://github.com`. One-time client registration (done by the repository owner or the user, not headless) is at https://github.com/settings/apps/new — request the minimum scopes (`public_repo` is enough for public repository content).

With a user present, use GitHub's device authorization flow (RFC 8628):

### 1a. Request a device code

```http
POST /login/device/code HTTP/1.1
Host: github.com
Content-Type: application/json
Accept: application/json

{
  "client_id": "<your client_id>",
  "scope": "public_repo"
}
```

Response (200):

```json
{
  "device_code": "…",
  "user_code": "XXXX-XXXX",
  "verification_uri": "https://github.com/login/device",
  "expires_in": 900,
  "interval": 5
}
```

### 1b. Send the user to the verification page

Surface `verification_uri` and `user_code` in your agent UI. Suggested prompts:

- Default ask: "Open https://github.com/login/device and enter the code `XXXX-XXXX` to authorize this agent to act on your behalf."
- If the user has no GitHub account: "You'll be prompted to sign in or create one first — that's expected."

### 1c. Poll for the access_token

```http
POST /login/oauth/access_token HTTP/1.1
Host: github.com
Content-Type: application/json
Accept: application/json

{
  "client_id": "<your client_id>",
  "device_code": "<device_code>",
  "grant_type": "urn:ietf:params:oauth:grant-type:device_code"
}
```

Response (200):

```json
{
  "access_token": "gho_…",
  "token_type": "bearer",
  "scope": "public_repo"
}
```

Poll every `interval` seconds until an `access_token` appears (see Errors for `authorization_pending` and `slow_down`).

## Step 2 — Use the credential

```http
GET /repos/llm-ambiguity/llm-ambiguity.github.io HTTP/1.1
Host: api.github.com
Authorization: Bearer <access_token>
```

GitHub also issues other credential types through the same authorization server: OAuth access tokens, GitHub App installation tokens, and fine-grained personal access tokens. All are used as Bearer tokens against `api.github.com`.

## Errors

| Status              | Where                 | Meaning                                   | What to do                                                       |
| ------------------- | --------------------- | ----------------------------------------- | ---------------------------------------------------------------- |
| authorization_pending | token polling       | User hasn't entered the code yet          | Keep polling at `interval`.                                       |
| slow_down           | token polling         | You are polling too fast                  | Increase your polling interval by 5 seconds.                      |
| expired_token       | token polling         | `device_code` expired (15 minutes)        | Restart at Step 1a with the user.                                 |
| 401                 | api.github.com        | `access_token` revoked or expired          | Restart at Step 1a with the user; do not stash and retry.        |
| 429                 | any                   | Rate limited                              | Exponential backoff, retry.                                       |
| 5xx                 | any                   | Transient server error                    | Exponential backoff, retry the same request.                      |

Revoke access at https://github.com/settings/applications (user side) or via the GitHub API's OAuth application endpoints (https://docs.github.com/rest/apps/oauth-applications).

## Discovery metadata

This site publishes discovery metadata for the flows above:

- `/.well-known/oauth-protected-resource` — RFC 9728: this site is the resource, `https://github.com` is the authorization server
- `/.well-known/oauth-authorization-server` — RFC 8414-style metadata mirroring GitHub's real endpoints (`issuer: https://github.com`; GitHub itself does not publish a well-known file). Its `agent_auth` block points back at this document: `skill: https://llm-ambiguity.github.io/auth.md`, `identity_types_supported: ["anonymous"]`
