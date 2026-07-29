---
title: "NangoHQ/nango: Build product integrations with AI."
source: "https://github.com/NangoHQ/nango"
author:
published:
created: 2026-07-28
description: "Build product integrations with AI. Contribute to NangoHQ/nango development by creating an account on GitHub."
tags:
  - "clippings"
---
[![](https://github.com/NangoHQ/nango/raw/master/assets/nango-logo.png?raw=true)](https://github.com/NangoHQ/nango/blob/master/assets/nango-logo.png?raw=true)

## Build product integrations with AI.

Connect your product & AI agents with 900+ APIs. Build, run, and maintain integrations with AI in code, on infrastructure built for scale.

[Website](https://nango.dev/) · [Docs](https://nango.dev/docs/) · [900+ APIs](https://nango.dev/docs/integrations/overview/) · [Slack Community](https://nango.dev/slack)

## What is Nango?

Nango is an open-source platform for building product integrations. It supports [**900+ APIs**](https://nango.dev/docs/integrations/overview) and works with any backend language, AI coding tool, and agent SDK.

You write integration logic as TypeScript functions, or let AI generate them for you, and deploy to Nango's production runtime. Nango handles auth, execution, scaling, and observability.

**Used in production by Replit, Ramp, Mercor, and hundreds more.**

## How it works

Nango gives you three primitives that cover every integration pattern:

### 1\. Auth

Managed OAuth, API keys, and token refresh for 900+ APIs. Embed a white-label auth flow in your app. Nango handles credentials, token storage, and multi-tenant connection management.

```
// Embed auth in your frontend
nango.openConnectUI({ onEvent: (event) => { /* handle completion */ } });
```

### 2\. Proxy

Make authenticated API requests on behalf of your users. Send requests through Nango's proxy: it resolves the provider, injects credentials, handles retries and rate limits, and returns the response.

```
import { Nango } from '@nangohq/node';

const nango = new Nango({ secretKey: '<NANGO-SECRET-KEY>' });

// Make an authenticated request to any API
const response = await nango.get({
    endpoint: '/v3/contacts',
    providerConfigKey: '<INTEGRATION-ID>',
    connectionId: '<CONNECTION-ID>'
});
```

### 3\. Functions

Write integration logic as TypeScript functions and deploy to Nango. Functions execute on a production runtime with built-in API access, retries, storage, and observability.

Use the **AI builder** to generate them from a description of your use case.

```
export default async function run(nango: Nango) {
    const { owner, repo, title, body } = nango.input;
    
    const response = await nango.post({
        endpoint: \`/repos/${owner}/${repo}/issues\`,
        data: { title, body }
    });
    
    return response.data;
}
```

## What you can build

Nango supports every common integration pattern:

| Use case | Description |
| --- | --- |
| [**AI tool calling & MCP**](https://nango.dev/docs/getting-started/use-cases/tool-calling) | Give AI agents the ability to act on external APIs |
| [**Data syncing**](https://nango.dev/docs/guides/functions/syncs/sync-functions) | One or two-way sync for RAG pipelines, indexing, and triggers |
| [**Webhook processing**](https://nango.dev/docs/getting-started/use-cases/webhooks-from-external-apis) | Receive and process webhooks from external APIs reliably |
| [**API unification**](https://nango.dev/docs/getting-started/use-cases/unified-apis) | Normalize APIs to your own universal schema |
| [**Actions**](https://nango.dev/docs/guides/functions/action-functions) | Write data and execute operations on behalf of your users |
| [**Per-customer config**](https://nango.dev/docs/getting-started/use-cases/customer-configuration) | Customize integration behavior for each customer |

## Quickstart

Get up and running in under 5 minutes:

**1\. Create an integration.** [Sign up](https://app.nango.dev/signup) (free, no credit card), then configure a new integration in the Integrations tab.

**2\. Authorize the API.** On the Connections tab, create a connection and complete the auth flow. Later, embed this in your product:

```
nango.openConnectUI({ onEvent: (event) => { /* handle completion */ } });
```

**3\. Access the API.** Retrieve credentials and make authenticated requests:

```
import { Nango } from '@nangohq/node';

const nango = new Nango({ secretKey: '<NANGO-SECRET-KEY>' });

const connection = await nango.getConnection('<INTEGRATION-ID>', '<CONNECTION-ID>');
console.log(connection.credentials);
```

Embed the [Auth](https://nango.dev/docs/guides/auth/auth-guide) flow in your product, make requests with the [Proxy](https://nango.dev/docs/guides/platform/proxy-requests), or build custom integrations with [Functions](https://nango.dev/docs/guides/functions/functions-guide).

## Why Nango?

**AI-generated, human-controlled code.** Nango's AI builder generates TypeScript integration functions from natural language. Unlike black-box solutions, you get readable code you can review, edit, and version control. With full type safety and a built-in testing framework.

**Production-grade infrastructure.** Nango processes billions of API requests. The runtime provides per-tenant isolation, elastic scaling, automatic retries, and rate-limit handling. Battle-tested by hundreds of companies in production.

**Auth for 900+ APIs, out of the box.** OAuth flows, token refresh, credential storage, and multi-tenant support handled for you. Connect to any API without building auth from scratch.

**Open source and self-hostable.** Nango is fully open source. Run it on Nango Cloud or self-host on your own infrastructure. SOC 2 Type II, HIPAA, and GDPR compliant.

**Fits your workflow.** Fully operable via CLI and API. Compatible with any backend language or framework, AI coding tools (Cursor, Codex, Claude Code), and agent SDKs (MCP, LangChain).

## Open-source vs. paid

Nango is available under the [Elastic License](https://github.com/NangoHQ/nango/blob/master/LICENSE). The Cloud and Enterprise Self-Hosted versions give you access to all features, based on your [plan](https://www.nango.dev/pricing). You can also [self-host for free](https://nango.dev/docs/guides/platform/self-hosting#free-self-hosting) with a limited feature set.

## Contributing

We welcome contributions — anyone can [add support for a new API](https://nango.dev/docs/integrations/contribute-or-request-api).

Thank you to all contributors ❤️

[![](https://camo.githubusercontent.com/56ffc2c6bd9670a629a121ad9fb80eb000719d056d8dff6cbe89de7e04944b64/68747470733a2f2f636f6e747269622e726f636b732f696d6167653f7265706f3d6e616e676f68712f6e616e676f)](https://github.com/nangohq/nango/graphs/contributors)