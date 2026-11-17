## K Vikram Kumar

QA engineer working on where AI actually helps testing — and where it doesn't.

Most "AI testing" tooling fails the moment the model is slow, rate-limited or
wrong. I'm interested in the harder version of the problem: automation that
uses a model when it adds something, falls back to deterministic logic when it
doesn't, and tells you which one it did.

### Featured

**[ai-e2e-framework](https://github.com/ai-stuff-qa/ai-e2e-framework)** —
Playwright + TypeScript E2E framework with self-healing locators, LLM failure
triage, and n8n workflow orchestration.

- **Self-healing locators.** A refactor renamed every selector on the login
  flow (`#username`, `#password`, `#login-btn`). The suite stayed green and
  reported the three rotted selectors as review items instead of passing
  silently. CI proves it by running the full suite against both markup
  variants on every push.
- **Failure triage.** A custom reporter groups failures by normalised error
  signature into root-cause buckets, separates flaky (passed-on-retry) from
  passed, and writes a Slack-ready summary plus machine-readable JSON.
- **n8n automation.** Nightly regression with triaged reporting, a
  webhook-driven PR smoke gate writing GitHub commit statuses, weekly
  flaky-test ranking, and an issue filer that de-duplicates by failure
  fingerprint.
- **The AI is optional.** Every AI feature has a deterministic fallback, so
  the suite runs identically with no API key. Model answers are content-hash
  cached on disk, making CI reproducible at zero marginal cost.

58 tests across unit, API, UI, accessibility and mobile projects. Green on both
markup variants, `tsc --noEmit` clean under `strict`, no API key required.

### Working with

`Playwright` · `TypeScript` · `Page Object Model` · `API testing` ·
`Accessibility testing (WCAG)` · `Custom reporters` · `GitHub Actions` ·
`n8n` · `Webhook integrations` · `Anthropic API` · `Flaky-test management`

### Something I learned the hard way

My triage classifier was filing genuine HTTP 503s as assertion failures.
Playwright colours its error output, and those ANSI escape sequences sit
between characters — so `\b5\d{2}\b` never matched the `503` inside
`\x1b[32m503\x1b[39m`. I only found it by running the suite against a
deliberately broken backend and reading the output instead of the pass count.
There's a regression test pinning it now.
