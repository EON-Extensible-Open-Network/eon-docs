<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Account — research and analysis

**20 November 2026.** Steps one and two of four. The plan is [plan.md](plan.md); the
decision is plan madde 46.

## 0. What was asked

The owner wants one account for every EON product. The requirements, in the owner's order:

1. **It works the moment it has a server.** The software is complete and the client is
   ready; the only missing piece is an address to talk to.
2. **One account across EON.** An e-mail address that already belongs to an account leads
   back to that account and never creates a second one.
3. **Guest mode.** On first start the application asks the person to sign in, and
   "continue as a guest" is always one of the choices. A guest's watch progress and scores
   stay on the device. A signed-in person's are kept on the server, so they follow them to
   another machine.
4. **Community features need an account.**
5. **Anonymity first.** A username and a password are enough. An e-mail address or a
   phone number can be added, and neither is ever required.
6. **Nothing about anything outside our applications is kept, and what is kept is listed
   openly.**

## 1. The plan already says things about this

| Item | What it says | What it means here |
|---|---|---|
| madde 7 | The viewer works without an account and is **fully functional** without one. | Guest mode is not a reduced mode. A first-start prompt is fine as long as "guest" is a full answer and the prompt never comes back on its own. |
| madde 30 | Faz L0 has no hosting, no accounts and no backend run by the project. Faz L1 needs five gates, a legal entity among them. | We can **write** the server and the client. **The project does not run the server** until the L1 gate is met. eon-stream-community is in the same position. |
| madde 31 | Watch or playback history is **never collected**. Progress stays local. | This conflicts with requirement 3 as written. Section 4 shows how to keep progress on a server without the server being able to read it, so the rule still holds. |
| madde 36 | No telemetry. | The account server logs no IP address, user agent or request history. |
| madde 34 | Signatures are Ed25519 over JCS. | Assertions that other EON services trust are signed the same way. |
| community-api v0 | Has its own accounts: username, **required** e-mail, Argon2id hash. | The community server should accept EON Account sign-ins instead. The required e-mail goes away. |

## 2. How others do it

**Mullvad (VPN).** You get an account number and nothing else: no e-mail and no password.
The number is both the login and the secret. This is the strongest form of anonymity, but
anyone who sees the number owns the account, and if you lose it the account is gone. The
owner asked for a *username* that people choose and other people can see, so we cannot
adopt this as it stands. Two ideas carry over. A random secret can serve as the recovery
path, and leaving the e-mail field empty is the expected choice, not a gap.

**Bitwarden, Proton, Standard Notes, Ente (end-to-end encrypted sync).** The client turns
the password into keys with a slow KDF; Bitwarden defaults to Argon2id with 64 MiB,
3 iterations and 4 threads. The server stores only ciphertext and a value derived from
the password. Bitwarden states that it "never stores and cannot access your master
password or your cryptographic keys". The price is the same everywhere: **if you forget
the password and have no recovery key, the encrypted data is gone.** Every one of these
services says so plainly when you sign up, and so must we.

**OPAQUE, RFC 9807 (2025).** An augmented PAKE. The password never reaches the server, not
even during registration and not even as a hash. The server keeps a record that it cannot
use to sign in as you. It is also resistant to precomputation: a stolen database still
requires a separate dictionary attack for each account. Each successful login gives the
client an `export_key` that the server never learns, which is exactly the root an
end-to-end encrypted vault needs. The Rust implementation is
[`opaque-ke`](https://github.com/facebook/opaque-ke), now version 4.0.0 and built to
RFC 9807. WhatsApp uses it for end-to-end encrypted backups. NCC Group audited it in
2021, before v4; no audit of v4 was found. An unaudited version is the cost we accept and
write down.

**Argon2id, password storage.** OWASP's minimum is m = 19 MiB, t = 2, p = 1, or an
equivalent trade-off. The usual target is a few hundred milliseconds on the machine that
does the hashing. In OPAQUE, Argon2id runs on the **client** as its key-stretching
function, so the server pays nothing per login and a stolen record still costs one
Argon2id call per guess.

**OpenID Connect pairwise subjects.** Each service that relies on the identity provider
gets a different, non-reversible identifier for the same person, so two services cannot
match their users with each other. Implementations derive it from a server secret, the
service's sector and the local account id (Keycloak uses SHA-256; Connect2id uses
AES-SIV). EON Account does the same for every service that trusts it.

**Turkish law, 5651.** A hosting provider (*yer sağlayıcı*) must keep traffic data for a
period set by regulation, between one and two years. The 2007 regulation said six
months, and the current text was not verified. Whether an account server run by a
non-profit counts as a hosting provider, and which traffic data that would cover, is one
of the questions for the lawyer (madde 13, madde 30 gate 3). **The server is written to
keep no traffic data.** If the law requires some, adding it becomes a decision recorded
in the plan, published on the transparency endpoint, and never done quietly.

## 3. What is wrong with the obvious design

The obvious design is the one community-api v0 already has: username, e-mail and an
Argon2id hash on the server, plus a sync table of progress rows. It has four problems.

1. **The server reads the password at every sign-in.** Hashing protects the stored copy,
   not the moment of arrival. A logging proxy, a debugging print or a compromised server
   collects passwords. People reuse passwords, so this is the most valuable thing a
   breach can yield.
2. **Progress rows are a watch history.** madde 31 forbids exactly that, and for good
   reason: the server, its operator and anyone who breaks in would know what every
   account watched and when.
3. **A required e-mail ties the account to a real person.** That defeats requirement 5.
4. **One identifier everywhere lets services link their users.** Once a second service
   trusts the account (a community server run by someone else, for example), the two
   can match their users.

## 4. Analysis: what EON Account must be

**Who knows what.**

| Data | Guest | Signed in: the server stores | The server can read it |
|---|---|---|---|
| Username | – | yes, public | yes, it is a name meant to be seen |
| Password | – | **no**: an OPAQUE record | no |
| E-mail (optional) | – | encrypted at rest, plus a keyed hash for lookup | only to send a message the person asked for |
| Phone (optional) | – | the same | the same |
| Watch progress, scores, the library | on the device | **ciphertext** | **no** |
| Size and last-change time of that ciphertext | – | yes | yes, and the transparency list says so |
| Sessions | – | a SHA-256 of each token, a device name, the creation time and the last-use day | yes |
| IP address, user agent, request log | – | **no**; rate limits are held in memory only | – |

That is how requirement 3 and madde 31 both hold. Progress lives on the server, but it is
encrypted on the device with a key the server never has. The server keeps data it cannot
read, which is not collecting a history. The plan item must say this explicitly, so that
nobody later "improves" the vault with a server-side index.

**The keys.** The OPAQUE `export_key` goes through HKDF to give a *password key*. A
random 256-bit *vault key* encrypts everything with XChaCha20-Poly1305. The server
stores the vault key wrapped twice: once under the password key and once under a *recovery
key*, a random 160-bit value shown once at sign-up as 32 base32 characters. Changing the
password re-wraps the vault key; the data itself is not re-encrypted.

**Recovery.** There are three cases, and the sign-up screen explains them before an
account exists.

- **Recovery key:** sets a new password and keeps everything.
- **Verified e-mail or phone, no recovery key:** sets a new password, but the vault starts
  empty because the old one cannot be decrypted.
- **Neither:** the account cannot be recovered. That is the price of needing nothing.

**"An e-mail that already belongs to an account leads back to it."** It must not become a
way to test whether an address is registered. Adding an address always answers "we sent
a code to it":

- If the address is free, the code verifies it.
- If it already belongs to an account, the message sent there says so and names that
  account's username, so the owner of the address learns where to sign in. Nobody else
  learns anything.

Sign-in accepts a verified e-mail address in place of the username.

**One account, many services.**
- **First-party EON applications** talk to the account server directly.
- **Every other service** gets a signed assertion from the server, made with the madde 34
  shape (Ed25519 over JCS). An assertion contains:
  - a pairwise subject for that audience;
  - the username, only when the person agrees to share it with that service;
  - an expiry.

  The service checks the assertion against the server's published keys and never sees a
  password or a session token.

**The community needs an account.** This is a client rule, not just a server one. With no
signed-in session, the EON Stream window shows the community pages as an invitation to
sign in, and the community client sends nothing.

**Transparency is a document the server serves.** `GET /v0/transparency` returns, in
English and Turkish, every kind of data the running server stores: why, for how long and
who can read it. It is generated from the same constants the schema uses, and a test fails
if a table appears that the list does not mention. `GET /v0/me/export` returns everything
about the account, ciphertext included. `DELETE /v0/me` removes all of it.

**Size.** A reference server anyone can read, like eon-stream-community: one process,
one SQLite file, `tiny_http`, no async runtime. E-mail and SMS are sent by an
operator-configured command such as `sendmail -t`. This keeps the server free of mail
libraries, and the server itself never contacts a provider.

## Sources

- RFC 9807, *The OPAQUE Augmented Password-Authenticated Key Exchange (aPAKE) Protocol*.
- `opaque-ke` 4.0.0: [docs.rs](https://docs.rs/crate/opaque-ke/latest), [README](https://docs.rs/crate/opaque-ke/4.0.0/source/README.md) (NCC Group audit, 2021).
- OWASP, [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html); the Argon2id minimum as reproduced by [Odin's `core:crypto/argon2id`](https://pkg.odin-lang.org/core/crypto/argon2id).
- Bitwarden, [Security white paper](https://bitwarden.com/help/bitwarden-security-white-paper/) and [KDF algorithms](https://bitwarden.com/help/kdf-algorithms/).
- Mullvad, [account numbers get longer and safer](https://mullvad.net/en/blog/mullvads-account-numbers-get-longer-and-safer) (2017).
- Pairwise subjects: [Curity](https://curity.io/docs/idsvr/latest/profiles/token-profile/clients/pairwise-subject-identifiers), [Connect2id](https://connect2id.com/products/server/docs/guides/pairwise-subject-identifiers).
- 5651 sayılı Kanun and the 2007 regulation on procedures and principles, through
  [Lexpera](https://www.lexpera.com.tr/mevzuat/yonetmelikler/internet-ortaminda-yapilan-yayinlarin-duzenlenmesine-dair-usul-ve-esaslar-hakkinda-yonetmelik).
  These are secondary copies; the current text must be checked on mevzuat.gov.tr.
