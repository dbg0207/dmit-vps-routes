# DMIT Linux VPS: How the CN2 GIA, Eyeball and Tier 1 Lines Differ, What Each Plan Costs, and Which One You Actually Need

Search "DMIT Linux VPS" and you'll land in a strange place. The provider has a clean website, real hardware, and some of the best China-facing routes you can rent — and then it names its plans after California towns, splits everything into three network profiles across three CPU generations, and floods half the catalog with "Out of Stock" badges.

So most people who search this term aren't asking "what is DMIT." They're asking three things: does it actually run Linux only, what does a plan really cost, and which of these route labels do I need to pay for. That's what this covers.

## What DMIT actually sells

DMIT is a US-registered host that has been selling KVM virtual servers since the late 2010s — published profiles put the start of the VPS business at 2017 in some places and 2018 in others, so treat the exact founding date as fuzzy. What isn't fuzzy: the catalog is small, deliberate, and expensive relative to the $2-a-month crowd.

Three things define the product:

- **KVM virtualization, AMD EPYC silicon.** Hardware generations are labelled AS3 (EPYC 7003), AN4 (EPYC 9004) and AN5 (EPYC 9005). The AN4 line has largely disappeared from the store, and AN5 is now the generation DMIT pushes.
- **Route quality is the product.** For the China-optimized lines you're buying CN2 GIA, CMIN2 and AS9929 transit, not raw CPU. The hardware is good; the routes are why the prices look the way they do.
- **Three data centers.** Los Angeles, Hong Kong and Tokyo. Nothing else.

A note on how DMIT names things, because it trips people up constantly:

| Label in the store | Short name | What it means |
| --- | --- | --- |
| Premium Network | Pro | Tier 1 transit plus DMIT's own backbone plus China Telecom CN2 GIA. The best route to mainland China, the highest price. |
| Eyeball Network | EB | China-carrier "eyeball" optimization — CMIN2 and AS9929 return paths rather than full CN2 GIA. Most traffic allowance per dollar of the China-optimized options. |
| Tier 1 Network | T1 | International transit, no mainland China optimization. Cheap, big ports, big traffic allowances. |

The plan names — TINY, Pocket, STARTER, MINI, MICRO, MEDIUM, LARGE, GIANT, plus WEE at the bottom and oddities like CORONA and MALIBU — are size tiers, not products. LAX.AN5.Pro.MINI is "Los Angeles, AN5 hardware, Premium network, fourth size up." Once that clicks, the store stops being a maze.

## Yes, it's Linux only — here's what that means in practice

This is the single most-searched angle on the keyword, so let's be blunt. DMIT's OS template list is Linux. Debian and Ubuntu are the usual picks; RHEL-family images (CentOS and friends) have historically been available too. Windows is not offered, and it's not an oversight — it's a positioning choice by a host whose customers are running web stacks, proxies, bots and relay nodes, not Remote Desktop.

What you get instead:

- Root access and a full KVM virtual machine, so any Linux you can boot, you can run.
- One IPv4 and one IPv6 /64 block on essentially every plan.
- A self-managed box. No control panel, no managed support tier, no hand-holding. If you want cPanel and someone else to fix your PHP config, this isn't the shop.

Roughly: if you're comfortable with `apt` or `dnf`, you're the target customer. If the phrase "SSH key" makes you nervous, you can still learn here, but you're paying premium-route prices while you do it.

## The route question, because it's the only decision that really matters

Every DMIT purchase is really two purchases: capacity (CPU, RAM, disk, traffic) and route. Capacity is easy to compare. Route is where people get burned.

**Premium (Pro)** — CN2 GIA in both directions for China Telecom, with Unicom and Mobile steered through premium transit as well. This is the line for a site or service whose users are in mainland China and who notice when latency doubles at 9pm. Third-party test write-ups on the Los Angeles Pro line land around 155–162 ms from Chinese networks with negligible packet loss — normal for the US west coast, and notably stable across the evening peak.

**Eyeball (EB)** — China Mobile's CMIN2 for the return path, AS9929 for Unicom, and a load-balanced mix for Telecom. The pitch is that you give up a slice of Telecom-optimal routing and get noticeably more traffic for the same money. If your audience is mobile-heavy, this is usually the better buy, and reviewers consistently call the LAX EB ladder the best value-per-gigabyte of the China-optimized options.

**Tier 1 (T1)** — international BGP with no mainland optimization at all. Cheapest, and the traffic allowances are enormous: 10 Gbps ports and figures like 80 TB a month at the top end. Point it at overseas users, or use it as a landing node behind a separate relay.

One trap worth stating plainly: T1 in Hong Kong is not a China-optimized plan just because it's in Hong Kong. On a Telecom connection you can end up with worse latency than a Los Angeles Pro box. Buy Hong Kong T1 only if your traffic is genuinely international.

## Los Angeles plans and prices

This is the busiest part of the catalog and the one most "DMIT Linux VPS" searches are really about. Current AN5 Premium lineup:

| Plan | vCPU | RAM | SSD | Traffic / port | After overage | Monthly |
| --- | --- | --- | --- | --- | --- | --- |
| LAX.AN5.Pro.TINY | 1 | 2 GB | 20 GB | 1 TB @ 1 Gbps | 4 Mbps unmetered | $12.98 |
| LAX.AN5.Pro.Pocket | 1 | 2 GB | 40 GB | 1.5 TB @ 4 Gbps | 4 Mbps unmetered | $18.90 |
| LAX.AN5.Pro.STARTER | 2 | 2 GB | 80 GB | 3 TB @ 10 Gbps | 4 Mbps unmetered | $38.90 |
| LAX.AN5.Pro.MINI | 4 | 4 GB | 80 GB | 5 TB @ 10 Gbps | 8 Mbps unmetered | $79.90 |
| LAX.AN5.Pro.MICRO | 4 | 4 GB | 160 GB | 7 TB @ 10 Gbps | 8 Mbps unmetered | $110.90 |
| LAX.AN5.Pro.MEDIUM | 6 | 8 GB | 160 GB | 14 TB @ 10 Gbps | 10 Mbps unmetered | $289.90 |
| LAX.AN5.Pro.LARGE | 8 | 16 GB | 320 GB | 25 TB @ 10 Gbps | 10 Mbps unmetered | $499.90 |
| LAX.AN5.Pro.GIANT | 12 | 24 GB | 640 GB | 50 TB @ 10 Gbps | 10 Mbps unmetered | $1,009.90 |

👉 [Check current LAX Premium availability and pricing](https://www.dmit.io/cart.php?region=los-angeles&generation=an5&network=premium&aff=12046)

Note the "after overage" column. DMIT throttles rather than disconnects: blow through your monthly traffic and the box keeps running at the listed floor speed, unmetered. On TINY that's 4 Mbps, which is fine for SSH, DNS and light web serving, and useless for pushing large files. Higher tiers get 8 or 10 Mbps. Anyone who has been suspended by a cheaper host at 1am over a traffic spike will recognize why this matters.

The Eyeball line reuses the same size names with different capacity and slightly lower prices at the top of the ladder:

| Plan | vCPU | RAM | SSD | Traffic / port | After overage | Monthly |
| --- | --- | --- | --- | --- | --- | --- |
| LAX.AN5.EB.TINY | 1 | 2 GB | 20 GB | 1.5 TB @ 2 Gbps | 4 Mbps unmetered | $12.98 |
| LAX.AN5.EB.Pocket | 2 | 2 GB | 40 GB | 3 TB @ 4 Gbps | 4 Mbps unmetered | $18.90 |
| LAX.AN5.EB.STARTER | 2 | 2 GB | 80 GB | 5 TB @ 10 Gbps | 4 Mbps unmetered | $38.90 |
| LAX.AN5.EB.MINI | 4 | 4 GB | 80 GB | 10 TB @ 10 Gbps | 8 Mbps unmetered | $76.90 |

👉 [See the live Eyeball plan list](https://www.dmit.io/aff.php?aff=12046)

Same money, more traffic, different return path. That's the whole EB pitch, and the two lines sit side by side in the cart so you can compare them directly.

Tier 1 in LAX splits into two sub-series on identical hardware — VOLUME for traffic-hungry workloads, GENERAL for a more balanced CPU-to-RAM ratio:

| Plan | vCPU | RAM | SSD | Traffic @ 10 Gbps | Monthly |
| --- | --- | --- | --- | --- | --- |
| LAX.AN5.T1.V2C2G | 2 | 2 GB | 40 GB | 5 TB | $14.90 |
| LAX.AN5.T1.V2C4G | 2 | 4 GB | 80 GB | 10 TB | $23.90 |
| LAX.AN5.T1.V4C4G | 4 | 4 GB | 120 GB | 20 TB | $36.90 |
| LAX.AN5.T1.V4C8G | 4 | 8 GB | 160 GB | 40 TB | $52.90 |
| LAX.AN5.T1.V8C16G | 8 | 16 GB | 240 GB | 80 TB | $119.90 |
| LAX.AN5.T1.G2C4G | 2 | 4 GB | 80 GB | 4 TB | $16.90 |
| LAX.AN5.T1.G4C8G | 4 | 8 GB | 160 GB | 8 TB | $36.90 |
| LAX.AN5.T1.G8C16G | 8 | 16 GB | 320 GB | 12 TB | $79.90 |

👉 [View Tier 1 plans and stock](https://www.dmit.io/aff.php?aff=12046)

Tier 1 overage throttling runs 100–500 Mbps depending on tier — an order of magnitude gentler than the optimized lines, which tells you where the cost pressure actually sits.

If you're hunting for the cheapest possible entry, the WEE annual plan is the one to watch: 1 core, 1 GB RAM, 20 GB SSD, 1 TB at 4 Gbps, unmetered at 100 Mbps afterwards, at **$36.90 per year**. It appears and vanishes across Los Angeles, Hong Kong and Tokyo, and it's usually gone within hours of a restock.

## Hong Kong: the low-latency option, priced accordingly

Hong Kong is where the physics works in your favor. A published test of an HKG.AS3.Pro.TINY box recorded roughly 11 ms to Guangdong telecom, under 10 ms to Guangdong Unicom, and 27–31 ms to Shanghai. That's a different universe from a US west coast box, and it's why people pay triple for the same specs.

| Plan | vCPU | RAM | SSD | Bandwidth / traffic | Monthly |
| --- | --- | --- | --- | --- | --- |
| Premium AS3 TINY | 1 | 1 GB | 20 GB | 1 Gbps / 500 GB | $39.90 |
| Premium AS3 STARTER | 1 | 2 GB | 40 GB | 1 Gbps / 1 TB | $79.90 |
| Premium AS3 MINI | 2 | 2 GB | 60 GB | 1 Gbps / 1.5 TB | $126.90 |
| Premium AS3 MICRO | 2 | 4 GB | 80 GB | 1 Gbps / 2 TB | $179.90 |
| Premium AS3 MEDIUM | 4 | 4 GB | 160 GB | 1 Gbps / 2.5 TB | $239.90 |
| Premium AN5 MINI | 2 | 2 GB | 80 GB | 1 GB / 1.5 TB | $149.90 |
| Premium AN5 MEDIUM | 4 | 4 GB | 160 GB | 1 Gbps / 2.5 TB | $180.00 |
| Premium AN5 LARGE | 4 | 8 GB | 320 GB | 1 Gbps / 3 TB | $240.00 |
| Premium AN5 GIANT | 8 | 16 GB | 640 GB | 1 Gbps / 6 TB | $500.00 |

👉 [Compare Hong Kong Premium plans](https://www.dmit.io/aff.php?aff=12046)

Two things to flag. First, in the listings reviewed for this piece the AS3 platform covered TINY through MEDIUM while the newer AN5 platform started at MINI — so if you want the cheapest Hong Kong entry point, you're looking at older hardware. Second, the Hong Kong Eyeball line has been showing as unavailable for extended stretches and reports suggest it may be on its way out, so don't plan a purchase around it.

Hong Kong Tier 1 is the budget play: WEE at $36.90/year, then TINY at $6.90, STARTER at $12.90, MINI at $21.90, MICRO at $32.90, MEDIUM at $49.90, LARGE at $99.90 and GIANT at $199.90 monthly, with overage throttles from 50 Mbps up to 1 Gbps. Remember the caveat from earlier — no mainland optimization on this line, regardless of the flag on the building.

## Tokyo: the middle ground, and the Eyeball question

Tokyo sits between the two on both latency and price, and it's the only region where Eyeball plans were historically cheaper than Premium at several tiers.

| Size | Premium (CN2 GIA) | Tier 1 | Eyeball (CMI) |
| --- | --- | --- | --- |
| WEE | — | $36.90/year | — |
| TINY | $21.90 | $6.90 | $25.90 |
| STARTER | $39.90 | $12.90 | $55.90 |
| MINI | $79.90 | $21.90 | $85.90 |
| MICRO | $159.90 | $32.90 | $119.90 |
| MEDIUM | $259.90 | $49.90 | $179.90 |
| LARGE | $429.90 | $99.90 | $369.90 |
| GIANT | $799.90 | $199.90 | $749.90 |

👉 [Browse Tokyo plans and live stock](https://www.dmit.io/aff.php?aff=12046)

All prices in monthly terms. Tokyo Premium is meaningfully cheaper than Hong Kong Premium for comparable specs — $21.90 versus $39.90 at the entry tier — and a hop to Shanghai runs around 75 ms in published tests. If your users are in eastern China or Japan, that trade is usually worth taking.

Tokyo Eyeball is the caveat: multiple write-ups note that the TYO Eyeball plans stopped appearing in the store with no official announcement. If they show up again, the pricing above is the reference point.

## Billing cycles, payment, and the bits people forget

DMIT bills in US dollars through a standard WHMCS cart, with monthly, quarterly, semi-annual and annual cycles — though some entry-level plans have dropped monthly billing entirely and start at quarterly, so check what the cart actually offers for the tile you want.

Payment options are the reason this host has a large Chinese-speaking user base: **Alipay, WeChat Pay, PayPal and cards** are all accepted, and support operates in Chinese as well as English. That combination is rarer than it should be.

Three practical things:

- **Traffic is counted in both directions.** Plan accordingly if you're proxying or relaying, because a 1 TB allowance can evaporate faster than the number suggests.
- **Upgrades are billed, not free.** Extra IPs, extra RAM and extra bandwidth are priced add-ons in the cart.
- **Inventory is the real constraint.** Plans go out of stock for weeks. If a plan you want is showing available, the honest advice is not to sleep on it.

On IP reputation: DMIT IPs are datacenter IPs, and quality varies by subnet. One published test of a Hong Kong Premium box found it unlocking TikTok, Netflix, YouTube and Prime Video natively with a clean risk profile, while other users report higher fraud scores on busier ranges. Treat streaming unlock as a bonus, not a contract — it isn't guaranteed.

If an IP gets blocked, DMIT has historically allowed a free replacement roughly every 15 days, with later swaps billed at a few dollars. Confirm that in a ticket before you build it into your plans.

## Discount codes: what's real, what's expired

DMIT runs recurring discounts rather than one-time coupons, which means the code keeps applying on renewal. The ones that have circulated most widely and consistently:

| Code | Applies to | Discount |
| --- | --- | --- |
| `Lite-Annually-Recur-30OFF` | Standard/Lite network, annual billing | 30% recurring |
| `Lite-Semi-Annually-Recur-20OFF` | Standard/Lite network, semi-annual | 20% recurring |
| `LAX-EB-LAUNCH-NON-MONTHLY-RECURRING-20OFF` | LAX Eyeball, TINY and above, non-monthly | 20% recurring |
| `2025-TYO-T1-HI-GSL-NON-MONTHLY-30OFF` | Tokyo Tier 1, TINY and above, quarterly or longer | 30% |
| `202510_HKG_TYO_PRO_20OFF_RECURRING` | Hong Kong and Tokyo Premium, quarterly or longer | 20% recurring |

Two caveats that save you a wasted checkout. First, codes are scoped tightly — a Tokyo code will not touch a Los Angeles cart, and "non-monthly" means exactly that. Second, the cheapest tiers are frequently excluded; entry plans like TINY and Tier 1 WEE often have no applicable coupon at all, by design. The last row above carries an October 2025 stamp in its name, so validate it rather than assuming.

Enter the code in the cart, hit **Validate Code**, and read the new total before paying. If nothing changes, the code doesn't apply to what's in your basket.

## Buying: the actual sequence

1. Open the affiliate link below and pick your region on the cart page.
2. Choose the network profile — Premium, Eyeball or Tier 1.
3. Choose the hardware generation if more than one is offered.
4. Select a size, then a billing cycle.
5. Choose a Linux distribution and set your root password or SSH key.
6. Validate any discount code, then pay via Alipay, WeChat Pay, PayPal or card.
7. The server provisions after payment; you'll get IP details in the client area.

👉 [Open the DMIT cart and pick a plan](https://www.dmit.io/aff.php?aff=12046)

## Which one to buy, by situation

**A blog or small service with mainland China readers:** LAX.AN5.Pro.TINY or Pocket. $12.98–$18.90 a month for CN2 GIA both ways is the cheapest credible way into that route class, and the throttled overage means a traffic spike won't take you offline.

**A China-facing service with heavy traffic:** LAX Eyeball. More gigabytes per dollar, and CMIN2 is genuinely good outside pure Telecom scenarios.

**Latency-sensitive users in southern China, budget not the issue:** Hong Kong Premium. Nothing in Los Angeles comes close to sub-15 ms.

**Users in Japan or eastern China:** Tokyo Premium. Noticeably cheaper than Hong Kong for similar routing quality.

**Overseas audience, or you already have a relay:** Tier 1. Don't pay for China optimization you won't use, and enjoy 10 TB for twenty-something dollars.

**Absolute lowest cost that still works:** the $36.90/year WEE plan, if you can catch it in stock.

## Questions that come up before ordering

**Can I install Windows?** No supported Windows image exists. It's a Linux product.

**What happens when I exceed my traffic?** The server keeps running, throttled to the speed listed on the plan. No suspension, no surprise invoice.

**Is it managed hosting?** No. You get root and an OS template. Everything above that is yours.

**Do the discount codes stack?** No — one code per order, scoped to specific product lines and billing cycles.

**Why do plans disappear?** DMIT sells a fixed amount of capacity per platform rather than oversubscribing, so stock empties and refills in waves. Out-of-stock is normal, not a sign of trouble.

**Will my IP unlock Netflix?** Sometimes, on some subnets, with no guarantee. Test it, don't budget around it.

The short version: DMIT sells routes and honest capacity at a price that only makes sense if you need those routes. If you do, the pricing above is the entry ticket — and the cart is the only place where stock and final numbers are real.
