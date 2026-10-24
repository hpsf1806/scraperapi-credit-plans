# Ecommerce Data Scraping: How the Credit System Really Works, What Each Plan Costs, and How to Pick One for Price Monitoring

Most people searching for ecommerce data scraping want one of two things: a way to track competitor prices and stock levels without maintaining a proxy fleet, or a realistic cost estimate before committing to a scraping API. This article covers both. Quick summary of what matters: Amazon product pages cost 5 API credits each, not 1; a $49 plan with "100,000 credits" delivers far fewer real fetches than the number suggests; and the whole decision comes down to how hard your target sites fight back.

Before the details, here's the short version for readers who just want to pick a plan.

> **Quick picks for ecommerce projects**
> - Testing or small catalogs (under ~1,000 product pages/month): start with the **7-day free trial (5,000 credits, no credit card)** plus the permanent free plan of 1,000 credits/month.
> - Ongoing price monitoring on Shopify stores and easy targets: **Hobby at $49/month** or **Startup at $149/month** covers it.
> - Heavy catalogs or Amazon/Walmart at scale: **Business ($299/month, 3M credits)** or **Scaling ($475/month, 5M credits)**.
>
> 👉 [Open ScraperAPI's pricing page and claim the 5,000-credit free trial](https://www.scraperapi.com/?fp_ref=coupons)

## What ecommerce data scraping actually involves

Ecommerce data scraping is the automated extraction of product prices, stock status, listings, ratings, and reviews from online storefronts, on a schedule, into a database. It sounds broad, but in practice one use case dominates: price. Roughly 4 in 10 ecommerce scraping projects are built around price data — competitor monitoring, MAP compliance, and repricing. Everything else (listings, availability, reviews) mostly feeds that core.

The data teams actually collect, in rough order of how often it appears in production systems:

1. **Prices** — the field that changes most often on any product page, and the direct business signal behind most projects.
2. **Catalog data** — title, SKU, category, variant options, used to match the same product across Amazon and Walmart.
3. **Stock status** — in stock, out of stock, backorder, and expected ship dates; critical for arbitrage and drop-shipping.
4. **Ratings and reviews** — aggregate scores and counts are facts you can freely compare; review text and reviewer usernames carry privacy obligations under GDPR-style laws, so only collect them if you actually use them.
5. **Images and variants** — image changes are an early signal that a listing was reworked or the product swapped; note that a page's listed price is usually the lowest variant's price, which is where simple scrapers get prices wrong.

One detail worth knowing before you pay for anything: check whether the store exposes its own JSON first. Shopify stores often have a public `/products.json` endpoint, and many shops publish their catalog in the sitemap or an embedded JSON blob. Rendering a full browser costs roughly 10x more and is the most detectable footprint on the web. If a plain JSON endpoint gives you the data, you may not need a heavy scraping API at all.

## ScraperAPI's full plan lineup

ScraperAPI charges per API credit, monthly or annually (annual billing takes 10% off every paid tier). Every plan includes JS rendering, premium proxies, JSON auto-parsing, CAPTCHA handling, automatic retries, custom session support, unlimited bandwidth, and a 99.9% uptime guarantee. Here is the complete current lineup:

| Plan | Price/month | Annual price | Credits | Concurrent threads | Buy |
| --- | --- | --- | --- | --- | --- |
| Free plan | $0 | — | 1,000/mo | 5 | [Sign up for the free plan](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [Start Hobby](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [Start Startup](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [Start Business](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [Start Scaling](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [Start Professional](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [Start Advanced](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ | [Contact sales for Enterprise](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Two plan-level differences that don't show in the credit counts: Geotargeting on Hobby and Startup is limited to US and EU, while Business and above get country-level targeting; the analytics dashboard on the two lower tiers only keeps 30 days of history, higher tiers get unlimited.

## The credit math nobody front-pages: a credit is not a request

This is the part most comparison articles gloss over, and it changes which plan you should buy. ScraperAPI's own pricing FAQ states it plainly: a standard page costs 1 credit, **Amazon costs 5**, Google and Bing cost 25, LinkedIn costs 30, and sites behind Cloudflare, Datadome, or PerimeterX add 10 credits per request when the bypass succeeds.

For ecommerce work that means the headline numbers shrink fast. Run the numbers on Hobby's 100,000 credits:

- Plain Shopify product pages at 1 credit: ~100,000 pages/month.
- Amazon product pages at 5 credits: ~20,000 pages/month.
- Amazon pages with JS rendering (5–10 credits on top of the target cost): closer to 10,000–20,000 real fetches, per third-party review math.

A practical trick that keeps the bill sane: you can set a `max_cost` per request and use the Domain Cost Estimator in the dashboard to check the exact credit cost of any URL before you commit a crawl schedule to it. If your monitoring list is 500 Amazon ASINs checked daily at 5 credits each, that's 75,000 credits/month — Hobby barely holds, Startup doesn't break a sweat. If the same list is unprotected Shopify stores at 1 credit, Hobby leaves you huge headroom.

The other half of the math is what happens when credits run out. By default, scraping pauses until renewal. Your options, straight from the official docs:

- **Auto-upgrade** (opt-in, on Hobby through Professional): the account moves to the next tier automatically when credits run out, usually at a better per-credit price.
- **Pay-as-you-go** (Scaling, Professional, Advanced, Enterprise): keep scraping past your limit at a fixed per-credit rate, up to a monthly spending cap you set — a spending limit, not a prepayment.
- **Annual plans** (Hobby, Startup, Business, Professional): if you exhaust the yearly allocation early, you can re-subscribe near your billing date for a fresh batch, or switch to a higher monthly tier.

## Where ScraperAPI sits in an ecommerce pipeline

A price-monitoring pipeline has five stages: catalog discovery, fetching product pages, structured parsing, storage, and change detection. The first four are plumbing. The last one — change detection — is where the value lives, and it's cheap to do right: store a hash of the fields you care about, re-crawl on a schedule, and alert only when the hash changes. That keeps both your credit bill and the target site's load low, which matters a lot when every Amazon fetch costs 5 credits.

ScraperAPI's role is stages two and three. It handles proxies, retries, CAPTCHA attempts, and optional JS rendering at the fetch layer, and its Structured Data endpoints return pre-parsed JSON for supported domains like Amazon, eBay, Walmart, and Google — so for those targets you skip writing a parser entirely. Outside the supported endpoint list, you still write and maintain your own product parser; the API gets you the HTML, and the schema (stable key like SKU or canonical URL, title, brand, price, currency, availability, image URL, variant options, scraped_at) remains your job.

On cost per page for context: self-hosted static scraping runs about $0.03 per 1,000 pages in compute, while a rendered page through an API runs around $4.50 per 1,000. That gap is what you pay to not own the fetch-render-proxy layer yourself. Whether it's worth it depends almost entirely on how many engineering hours you'd otherwise spend keeping a homegrown scraper alive against sites that redesign every few months.

## How it compares with the other well-known tools

If you're shopping this category, you'll cross paths with a few names. Here are their publicly listed prices as of May 2026, information only:

| Tool | Pricing signal | Notable difference |
| --- | --- | --- |
| Bright Data | Web Scraper API at $1.50 per 1K successful records; Scale tier $499/month with 384K records | Product matching and taxonomy work remains yours; much higher success on protected targets |
| Apify | Starter $29/mo, Scale $199/mo, Business $999/mo, plus pay-as-you-go platform usage | You run or customize individual Actors; output schema varies per Actor |
| Oxylabs | Ecommerce scraper from $49/month, priced by successful results per target | Strong parsing on supported targets; enrichment sold separately |
| Firecrawl | 1 credit/page for page content, JSON mode +4 credits; Standard $83/mo (billed yearly) for 100K credits | Built for clean page content for AI apps, no unified ecommerce schema |
| Extralt | $29/mo for 10K credits; Scale from $100/mo for 100K credits | Ecommerce-only product extraction with a consistent schema, not a general scraper |

Where ScraperAPI wins on paper: a simple endpoint that treats everything as "send a URL, get a page," good docs, SDKs for Python, Node, PHP, Ruby, and Java, and pricing that starts at $49 with a genuine free tier. Where competitors beat it: Bright Data's Web Unlocker is independently measured around 95–98% success on the same hard-target class where ScraperAPI's overall success lands around 62–64%, at a higher price and with more setup friction.

## Where it struggles: know this before you buy

Independent benchmarks are the honest weak spot. One measured ScraperAPI at 72.57% success; another put it at 62.8–63.7%, just above a rough industry average of 58–59%. On pages behind Cloudflare and DataDome, users report outright 500 errors, and there's a billing quirk to watch: CAPTCHA pages or error pages have been reported as being returned as "successes" and billed accordingly. The company says it doesn't charge for failed requests (expected failure rate 1–3%) and support refunds misbilled credits when pointed out — but you should spot-check your responses for CAPTCHA pages billed as wins.

Speed is the other constraint: benchmarks recorded 20–40 seconds per request on Amazon and similar targets. That's fine for nightly price crawls and async batch jobs, wrong for anything time-sensitive.

If your target list is mostly Amazon and Walmart at high volume, or heavily protected sites, this tool will frustrate you — those are the hardest targets to scrape, with aggressive rate limits and CAPTCHAs, and their ToS explicitly prohibit scraping (a contract risk that exists regardless of which tool you use). If your list is Shopify stores, mid-size retailers, and lightly protected marketplaces, the economics look very different.

## Free trial, refund, and the fine print that affects your first month

The risk of trying this is essentially zero, and the details are official:

- **7-day free trial**: 5,000 credits, no credit card required.
- **Permanent free plan**: 1,000 credits/month at 5 concurrent connections, for small projects; more testing credits available on request via support.
- **7-day no-questions-asked refund** on paid plans — contact support and they process it.
- **Cancel anytime** from the dashboard, no cancellation fee.
- Billing is strictly per request to the API; there is no bandwidth-based pricing, and you can't buy individual proxies from the pool.

👉 [Start the 7-day trial with 5,000 free credits](https://www.scraperapi.com/?fp_ref=coupons)

One workflow suggestion that costs nothing: before paying, run your actual target URLs through the free credits and record two numbers — real success rate on your specific sites, and actual credits consumed per page. Both numbers are on your dashboard, and together they tell you exactly which plan your workload needs, better than any comparison table.

## FAQ

**How many products can I monitor with 100,000 credits?**
Depends entirely on the target: roughly 100,000 plain product pages, or about 20,000 Amazon pages, or 10,000–20,000 Amazon pages with JS rendering. Compute your list against the per-domain credit costs before choosing a plan.

**Is ScraperAPI good for scraping Amazon?**
It works — the Structured Data endpoint returns parsed Amazon data — but Amazon pages cost 5 credits each and it's one of the hardest targets, so verify the success rate on your ASIN list with the free trial first.

**What happens if I go over my plan's credits?**
Scraping pauses by default. You can enable auto-upgrade (Hobby through Professional), use pay-as-you-go with a self-set monthly cap (Scaling and above), or renew an annual plan early for fresh credits.

**Can I get a refund if it doesn't work for my sites?**
Yes — ScraperAPI offers a 7-day no-questions-asked refund policy on paid plans, and cancellation is free at any time.

**Is scraping ecommerce sites legal?**
In the US, politely scraping publicly available data is broadly legal per the hiQ v. LinkedIn line of cases, but Amazon's and Walmart's terms of service explicitly prohibit scraping, so treat those targets as a contract risk. Prefer official APIs where they exist, respect robots.txt, and don't resell content wholesale.

## Picking a plan, by situation

Just validating the idea or tracking a few dozen Shopify products → stay on the free plan and trial; 6,000 combined credits is plenty for a proof of concept. Steady price monitoring on easy targets, up to ~15,000 Amazon-page-equivalents a month → Hobby at $49, or Startup at $149 if you need EU/US geotargeting plus more concurrency. Amazon/Walmart-heavy catalogs above ~20,000 pages a month → Business at $299, and if you regularly blow through it, enable pay-as-you-go on Scaling rather than auto-upgrading tier by tier. Mostly Cloudflare-protected targets → ScraperAPI will fight you; a dedicated unlocker with higher success rates is the better fit, even at a higher price.

Before you click buy, confirm two things: your monthly page count in real requests (not credits), and the credit cost of each domain on your list. Then 👉 [check current plan prices and start your free trial](https://www.scraperapi.com/?fp_ref=coupons) — the trial makes both numbers concrete before any money changes hands.
