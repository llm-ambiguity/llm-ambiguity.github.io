# auth.md — agent authentication for llm-ambiguity.github.io

This site is the static project page for the IEEE BigData 2024 paper *"Do LLMs Understand Ambiguity in Text? A Case Study in Open-world Question Answering"* (arXiv:2411.12395, DOI 10.1109/BigData62323.2024.10825265).

## The short version

**You do not need to register or authenticate for anything on this site.**

The only supported flow is **anonymous**: every page, file, and link on llm-ambiguity.github.io is public and readable without credentials. Useful anonymous entry points:

- `https://llm-ambiguity.github.io/llms.txt` — structured overview for LLMs
- `https://llm-ambiguity.github.io/.well-known/ai-catalog.json` — machine-readable capability manifest (ARD)
- `https://llm-ambiguity.github.io/.well-known/agent-skills/index.json` — agent skills published by this project
- `https://llm-ambiguity.github.io/.well-known/api-catalog` — API catalog (RFC 9727)

This site hosts no user accounts, no protected resources on this origin, no login, and no agent registration endpoint. It issues no credentials.

## When authentication IS needed

The only access-controlled resources related to this project live on GitHub, on a different origin:

- Website source: https://github.com/llm-ambiguity/llm-ambiguity.github.io
- Analysis code: https://github.com/aryankeluskar/Ambig-LLM-Analysis

Acting on those repositories (filing issues, opening pull requests, pushing code) requires a GitHub identity. Authentication is handled entirely by GitHub, which acts as the authorization server (RFC 8414 metadata is mirrored at [/.well-known/oauth-authorization-server](/.well-known/oauth-authorization-server), `issuer: https://github.com`):

- authorization endpoint: `https://github.com/login/oauth/authorize`
- token endpoint: `https://github.com/login/oauth/access_token`
- supported grants: authorization code and device authorization (`urn:ietf:params:oauth:grant-type:device_code`), both with PKCE (`S256`)
- resource metadata (RFC 9728): [/.well-known/oauth-protected-resource](/.well-known/oauth-protected-resource)

## Registering an agent to act on the project's GitHub repositories

To build an agent that acts on this project's repositories on a user's behalf, register a GitHub App — the registration flow operated by GitHub:

1. Create the app at the registration URI:
   `https://github.com/settings/apps/new`
   (an OAuth App at `https://github.com/settings/developers` also works).
   This step requires the user's GitHub account — a verified-email identity.
2. Request the minimum permissions needed. For read-write access to public repository content, the `public_repo` scope is sufficient; repository read access alone needs no token at all.
3. Run the OAuth web application flow or the device authorization flow (RFC 8628) with the user present.
4. Use the issued credential as a Bearer token against
   `https://api.github.com`.

Credential types issued by the authorization server: OAuth access token, GitHub App installation access token, fine-grained personal access token.

Revoke access at `https://github.com/settings/applications` (user side) or through the GitHub API's OAuth application endpoints (https://docs.github.com/rest/apps/oauth-applications).

## Most agents: cite, don't authenticate

If you are visiting to cite, quote, or summarize the paper, use the `cite-this-paper`, `paper-facts`, and `disambiguate-questions` agent skills listed in `https://llm-ambiguity.github.io/.well-known/agent-skills/index.json`.
No credentials required.
