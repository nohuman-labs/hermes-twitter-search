# Twitter Search for Hermes Agent

An open-source [Hermes Agent](https://github.com/NousResearch/hermes-agent) plugin for Twitter search across public posts and profiles through [Twitee](https://twitee.co)—no X login, X API key, or OAuth.

Try [Twitter search](https://twitee.co/search) on Twitee.

## Why this plugin

Hermes already includes an [`x_search` tool](https://hermes-agent.nousresearch.com/docs/user-guide/features/x-search), but it requires either a SuperGrok/X Premium+ OAuth login or an `XAI_API_KEY`. It asks xAI to search Twitter/X and return a synthesized answer with citations; filtered searches can degrade to an unsourced response when no citations are found.

Hermes Twitter Search takes a different path: read-only access to structured public profile and post records through Twitee, without another X-specific credential or search-service account. It gives Hermes source-linked evidence for research and social intelligence while preserving freshness, pagination, and completeness signals.

Use it to:

- search public Twitter profiles;
- search public posts by keyword, hashtag, or mention;
- resolve a public handle;
- inspect public posts from a profile; and
- preserve source and collection context while analyzing results.

It does not support posting, replying, liking, reposting, following, DMs, private data, exhaustive archive research, or guaranteed real-time monitoring.

## How it works

The plugin follows the official [Hermes plugin system](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins) instead of modifying Hermes core.

```text
User request
    → Hermes Agent
        → hermes-twitter-search
            → Twitee public search
                → structured records and source URLs
                    → Hermes research or social intelligence
```

### Search interface

| Operation | Expected input | Required result signals |
| --- | --- | --- |
| Search public profiles | Query and pagination | Profile records, source URLs, collection time, pagination, completeness |
| Search public posts | Query and pagination | Post records, source URLs, collection time, pagination, completeness |
| Get a public profile | Handle | Profile record, source URL, freshness, availability |
| Get profile posts | Handle and pagination | Post records, source URLs, freshness, pagination, completeness |

The result contract distinguishes original Twitter/X source URLs from Twitee links. It also represents no-result, unavailable, protected, incomplete, and rate-limited states without encouraging unlimited automatic retries.

### Data boundaries

- Hermes still needs its normal model-provider configuration and network access.
- Search queries, handles, pagination input, and normal request metadata are sent to Twitee for processing.
- Read-only means the plugin cannot change Twitter/X account state. A request may still initiate retrieval or refresh work inside Twitee.
- Results may be cached, delayed, partial, stale, rate-limited, or unavailable.
- Protected, private, deleted, withheld, or uncollected content may not appear.
- Fair-use limits keep access without additional X credentials sustainable.

## Feature comparison

| Option | Best for | Additional search credential | Result shape | Account writes |
| --- | --- | --- | --- | --- |
| This plugin via Twitee | Public profile and post records with explicit freshness | None for Twitter search | Structured records and source links | No |
| Hermes X search ([`x_search`](https://hermes-agent.nousresearch.com/docs/user-guide/features/x-search)) | Broad, Grok-synthesized Twitter research | xAI OAuth or `XAI_API_KEY` | Synthesized answer and citations; filtered results may degrade | No |
| Hermes [`xurl`](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/social-media/social-media-xurl) | Authenticated X API reads and account actions | X developer app and account OAuth | Authenticated API output | Yes |
| [Hermes Tweet](https://github.com/Xquik-dev/hermes-tweet) / [Hermes XAPI](https://github.com/twexapi-dev/hermes-xapi) | Provider-specific read, monitoring, and action catalogs | `XQUIK_API_KEY` / `TWEXAPI_KEY` for live reads | Provider-specific structured endpoints | Available behind explicit gates |

Choose this plugin when you need structured public records and explicit freshness without additional xAI or Twitter-search-provider credentials. Prefer `x_search` for Grok-synthesized Twitter research, and use `xurl` or another authenticated integration for account-specific reads and writes.

## Installation

Install a tagged, verified release through the native Hermes plugin manager. Third-party Hermes plugins are opt-in and must be explicitly enabled.

Use the version-specific command in [GitHub Releases](https://github.com/nohuman-labs/hermes-twitter-search/releases) rather than installing an untagged branch. Each release identifies its supported Hermes versions and platforms.

## FAQ

### Does this require a Twitter/X login?

No Twitter/X login is required for the search capability.

### Does it require a Twitter API key or OAuth?

No additional Twitter/X API key, OAuth flow, cookie, or separate search-service account is required. Hermes still requires its normal model-provider configuration.

### How is this different from Hermes `x_search`?

Hermes `x_search` uses xAI to return a synthesized answer and requires xAI credentials. This plugin returns structured public records from Twitee with explicit source and freshness information.

### Can it post or interact with a Twitter account?

No. The plugin is read-only. Use an authenticated integration such as Hermes `xurl` for account actions.

### Is the search real-time, complete, unlimited, or fully anonymous?

No. Results may be cached, delayed, partial, rate-limited, or unavailable. No login does not mean unobservable: normal network and service logs may be processed.

### What is Twitee?

[Twitee](https://twitee.co) is an independent search and viewing service for public profiles and posts from X. It may return cached data or request updated public information, so results may not always match X in real time. Read the [Twitee About page](https://twitee.co/about), [Terms](https://twitee.co/terms), and [Privacy Policy](https://twitee.co/privacy) for the current service and data boundaries.

## License

[MIT](LICENSE)

Independent project built for Hermes Agent and powered by Twitee. Not affiliated with Nous Research or X Corp. “Twitter” and “X” are trademarks of X Corp.
