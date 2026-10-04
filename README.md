# backconnect rotating proxies: how per-request rotation and sticky sessions work, and what they cost per IP and per GB

Most people searching this term want one of two things. Either they've been handed a proxy list and are tired of it, or they've heard that a single endpoint with a rotating pool behind it solves rate limits and keep running into pricing pages that never quite say what rotation actually costs.

Both questions have concrete answers, and 9Proxy happens to be a usable case study because it sells the same residential pool through two completely different billing models: per IP and per GB. The rotation behavior is not identical between them, and that difference matters more than most "best proxy" listicles admit.

## What a backconnect proxy is, minus the vendor gloss

A backconnect proxy is a gateway. You connect to one host and port, and the provider's pool management layer picks an IP from the rotation queue for each request, forwarding it to your target. The target site sees a residential IP, not your real address and not the gateway. When the response comes back, the system assigns a new IP according to your rotation policy.

That's the whole trick. Backconnect and rotating proxies describe the same arrangement from two angles: "backconnect" is about the connection topology, "rotating" is about what the IP does over time. Providers that sell both terms separately are usually selling the same thing with different labels.

The reason it exists comes down to how sites defend themselves. Two block types cause most failures: IP blocking, where datacenter ranges or unwanted geolocations get rejected outright, and rate blocking, where a single IP is allowed a fixed number of requests before it gets throttled or silently fed inaccurate data. Rotating through a residential pool attacks both, because no single IP accumulates enough requests to trip a threshold, and the IPs look like ordinary home connections.

The tradeoffs are just as concrete. Every rotation adds latency, generally in the 200–500 ms range compared with datacenter proxies, because traffic is routed through real devices rather than co-located servers. Residential bandwidth also costs more per gigabyte. And rotation breaks state: if your workflow logs in, browses, then checks out, a fresh IP mid-flow will look like a stolen session.

## Per-request, session-based, and sticky: pick one before you buy

Three rotation modes show up across providers, and they map to different jobs.

Per-request rotation issues a new IP for every call. It's the default for scraping and monitoring, where each request is independent and low-bandwidth. It's also the wrong choice for anything with a login.

Session-based rotation holds one IP for a set window, then swaps. This is what you want for multi-step flows, cart checks, or anything where a captcha should appear once rather than on every step.

Sticky sessions keep one IP until the session ends or you cut it. Useful for account work and long browser profiles, less useful for volume.

A lot of frustration with backconnect proxies comes from skipping this decision. If your script does a per-request rotation against a site that tracks sessions, you'll see failures that look like bad IP quality but are really a configuration problem.

## How 9Proxy wires it up

9Proxy runs a residential network it advertises as 20M+ IPs across 90+ countries. It sells access through two products, and the rotation mechanics differ enough that they should be treated as separate tools.

|  | Residential by IP | Residential by GB |
| --- | --- | --- |
| Billing | Per IP, unlimited bandwidth | Per GB of traffic |
| IP balance | Never expires until activated | 180-day validity (unlimited on Enterprise) |
| IP lifetime | A few hours up to ~24h | No fixed lifetime on rotating sessions |
| Rotation | Not native; auto-rotation proxy at custom intervals on selected ports | Rotating per request/session, or sticky |
| Endpoints | 1 IP = 1 usage when forwarded | Unlimited endpoints generated from purchased traffic |
| Targeting | Location-based IP selection | Country, state, city, ZIP, ISP |
| Authentication | 9Proxy desktop app, optional proxy auth | Username/password or IP whitelist |
| Setup | Desktop app required | Runs from the dashboard |

The per-IP model is a fixed-asset model: you buy IPs, each one gives you unlimited traffic while it's live, and unused balance doesn't expire. You are effectively paying for identity slots, not data. Its rotation option exists, but it's a port-level feature rather than the product's core behavior.

The GB model is the one that behaves like a classic backconnect setup. Traffic is metered, endpoints are free to generate in any quantity, and the rotation policy is a parameter you set rather than a property of the IP itself. If you're searching for a backconnect rotating proxy in the traditional sense, this is the product that answers the description.

👉 [Start a 9Proxy account and generate your first rotating endpoint](https://bit.ly/9-Proxy)

## What you can actually configure

The dashboard-side Proxy Generator is where the rotation settings live, and it covers the parameters that matter for a real pipeline:

- Session mode: sticky or rotating, chosen before you extract endpoints.
- Targeting: country, state, city, ZIP, or ISP level.
- Authentication: sub-users with assigned traffic, or IP whitelisting with no password at all.
- Extraction: endpoints exported as .txt or .csv, plus ready-made code samples in several languages.
- Sub-user logins: sub-accounts can sign into the dashboard directly, with permission limits, which is useful when several people or clients share one traffic pool.

On the IP-based side, the configurable set is different: auto-refresh to replace an active IP, auto-rotation at custom intervals on selected ports, port configuration, and full HTTP and SOCKS5 support, which is what makes it work in AdsPower, Dolphin Anty, BitBrowser and anything else that accepts host:port:user:pass. There's also an API for provisioning, and a "Today List" that lets you reuse an IP forwarded within the last 24 hours without spending a new one from your balance while that IP is still online.

## Setting up a rotating backconnect session

For the GB model, the sequence looks like this:

1. Buy a GB package, or start with a small one if you're testing target compatibility.
2. Create a sub-user in the dashboard and assign traffic to it.
3. Open Residential Proxies → GB → Proxy Generator.
4. Choose the session mode. Rotating for independent requests, sticky with a duration for logged-in flows.
5. Set the location filter if the target is region-specific.
6. Extract your endpoint and export the format your tool wants.
7. Paste credentials into your scraper or antidetect browser and run a small batch first.

Step 7 is the one people skip. Running a few dozen requests before committing to a large scraping job tells you more about success rates on your specific targets than any review, including this one.

For the IP model, the flow is different: you install the desktop app, forward ports, then manage IPs, auto-refresh, and rotation intervals from inside the app. It's more moving parts, but you get unlimited bandwidth per live IP, which changes the cost math completely for data-heavy work.

## Pricing: every package 9Proxy currently lists

One thing to know before reading the numbers. On June 1, 2026, 9Proxy raised prices on IP-based and bundle packages for the first time since launch, while leaving GB-based pricing untouched. Published figures show roughly a 20% increase: the 100-IP package went from $20 to $24, the 500-IP package from $60 to $72, and the three bundles moved from $25/$150/$600 to $30/$180/$720. If you're comparing 9Proxy against a review written before June 2026, the IP and bundle columns are stale.

**IP-based packages** (unlimited bandwidth per IP, IPs don't expire until activated)

| Package | Effective rate | Price | Purchase |
| --- | --- | --- | --- |
| 100 IPs | $0.24/IP | $24 | [Grab the 100-IP starter package](https://bit.ly/9-Proxy) |
| 500 IPs | $0.144/IP | $72 | [Order 500 IPs](https://bit.ly/9-Proxy) |
| 1,000 IPs + 500 bonus | ~$0.084/IP | $126 | [Take the 1,500-IP package](https://bit.ly/9-Proxy) |
| 100,000 IPs | $0.023/IP | $2,300 | [Check large-volume IP pricing](https://bit.ly/9-Proxy) |
| 500,000 IPs | $0.017/IP | $8,625 | [Request reseller-scale IP inventory](https://bit.ly/9-Proxy) |

Intermediate tiers exist between 1,500 and 100,000 IPs, and they scale down the per-IP rate as volume increases; the exact current numbers belong on the pricing page at checkout, since the whole ladder moved with the June adjustment.

**GB-based packages** (unchanged pricing, 180-day validity)

| Package | Rate | Total | Purchase |
| --- | --- | --- | --- |
| 5 GB | $3.00/GB | $15 | [Buy 5 GB to test rotation](https://bit.ly/9-Proxy) |
| 50 GB + 5 GB bonus | $2.10/GB | $105 | [Get the 55 GB pack](https://bit.ly/9-Proxy) |
| 100 GB | $1.50/GB | $150 | [Order 100 GB](https://bit.ly/9-Proxy) |
| 200 GB | $1.00/GB | $200 | [Pick up 200 GB](https://bit.ly/9-Proxy) |
| 1,000 GB | $0.80/GB | $800 | [Scale to 1,000 GB](https://bit.ly/9-Proxy) |
| 2,000 GB | $0.75/GB | $1,500 | [Get 2,000 GB](https://bit.ly/9-Proxy) |
| 10,000 GB | $0.68/GB | Scales with volume | [Ask about the $0.68/GB tier](https://bit.ly/9-Proxy) |

**Bundle packages** (IPs plus GB traffic, traffic valid 180 days)

| Bundle | Contents | Price | Purchase |
| --- | --- | --- | --- |
| Starter | 100 IPs + 5 GB | $30 | [Start with the Starter bundle](https://bit.ly/9-Proxy) |
| Growth | 1,500 IPs + 50 GB | $180 | [Get the Growth bundle](https://bit.ly/9-Proxy) |
| Pro | 5,000 IPs + 500 GB | $720 | [Take the Pro bundle](https://bit.ly/9-Proxy) |

**Enterprise** isn't a fixed package; it's a plan level with unlimited data validity, team mode for one owner plus up to five members, no-expiration bandwidth sharing inside the team, per-member traffic controls, full activity logs, unlimited share codes, VIP pricing and dedicated support.

## Which model fits which workload

The clean way to decide is to look at how your traffic behaves, not at how many requests you send.

If each request is small and each one should come from a different IP, per-GB is cheaper and simpler. Data metering is the only cost, endpoints are unlimited, and rotation is a checkbox. Ad verification, SERP checks, price monitoring, and lightweight scraping all land here.

If a session is long, bandwidth-heavy, and needs a stable identity, the per-IP model wins, because bandwidth stops being a line item. Video-adjacent work, large file transfers, or long unattended jobs with unpredictable data volume are the natural fits. The catch is that each IP lives somewhere between a few hours and roughly 24 hours, so you're managing a rotating stock of identities rather than a permanent one.

Mixed workflows are what bundles are for, and the 180-day traffic validity matters more than the discount. Project-based usage tends to arrive in bursts, and a plan whose unused balance effectively disappears at month-end is a bad match for that pattern.

A practical note on the numbers: independent comparisons place 9Proxy in the budget tier, with success rates in the 90–95% range on moderately protected targets and lower on hard ones. One third-party review of the platform reported around a 99.5% success rate and roughly 0.6 seconds average response time on its test set, while a cost comparison put pay-as-you-go rates nearer $1.30/GB. Treat both as directional. Your target mix decides your real numbers.

## Before you top up: five things worth checking

**Residential only.** 9Proxy's catalog is residential proxies. If your job needs datacenter IPs for speed at low cost, this isn't the provider for it.

**Two different authentication philosophies.** The GB side works with username/password or IP whitelisting from the dashboard. The IP side depends on a desktop app for local port forwarding. Teams running everything in the cloud should default to GB.

**Prepaid balances live with the provider.** This is true of every balance-based proxy vendor, not just this one. A reseller-catalog review published in August 2026 claimed 9Proxy went dark twice during mid-2026 and advised against parking an entire budget in its wallet. That claim comes from a site selling competing products, so weigh it accordingly, but the underlying advice is sound regardless of vendor: buy in smaller increments while you're still measuring reliability on your own targets.

**Trials are limited.** 9Proxy offers trial access for new accounts subject to availability, and the company has said it depends on stock at the time you ask. Don't build a plan around a guaranteed free trial.

**Payment options are broad.** Credit cards, bank cards, cryptocurrency (USDT, BTC, ETH, LTC, DOGE and others), Alipay, Apple Pay and Google Pay are all accepted, which removes most of the friction for users outside card-friendly regions.

Also worth knowing if you're signing up through a referral link: 9Proxy runs a lifetime affiliate program with commissions up to 15%, and referred users get a 5% discount on their purchase.

## FAQ

**Is a backconnect proxy the same as a rotating proxy?**
Practically, yes. Backconnect describes connecting through one gateway that fronts a pool; rotating describes the IP changing per request or per interval. Some providers use the terms interchangeably, and 9Proxy's GB product is the rotating implementation of a single entry point.

**Can I keep one IP across a multi-step flow?**
Yes, on the GB model, by choosing a sticky session with a defined duration instead of per-request rotation. On the IP model, an individual IP stays live for hours to roughly a day, which gives you similar stability but with less control over the exact cutover.

**Do I pay per request?**
No. Requests are free; you pay for data (GB model) or for IPs (IP model). Generating endpoints costs nothing on the GB side, which is why heavy rotation at low data volume is cheap there.

**Does rotation slow things down?**
It adds latency compared with datacenter IPs, typically a few hundred milliseconds per request, because traffic traverses real residential devices. That's the price of not getting blocked.

**What's the lowest-risk way to start?**
Buy the smallest GB package, run a few hundred requests against your actual targets, and log the success rate and response times. Scale once you have your own numbers rather than someone else's.

👉 [Create your 9Proxy account and test rotating residential endpoints](https://bit.ly/9-Proxy)
