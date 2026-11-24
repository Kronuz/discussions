# Kronuz Discussions

A self-hosted, multi-tenant comment system for static sites. It includes a framework-free
browser widget, an Astro component, and a Cloudflare Worker backed by D1.

Each tenant has its own site metadata, allowed origins, OAuth client, moderators, limits,
notifications, optional private Atom feed, and optional static access capability. Comment
Markdown is rendered and sanitized by the Worker, then stored as HTML in D1.

## Features

- Threaded comments, replies, reactions, editing, deletion, and moderation.
- Generic OAuth 2.0 identity mapping, including separate OAuth clients per tenant.
- Public or capability-protected tenants selected by the URL path.
- Server-rendered Markdown with Shiki syntax highlighting.
- Discord, Slack, or Telegram notifications.
- Private per-tenant Atom feeds for recent comments.
- Local Astro development against either a local or production Worker.

## Layout

```text
astro/      ready-to-import Astro component
widget/     framework-free JavaScript and CSS widget
worker/     Hono Worker, D1 migrations, deployment scripts, tests, and operations docs
```

## Use in an Astro site

Install the client directly from this repository:

```sh
npm install github:Kronuz/discussions
```

Pin a commit in a site's `package.json` for reproducible builds. The package is not
published to npm.

```astro
---
import Discussions from '@kronuz/discussions/astro';
---

<Discussions
  backend="https://comments.example.com/my-blog"
  term="blog/my-post"
  title="My post"
  url="https://blog.example.com/blog/my-post/"
/>
```

The backend URL includes the tenant path. Keep `term` stable for the page.

## Deploy the Worker

1. Follow [`worker/TUTORIAL.md`](worker/TUTORIAL.md) to create D1, configure the Worker,
   register an OAuth client, and add a tenant.
2. Embed [`widget/`](widget/) directly for a non-Astro site.

The reader session is tenant-scoped. The widget stores the bearer token in local storage
after OAuth so sign-in works when browsers block cross-site cookies, with the signed cookie
as a fallback.

Tenants with an empty `accessKey` are public. A non-empty key is a static capability sent
by the widget in `X-Discussions-Key`. It is suitable for a private static site whose readers
can already access the generated page, but it is not a replacement for individual reader
authorization.

## Documentation

- [`astro/README.md`](astro/README.md): Astro component integration.
- [`widget/README.md`](widget/README.md): standalone embedding, attributes, and theming.
- [`worker/README.md`](worker/README.md): architecture, configuration, and API summary.
- [`worker/TUTORIAL.md`](worker/TUTORIAL.md): first deployment and tenant setup.
- [`worker/OPERATIONS.md`](worker/OPERATIONS.md): D1 inspection, backup, recovery, and
  routine administration.

Deployment-specific values and tenant documents are gitignored. The Worker remains a
separate deployable package under `worker/`.

Kronuz Discussions is MIT-licensed ([`LICENSE`](LICENSE)).
