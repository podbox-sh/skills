# Set up PodBox

Use this for step 1 of the first run. It gets a token by the most autonomous path your rules allow. Do a step yourself when your rules allow it. When they do not, the step names the one thing the person does.

## Account

Call `account_status` when the PodBox tools are already present. Success means the account is open. Skip sign-in.

When you have no tools, add the remote server at `https://api.podbox.sh/mcp/v1` if your client can add one. Then get a token. When the tools are present and you have no token, get a token. Do not add the server again.

One screen signs up a new person and signs in an existing one. Do not ask whether they have an account.

- When you may open the flow, try the PKCE browser sign-in first. If it returns a token, use that token. If it fails, use the device code. Show the one link, once.
- When your rules require approval before you open the screen, ask once. Then use that same flow.
- When your rules forbid creating accounts, or you cannot open a link, stop. The person opens the screen. You wait. Never ask the person to paste a token.

Put the first-run questions in the same message as the link, so the person answers them while they sign in.

## Token

To get a token yourself, call `POST https://api.podbox.sh/mcp/v1` (JSON-RPC `tools/list` and `tools/call`) with `Authorization: Bearer <access token>`.

PodBox production authentication is the protected-resource document at `https://api.podbox.sh/.well-known/oauth-protected-resource/mcp/v1`. Read `client_id` and `authorization_servers` from that document. The authorization server is `https://clerk.podbox.sh`. Leave any saved Clerk application, client id, and issuer unused. Copy `client_id` from that document.

1. Call it without a token. The 401 `WWW-Authenticate` header names a `resource_metadata` URL. That URL is `https://api.podbox.sh/.well-known/oauth-protected-resource/mcp/v1`. Fetch it and use its `client_id` and `authorization_servers`.
2. Fetch `https://clerk.podbox.sh/.well-known/oauth-authorization-server`. `code_challenge_methods_supported` includes `S256`.
3. Build the authorization URL with `code_challenge` (the S256 hash of a `code_verifier`), `code_challenge_method=S256`, the document's `client_id`, `redirect_uri`, `response_type=code`, and `scope=podbox offline_access`. Before you show that link, GET `https://clerk.podbox.sh/oauth/authorize-with-immediate-redirect` with the same query and do not follow redirects. A 400 whose body says the redirect_uri is not registered means do not show the link. Use the device code in the next step. A 302 means show the person the authorization link you built. They open it. Exchange the code at the token endpoint with that `code_verifier` and no client secret.
4. If that sign-in fails, or you cannot open a redirect, POST the device authorization endpoint with the document's `client_id` and `scope=podbox offline_access`. Show the person the verification link. They open it. Poll the token endpoint until a token arrives. If the code expires, start again.
5. Store the refresh token and refresh when the access token expires, so the person does not sign in again.

Then return to step 2 of the first run in the skill.
