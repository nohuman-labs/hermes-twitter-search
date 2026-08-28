# Twitter Search for Hermes Agent

An open-source [Hermes Agent](https://github.com/NousResearch/hermes-agent) plugin for Twitter search across public posts and profiles through Twitee—no X login, X API key, or OAuth.

Try [twitter search](https://twitee.co/search) on Twitee.

## Why this plugin

Getting public Twitter evidence into Hermes should not require an X developer app, API key, OAuth flow, or persistent browser session.

Hermes already includes an [X search tool (`x_search`)](https://hermes-agent.nousresearch.com/docs/user-guide/features/x-search) for Grok-synthesized research, but it requires xAI OAuth or an `XAI_API_KEY`. Authenticated tools such as [`xurl`](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/social-media/social-media-xurl) are built for raw X API access and account actions. Keyless alternatives also exist: remote MCP services require separate MCP configuration, while browser-based tools require a local browser runtime.

Hermes Twitter Search defines a focused, read-only retrieval interface powered by Twitee and packaged for the Hermes plugin system. The interface centers on structured public records instead of asking another model to summarize the search first:

- public profiles and posts;
- original source URLs;
- collection and freshness context;
- pagination state; and
- completeness or degraded-result signals.

Use this plugin for source-linked Twitter search and social intelligence without another X-specific credential or search-provider account. Use another integration for posting, replying, liking, reposting, following, DMs, private account context, exhaustive archive access, or guaranteed real-time coverage.

Twitee also provides a [twitter web viewer](https://twitee.co/) for human inspection, so source-linked results can be reviewed outside the agent workflow.

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

| Option | Best fit | Additional setup or credentials | Result and action model |
| --- | --- | --- | --- |
| **Hermes Twitter Search via Twitee** | Structured public profile and post evidence through a native Hermes plugin | No additional X-specific login, API key, OAuth, browser session, or search-provider account; normal Hermes model setup still applies | Structured-record contract with source, freshness, pagination, and completeness context; read-only |
| Hermes [`x_search`](https://hermes-agent.nousresearch.com/docs/user-guide/features/x-search) | Broad Twitter research synthesized by Grok | SuperGrok/X Premium+ OAuth or `XAI_API_KEY` | Synthesized answer and citations when available; no writes |
| Hermes [`xurl`](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/social-media/social-media-xurl) | Exact official X API reads and authenticated account actions | X developer app credentials and account OAuth | Raw authenticated API output; supports writes |
| [Hermes Tweet](https://github.com/Xquik-dev/hermes-tweet) / [Hermes XAPI](https://github.com/twexapi-dev/hermes-xapi) | Provider-backed reads, monitoring, and gated actions | `XQUIK_API_KEY` / `TWEXAPI_KEY` for live reads | Provider-specific structured endpoints; actions available behind explicit gates |
| [x-mcp](https://github.com/kandotrun/x-mcp) | Generic public, read-only retrieval across MCP clients | Remote MCP and skill configuration in Hermes | Structured MCP tools; no account writes |
| [Twitter MCP](https://github.com/Miles0sage/twitter-mcp) | Local browser-based reads and account actions | Playwright/Chromium runtime; persistent browser login for account-specific operations | Browser-derived records; supports writes after login |

Choose Hermes Twitter Search for structured Twitee records inside Hermes without additional X-specific or xAI search credentials. Choose `x_search` for Grok synthesis, `xurl` for official authenticated X API objects or confirmed writes, Hermes Tweet/XAPI for provider-backed monitoring or gated actions, `x-mcp` for a generic remote MCP across clients, and Twitter MCP for a local Playwright workflow.

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

[Twitee](https://twitee.co/about) is an independent [twitter viewer](https://twitee.co/) for public profiles and posts from X. It works as a [twitter viewer without account](https://twitee.co/features), although searches and page requests still generate normal service and network metadata.

Read the [Terms](https://twitee.co/terms) and [Privacy Policy](https://twitee.co/privacy) for the current service and data boundaries.

### Can I inspect public posts and profiles on Twitee?

Yes. Browse the [twitter post viewer](https://twitee.co/trending/posts) for public post discovery, or use this live [twitter profile viewer](https://twitee.co/thsottiaux) to inspect Tibo's public posts about Codex resets. Results may be cached, delayed, incomplete, or different from what currently appears on X.

## License

[MIT](LICENSE)

Independent project built for Hermes Agent and powered by Twitee. Not affiliated with Nous Research or X Corp. “Twitter” and “X” are trademarks of X Corp.
