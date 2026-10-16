---
title: Welcome to ELECQ
excerpt: Guides and API documentation for integrating with ELECQ Open Access.
hidden: false
---
Integrate with ELECQ Open Access to discover authorized devices and manage charging. Start with the guides below, then use the API Reference for request parameters and examples.

## Start building

<Cards>
  <Card title="Quick Start" href="/docs/quick-start" icon="fa-duotone fa-rocket-launch">Prepare your integration, obtain an access token, and make your first API request.</Card>

  <Card title="Authentication Guide" href="/docs/authentication-guide" icon="fa-duotone fa-key">Follow the OAuth2 authorization code flow, refresh tokens, and troubleshoot authentication.</Card>

  <Card title="API Reference" href="/reference" icon="fa-duotone fa-code-simple">Explore endpoint parameters, request examples, and responses.</Card>
</Cards>

## Find the right endpoint

| What you want to do | API documentation |
| --- | --- |
| Exchange an authorization code or refresh an access token | [Obtain / Refresh Tokens](/reference/post_api-v1-oauth2-token) |
| Check API connectivity and authentication | [Hello Test](/reference/get_api-v1-access-hello) |
| Discover the user’s devices | [List Devices](/reference/get_api-v1-access-user-devices) |
| Inspect a device using its `deviceId` | [Device Details](/reference/get_api-v1-access-user-device-deviceid) |

## Before your first request

Obtain your client credentials from your ELECQ integration contact, register your callback URL, and confirm your allowed scopes. Follow the [Authentication Guide](/docs/authentication-guide) to obtain an access token, then try [Hello Test](/reference/get_api-v1-access-hello).

The API base URL is `https://api.elecq.com`.