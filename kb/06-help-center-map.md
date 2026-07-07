# Shuffle.us — Help Center Map & Support Channels

> Part of the Shuffle.us knowledge base. See [README](../README.md) for index and sourcing notes.
> Root: **https://help.shuffle.us/en/** (Intercom knowledge base).
> ✅ **Fully crawled 2026-07-07** — all 75 articles mirrored verbatim (as markdown) under [kb/help-center/](help-center/INDEX.md).

## Collections (actual counts from crawl)

| Collection | Articles | Mirror | Live URL |
|---|---|---|---|
| General Information | 5 | [dir](help-center/general-information/) | [14287483](https://help.shuffle.us/en/collections/14287483-general-information) |
| Account & Verification | 5 | [dir](help-center/account-verification/) | [14287526](https://help.shuffle.us/en/collections/14287526-account-verification) |
| Payments | 19 | [dir](help-center/payments/) | [14287548](https://help.shuffle.us/en/collections/14287548-payments) |
| VIP and Bonuses | 13 | [dir](help-center/vip-and-bonuses/) | [14287706](https://help.shuffle.us/en/collections/14287706-vip-and-bonuses) |
| Shuffle Cash and Gold Coins | 4 | [dir](help-center/shuffle-cash-and-gold-coins/) | [14287755](https://help.shuffle.us/en/collections/14287755-shuffle-cash-and-gold-coins) |
| Security | 4 | [dir](help-center/security/) | [14287757](https://help.shuffle.us/en/collections/14287757-security) |
| Provable Fairness | 5 | [dir](help-center/provable-fairness/) | [14287769](https://help.shuffle.us/en/collections/14287769-provable-fairness) |
| FAQ | 16 | [dir](help-center/faq/) | [14287900](https://help.shuffle.us/en/collections/14287900-faq) |
| Shuffle.us Affiliate Program | 4 | [dir](help-center/shuffle-us-affiliate-program/) | [15157430](https://help.shuffle.us/en/collections/15157430-shuffle-us-affiliate-program) |

**Total: 75 articles.** Full per-article index with titles: [kb/help-center/INDEX.md](help-center/INDEX.md).

Article-ID observation: IDs cluster around `118820xx` (launch-era) and `121739xx`/`121740xx` (later additions) — a rough proxy for content age.

## Highlights worth knowing cold (support-critical articles)

- [Restricted States](help-center/account-verification/restricted-states-on-shuffle-us.md) — canonical accepted-state list (⚠️ conflicts with the Sweepstakes Rules; see [05-legal](05-legal-compliance.md)).
- [Redemption limits and requirements](help-center/payments/redemption-limits-and-requirements.md) — 1x playthrough, FL 5,000 SC/day cap, fiat 100 SC min & 100k SC/24h cap, mode-matching rule, L2 KYC.
- [How long will my bank transfer take?](help-center/payments/how-long-will-my-bank-transfer-take.md) — ACH ~1 business day, instant ACH hours, escalate after 4 business days.
- [Supported crypto assets and chains](help-center/payments/supported-crypto-assets-and-chains.md).
- [Purchase limit](help-center/payments/understanding-your-account-s-purchase-limit.md) — $9k/day fiat, unlimited crypto.
- [Daily bonuses](help-center/vip-and-bonuses/daily-bonuses-everything-you-need-to-know.md) — 5AM UTC, streak plateau 0.4 SC + 25k GC.
- [VIP progress](help-center/vip-and-bonuses/how-to-calculate-your-vip-progress.md) & [rakeback](help-center/vip-and-bonuses/what-is-rakeback-how-it-works-and-how-to-get-it.md) — XP model, 5% rakeback.

## Support channels

- **Live chat**: on-site widget ("Live Support" in nav); chatbot → live agent; review sites report ~2-minute responses vs 30-minute SLA, 24/7. ✅ exists / 🟡 SLA claims
- **Email**: **support@shuffle.us** ✅ (site nav + help-article contact blocks; appears obfuscated as `[email protected]` in mirrored markdown due to Cloudflare email protection)
- **Help center**: help.shuffle.us. ✅

## Re-crawl procedure

Scripts preserved in session scratchpad pattern; to refresh: re-run the collection-enumeration crawl (fetch `/en/`, extract `collections/` links, fetch each, extract `articles/` links, convert with html2text). Diff against `kb/help-center/` to spot new/changed articles. Note: Intercom image URLs in mirrors carry signed, expiring query params — expect image-link rot, text remains stable.
