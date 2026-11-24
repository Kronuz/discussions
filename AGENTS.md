# AGENTS.md: Kronuz Discussions

This repository is the source of the Astro component, browser widget, and
Cloudflare Worker used by Kronuz sites. The blogs pin a commit of this repository
as `@kronuz/discussions`; Xapiand does not mount comments. Change shared code
here and update consumers after the checks pass.

The root package contains the client and remains private to the npm registry.
`worker/` is a separate deployable package. Deployment configuration, tenant
documents, and secrets stay in the gitignored files named by
`worker/TUTORIAL.md`.

Public commits use `Germán Méndez Bravo <german.mb@gmail.com>` on `main`.
Do not add AI attribution. Commit messages have an imperative subject and a
short, wrapped body. Run `quiet check` before tests or measurements and wrap
them in `quiet light` on the shared development machine.
