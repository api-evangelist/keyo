---
name: Enroll a palm biometric identity
description: >-
  Create a Keyo Identity for a user and start palm biometric enrollment on a
  Keyo device, then confirm via webhook.
api: openapi/keyo-openapi.yml
operations:
  - OAuthClientCredentials
  - IdentitiesPost
  - IdentitiesStartEnroll
method: generated
source: https://developers.keyo.co/rest-api/identities
---

# Enroll a palm biometric identity

Use this skill to onboard a user into Keyo's palm biometric system.

## Prerequisites
- A base64 secret key (client_id:client_secret) from your organization dashboard
  → API credentials page.
- A Keyo `device_id` for the device where the user will enroll.

## Steps

1. **Get an access token** — `OAuthClientCredentials`
   `POST /oauth/token/` with header `Authorization: Basic <secret-key>` and body
   `grant_type=client_credentials` (form-urlencoded). Read `access_token` and
   `expires_in` from the response. Send `Authorization: Bearer <access-token>`
   on all later calls and refresh before expiry.

2. **Create the identity** — `IdentitiesPost`
   `POST /identities/` with `first_name` and `last_name` required, plus either
   `email` or `phone`. Put your own user id in `metadata` (string key-values) so
   you can reconcile later. Store the returned `id`.

3. **Start biometric enrollment** — `IdentitiesStartEnroll`
   `POST /identities/{id}/start-enroll/` with `{ "device_id": <id> }`. This
   starts the guided palm-capture workflow on the device. Ensure the correct,
   authorized individual is present (verify ID / OTP as your risk model requires).

4. **Confirm via webhook** — listen for `palm.enrollment.succeeded` (or handle
   `palm.enrollment.failed`) on your registered webhook endpoint. Verify the
   `X-Keyo-Token` header if you configured one, and return HTTP 200.

## Rules
- No idempotency key is supported; guard against duplicate `IdentitiesPost`
  calls yourself (e.g. check `metadata` for an existing user id first).
- Errors on the token call use OAuth codes (`invalid_client`, etc.) — see
  errors/keyo-problem-types.yml.
