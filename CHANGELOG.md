# Changelog

All notable changes to this package are documented here. Format
loosely follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [2.0.0]

Updated to match the `enciphers` 2.0 rewrite (standard AEAD ciphers
instead of the previous custom substitution design). See that
package's own [CHANGELOG.md](https://github.com/mjlad/enciphers/blob/main/CHANGELOG.md)
for the underlying rationale.

### Added
- Session cookies with `session.permanent` set now bind their expiry
  inside the encrypted token itself (`expires_at`), not just in the
  cookie's own `Max-Age`/`Expires` attributes — a captured cookie can't
  be replayed past that point even if a client ignores its expiration.
  Non-permanent sessions are unaffected.

### Changed
- `ENCIPHERS_STEP` config key replaced with `ENCIPHERS_BACKEND`
  (`"AES256_GCM"` or `"XCHACHA20_POLY1305"`, defaults to
  `"AES256_GCM"`).
- The randomly-generated default key (when neither `ENCIPHERS_KEY` nor
  `ENCIPHERS_KEY_ENV` is set) grew from 64 to 128 bits, matching
  `enciphers`' own key size increase.
- `enciphers` dependency pinned to `>= 2, < 3` (previously unpinned,
  which is what let this package silently break against the 2.0
  release in the first place).

### Removed
- `ENCIPHERS_STEP` and the substitution-cipher concept it configured.

### Upgrading from 0.1.x
Existing session cookies minted by 0.1.x cannot be read after
upgrading — they'll simply fail to decrypt and the user gets a fresh,
empty session (the existing `except Exception` fallback in
`open_session` already handles this gracefully; no code change needed
on your end). This only matters for sessions with `permanent=True`, as
plain session cookies are cleared when the browser closes anyway.

## [0.1.1] and earlier

Initial releases, built on `enciphers` 0.x (custom substitution cipher
with HMAC-SHA256).
