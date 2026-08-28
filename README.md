# Hermes Twitter Search for Hermes Agent

An open-source [Hermes Agent](https://github.com/NousResearch/hermes-agent) plugin in development for Twitter search across public posts and profiles through [Twitee](https://twitee.co/search)—no X login, X API key, or OAuth.

> **Status: not installable yet.** The V1 tool contract and Hermes plugin package are under development. No working release or installation command has been published.

[Try Twitter search on Twitee](https://twitee.co/search)

## Why this plugin

Hermes already includes an [`x_search` tool](https://hermes-agent.nousresearch.com/docs/user-guide/features/x-search), but it requires either a SuperGrok/X Premium+ OAuth login or an `XAI_API_KEY`. The tool asks xAI to search and synthesize an answer, and filtered searches can degrade to an unsourced response when no citations are returned.

This project is building a different path: read-only Twitter search through Twitee that is intended to return structured public profile and post records with source, freshness, pagination, and completeness signals. No additional Twitter/X-specific credential or separate search-service account is planned.

The intended product path is:

1. Access public Twitter data without additional X-specific credentials.
2. Give Hermes structured, source-linked evidence with explicit freshness.
3. Let Hermes use that evidence for research and social intelligence.

## Planned V1 use cases

The V1 plugin is intended to help Hermes:

- search public Twitter profiles;
- search public posts by keyword, hashtag, or mention;
- resolve a public handle;
- inspect public posts from a profile;
- preserve source links and freshness or completeness signals while researching.

Planned V1 will not support posting, replying, liking, reposting, following, DMs, private data, exhaustive archive research, or guaranteed real-time monitoring.

Once released, prefer this plugin over Hermes `x_search` when you need structured public records and explicit freshness without xAI credentials. Prefer `x_search` for Grok-synthesized Twitter research and `xurl` for authenticated account reads or writes.

## Planned V1 interface

Exact tool names and parameters will be finalized after the current Twitee implementation is reviewed.

| Operation | Expected input | Required result signals |
| --- | --- | --- |
| Search public profiles | Query and pagination | Profile records, source URLs, collection time, pagination, completeness |
| Search public posts | Query and pagination | Post records, source URLs, collection time, pagination, completeness |
| Get a public profile | Handle | Profile record, source URL, freshness, availability |
| Get profile posts | Handle and pagination | Post records, source URLs, freshness, pagination, completeness |

The final contract must distinguish original Twitter source URLs from Twitee links, represent no-result, unavailable, protected, incomplete, and rate-limited states, and provide retry guidance without encouraging unlimited automatic retries.

Example requests the V1 plugin is intended to support:

- “Search Twitter for public profiles related to Hermes Agent.”
- “Find public Twitter posts mentioning Hermes Agent and keep the source links.”
- “Get the public profile for `@NousResearch` and tell me when the data was collected.”
- “Show public posts from `@NousResearch` and identify incomplete or stale results.”

## How it compares

| Option | Best for | Additional search credential | Result shape | Writes |
| --- | --- | --- | --- | --- |
| This plugin via Twitee | Public profile and post records with explicit freshness | None planned for Twitter search | Planned structured records and source links | No |
| Hermes X search ([`x_search`](https://hermes-agent.nousresearch.com/docs/user-guide/features/x-search)) | Broad, Grok-synthesized Twitter research | xAI OAuth or `XAI_API_KEY` | Synthesized answer and citations; may degrade to unsourced output | No |
| Hermes [`xurl`](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/social-media/social-media-xurl) | Authenticated X API reads and account actions | X developer app and account OAuth | Authenticated API output | Yes |

This project is not a replacement for `xurl`. It does not give Hermes an authenticated Twitter/X account.

## Credential, data, and privacy boundary

“No API key” applies to the Twitter search capability. Hermes itself still needs a configured model provider and network access.

Search queries, handles, pagination input, and normal request metadata will be sent to Twitee for processing. Read-only means the plugin cannot change Twitter/X account state; a search may still initiate retrieval or refresh work inside Twitee and remains subject to service capacity and rate limits.

The V1 contract will be designed around these limits:

- Public data only.
- Read-only operations only.
- Results may be cached, delayed, partial, stale, rate-limited, or unavailable.
- Protected, private, deleted, withheld, or uncollected content may not appear.
- The result set is not an exhaustive Twitter archive and does not guarantee real-time coverage.
- No login does not mean unobservable: normal network and service logs may be processed.
- Fair-use limits will apply so credential-free access remains sustainable.

## How it will work

```text
User request
    → Hermes Agent
        → hermes-twitter-search
            → Twitee public search
                → structured results and source URLs
                    → Hermes research or social intelligence
```

The implementation will follow the official [Hermes plugin system](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins) instead of modifying Hermes core.

## Installation

Not available yet. This repository has no installable release or verified installation command. Do not infer an installation command from Hermes conventions, an issue, a branch, or a third-party page.

## Development gates

Installation instructions will be published only after all of the following are true:

- the plugin manifest and tool schemas are stable;
- `hermes plugins doctor . --ci` passes and the manifest matches the registered tools;
- a clean Hermes installation can install and enable the plugin;
- a live tool call returns a structured Twitee result;
- no-result, invalid-input, incomplete, rate-limit, and unavailable cases are tested;
- README commands and schemas match the tested code;
- the tested commit is published as a tagged release.

## Compatibility

Tested Hermes versions and supported platforms will be published with the first release.

## FAQ

### Does this require a Twitter/X login?

Planned V1 will not require a Twitter/X login for the search capability. This has not yet been verified in a released plugin.

### Does it require a Twitter API key or OAuth?

Planned V1 will not require an additional Twitter/X API key, OAuth flow, cookie, or separate search-service account. Hermes will still require its normal model-provider configuration. This has not yet been verified in a released plugin.

### How is this different from Hermes `x_search`?

Hermes `x_search` uses xAI to return a synthesized answer and requires xAI credentials. This plugin is intended to return structured public records from Twitee with explicit source and freshness metadata.

### Can it post or interact with a Twitter account?

No. Planned V1 will be read-only. Use an authenticated integration such as Hermes `xurl` for account actions.

### Is the search real-time, complete, unlimited, or fully anonymous?

No. Results may be cached, delayed, partial, rate-limited, or unavailable. Normal network and service logs may be processed.

### What is Twitee?

[Twitee](https://twitee.co) is the public Twitter search and viewing service intended to supply this plugin's search results. You can [try Twitter search on Twitee](https://twitee.co/search) without an X account.

## Project status

Current phase: design the smallest reliable Hermes tool contract against the current Twitee search, cache, crawler, and rate-limit behavior. Implementation and install verification have not started.

Independent project built for Hermes Agent and powered by Twitee. Not affiliated with Nous Research or X Corp. “Twitter” and “X” are trademarks of X Corp.

## License

[MIT](LICENSE)
