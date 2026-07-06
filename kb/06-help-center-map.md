# Shuffle.us — Help Center Map & Support Channels

> Part of the Shuffle.us knowledge base. See [README](../README.md) for index and sourcing notes.
> Root: **https://help.shuffle.us/en/** (Intercom-style knowledge base).
> ⚠️ Direct crawling was blocked by this session's network policy — this map was assembled from search-index results only and is **incomplete**. Re-crawl when network access is granted.

## Collections

| Collection | Articles (reported) | URL |
|---|---|---|
| General Information | ? | [collections/14287483](https://help.shuffle.us/en/collections/14287483-general-information) |
| Account & Verification | 5 | not surfaced |
| Payments | 5 | not surfaced |
| Shuffle Cash and Gold Coins | 16 | not surfaced |
| VIP and Bonuses | 4 | [collections/14287706](https://help.shuffle.us/en/collections/14287706-vip-and-bonuses) |
| Security | 13 (or 4 — snippet conflict ❓) | not surfaced |
| Provable Fairness | 4–5 | not surfaced |
| Shuffle.us Affiliate Program | 5 | not surfaced |
| FAQ | 4 | not surfaced |

## Confirmed article URLs (exact titles from search index)

| Article | URL | Notes |
|---|---|---|
| How to close your account? | [articles/11882027](https://help.shuffle.us/en/articles/11882027-how-to-close-your-account) | RG/account closure |
| How can I verify my account? | [articles/11882034](https://help.shuffle.us/en/articles/11882034-how-can-i-verify-my-account) | 3-tier KYC: L1 phone/info to play & purchase; L2 to redeem |
| Restricted States on Shuffle.us | [articles/11882042](https://help.shuffle.us/en/articles/11882042-restricted-states-on-shuffle-us) | Canonical state-eligibility source |
| Why hasn't my purchase arrived? | [articles/11882060](https://help.shuffle.us/en/articles/11882060-why-hasn-t-my-purchase-arrived) | Purchase processing |
| Bonus drops and how to redeem them | [articles/11882126](https://help.shuffle.us/en/articles/11882126-bonus-drops-and-how-to-redeem-them) | Promo mechanism |
| How are the weekly and monthly bonuses calculated? | [articles/12173970](https://help.shuffle.us/en/articles/12173970-how-are-the-weekly-and-monthly-bonuses-calculated) | VIP bonus math |
| Daily Bonuses – everything you need to know | [articles/12173977](https://help.shuffle.us/en/articles/12173977-daily-bonuses-everything-you-need-to-know) | Daily login mechanics |

Article-ID observation: IDs cluster around `118820xx` (launch-era articles) and `121739xx` (later additions) — useful for guessing/probing sibling article URLs when crawling.

## Support channels

- **Live chat**: widget bottom-right on shuffle.us; chatbot → live agent escalation; ~2-minute observed response vs 30-minute stated SLA; described as 24/7. 🟡 (review-site claims)
- **Email**: address not resolved from search (obfuscated in snippets); likely `support@shuffle.us` by convention. ❓
- **Help center**: self-serve KB at help.shuffle.us. ✅

## Crawl plan (when network access is granted)

1. Fetch `https://help.shuffle.us/en/` → enumerate all collections.
2. Fetch each collection page → enumerate all article URLs.
3. Fetch each article → store as markdown under `kb/help-center/<collection>/<slug>.md`.
4. Diff against this map; resolve the ❓ items in the other KB files (state lists, redemption SLAs, AMOE details, VIP tiers).
