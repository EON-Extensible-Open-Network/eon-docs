<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Account — plan

**20 November 2026.** Step three of four, after [research.md](research.md). The decision is
plan madde 46.

The goal of this plan is threefold:

- the server and the client are written and tested against each other;
- EON Stream offers "sign in or continue as a guest" on first start;
- a signed-in person's progress and scores follow them, encrypted.

**The project runs no server** until madde 30's Faz L1 gate is met. The software is
ready for the day it is.

## Shape

| Repository | What | Licence |
|---|---|---|
| `eon-account-spec` | The contract: the HTTP API, the cryptography (OPAQUE suite, key derivation, vault and assertion formats), JSON Schemas with valid and invalid examples checked in CI. | `Apache-2.0` |
| `eon-account-client` | A Rust library every EON application links. It runs OPAQUE, holds the keys, encrypts and decrypts the vault, and keeps the session. It has no GUI dependency. | `MIT`, like the SDKs, so linking it never raises a licensing question |
| `eon-account-server` | The reference server. It is one process with one SQLite file and uses `tiny_http`, like eon-stream-community. Its tests drive the real client over HTTP. | `AGPL-3.0-or-later` |

## Cryptography (v0, fixed in the spec)

- **OPAQUE, RFC 9807.** We use `opaque-ke` 4. The OPRF and key exchange both run over
  ristretto255, the key exchange is 3DH with SHA-512, and the key-stretching function is
  Argon2id with m = 64 MiB, t = 3, p = 1, run on the client. The credential identifier is
  the lower-cased username.
- **Keys.**
  - `password_key = HKDF-SHA256(export_key, info = "eon-account v0 password key")`.
  - The vault key is 32 random bytes.
  - The recovery key is 20 random bytes, written as 32 base32 characters in groups of
    four.
  - `recovery_wrap = HKDF(recovery, "eon-account v0 recovery key")` and
    `recovery_proof = HKDF(recovery, "eon-account v0 recovery proof")`. The server stores
    `SHA-256(recovery_proof)`.
- **Sealed box.** Format: `0x01 ‖ 24-byte nonce ‖ XChaCha20-Poly1305 ciphertext`.
  - The associated data names what the box is: `eon-account v0 wrap password`,
    `eon-account v0 wrap recovery`, or `eon-account v0 vault <app>/<name>`.
  - So a box cannot be moved to another place and still open.
- **Sessions.** A token is 32 random bytes; the server stores its SHA-256. A session lasts
  180 days from its last use.
- **Assertions.** An assertion is `base64url(JCS payload) . base64url(Ed25519 signature)`.
  - The payload is `{v, iss, aud, sub, name?, iat, exp, kid}`.
  - `sub` is the first 16 bytes of `HMAC-SHA256(pairwise secret, account id ‖ 0x00 ‖ aud)`.
    A service therefore sees one stable identifier, different from every other service's.
  - `name` is present only when the person chose to share their username with that
    audience.

## API (v0)

JSON over HTTPS. Binary values are base64url without padding. Errors are
`{"error": code, "message": text}`.

| Area | Endpoints |
|---|---|
| Server | `GET /v0/info` (name, version, whether registration is open, mail and SMS available), `GET /v0/keys`, `GET /v0/transparency` |
| Sign-up | `POST /v0/register/start` → `/finish` (the finish carries the OPAQUE record and the wrapped vault keys, and returns a session) |
| Sign-in | `POST /v0/login/start` → `/finish`. The login is a username or a verified e-mail. Unknown logins get a dummy OPAQUE answer, so the reply does not reveal whether an account exists. |
| Session | `GET /v0/me`, `GET /v0/me/sessions`, `DELETE /v0/me/sessions/{id}`, `POST /v0/logout` |
| Password | `POST /v0/me/password/start` → `/finish`. Every other session ends. |
| Contacts | `PUT /v0/me/email`, `POST /v0/me/email/verify`, `DELETE /v0/me/email`; the same under `/phone` |
| Recovery | `POST /v0/recover/start` → `/finish` with the recovery proof (keeps the vault). `POST /v0/reset/request` and `POST /v0/reset/verify` give a reset token from a code sent to a verified contact, then `recover` with that token. That path starts a new, empty vault. |
| Vault | `GET /v0/vault`, `GET /v0/vault/{app}/{name}`, `PUT /v0/vault/{app}/{name}` with `base_version` (a stale base gets `409` and the current version). Limits: 1 MiB per entry, 64 entries, 8 MiB in total. |
| Other services | `POST /v0/assertions` `{audience, share_username}` |
| The person's data | `GET /v0/me/export`, `DELETE /v0/me` |

Three rules about privacy behaviour:

- Adding an e-mail or phone always answers `202`. If the address already belongs to
  another account, the message sent to it names that account instead of carrying a code.
- `reset/request` always answers `202`.
- Rate limits live in memory: per login, per contact, and per address for the life of the
  process. They are never written to disk.

## The server

`eon-account-server --data DIR --listen ADDR`, with three options:

- `--mail-command "sendmail -t"`: an RFC 5322 message goes to the command's stdin.
- `--sms-command CMD`: the number is the argument and the text goes to stdin.
- `--outbox DIR`: for tests, each message becomes a file.

On first start the server writes its secrets to `DIR/secrets.json` with owner-only
permissions. The secrets are:

- the OPAQUE server setup;
- the Ed25519 assertion key;
- the pairwise secret;
- the contact lookup key and the contact encryption key.

It keeps the tables the transparency list names and nothing else. A test compares the
schema with the list, and CI greps for request logging, IP addresses written to disk and
any history table.

## The client

The interface is a single `Account` type over a folder, which is where the session and
the cached keys are kept:

- `register`, which returns the recovery key exactly once;
- `login`, `logout`, `me`;
- `change_password`, `recover_with_key`, `reset_with_code`;
- the contact methods;
- `vault_get`, and `vault_put` with a merge callback that runs when the base is stale;
- `assertion`, `export`, `delete`.

HTTP goes through `ureq` with rustls. The vault key is kept on the device, encrypted
under the session's own key, so that a restart does not ask for the password again.
Signing out removes it.

## EON Stream

1. **First start** shows a sheet with three choices: sign in, create an account, continue
   as a guest.
   - The guest choice is a full answer and the sheet never returns on its own (madde 7).
   - The sheet can always be reached again from the profile menu.
   - The server address is a field that starts empty, because the project runs no server
     yet (madde 30). The build carries the constant where the default will go.
2. **Sign-up** shows the recovery key and asks the person to keep it before continuing.
   The three recovery cases are explained in one short paragraph.
3. **Signed in.** Watch progress and scores are stored on the device as now, and also in
   the vault as `stream/history` and `stream/ratings`.
   - Merging is per entry, and the newest change wins.
   - Sync runs at sign-in, after playback stops, after a score changes, and on start.
4. **The community** pages need a signed-in account. They ask the community server with
   an EON Account assertion, not a separate community password. community-api moves to
   v0.1 in the same change.
5. **"What we keep"** is a page in Settings that renders the server's transparency
   document, so the person reads it from the server they actually use.

## Order of work

1. This plan, the research and madde 46, as an `eon-docs` PR.
2. `eon-account-spec` v0. Schemas and examples are checked in CI.
3. `eon-account-client` 0.1.0 with unit tests: the cryptography, the formats, and OPAQUE
   against an in-process server.
4. `eon-account-server` 0.1.0, tested end to end with the real client over loopback HTTP.
   Every route is covered, including recovery, deletion and the transparency check.
5. EON Stream: the first-start sheet, sign-in and sign-up, sync of progress and scores, and
   the community moved to assertions. Released as the next alpha, `v0.31`.
6. eon-stream-community accepts assertions. Its own password accounts remain for servers
   that want them, until the owner decides otherwise.

Other products link the client when they have something to keep. EON Download Manager
has nothing to sync today, so it gets no account code yet.
