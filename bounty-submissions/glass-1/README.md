# GLASS quote-side pool price correction

Target: https://github.com/blake-guthrie/GLASS/issues/1

This is a complementary patch for `geckoTopPool().priceUsd`. It does not replace PR #29's OHLCV selector fix and does not claim to resolve the whole $100 issue.

For a pool whose base is USDC and quote is the requested token, read `quote_token_price_usd`, rather than the USDC `base_token_price_usd`. If the quote price is absent, return null. Existing base-side behavior is preserved.

## Apply

From the GLASS repository root:

```sh
git apply /path/to/quote-pool-price.patch
npm test
```

## Verification

Node v24.19.0; repository files read from current main on 2026-10-26 (Asia/Taipei).

- New tests against unchanged source: base test passed; quote and missing-quote-price cases failed (1 versus 0.002, and 1 versus null).
- Patched `npm test`: all 5 test files passed.
- Patched `node --test --test-isolation=none test/*.test.mjs`: 40 passed, 0 failed.
- Tests mock HTTP; no live API behavior or production price verification is claimed.

## Commercial and submission status

Proposed bounded milestone: USD 5 upon maintainer acceptance, subject to agreement. No funding or payment agreement has been confirmed. Actual receipts: USD 0.

The GitHub integration returned HTTP 403 when attempting to post the full patch and milestone proposal to the upstream issue. No upstream PR or successful upstream submission exists. This folder preserves the complete patch for review and later submission when repository access is available.

AI disclosure: OpenAI Codex authored and executed the patch and regression tests on behalf of FkInCnD. The upstream project is MIT licensed; the original repository's license applies to its code.
