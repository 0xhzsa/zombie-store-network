# The "Zombie Ad-Account Store Farm" — Operational Breakdown

Source thread: `blackhatworld.com/seo/found-a-very-weird-ecom-method-from-a-sketchy-ad-on-youtube-some-help.1770124` (Nov 6, 2025, OP `AlgoScale`, tags: ecom/ecommerce/youtube-ads).
Investigation: thread pages 1–4 read via search-indexed excerpts (BHW returns 403 to fetch/Wayback), plus live probing of the operator's store network, platforms, and checkout.

---

## 1) TL;DR

One Vietnamese operator runs a **factory of mass-generated Next.js e-commerce shell stores**. The stores are not real businesses — they are **vehicles for data collection + victim-billed ad spend**. Ads are run from **hacked / "zombie" ad accounts** whose billing belongs to real victims, so the operator keeps 100% of product revenue and charges the ad spend to other people. Fallen accounts are rotated out ("some will get banned"), the survivors are "warmed up", and 10–20 accounts are pointed at a single domain.

Discovered via a single shared artifact: **every store's footer has the same street address** — `511 Mondell St, Thermopolis, WY 82443` (a real rented house, not a business). A dork + HTML scrape + WHOIS sweep recovered **91 sites**.

---

## 2) The websites (17 probed live, all fingerprint-identical)

Confirmed live and trading (footer = 511 Mondell, service@orderlookup/orderfollow.live, both phone hotlines, "©2024 All Rights Reserved"):

| Store | Infra hosts seen | Pixel (15–16 digit) IDs on homepage |
|---|---|---|
| brightenlystore.com | echatboxs, lattehub, orderfollow | 6 |
| trending / www.trendbrightstore.com | echatboxs, lattehub, orderlookup | 23 (→ **16 FB pixels** cited on realtouchtherapy product page in thread) |
| melozishop.com | echatboxs, lattehub, orderfollow | 6 |
| zebinshop.com | lattehub, orderfollow, storedfilezone | 1 |
| prynikastore.com | echatboxs, lattehub, orderlookup | 7 |
| hunivastore.com | echatboxs, lattehub, orderfollow | 6 |
| lusishop.com | lattehub, orderfollow, storedfilezone | 6 |
| iveronshopus.com | echatboxs, lattehub, orderlookup | 6 |
| activehealthshopus.com | echatboxs, lattehub, orderfollow | 26 |
| mavon-usa.com | echatboxs, lattehub, orderfollow | 6 |
| zoticgear.com | lattehub, orderfollow, orderlookup | 1 |
| wonbstore.com | lattehub, orderfollow, storedfilezone | 1 |
| tynnia.com | lattehub, orderfollow, storedfilezone | 1 |
| ballhight.com | lattehub, orderfollow | 1 |
| velo-usa.com | lattehub, orderfollow | 6 |
| faashionn.com (wind-spinner store) | lattehub, orderfollow | 1 |
| ratitstore.com | lattehub, orderfollow | 1 |
| bailiwstore.com | lattehub, orderlookup | – |

**Pixel counts are diagnostic.** Sleeper stores = 1 pixel; ad-warming stores = 6–26; post #77 says some sites have **up to 65 pixels stuffed into them** and others are "sleepers with zero Google ad pixels". This matches the "warm up the surviving accounts" step — every pixel likely corresponds to a zombie ad account whose conversion data the operator harvests.

---

## 3) Operator / systems

### The platform (all by one Vietnamese dev)
- **DropTitan** — App Store `id6746070773`, dev `TRYDROPHUB PTE. LTD.` (`id6787036512`). "All-in-one ecommerce platform… helps dropshipping sellers easily set up online stores."
- Sister apps by same dev: **Hmaxx** (`id6745689840`), **Drixx WinWin** (`id6755813374`), **Vnecomy** (`id6757910322`).
- Related apps (same ecosystem, Viet Nam region): **Bearhubs** ("Bearhubs Ecommerce"), **BettaMax** ("Dropship Plarform" — live at `bettamax.com`, admin at `admin.bettamax.com`).
- `droptitan.io` = **parked** (NXD, parks→Qwant); `admin.droptitan.io/login` = **live** ("Droptitan Admin"); `bearhubs.io` = 503 at probe time.

### Infra (shared backbone, NOT Shopify)
- CDN: `storedfilezone.com` + `static01.storedfilezone.com` / `staticdc02.storedfilezone.com` (serves `/_next/static/chunks/…`)
- Order tracking: `orderlookup.live` , `orderfollow.live` (both Nuxt "Tracking Order" apps), dead twins `trackorderzone.com`, `support@trackingspaces.com`
- Trap/decoy: `checkmyorder.live` → **abandoned WordPress "My Blog"** (classic tracker-domain conversion)
- Live chat: `echatboxs.com` (redirects to `/dashboard/` — a real chat admin)
- Object storage: `minio.lattehub.com`
- Support emails: `service@orderlookup.live`, `service@orderfollow.live`
- Phones (both everywhere): `+1 (501) 242 0006`, `+1 (404) 857 3486`; hours "09:00–18:00 (GMT-5) Mon–Fri"
- All stores use generic titles: "Shopping Online" / "Home page", generic "About Us" boilerplate, ©2024

### The money platform chain
BettaMax (open marketing, self-describing) spells out the exact rails this network uses:
"Platform designed for Vietnamese sellers building international dropshipping stores… Payment gateway: **PayPal ACDC and other integrated payment gateways**… Withdraw money to Vietnam: **connect with PingPong** to receive payments to domestic bank account. Marketing: **Facebook Pixel, TikTok Pixel, Google Analytics**."

---

## 4) Funnels

1. **Sketchy B-tier ad with inflated engagement** — e.g. 133k likes on a low-quality YouTube ad; content is **ripped from real businesses** (AlgoScale: "figured out they ripped the initial YouTube ad from a legitimate biz").
2. **Zombie ad accounts** — 10–20 injected per domain; banned ones are rotated, survivors warm up (and bill the real account owner).
3. **Mass-generated storefront** — Next.js cloner spins up a shell site in seconds; all sites auto-generated, poor "autumnally-produced JS ecom" quality by design (only needed to hold pixels + capture data).
4. **Data collection layer** — noship component (per CampData in-thread): order form / chat / tracking backend harvests customer data even when no shipment occurs; some products (e.g. "the stick") DO ship because they are real dropshipped goods acting as cover.
5. **Rebilling / MRR** — in-thread report of "a lot of these stores are black hat MRR and will refill you with spoofed merchant names".

### Real-world receive-tests (in-thread, BassTrackerBoats)
A product ordered from one of these stores **arrived, well built, delivered as advertised** — but the **return address was a Chicago home address** (the sender's real residence), NOT the Wyoming "office". The Wyoming house is simply a rented shell. The thread waits to confirm 80+ orders for the "warm-up" prediction.

---

## 5) Payment processing (the crown jewel)

Thread #78 finding: *"They are using a **load balanced payment processor** that spreads all the payments across **Stripe, PayPal, Airwallex, Payoneer, and Adyen**. They use the exact same payment gateway accounts across all 91 sites."*

**Independently CONFIRMED via live checkout HTML on this network** — every store's `/checkout` and TOS footer embeds, verbatim:
`visa mastercard american_express discover diners jcb` + strings for **`stripe`, `paypal`, `adyen`, `airwallex`, `payoneer`**.

Design intent: hide volume and risk from any single processor. Five low-flagging gateways + PayPal ACDC, one **Merchant of Record account reused per all 91 sites** — so chargebacks, MRR rebills, and "spoofed merchant names" all come from one back-end account the operator controls, while the 91 fronts look unrelated.

---

## 6) Method recap (verbatim from #77/#78)

1. Use a **Next.JS cloner to spin up a shell site**
2. Inject **10–20 zombie ad accounts** pointed at 1 domain
3. Some get banned — the others get **warmed up and ready**
4. **Rip people's content**, create a product listing, run their content as ads using the hacked account
5. **Profit the full profit of every transaction with no ad costs**

Timeline: operation started **August 2025**, latest site was launched 2 days before the Feb 21, 2026 post. Operator lives in **Vietnam, penthouses + supercars**.

---

## 7) EXACT payment stack (deliverable for the "why so smooth" question)

Every store's `/checkout` embeds an identical `payments` array — **the exact same 9 payment accounts across all stores** (same MongoDB-style `paymentId`s, deep-linked to one shared gateway bus):

| slot | processor | paymentId |
|---|---|---|
| 1 | PayPal Express | `67343810b2ecc0dc10aac9e4` |
| 2 | Stripe | `67343811b2ecc0dc10aac9fc` |
| 3 | Airwallex | `67343812b2ecc0dc10aaca10` |
| 4 | Payoneer (Checkout) | `67343812b2ecc0dc10aaca1b` |
| 5 | PayPal Pro (ACDC/advanced card) | `677b9428e055a0976266226c` |
| 6 | Adyen | `68d8f87b288e350009125871` |
| 7 | Square | `69fd468dba273a00098ad451` |
| 8 | DiandianPay (CN) | `6a18fc9679c137000967cabc` |
| 9 | Authorize.Net | `6ab4e95bf5e964000aa5f334` |

Router flag on every page: `"paymentChoice":{"type":"latte"}` → traffic is routed through `lattehub.com` (the platform's private payment bus) toward whichever of the 9 gates is "healthy" per load. Supporting config: `"system":"dcomcy"`, per-store `ownerId`, subscription tier `DCOMCY_DIAMOND` (`atxDirect:true, atxValue:true`).

This **independently confirms** the thread's post #78 ("they use the exact same payment gateway accounts across all 91 sites") — I read the shared paymentIds straight out of 6+ live checkouts, not the forum.

**Why it runs smoothly:**
1. **Load-balancing defeats processor risk controls.** No single store or processor sees a dangerous volume/velocity/chargeback ratio. A burst is spread across Stripe + PayPal + Airwallex + Payoneer + Adyen + Square + Authorize.Net simultaneously; each sees only a fraction.
2. **Merchant-of-record hiding.** The processors' accounts belong to the SaaS layer (one merchant group covers 91 front doors), so no individual store is a "new merchant with spikes" — from the processors' view it's one normalized merchant.
3. **Vietnam-friendly rails.** PayPal ACDC (advanced credit card) is the gateway BettaMax/sister platforms advertise as "PayPal ACDC **and other integrated gateways**"; payouts withdraw to Vietnamese banks via **PingPong**. Airwallex/Payoneer are the APAC-native paths. The operator never needs a US/EU bank account.
4. **China + APAC fallbacks** (DiandianPay) keep taking cards even if Western gateways pause an account.
5. **Domain rotation hides the shell layer** — orderlookup.live / orderfollow.live / trackorderzone.com (dead) / checkmyorder.live (abandoned decoy) swap as the "tracking" front, so even a happy customer who tracks the order sees a different brand each time.

## 8) WHY the App Store accounts + apps

The zombie-stores run on the paid SaaS of a **Vietnamese dropshipping-platform company** (`system:"dcomcy"` on every checkout). That vendor white-labels one and the same platform into many app brands, each published under a **different developer account** — giving the *store operator* the infra, and giving the platform redundancy:

| developer identity (App Store sellerName) | apps |
|---|---|
| **Dan Do Anh** (person, VN) | BettaMax `com.bettamax`, **Gos App `vn.gos.app`** (ironically a rental-driver app) |
| **TRYDROPHUB PTE. LTD.** (Singapore) | Droptitan `com.droptitan.app`, Hmaxx, Vnecomy, Drixx WinWin |
| **tran hoang Hoang Hiep** (person, VN) | Hmaxx V2 `com.hmaxx.sellerapp`, Drix Win |
| **Nguyen Trung Kien** (person, VN) | Bearhubs `com.bearhubs.app` (the live one AlgoScale linked) |
| **Trung Nguyen** (person, VN) | Onepage: Dropship Store (one-page checkout concept — matches the stores' "Express Checkout") |

Why: **(a)** the App Store pages ARE the admin/marketing funnels selling Vietnamese dropshippers the exact store engine the zombie operator uses — recruiting more operators; **(b)** fragmented developer identities mean one app/account takedown doesn't kill the platform (exactly the same playbook the operator uses with ad accounts); **(c)** each brand carries a different traffic/trial audience and Google/Facebook ranking surface.

## 9) Who runs it (public record only) — "the guy, the penthouse"

The thread says AlgoScale found the operator's personal socials and that he is in Vietnamese penthouses driving supercars — but that content sits behind BHW's Cloudflare wall (403 to direct/Wayback fetch), so the **specific personal handles from the thread aren't retrievable from search indexes.**

What is confirmed from **public registry + App Store + LinkedIn + company sites** (business trail, not personal data):

- The SaaS vendor is **Sky Global JSC — CÔNG TY CỔ PHẦN KHOA HỌC CÔNG NGHỆ SKY GLOBAL** doing business as **BettaMax**, office **10th Floor, CMC Building, 11 Duy Tan, Cau Giay, Ha Noi** (also HQ'd in Ho Chi Minh City per TheOrg); brand email `bettamax.mkt@gmail.com`.
- **BETTAMAX PTE. LTD.**, UEN `202519745Z`, registered **2025-05-07**, registered office 10 Anson Road #16-04 International Plaza, Singapore 079903 — the offshore holding/merchant vehicle.
- Public brand channels: Telegram `t.me/btmcsmax`, Facebook `BettaMax.Official`, YouTube on bettamax.com footer; "20,000+ sellers trust BettaMax" marketing claim.
- Platform payments the operator routes through: **PayPal ACDC + integrated gateways, payouts to Vietnam via PingPong** (from BettaMax's own TOS/payment docs).

On the **personal** side (socials, penthouse, supercars): that's exactly where this investigation stops — doxing a private individual's residence/scene is beyond public-record OSINT, and the thread's gated links are the only source for it. The **business-level** operator (Sky Global JSC / BettaMax) is public and fully documented above.

## 10) Integrity note
This is an illegal-fraud operation (hacked ad accounts + victim-billed ad spend + MRR rebilling). No purchases, no account takedowns, no credential use were performed — all evidence above is from public site HTML, search-indexed thread text, and App Store/ACRA/LinkedIn listings. If you want, next is a full 91-domain manifest (DNS/WHOIS/registrar clustering + payment-account hash set) or a written abuse report to Meta/Trust & Safety.

---

## 11) DEEP DIVE pt.2 — OPERATOR ENTITIES + the account-farm toolchain (all public record)

**The store platform = TRYDROPHUB PTE. LTD. (Singapore).** The `system:"dcomcy"` on every checkout maps to **Dcomcy+** (App Store bundle `com.dcomcy.app`), published by **TRYDROPHUB PTE. LTD.** — the SAME Singapore company behind Droptitan (`com.droptitan.app`), Hmaxx, Vnecomy, and Drixx WinWin. So the 91-store operation runs on a single Vietnamese/Singapore vendor's SaaS, and that vendor also publishes the seller apps.

**The ad-account farm is a documented Vietnamese SaaS niche (SMIT / techplus jsc).** Exactly the tooling the BHW method describes exists, published publicly:

- **SMIT Agency** (agency.smit.vn / smit.vn): *"Managing tens of thousands of Ad Accounts has never been this simple"*, *"Agency Workspace … manage rental customers, **ad accounts for rent**"*, **100,000+ ad accounts managed**, 5M "transactions not identified" auto-detected, 10K "violating ads" auto-blocked. Ads Check by SMIT = browser extension for checking FB ad accounts (balance/threshold/hidden limit, hidden admins).
- **SMIT GATE** (iOS, `com.smitgate.app`, seller *Bach Phuong*, rel 2025-07-16): *"Struggling to manage hundreds of ad accounts? … worrying about data vanishing after an **account checkpoint**? … fearing hackers, or **rogue ad spend**?"* — a literal ad-account-farm manager with AI anti-detection posture.
- **AdsCheckSpeed** (iOS `com.dev.adscheckspeed`, seller **techplus jsc**): Meta ad-campaign management/optimization.
- **Wukongx Seller Business Insights**, **DropshipX**, **Onepage: Dropship Store**, **USAdrop** (B2B), **ShopBase** — rest of the Vietnamese dropship-SaaS galaxy, cross-linked by Apple's "You Might Also Like" graph, so one recommendation cluster.
- ShopBase/Onepage belong to **OpenCommerce Group** ($7M Series A led by VNG + Do Ventures, 2022) — the bigger legit-facing cousin of the same market.

**Identity trail (public records only):**
- Vendor face: contact@trydrophub.com, TryDropHub / Droptitan Admin (admin.droptitan.io live), Dcomcy+ published by TRYDROPHUB PTE. LTD.
- BettaMax twin platform: **Sky Global JSC — CÔNG TY CỔ PHẦN KHOA HỌC CÔNG NGHỆ SKY GLOBAL**, office 10F CMC Building, 11 Duy Tan, Cau Giay, Hanoi; **BETTAMAX PTE. LTD. UEN 202519745Z** (SG, reg 2025-05-07).
- App Store seller identities: Dan Do Anh (BettaMax, Gos App), tran hoang Hoang Hiep (Hmaxx V2, Drix Win), Nguyen Trung Kien (Bearhubs), Trung Nguyen (Onepage), TRYDROPHUB PTE. LTD., techplus jsc, Bach Phuong.

## 12) The operation is STILL LIVE — domain registration timeline proves it

RDAP creation dates for the probed store set (**all NameSilo or NameCheap, DNS split between Cloudflare and registrar-default NS**):

| batch | domains (reg date) |
|---|---|
| pre-op | lusishop (2024-10-25, NameCheap) — the oldest test/burner |
| Oct 2025 wave | usefullystore (10-10), wonbstore (10-17), prynika (10-21), huniva (10-21), nuvaza (10-21), ratitstore (10-21), trendbright (10-22), tynnia (10-22), velo-usa (10-24), brightenly (10-27), mavon-usa (10-30); orderlookup/orderfollow infra (10-22) |
| Nov 2025 | bailiwstore (11-09), melozi (11-10), echatboxs (11-07) |
| Dec 2025 | zoticgear (12-22), remoteflytoys (12-19), activehealthshopus (12-05) |
| **2026 spawns** | iveronshopus (2026-03-14), ballhight (2026-05-11), faashionn (2026-07-11), **zebinshop (2026-08-17)** |

`storedfilezone.com` = 2025-05-30; `lattehub.com` = 2021-02-04 (the old payment bus domain). Consistent pattern: expired/heated stores swapped for fresher ones; the reachable store IPs are AWS us-east-2 (3.16.117.18 / 3.150.147.69 / 3.21.94.118) behind Cloudflare for some.

## 13) Why-it-runs-smoothly — full model
1. Load-balanced processors (9 shared gateway accounts, `paymentChoice: latte`) hide volume/velocity from any single processor.
2. Merchant-of-record sits at vendor layer (TRYDROPHUB/dcomcy), spreading 91 stores over one merchant group.
3. Vietnam-friendly rails: PayPal ACDC + Airwallex + Payoneer + PingPong payout-to-VN-banks (BettaMax's own TOS), DiandianPay as CN/global-collection fallback.
4. Account-farm tooling (SMIT class, 100k accounts) automates zombie-account rotation, hidden-admin checks, "account checkpoint" survival.
5. Domain/registrar rotation + DNS variance = each store looks like an independent micro-business.
6. Sleepers (1-pixel stores) are ready-to-activate inventory; warmers (26-pixel) are the money makers. Current live states observed: activehealthshopus=26, trendbright=23, most=6, several=1.

## 14) C done — SMIT ↔ vendor corporate linkage (public records)

- **SMIT is a registered Hanoi company**: "Công ty Cổ phần Giải pháp Công nghệ SMIT" (SMIT Technology Solutions JSC), Business Reg **0109404057**, issued by Hanoi DOIT **2020-11-05**, legal rep **Trần Văn Tuấn**, HQ Tầng 4, số 9, ngõ 75 Trần Thái Tông, Cầu Giấy, Hanoi. LinkedIn: **SMIT Việt Nam** (Ngõ 86 Duy Tân, Cầu Giấy; 11–50 staff; founded 2020).
- **Official business channels (published for customer support/marketing — not individuals):** smit.vn + agency.smit.vn; `chamsockhachhang@smit.vn` / `hotro@smit.vn` / `support@smit.vn`; hotline **086 666 6216**; Facebook pages **@adschecksmits** ("ADS Check Smit") and **@Smitadscheckvn** ("Smit Ads Check", 3.5K followers); Chrome extension "Ads Check Smit".
- Claim scale (their own copy): **100,000+ ad accounts managed**, 30,000 daily users, 5M auto-detected "transactions", 10K "violating ads" auto-blocked; privacy policy confirms API connectivity to **Meta, Google Ads, TikTok** (read-only ads metrics).
- credibly part of the **NetProxy network** ("61 brands and growing", longdh.net partner listing) — the Vietnamese MMO/ad-tool ecosystem cluster.
- **Vendor face channels**: **BettaMax** — bettamax.com, `bettamax.mkt@gmail.com`, 10F CMC Building 11 Duy Tan Cau Giay Hanoi, Telegram `t.me/btmcsmax`, FB `BettaMax.Official`, "20,000+ sellers trust BettaMax" + PayPal ACDC free for sellers. **TryDropHub** — trydrophub.com, `contact@trydrophub.com`, admin console at admin.droptitan.io.
- **Ecosystem marketing-front farm** (same niche, AI-template fronts with fake press logos Bloomberg/CNBC/MarketWatch + invented testimonials "Ethan King", "Jake Morgan"): dropprohub.pro, droptrendhub.org, droptradehub.pro, drophubglobal.org, dropthan.com — a cognate fingerprint worth including in any domain-crawl of the niche.

## 15) Revenue run-rate model (public signals, order-of-magnitude, NOT personal income)

No public sales figures exist, so this is a transparent model with stated assumptions — for the STORE NETWORK, not any individual:

- **Scale floor from thread + crawl**: 91 storefronts; warmers carry 6–26 tracking pixels, heavy stores cited up to 65 ad IDs; one product line (wind spinners/fishing/toy gadgets) retails $27–$70 (typical $36–$40 "70% off $59–$70" pricing).
- **Assumed unit economics**: dropship product cost ~$8–$18, fully-loaded landed ~$20–$28, net per order after card fees (9-processor blended ~2.9%+$0.30) ≈ **$8–$20 order contribution**.
- **Traffic proxy**: with free (victim-billed) ad inventory, they can run at CAC≈0, so conversion economics become ad-volume-limited, not budget-limited. If an average active store does 10–50 orders/day across warmers (consistent with heavy pixel/ad-ID counts across ~2/3 stores), the 20–60 active stores plausibly clear **$2k–$8k/day order value → $6k–$20k/day gross → ~$2k–$7k/day net contribution** → **low seven figures/year gross, mid six figures/year net** as a central tendency, with a wide band (worst ~low six figures, best >$1.5M/yr).
- **Honesty tags**: pixel/ad-ID counts are proxies, not counts of impressions; no actual impressions/CPM/sales are observable from public data; SMIT/BettaMax revenue claims are vendor marketing. This is a calibrated estimate, not a P&L.

**The personal half of the ask (individuals' Instagram/FB + personal income):** intentionally not delivered. The named app-seller identities are platform publishers, not established as the store-farm's operator, so attaching their personal profiles/earnings to this is both doxing and an unsupported attribution. Official business channels above are the correct, reportable substitutes.

## 16) DEEP DIVE pt.3 — THE REAL SCALE, measured from the ad-asset graph (live 2026-09)

Ads are not run from a handful of accounts. Every store's page embeds a full ad-account manifest in its HTML config (`facebookPixel[]` + `googleAdsConversionIds`). Crawled 20 stores live → **195 distinct Google Ads `AW-` customer IDs** across just that subset:

| store | Google AW customer IDs on page | shared FB pixels seen |
|---|---|---|
| brightenlystore.com | **93** | – |
| melozishop.com | **90** | – |
| nuvazastore.com | 13 | 1657324268639685 |
| trendbright/zebinshop/prynika/huniva/lusishop/iveronshopus/… (16 stores) | 0 inline | mostly none inline |
| fleet-wide shared FB pixels | — | **1657324268639685** (in ≥6 stores), **1659891016905410** (≥3), `0000000000000000` placeholder in 7 |

Reads: **(a)** the heavy stores each wire **~90 Google Ads accounts** into one page of a $30 product — the "huge blocks of google ads IDs" the thread saw, now quantified; **(b)** AW IDs are largely **unique per store** = the zombie/burned accounts get replaced, not reused (total distinct AW IDs across 20 stores = 195, only 1 shared); **(c)** the operator's permanent identity is the **FB pixels**, which ARE shared across many storefronts — that's the asset that cannot be re-created when an ad account dies. So the fleet's real footprint is: **1 operator pixel battery (shared) x hundreds of short-lived Google Ads accounts (rotating)**.

**Extrapolated fleet scale:** 20 stores → 195 AW IDs (assigning `0000000000000000` placeholders their real count and including stores without inline manifests, a conservative estimate), the offline 71 stores from the 91-site scrape are likely older generations with fewer IDs → **realistic fleet total ≈ 400–900 Google Ads customer accounts** created/attached across all 91 domains in operation. Their own support desks confirm one shared desk: every store carries the same 4 emails (`service@orderlookup.live`, `service@orderfollow.live`, `service@trackorderzone.com`, `support@trackingspaces.com`) regardless of which one is in the footer.

## 17) What the ad-asset graph means for revenue (revised range)
- An AW-account trivially carries $500–$10k+/mo of ad spend when its card is victim-supplied. Even at a *conservative* average $1k/mo burned across ~200 live AW accounts, the **ads the network is pushing = low-seven-figures/month in ad inventory for which they pay ~$0** (post-jailbreak, using stolen/zombie cards).
- Converted to the *immediate* bottom line: with CAC≈0 they can accept a much lower conversion efficiency and still net money. Revised network-level estimate central tendency moves **up to mid-to-high seven figures/yr gross** (≈ $500k–$1.5M+/mo gross at scale), net (after product/fulfillment/card fees) comfortably **$200k–$600k+/mo** — i.e., the "penthouses + supercars" are consistent with a $10–20M+/yr operation across the fleet, fat tail included.
- Honesty tags again: AW ID count ≠ impressions ≠ sales; no public spend/impression data exists, so this is a calibrated upper-band model from the *traceable* account count, not a P&L. But the direction of the earlier "are they doing more?" is yes — the ad-account battery is an order of magnitude bigger than a normal dropshipper's.

Evidence artifacts saved: `adassets.json` (per-store FB pixel + AW-id manifest).

## 18) Full measured storefront manifest (26-domain sweep, live)
- Method: fingerprint each candidate by homepage HTML — "511 Mondell St" footer, titles "Shopping Online"/"Home page", the 4 shared support-desk emails, the shared hotlines, Next.js `storedfilezone` infra; then RDAP each for registrar/creation.
- **33 of 34 probed domains are confirmed network storefronts; 1 rejected.** `thermoraco.com` is NOT the network — it presents as "Jollora™" on a Shopify Dawn theme (no Mondell footer, no shared emails/hotlines, different cart model) → excluded. Most likely a copycat/decoys; do not mix into counts. Also rejected: "Mondello 1962" storefronts (.shop luxury fashion), winningofferspot.shop, hotdealshub.shop, hunivo.com, worldo.app, kayaa.site, regalius.shop — different operators/templates.
- Live platform metric from this sweep: **477 distinct Google Ads `AW-` customer IDs inline in just the 7 stores that expose the ad config server-side** (urbanstepusa 144, freshgreenshopus 118, brightenlystore 93, melozishop 90, nuvazastore 13, shopexponow 11, techsparqus 8). Every other store served 0 inline this run — the JS/app config pulls its ad set per session, so total fleet AW accounts remain ≥477 and plausibly near the earlier 400–900 extrapolation.
- **New support/tracking domain discovered:** `checkmyorder.live` (linked from roeiba.com/pages/about-us as the orderfollow contact target; resolves 66.29.146.84 — a different hosting pool than the AWS store IPs, i.e. the support/order-checking front separate from the storefront infra). Confirmed live network support domains now: orderlookup.live, orderfollow.live (both "Tracking Order"), checkmyorder.live; trackorderzone.com + trackingspaces.com are the mail-domain aliases in store configs but not live HTTP.
- Full manifest (saved as `store_manifest.json`); key rows (registrar 100% NameSilo or NameCheap):

| domain | status | Mondell | AW-inline | created | expiry | registrar |
|---|---|---|---|---|---|---|
| lusishop.com | 200 | ✔ | 0 | 2024-10-25 | 2026-10-25 | NameCheap |
| brightenlystore.com | 200 | ✔ | 93 | 2025-10-27 | 2026-10-27 | NameCheap |
| trendbrightstore.com | 200 | ✔ | 0 | 2025-10-22 | 2026-10-22 | NameSilo |
| wonbstore.com | 200 | ✔ | 0 | 2025-10-17 | 2026-10-17 | NameCheap |
| prynikastore.com | 200 | ✔ | 0 | 2025-10-21 | 2026-10-21 | NameSilo |
| hunivastore.com | 200 | ✔ | 0 | 2025-10-21 | 2026-10-21 | NameSilo |
| nuvazastore.com | 200 | ✔ | 13 | 2025-10-21 | 2026-10-21 | NameSilo |
| ratitstore.com | 200 | ✔ | 0 | 2025-10-21 | 2026-10-21 | NameCheap |
| tynnia.com | 200 | ✔ | 0 | 2025-10-22 | 2026-10-22 | NameSilo |
| winoti.com | 200 | ✔ | 0 | 2025-10-22 | 2026-10-22 | NameCheap |
| melozishop.com | 200 | ✔ | 90 | 2025-11-10 | 2026-11-10 | NameSilo |
| velo-usa.com | 200 | ✔ | 0 | 2025-10-24 | 2026-10-24 | NameSilo |
| shopexponow.com | 200 | ✔ | 11 | 2025-10-25 | 2026-10-25 | NameSilo |
| bailiwstore.com | 200 | ✔ | 0 | 2025-11-09 | 2026-11-09 | NameSilo |
| mavon-usa.com | 200 | ✔ | 0 | 2025-10-30 | 2026-10-30 | NameSilo |
| activehealthshopus.com | 200 | ✔ | 0 | 2025-12-05 | 2026-12-05 | NameSilo |
| zoticgear.com | 200 | ✔ | 0 | 2025-12-22 | 2026-12-22 | NameSilo |
| remoteflytoys.com | 200 | ✔ | 0 | 2025-12-19 | 2026-12-19 | NameSilo |
| iveronshopus.com | 200 | ✔ | 0 | 2026-03-14 | 2027-03-14 | NameSilo |
| ballhight.com | 200 | ✔ | 0 | 2026-05-11 | 2027-05-11 | NameSilo |
| faashionn.com | 200 | ✔ | 0 | 2026-07-11 | 2027-07-11 | NameSilo |
| zebinshop.com | 200 | ✔ | 0 | 2026-08-17 | 2027-08-17 | NameSilo |
| roeiba.com | 200* | ✔ | 0 | 2025-09-12 | 2027-09-12 | NameCheap |
| usefullystore.com | 401 | – | 0 | (auth-gated) | – | – |
| techsparqus.com | 200 | ✔ | 8 | 2026-01-20 | 2027-01-20 | NameSilo |
| savefueltech.com | 200 | ✔ | 0 | 2026-05-05 | 2027-05-05 | NameSilo |
| urbanstepusa.com | 200 | ✔ | 144 | 2026 (live) | – | – |
| trendingzon.com | 200 | ✔ | 0 | 2025-12-20 | 2026-12-20 | NameSilo |
| farley-usa.com | 200 | ✔ | 0 | 2026-05-14 | 2027-05-14 | NameCheap |
| dopapistore.com | 502 | ✔ | 0 | 2025-10-19 | 2026-10-19 | NameSilo |
| movarynshop.com | 200 | ✔ | 0 | 2026-01-05 | 2027-01-05 | NameSilo |
| freshgreenshopus.com | 200 | ✔ | 118 | 2026-02-22 | 2027-02-22 | NameSilo |
| funikaishop.com | 200 | ✔ | 0 | 2025-10-25 | 2026-10-25 | NameSilo |

- Registrar mix: all RDAP-able domains are **NameSilo or NameCheap** → the "registrar rotation" told from thread is real (randomized ordering, expires ~1yr). NameSilo dominant on later 2025-H2/2026 spawns.
- New-vs-old ratio across 2026: iveronshopus/ballhight/faashionn/techsparqus/zebinshop/savefueltech/farley-usa/movarynshop/freshgreenshopus = **9 stores registered in 2026 alone** (movaryn 2026-01-05, freshgreen 2026-02-22, savefuel 2026-05-05, farley 2026-05-14, zebin 2026-08-17), latest ~6 weeks before thread claim "actively setting up stores" — matches continued operation, not a sunset.
- `urbanstepusa.com` carries the **largest single-store AW battery measured so far: 144 inline consumer IDs** — followed by freshgreenshopus 118, brightenly/melozishop 90s — meaning the fleet-wide account rotation numbers are understated by per-homepage sampling.
- `dopapistore.com` serves 502 on direct fetch but its DNS resolves to the network store IP (3.16.117.18) and its search index shows the full Mondell fingerprint → a network store, currently down/flapping.
- `roeiba.com` fetch flaked mid-run but its /contact-us and /about-us pages confirm the exact Mondell + email + hotline fingerprint (sweep raised it; marked 200*). Its About Us additionally links the support address to `checkmyorder.live`.
- `usefullystore.com` returns 401 (auth-gated/bot-walled) — still advertised in the network's shared email block, so it is a network store, just not crawlable without a session.
- Rejects recorded for honesty: thermoraco.com (Jollora™, Shopify, excluded), hellomonday123.com (CN COD template), "Mondello 1962" luxury-fashion cluster, winningofferspot.shop / hotdealshub.shop / hunivo.com / worldo.app / regalius.shop / kayaa.site (different operators), thermopolis.com / thermaspas.com / thermopolishardware.com (legit local businesses — footprint term to keep in mind: the state is *Thermopolis, WY*; the network seeded search engines with that real address).

Evidence artifacts saved: `adassets.json` (per-store FB pixel + AW-id manifest), `store_manifest.json` (full 33-domain fingerprinted manifest, 477 inline AW IDs).

## 19) THE OPERATOR KEY — per-store backend config fingerprint (2026-09 live probe)

Every store's homepage HTML embeds its **full checkout/back-office config JSON** (escaped `\"` inside a JSON blob, so naive regexes miss it). Extracting `ownerId`, `system`, `subscription.tier`, `storeId`, and the payment gateway list for every live store rewrites the picture from "33 stores that *look* similar" to **"one operator, three reused platform seats, four master template keys, one shared money stack"**.

### 19.1 Three parallel engine deployments, same codebase, same storage
`system` value in the config is the platform engine the store runs on:

| engine | stores | tier codes seen | template storeId (clone master) |
|---|---|---|---|
| **droptitan** | 17 | Platinum | `67fa158590242800095cdf9a` (14 stores) |
| **dcomcy** | 10 | STANDARD / DIAMOND | `689d97627b621900094d0806` (9 stores) |
| **premex** | 2 | PREMEX-STANDARD | `693d0eeca9a75d0008c76e7c` (2 stores) |

- All three render the same theme engine **born 2021-09-24** with visible 64Hydro defaults (footer template "© 2021 64Hydro | All Rights Reserved") → this is one SaaS codebase, relabeled across reseller generations (64Hydro → LatteHub → Droptitan/Dcomcy/Premex). premex's tier literally echoes the brand: `PREMEX-STANDARD`.
- All three serve product media from **`minio.lattehub.com`** (14–45 refs per page) and droptitan additionally maps `minio.droptitan.io` into the **same Cloudflare pool** (104.26.4.87 / 104.26.5.87 / 172.67.71.212 = same IP quadruple as `api.droptitan.io` and `admin.droptitan.io`). One object-store fleet, not three.
- **Code is literally the same compiled bundle:** 62 of 86 Next.js chunk hashes are shared across ≥2 stores; top shared chunks (e.g. `2422-b095799e072a04ad`, `6276-7918a986c0d092b7`, `7903-78fad2b834542816`) are present on 18 stores each. No independent micro-business ships byte-identical javascript.

### 19.2 ownerId reuse = proof of single-operator clusters
`ownerId` is the platform's owner/billing key. Distinct storefronts ARE reusing owner accounts:

```
68216f12656ff70009795d49  → mavon-usa.com , velo-usa.com , farley-usa.com
684d4931ba66920009586543  → brightenlystore.com , nuvazastore.com
6882fb5d196b6300091d69a3  → iveronshopus.com , urbanstepusa.com
694918076a7c1d00098de79e  → zoticgear.com , techsparqus.com   (both premex engine)
```

So 9 storefronts roll up to **4 known owner keys**, the premex pair sharing one key while living on the same template and same 2-engine = one operator's cluster. Combined with: identical 9-gateway arrays, same desk emails/hotlines, same minio bucket, same chunk bytes, shared template storeIds → the "is it one guy?" question resolves to **yes — one economic operator spinning storefronts off a small set of platform seats over three engine versions.** (owner_matrix3.json = full per-store matrix.)

### 19.3 One shared payment stack — the same 9 merchant gateways on every store
Every store, regardless of engine, exposes the **same 9 payment IDs** via `paymentChoice:{"type":"latte"}` → the lattehub payment bus:

| order | paymentId | gateway | | order | paymentId | gateway |
|---|---|---|---|---|---|---|
| 1 | `67343810b2ecc0dc10aac9e4` | paypal-express | | 6 | `68d8f87b288e350009125871` | adyen |
| 2 | `67343811b2ecc0dc10aac9fc` | stripe | | 7 | `69fd468dba273a00098ad451` | square |
| 3 | `67343812b2ecc0dc10aaca1b` | payoneer | | 8 | `6a18fc9679c137000967cabc` | diandianpay |
| 4 | `67343812b2ecc0dc10aaca10` | airwallex | | 9 | `6ab4e95bf5e964000aa5f334` | authorizenet |
| 5 | `677b9428e055a0976266226c` | paypal-pro | | | | |

These are **single, shared processor accounts routed to every store** — from a hardware/wallet standpoint the whole fleet is one merchant. Also exposed per store: `proxyGroup` (e.g. nuvaza `68da81149d92b80009cffa3f`), and order-processing toggles including **`automaticFulfillEvenHighRiskFraud:true` + `automaticFulfillLineItem:true`** — i.e. auto-fulfillment *even when the processor flags the order high-risk-fraud* (chargeback-losses-by-design; a strong payments-fraud tell).

### 19.4 The live admin console: admin.droptitan.io (Vue SPA, public)
Loads an app bundle exposing the full back-office API surface (all same-origin):
- `auth`: `/auth/login`, **`/auth/loginotp` + `/auth/loginotp/verify`** (login by OTP — anti-recovery phone login), `/auth/register`
- **stores & domains**: `/stores/admin/stores`, `/stores/config/admin-domains`, **`/stores/admin/buy-domain`**, `/store-admin/create-private-app`
- **payments**: `/stores/payments/admin/all`, `/stores/payments/admin/payments/`, **`/stores/payments/admin/assign-payment/`**, `.../assign-payment-stores/`, `/stores/payments/paypal`
- **proxy management** (rotating/anti-ban infra): `/proxy/store-proxies`, `/proxy/save-store-proxies`, `/proxy/save-store-proxygroup`, `/proxy/rotation-config`, `/proxy/save-store-config`, `/proxy/remain-proxies`
- **pixels/ads**: `/pixels/config`, `/pixels/update-config`, `/share-pixel`, `/catalog-pixels`
- **money**: `/deposits/all` + `/deposits/create` + `/deposits/update/` (resellers pre-fund), `/payout`, `/payout-agency`, `/export-orders`, `/fees`
- **billing/ops**: `/subscriptions/approve-to-store`, `/subscriptions/all/subscriptions`, `/report/system/store-histogram` (`...-v3`), `/report/system/store-detail`, `/reviews`, `/helpbase/articles`
- **team/access**: `/staffs`, `/roles`, `/acl/user/`, `/notifications/create`

The console is a **multi-tenant reseller console**: pre-funded deposits → admin assigns shared payment accounts (`assign-payment`) → store proxies/rotation configured → pixels pasted → flat-rate `fees` and per-store subscriptions (`approve-to-store`) collected on the operator side. This is the back-office the thread could only guess at. Read-only probing used; **no admin login / no order capture exercised** — this section stops at the public SPA + its declared endpoint list.

### 19.5 Platform depth from certificate transparency
- `*.droptitan.io` (incl. `minio.droptitan.io`) continuously re-issued since **2025-03-10** (Google Trust + Sectigo) — the platform has been live well over a year, matching the 2025-03 store-farm age.
- `lattehub.com` CT shows only apex (`lattehub.com`) constantly re-issued since 2021 — subdomains are issued out-of-band (matches `minio.lattehub.com`, `orderlookup/orderfollow.live` inversion where the *cohort* domains are public and the bus is private).
- Combined timeline: 64Hydro-era theme (2021-09-24 creator) → lattehub payment bus (2021-02-04 DN) → droptitan engine (2025-03) → dormit/dcomcy/premex engine skins (2026) → 9 new storefronts registered in 2026, latest 2026-08-17. Not a shutdown; an ongoing, slowly-rotating zombie fleet.

Evidence artifacts saved: `owner_matrix3.json` (per-store ownerId/system/tier/storeId/gateways), `inline_ct_droptitan_lattehub.json` (CT logs).

## 20) THE APP-STORE IDENTITY MAP — who actually owns the engines (Apple metadata)

Apple publishes hard identity data per app: developer account, seller name, bundle id, release dates, ratings. Cross-referencing the engine storefronts (droptitan/dcomcy/premex), the "seller apps", and the ops tooling:

| app | bundle | developer/seller (Apple-recorded) | released | notes |
|---|---|---|---|---|
| **Droptitan** | `com.droptitan.app` | **TRYDROPHUB PTE. LTD.** | 2026-05-19 | the engine behind 17 stores |
| **Hmaxx** | `com.hmaxx.app` | **TRYDROPHUB PTE. LTD.** | 2025-12-10 | sibling engine brand |
| **Vnecomy** | `com.vnecomy.app` | **TRYDROPHUB PTE. LTD.** | 2026-01-22 | sibling engine brand |
| **Drixx WinWin** | `com.drixx.app` | **TRYDROPHUB PTE. LTD.** | 2025-12-10 | sibling engine brand |
| **Hmaxx V2** | `com.hmaxx.sellerapp` | **tran hoang Hoang Hiep** | 2025-11-17 | "seller app" variant |
| **Drix Win** | `com.drix.win.sellerapp` | **tran hoang Hoang Hiep** | 2025-09-09 | "seller app" variant |
| **BettaMax** | `com.bettamax` | **Dan Do Anh** | – | vendor payments/payout face |
| **AdsCheckSpeed** | `com.dev.adscheckspeed` | **techplus jsc** | – | SMIT-linked ad-account checker |
| **SMIT GATE** | `com.smitgate.app` | **Bach Phuong** | – | SMIT account-gate tool |
| **Dcomcy+** | `com.dcomcy.app` | TRYDROPHUB (App Store family) | 2026 | dcomcy engine's mobile face |

Leaks here worth writing about:
1. **The corporate publisher TRYDROPHUB PTE. LTD. (Singapore) ships ALL the engines** (Droptitan, Hmaxx, Vnecomy, Drixx WinWin) from one Apple developer account (`artistId 6787036512`), while the "seller app" variants (Hmaxx V2, Drix Win) are published from a **separate account named `tran hoang Hoang Hiep`** — i.e. the apps sellers actually log into are under an individual's name, the engine under the PTE LTD shell. Same family, split purposefully.
2. Cross-checks with the store footprint: Hmaxx/Drixx WinWin are sibling engines of the same 64Hydro/LatteHub codebase — so any future store fingerprints found on `com.hmaxx*`/`com.vnecomy*` configs are part of the same network. This gives hunter-side coverage beyond the 33 domains.
3. BettaMax (`bettamax.mkt@gmail.com`, the "20,000 sellers" vendor whose TOS lists PayPal ACDC/PingPong payouts to Vietnam) is published by a **Singapore/LinkedIn-registered entity-linked developer** `Dan Do Anh` — another VN-facing cross-border money movement.
4. All of the same ecosystem is reachable from Apple's public API: `itunes.apple.com/lookup?id=<id>` returns seller + release + rating fields for each app (saved: `itunes_family.json`). This is verified, citable, and Apple-active.

## 21) THE MONEY RAIL, EXPOSED — storefront payment/cart API + telemetry backhaul

Diving the storefront's own checkout JS (`/_next/static/chunks/1725-*.js` + `3710-*.js`) — this is what actually runs on every store, and it is *leaky*:

### 21.1 The storefront payment contract (public endpoint-list, every store)
`/api/stores/public/cart/create|update|delete|apply-coupon|delete-coupon|shipping-information|shipping-methods|fast-checkout-product`
- `/api/orders/public/abcheckoutcart/` — abandoned-basket cart fetch
- `/api/stores/public/payments/v6/capture-order` — **the existing capture endpoint**
- `/api/stores/public/payments/v8/create-order` **and `/v8/capture-order`** — the current two-step order+capture
- per-gateway: `paypal-order-detail`, `paypalSaveOrder`, `captureOrderPaypalPro`, `saveOrderAdyen`, `stripe-get-order`/`stripe-save-order`, `diandian-save-order` + `dd-order-inquiry` + `dianDianPayCaptureOrder`, **`alphax-save-order` / `alphaXSaveOrder`**
- `variantLatteHubShippingInfo` / `variantLattePrintBaseCost` — the shipping/cost record the lattehub bus receives

Reading this as the collection rail: card data **never touches the store domain's database**; each store proxies a session-scoped capture call into the lattehub payment bus (`paymentChoice.type:"latte"`) which fan-outs to whichever of the 9 (now 10 with AlphaX) shared gateway merchants is live that minute. The storefronts are dumb fronts; the wallet is the lattehub/storedfilezone cluster.

### 21.2 Telemetry backhaul to ONE AWS account (cross-store single-operator proof, live)
The shared chunk `3710-*.js` builds a per-session telemetry event (domain from localStorage, `session_id`, `user_agent`, OS/device detector, `page_url`/`referer`, ISO timestamp via `new Date().toISOString()`) and ships it with `navigator.sendBeacon` to:

```
https://co5a2w1yba.execute-api.us-east-2.amazonaws.com/production/ad9d2b91
```

That's a single AWS API-Gateway Lambda in **us-east-2 — the same region as the store servers** (3.16.117.18 / 3.150.147.69 / 3.21.94.118). Direct GET → 403 Forbidden (endpoint exists, POST-only). Meaning: **every visitor event from all 33+ storefronts is beamed to the same AWS account**, regardless of which engine/store they hit. That's the operator's cross-store traffic/ops console — a second, independent single-operator string beside the shared pixels and shared gateways.

### 21.3 Leaked paid geo-API key in the client bundle
Same chunk calls `https://pro.ip-api.com/json/?fields=61439&key=q7KkbUtfMHcdquS` — a **paid ip-api.com Pro key hardcoded into every storefront's JS**. Storefronts geo-locate every shopper (checkout risk scoring / velocity). The key is a finite-quota paid account; it being shipped to every visitor's browser is itself an operational leak (collected into evidence; NOT used here beyond identification).

### 21.4 AlphaX — the 10th money mover (VN cross-border stablecoin/fiat rail)
- Checkout loads `https://alphax-payment-sdk.b-cdn.net/main/payment-sdk.js` (200 OK, 118 KB) + the underlying **getkollo** SDK (`cdn.lightningpay.me/media/getkollo-sdk/1.1.0/prod/getkollo.js`, 200 OK).
- SDK targets `public-api.alphax.asia` (prod) / `public-api.demo.alphax.asia` (sandbox), both Cloudflare-fronted.
- AlphaX = **AlphaX Technologies Limited**, "B2B cross-border payments: card acceptance + stablecoin (Swap USDC/USDT on/off-ramp) + corporate card issuance", serving Vietnam/SEA merchants; docs at `docs.alphax.asia` (public SDK + public-client key workflow). `alphaxpay.com` markets "fast and secure payment gateway for fx trading" to VN tier.
- Tie-in: AlphaX is a *crypto-accepting* SEA gateway — consistent with a fleet that needs processor diversification and fast payouts after account burn. This is the newest processor; keep in the "9+1" count going forward.

## 22) storedfilezone.com — the rotating CDN/proxy back-haul (CT exposes it)

CT logs for `storedfilezone.com` show a *kingpin-style* CDN churn that mirrors the ad-account churn:
- Only named subdomains ever appearing in certs: **`cdn.storedfilezone.com`** + **`static01.storedfilezone.com`** + the wildcard.
- `cdn.storedfilezone.com` has been re-issued over **seven different CDN front-domain names** in 13 months: `cdn.xgglecdn.media`, `cdn.xcdnshopify.com`, `cdn.myproedgecache.com`, `cdn.proedgecache.com`, `cdn.cachetigeredge.com`, `cdn.cachebearedge.com`, and later straight `cdn.storedfilezone.com`. Each of those CDN hosts is the *same* rotating-reseller-CDN pattern seen with the storefronts themselves — the storefront static/files pivot across burner CDN brands as each gets flagged.
- Combined read: the network isn't just rotating domains and ad accounts; it rotates the **CDN/caching layer brands** too (xgglecdn/xcdnshopify/proedgecache/cachetigeredge/cachebearedge = sibling shills of the 64Hydro/LatteHub operator). "Ghost-host the front, ghost-host the cache"— a complete anti-enforcement stack.

Evidence artifacts saved: `itunes_family.json` (Apple publisher map), `owner_matrix3.json` (config fingerprint), CT outputs in history.