# Ledgerline — H1 2026 strategy memo

Author: Mara Voss
Date: 2026-02-17
Status: internal, written for myself and shared with Dee and Sam

## Where we are

Eight months since launch (2025-06). 41 paying customers, $23.4k MRR as of this
week. Growth is steady but slow — roughly 4 net new customers a month since
November — and almost entirely from the Shopify app store listing and two
podcast appearances. No paid acquisition yet.

## Who actually buys (ICP)

Looking at the 41 accounts, the pattern is clearer than I expected:

- E-commerce companies doing **$5M–$50M GMV**, selling on 2+ channels
  (Shopify plus Amazon and/or wholesale).
- A finance team of **2–8 people**. The buyer is almost always the controller
  or the first finance hire, not the founder.
- The best-fit accounts all have someone whose actual job is month-end close.
  When that person exists, we expand; when the founder does the books, we churn.

Wrong fit, confirmed the hard way: sub-$1M GMV solo operators. They sign up
from the app store, never finish setup, and cancel inside two months. We should
stop optimizing the listing copy for them.

## Pricing signal

We've been $59/seat/month since launch. Two data points say we're underpriced:

1. We lost the Brightcart deal in December 2025 — their VP Finance told Dee,
   verbatim, that $59 made us look "too cheap to be serious" for something
   touching their revenue numbers.
2. A second mid-market prospect (unnamed here; ~$30M GMV) went with Reconlily
   in January 2026 despite our demo scoring better on their own evaluation
   sheet. Budget wasn't the issue; credibility was.

Meanwhile the low end costs us real support hours. Decision needed in March:
raise the price, and decide what happens to existing customers.

## Churn — what Q4 taught us

7 cancellations in Q4 2025. I did exit calls for all of them:

- **4 of 7**: onboarding failure. They couldn't map their chart of accounts in
  the first week and gave up. This is our biggest fixable problem.
- **2 of 7**: missing Amazon settlement report support. They reconcile Amazon
  payouts manually and we don't help.
- **1 of 7**: price sensitivity (a sub-$1M account — see ICP above; this one
  is fine to lose).

## Competitive landscape

- **Reconlily** — the incumbent. Enterprise-grade, strong brand with VP-level
  finance buyers, but onboarding reportedly takes 6–8 weeks and pricing starts
  around $1,500/month. We win against them on time-to-value when we get the
  chance to demo.
- **Tallystack** — closer to our segment, cheaper ($39/seat), but Shopify-only.
  Multi-channel is our wedge against them.
- **The real competitor is a spreadsheet and a part-time bookkeeper.** Most of
  our lost demos don't buy anything at all. The pitch has to beat "we manage
  fine", not just other tools.

## Priorities for H1 2026

1. **Fix onboarding**: guided chart-of-accounts mapping, target time-to-first-
   reconciliation under 1 day. Sam owns a proposal by 2026-03-15.
2. **Ship Amazon settlement report support** — directly addresses 2 of 7 Q4
   churns and the multi-channel wedge against Tallystack.
3. **Pricing decision in March** (meeting scheduled for 2026-03-04).
4. Explicitly deprioritized: the API, the QuickBooks Desktop connector, and
   any paid acquisition until onboarding is fixed.
