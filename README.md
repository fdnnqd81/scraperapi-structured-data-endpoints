# ScraperAPI Structured Data Endpoints: Which Domains Return JSON, What Each Request Costs in Credits, and Which Plan Fits Your Volume (Full Endpoint and Plan Comparison)

ScraperAPI's structured data endpoints return finished JSON for a fixed list of high-demand domains: Amazon, Google, Walmart, Home Depot, Redfin, LinkedIn, eBay, Etsy and about a dozen others. The price of a request is set by the domain, not by which endpoint you call. Amazon runs 5 credits per request, Google 25, Walmart 1, LinkedIn 30. That single rule decides which plan you need, and it has nothing to do with how many endpoints you plan to use.

Below is the full endpoint list with per-request credit costs, what 1,000 requests actually cost on each tier, the billing rules that catch people out (404s are billable), and what the BigBox shutdown changed for Home Depot data.

> **Quick pick.** Amazon-only work at moderate volume fits Hobby at $49/month, which covers 20,000 product requests. Google SERP at 25 credits a request makes the same $49 buy 4,000 searches, which is thin for daily rank tracking, so budget Startup at $149. Teams pulling both, or needing country-level targeting outside the US and EU, should look at Business at $299. 👉 [Start with the Hobby plan at $49/month](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Plans and credit allowances

| Plan | Credits per month | Monthly price | Annual (per month) | Get it |
| --- | --- | --- | --- | --- |
| Free | 1,000 | $0 | — | [Free plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | 100,000 | $49 | $44.10 | [Hobby pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | 1,000,000 | $149 | $134.10 | [Startup pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | 3,000,000 | $299 | $269.10 | [Business pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | 5,000,000 | $475 | $427.50 | [Scaling pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom (5M+) | Custom | Custom | [Enterprise enquiries](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Annual billing knocks roughly 10% off each tier. Above Scaling there are larger self-serve tiers listed at $975 and $1,975 per month, plus custom Enterprise contracts; the pricing page lists concurrent-thread limits alongside the credit allowances if parallel jobs matter to you.

A free trial gives you 5,000 credits for seven days without a card, and the free plan renews at 1,000 credits a month. Five thousand credits is 1,000 Amazon product requests or 200 Google searches, which is enough to measure your real consumption before you commit to a tier.

## Every endpoint, grouped by domain

These are pre-built integrations. You send a URL to a domain-specific path and get named fields back, with ScraperAPI maintaining the parsing logic when a site changes its layout. The pattern looks like `https://api.scraperapi.com/structured/google/search`.

### Amazon — 5 credits per request

| Endpoint | Returns |
| --- | --- |
| Product | Title, price, rating, review count, ASIN, availability, images, description, specifications, seller info |
| Search | Result listings with title, price, ASIN, rating and a sponsored flag per result |
| Offers | Third-party seller offers: seller name, condition, price, shipping |

All three cost 5 credits. Pulling a product, then its offers, then its reviews is three requests and 15 credits.

### Google — 25 credits per request

| Endpoint | Returns |
| --- | --- |
| Search (SERP) | Organic results, ads, featured snippets, knowledge panel, related searches |
| Shopping | Listings with price, merchant, rating, availability |
| News | Articles with headline, source, date, summary |
| Jobs | Listings with title, company, location, salary, description |
| Maps | Businesses with name, address, phone, rating, reviews, hours |

### Walmart — 1 credit per request

Product, Search, Category and Reviews endpoints, all at standard-domain pricing. Walmart is the cheapest of the majors by a factor of five against Amazon and twenty-five against Google.

### LinkedIn — 30 credits per request

Job listings, priced above Google and the most expensive published rate on the list.

### Home Depot — rate not published

Product, Search and Reviews endpoints exist. Category browsing runs through the search endpoint with a `/b/` browse URL rather than a dedicated path, and there is no equivalent of the old `type=questions` parameter.

### Redfin — rate not published

Sale pages, search pages, rental properties and agent details. Zillow, Realtor.com, Rightmove, Zoopla, Homes.com, Idealista and LoopNet scrapers sit in the same real-estate group.

### Everything else

eBay product and search, Etsy, YouTube, Reddit, G2, Trustpilot, TechCrunch, Quora, GitHub, Google Play, Apple App Store, Indeed and a generic job-boards endpoint. Per-request credit costs for these aren't published individually. Use the Domain Cost Estimator in the dashboard against a sample URL before you batch anything.

👉 [Browse the full structured data catalogue](https://www.scraperapi.com/solutions/structured-data/?fp_ref=coupons)

## What 1,000 requests cost on each tier

Divide the credit allowance by the domain's per-request cost, then divide the price by that number of thousands. These figures assume standard requests with no rendering or premium proxy flags, which add credits on top.

| Plan | 1,000 Amazon requests | 1,000 Google requests | 1,000 Walmart requests |
| --- | --- | --- | --- |
| Hobby ($49) | $2.45 | $12.25 | $0.49 |
| Startup ($149) | $0.75 | $3.73 | $0.15 |
| Business ($299) | $0.50 | $2.49 | $0.10 |
| Scaling ($475) | $0.48 | $2.38 | $0.10 |

The Google column is the one that changes purchase decisions. At 25 credits a request, a plan buys one-fifth the volume it would on Amazon. A team tracking 200 keywords daily needs 6,000 SERP requests a month, or 150,000 credits, which already exceeds Hobby. Add shopping or Maps results to the same project and Startup becomes the floor.

Scaling looks barely cheaper than Business per request and it is. The reason to move up is volume headroom and pay-as-you-go, not the unit price.

## Billing rules worth reading before you batch

**Only HTTP 200 and 404 responses are charged.** Timeouts and server errors are free. The 404 part matters more than it sounds: a 10,000-ASIN refresh where 400 products have been delisted still bills 400 × 5 = 2,000 credits for pages that returned nothing useful. Budget for that if you're monitoring long-tail catalogues.

**Check the cost of any request two ways.** The `sa-credit-cost` response header reports what a request actually cost, and `GET /account` returns your remaining credits as `creditsLeft`. The dashboard's Domain Cost Estimator gives you the number before you send anything, and a `max_cost` parameter caps how many credits a single scrape can consume.

**Requests are capped at 50MB.** Relevant if you're pulling large documents or media rather than product pages.

**Sync versus async.** Synchronous requests are one GET each. Asynchronous requests accept arrays of item IDs or search terms in a single POST and deliver results by webhook, which is how you should be doing high-volume Home Depot or Amazon work. Async results stay retrievable for 72 hours, with 24 hours guaranteed.

**Refunds and trials.** The pricing FAQ states a seven-day, no-questions-asked refund policy alongside the free trial. If you're testing whether a 5-credit Amazon request returns the fields you need, that's the window to find out.

👉 [Open an account with the 5,000-credit trial](https://www.scraperapi.com/)

## Home Depot after BigBox

BigBox API shut down on November 2, 2026. From that date, Home Depot data comes through ScraperAPI's `/structured/homedepot/` endpoints only: product, search and reviews. If you're migrating, most of the work is field renaming rather than rearchitecting.

- Data moves from the response root to `payload.*`, sitting beside a `success` flag. `payload.product.title` instead of `title`.
- `request_info.success` becomes `success`.
- `request_info.credits_used` becomes the `sa-credit-cost` response header.
- `request_info.credits_remaining` becomes `creditsLeft` from `GET /account`.
- The `output` parameter is now `output_format`, and `customer_zipcode`, `max_page`, `csv_fields` and `include_html` are dropped.
- Category requests go to the search endpoint with a `/b/` URL. Sort, price and page parameters are ignored when a URL is set, so encode them in the URL or follow `pagination.next.link`.
- `request_metadata.*` and `request_parameters.*` are no longer returned.

ScraperAPI's version returns more per search result than BigBox did: store, price, start and end date, and delivery labels for ship-to-home and scheduled delivery, where BigBox only surfaced those on the product page. Accounts that relied on BigBox Collections and Destinations need to rebuild them as async batches with a scheduler and webhook, or as a DataPipeline project.

## Who these endpoints aren't for

A handful of one-off page fetches doesn't justify a paid plan. The free tier covers it.

Custom URL structures are the other common mismatch. These endpoints expect the URL shapes they were built for, so an Amazon product URL with your own affiliate tag or filters should go through Auto Parsing instead. Same for anything behind a login or on a domain that isn't in the catalogue; the general scraping API or the AI Parser is the right tool there, not a structured endpoint.

Finally, if your budget is built around request counts rather than credits, convert before you sign up. The credit multipliers are the most consistent complaint in third-party reviews, and they only bite when the plan was sized on the wrong unit.

## FAQ

**Do structured data endpoints cost extra on top of my plan?**
No. They're available on every plan including Hobby and Startup, and a request costs the same as calling the core API against that domain.

**How many credits is one request?**
Amazon 5, Google 25, Walmart 1, LinkedIn 30. The domain sets the price, so all three Amazon endpoints cost 5 and all five Google endpoints cost 25.

**Are failed requests billed?**
Only HTTP 200 and 404 responses consume credits. Timeouts and server-side failures don't.

**How long can I retrieve async results?**
Up to 72 hours, with 24 hours guaranteed.

**Can I scrape a custom Amazon URL with these endpoints?**
Not reliably. Use Auto Parsing for custom URL structures.

## Picking a tier

Amazon product tracking under 20,000 requests a month: Hobby at $49. Google SERP at any serious volume: work out your monthly search count, multiply by 25, and you'll usually land on Startup or Business. Country targeting outside the US and EU is reported to start at Business, so confirm that with support before ordering if your data has to be localised. High-volume work that can't afford to stop mid-cycle needs Scaling or above for pay-as-you-go; on lower tiers, enable auto-upgrade so the account moves up a plan instead of pausing.

Two things to check before you pay: whether the target domain appears in the structured data catalogue, and whether your monthly credit figure survives the per-domain multiplier. Both are answerable in ten minutes with the trial credits.

👉 [Compare the plans and pick your credit tier](https://www.scraperapi.com/pricing/?fp_ref=coupons)
