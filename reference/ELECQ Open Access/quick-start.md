---
title: Quick Start
excerpt: Make your first authenticated API request and discover the user’s devices.
deprecated: false
hidden: false
metadata:
  robots: index
---
Make your first authenticated request to ELECQ Open Access, then discover the devices available to the authorized user.

## API base URL

```text
https://api.elecq.com
```

## 1. Prepare your integration

Before making requests, obtain a `client_id` and `client_secret` from your ELECQ integration contact. Register your application’s callback URL and confirm the scopes available to your client.

The user must complete the authorization and consent flow before your application can access their resources. Follow the [Authentication Guide](/reference/authentication-guide) for the authorization URL, callback handling, and complete cURL examples.

## 2. Obtain an access token

1. Send the user through `GET /api/v1/oauth2/authorize`.
2. Receive the authorization `code` at your registered callback and verify the returned `state`.
3. Call [Obtain / Refresh Tokens](/reference/post_api-v1-oauth2-token) with `grant_type=authorization_code`, `code`, and the same `redirect_uri`.
4. Authenticate that Token request using HTTP Basic: your client ID as the username and client secret as the password.

Use the returned `access_token` for the following request. Store a refresh token if one is issued.

## 3. Make your first API call

The [Hello Test](/reference/get_api-v1-access-hello) endpoint checks connectivity and authentication without requiring device parameters.

```bash
curl --request GET 'https://api.elecq.com/api/v1/access/hello' \
  --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_ACCESS_TOKEN` with the value returned by the Token endpoint. A successful call returns HTTP `200 OK` with `Hello` in the response body.

### Try it in this documentation

1. Open [Hello Test](/reference/get_api-v1-access-hello).
2. Enter the access token value in the Bearer credential field.
3. Click **Try It!** and inspect the response.

The API Reference adds the Bearer header for you. You can also copy its generated request into your own integration.

## 4. Find the user’s devices

Call [List Devices](/reference/get_api-v1-access-user-devices) with the same Bearer access token. Use a returned `deviceId` when calling [Device Details](/reference/get_api-v1-access-user-device-deviceid) or other device-specific endpoints.

Access depends on the scopes granted to your client and the authorizing user’s permissions.

## 5. Keep your access token current

Use the token response’s `expires_in` value to plan refreshes. The same [Token endpoint](/reference/post_api-v1-oauth2-token) accepts `grant_type=refresh_token` and your latest refresh token, using HTTP Basic authentication.

If a replacement refresh token is returned, use it for subsequent refreshes. See the [Authentication Guide](/reference/authentication-guide) for refresh examples and common authentication errors.
