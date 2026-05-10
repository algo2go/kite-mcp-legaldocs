# kite-mcp-legaldocs

[![Go Reference](https://pkg.go.dev/badge/github.com/algo2go/kite-mcp-legaldocs.svg)](https://pkg.go.dev/github.com/algo2go/kite-mcp-legaldocs)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Embedded legal documents (Terms of Service, Privacy Notice) for the
algo2go ecosystem. Exposes the documents as `[]byte` slices suitable
for direct serving or for goldmark-rendered HTML.

Used by [`Sundeepg98/kite-mcp-server`](https://github.com/Sundeepg98/kite-mcp-server)
for the `/terms` and `/privacy` HTTP routes.

## Why a separate module?

Centralizing the legal-doc source-of-truth means every algo2go consumer
serves the same up-to-date Terms + Privacy Notice without copy-pasting.
When the documents are updated upstream and a new tag is published, all
consumers can opt in via a `go get -u` bump.

The `embed.go` scaffolding is trivial (~11 LOC); the value is in the
content + the version-as-source-of-truth contract.

## ⚠️ DRAFT documents — read before deploying

**TERMS.md and PRIVACY.md are DRAFT legal documents.** They are NOT
legal advice. Before serving them publicly:

1. Review for your jurisdiction (this draft is India-context, governed
   by Indian law)
2. Adapt placeholders to your deployment
3. Have a lawyer review for your specific use case + regulatory context
4. Replace `sundeepg8@gmail.com` with your own product/grievance email
   (the draft uses the upstream maintainer's contact as a default —
   appropriate ONLY for that maintainer's reference deployment)

The MIT license covers the embed.go scaffolding. The document content
is provided "AS IS" without warranty of legal sufficiency.

## Stability promise

**v0.x — unstable.** Variable signatures (`Privacy`, `Terms` as
`[]byte`) are unlikely to change, but the document content WILL evolve
as legal review surfaces issues. v1.0 ships only after at least one
external consumer ships against it AND a real lawyer review of the
documents.

## Install

```bash
go get github.com/algo2go/kite-mcp-legaldocs@v0.1.0
```

## Public API (embed.go)

```go
package legaldocs

import _ "embed"

//go:embed PRIVACY.md
var Privacy []byte

//go:embed TERMS.md
var Terms []byte
```

That is the entire API. Use `legaldocs.Privacy` and `legaldocs.Terms`
directly as byte slices. For HTML rendering, pass through
[goldmark](https://github.com/yuin/goldmark) or any markdown renderer.

## Reference consumer

[`Sundeepg98/kite-mcp-server`](https://github.com/Sundeepg98/kite-mcp-server)
— `app/legal.go` declares:

```go
var termsMarkdown = legaldocs.Terms
var privacyMarkdown = legaldocs.Privacy
```

Then renders them via goldmark for the `/terms` and `/privacy`
dashboard routes.

## License

MIT (scaffolding) — see [LICENSE](LICENSE). Document content provided
"AS IS" without warranty of legal sufficiency.

## Authors

Original design + draft documents: [Sundeepg98](https://github.com/Sundeepg98)
(Zerodha Tech). Multi-module promotion (2026-05-10): algo2go contributors.
