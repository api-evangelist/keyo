---
name: Manage biometric identities
description: >-
  List, retrieve, update, and delete Keyo biometric Identities, including full
  deletion of biometric data.
api: openapi/keyo-openapi.yml
operations:
  - OAuthClientCredentials
  - IdentitiesGet
  - IdentityGet
  - IdentitiesPatch
  - IdentitiesDelete
method: generated
source: https://developers.keyo.co/rest-api/identities
---

# Manage biometric identities

Use this skill to administer existing Keyo Identities.

## Steps

1. **Authenticate** — `OAuthClientCredentials`
   `POST /oauth/token/` (Basic secret key) → bearer `access_token`. Reuse until
   `expires_in` elapses.

2. **List identities** — `IdentitiesGet`
   `GET /identities/?limit=&offset=`. Page with `limit`/`offset`; read `count`,
   `next`, `previous`, `results`.

3. **Retrieve one** — `IdentityGet`
   `GET /identities/{id}/`.

4. **Update** — `IdentitiesPatch`
   `PATCH /identities/{id}/` with any of `first_name`, `last_name`, `email`,
   `phone`, `metadata`. Partial updates only send changed fields.

5. **Delete** — `IdentitiesDelete`
   `DELETE /identities/{id}/`. This **permanently deletes the identity including
   its biometric data** and returns 204. Irreversible — confirm intent first.

## Rules
- All calls require `Authorization: Bearer <access-token>`.
- Deletion is destructive and biometric data cannot be recovered.
- See conventions/keyo-conventions.yml for pagination and metadata semantics.
