# Hermes Twitter Search

Twitter/X search for [Hermes Agent](https://github.com/NousResearch/hermes-agent), powered by [Twitee](https://twitee.co).

**No X login. No X API key. No additional authentication for X search.**

## Why

Existing Hermes integrations for Twitter/X commonly require an X developer account, OAuth, an API key, or a separate paid service. This project aims to make public Twitter/X search available to Hermes immediately after installation, without asking the user for another credential.

The product path is:

1. Credential-free access to public Twitter/X data.
2. Structured results with source links and explicit freshness.
3. Hermes reasoning over those results for research and social intelligence.

## Planned v1

The first version will be intentionally small and read-only:

- Search public profiles.
- Search public posts.
- Get a public profile.
- Get public posts from a profile.
- Return source URLs, collection time, pagination, and incomplete or degraded status where applicable.

Bulk export, continuous monitoring, and write actions are not part of v1.

## Product principles

- Zero additional credentials for Twitter/X search.
- Useful immediately after installation.
- Read-only access to public data.
- Source-linked, structured responses for agent use.
- Honest freshness and completeness signals.
- Fair-use limits that keep anonymous access sustainable.

## Status

Product direction is approved. The Hermes plugin interface and Twitee API contract are being designed against the current Twitee implementation.

This repository does not claim unlimited, complete, real-time, or fully anonymous access. No account is required, but normal network and service logs may still be processed.

## License

[MIT](LICENSE)
