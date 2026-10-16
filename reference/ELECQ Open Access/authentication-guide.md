---
title: Authentication Guide
excerpt: >-
  Authorize a user, obtain an access token, and refresh it for ELECQ Open
  Access.
deprecated: false
hidden: false
metadata:
  robots: index
---
ELECQ Open Access uses the OAuth 2.0 authorization code flow. A user grants access to their resources, and your application exchanges the authorization code for an access token.

## Authentication at a glance

| Request | Authentication |
| --- | --- |
| `POST /api/v1/oauth2/token` | HTTP Basic: `client_id` as the username and `client_secret` as the password. |
| `/api/v1/access/**` resource endpoints | HTTP Bearer: the issued `access_token`. |

A `refresh_token` is submitted as a form field when refreshing an access token. Use the `access_token` to call resource APIs; an `id_token` is not a substitute.

## Before you begin

- Obtain a `client_id` and `client_secret` from your ELECQ integration contact.
- Register the callback URL for your application before starting authorization.
- Confirm the scopes available to your client. The example below requests `openid device.read`.
- Keep the client secret and refresh token in your backend. All example credentials on this page are placeholders.

## 1. Ask the user to authorize access

Open the authorization URL in the user’s browser. Replace the client ID, callback URL, scopes, and state with your application’s values:

```text
https://api.elecq.com/api/v1/oauth2/authorize?response_type=code&client_id=YOUR_CLIENT_ID&redirect_uri=https%3A%2F%2Fyour-app.example%2Foauth%2Fcallback&scope=openid%20device.read&state=YOUR_RANDOM_STATE
```

Generate a fresh random `state` for the authorization request. After consent, your callback receives `code` and `state`; verify that the returned state matches the value your application stored.

## 2. Exchange the authorization code

Send a form-encoded request to the Token endpoint. The `redirect_uri` must match the registered callback used in the authorization request.

```bash
curl --request POST 'https://api.elecq.com/api/v1/oauth2/token' \
  --user 'YOUR_CLIENT_ID:YOUR_CLIENT_SECRET' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=authorization_code' \
  --data-urlencode 'code=AUTH_CODE_FROM_CALLBACK' \
  --data-urlencode 'redirect_uri=https://your-app.example/oauth/callback'
```

Use the returned `access_token` for resource requests. Store a `refresh_token` if one is issued, and use the response’s `expires_in` value to plan when to refresh.

## 3. Call a resource endpoint

```bash
curl --request GET 'https://api.elecq.com/api/v1/access/hello' \
  --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

For manual testing in the API Reference, enter the token value in the Bearer credential field. ReadMe adds the `Authorization: Bearer` header.

## 4. Refresh an access token

Use the same Token endpoint and the same HTTP Basic authentication. This flow requires `grant_type=refresh_token` and the latest refresh token; it does not require an authorization code or redirect URI.

```bash
curl --request POST 'https://api.elecq.com/api/v1/oauth2/token' \
  --user 'YOUR_CLIENT_ID:YOUR_CLIENT_SECRET' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=refresh_token' \
  --data-urlencode 'refresh_token=YOUR_REFRESH_TOKEN'
```

Use the new access token. If the response includes a replacement refresh token, store it for subsequent refreshes; otherwise, keep the existing refresh token. If the refresh token is no longer valid, restart the authorization code flow.

## Troubleshooting

| Response | What to check |
| --- | --- |
| `400 invalid_grant` | The authorization code or refresh token is invalid, expired, or already used where applicable. Restart authorization if refreshing fails with this error. |
| `401 invalid_client` | Verify the client ID, client secret, and HTTP Basic authentication on the Token request. |
| `401` from a resource endpoint | Check that the Bearer access token is present and valid. Refresh an expired token. |
| `403` from a resource endpoint | Check the requested scopes, audience, and the user’s resource permissions. |

## API reference

- [Obtain / Refresh Tokens](/reference/post_api-v1-oauth2-token)
- [Hello Test](/reference/get_api-v1-access-hello)
- [List Devices](/reference/get_api-v1-access-user-devices)
