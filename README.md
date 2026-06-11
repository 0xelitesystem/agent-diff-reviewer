# agent-diff-reviewer

Paste a unified diff (from Claude Code, Cursor, Aider, or `git diff`), get a structured review with scary-changes detection. Browser only.

**Live demo:** https://0xelitesystem.github.io/agent-diff-reviewer/

## Use

Open [`index.html`](./index.html). Paste a diff. Click Review.

You get:

- Summary: files changed, lines added/removed, flag counts
- Flags by severity (high / medium / info)
- Per-file diff view with hunk parsing and add/remove highlighting

## What it flags

**High severity:**
- Modifications to security-relevant paths (`.env*`, `secrets/`, `auth/`, `credentials/`)
- Possible secrets in added lines (AWS keys, GitHub tokens, Stripe keys, OpenAI/Anthropic keys, generic credential literals, private key blocks)
- Deletion of security-relevant files

**Medium severity:**
- Test file deletions (verify behavior is tested elsewhere)
- Dependency manifest changes (package.json, pyproject.toml, Cargo.toml, etc.)
- Large unbalanced deletions (-50 lines while +5 lines added)

**Info:**
- File deletions outside security paths
- Very long lines added (could be data, generated code, minified content)
- Clean diffs with no flags ("read every line anyway")

## Why

After an agent finishes a task, the diff is the ground truth. The summary the agent writes is always more flattering than the diff. Reviewers who only read the summary merge things they didn't intend to.

This tool surfaces specific patterns worth scrutinizing, not as a replacement for reading the diff, but to focus attention.

## What it doesn't do

- Doesn't fetch from GitHub. Paste the diff yourself.
- Doesn't connect to any LLM for review.
- Doesn't run static analysis on the actual code.
- Doesn't prevent merging. It's a review aid, not a gate.

## Privacy

The diff stays in your browser. No upload, no analytics, no third-party scripts.

## Run locally

```
git clone https://github.com/0xelitesystem/agent-diff-reviewer
cd agent-diff-reviewer
```

Open `index.html` in a browser.

## Contribute

PRs welcome:

- More secret-detection patterns (especially region-specific cloud providers)
- Language-specific risky patterns (e.g. `eval`, `exec`, dangerous serialization)
- Configurable security path patterns
- GitHub PR URL parsing (with explicit user paste, no API)

Don't add: API integrations, telemetry, npm dependencies. Single file.

## Build

No build. Single HTML file.

## License

MIT.

## Related

- [ai-generated-code-review-rubrics](https://github.com/0xelitesystem/ai-generated-code-review-rubrics) - rubrics for thorough review
- [vibe-coding-anti-patterns](https://github.com/0xelitesystem/vibe-coding-anti-patterns) - patterns this tool helps detect
- [agentic-workflow-patterns](https://github.com/0xelitesystem/agentic-workflow-patterns) - diff-review-discipline pattern
- [secrets-scanner-bookmarklet](https://github.com/0xelitesystem/secrets-scanner-bookmarklet) - scan whole pages for secrets
