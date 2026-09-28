# The Zombie Ad-Account Store Network ("64Hydro → LatteHub → Droptitan/Dcomcy/Premex")

**Summary:** a Vietnamese operator runs a factory of mass-generated Next.js e-commerce shell stores. The
stores are vehicles for **data collection + victim-billed ad spend** — ads are run from hacked / "zombie"
ad accounts whose billing belongs to real victims, so the operator keeps 100% of product revenue and
charges every bill to other people. Fallen accounts are rotated out; survivors are "warmed up"; 10–20
accounts are pointed at one domain.

**Scale:** the source thread (BlackHatWorld) counted **91 sites** via an address dork over the shared
footer `511 Mondell St, Thermopolis, WY 82443`. This repo independently fingerprinted **33 of 34 probed
domains (1 rejected)** and measured the ad-account battery: **477 distinct Google Ads `AW-` customer IDs
inline** across just the 7 stores that expose their ad config server-side.

> All evidence here is **public** : raw store HTML, its embedded checkout/back-office config JSON,
> RDAP/WHOIS, Google Apps/BH search indexes, Apple App Store metadata, and certificate-transparency
> logs. **No purchases, no admin login, no order capture, no credentials, no account takedowns.**
> Nothing in this repo targets or harms victims — it identifies the network so it can be reported.

---

## The one-liner

```
33 storefronts fingerprinted live → 1 owner economy
  4 reused ownerId seats → 3 engine deployments (droptitan / dcomcy / premex)
  → 1 codebase (64Hydro theme, born 2021-09-24) → 1 media bucket (minio.lattehub.com)
  → 1 payment bus (lattehub.com, "paymentChoice":{"type":"latte"}) → 1 money stack (9 shared gateways)
  → 1 telemetry AWS account (us-east-2) → 1 geo-API key leak → 1 operator.
```

## Repository layout

| file | what it contains |
|---|---|
| `stores/STORE_INVENTORY.md` | every confirmed storefront, rejected candidates, owner clusters, template keys |
| `data/30-store-manifest.json` | 33-domain manifest — status, footer, AW IDs, emails, phones, RDAP |
| `data/40-owner-operator-matrix.json` | per-store `ownerId` / `system` / tier / `storeId` / 9 gateway IDs |
| `data/50-ad-account-assets.json` | per-store Facebook pixel IDs + Google Ads `AW-` IDs |
| `data/51-urbanstep-144-aw-ids.json` | urbanstepusa.com's 144 inline consumer IDs (largest single-store set) |
| `data/60-rdap-registrar.json` | RDAP creation/expiry/registrar for the probed set |
| `data/70-appstore-publisher-map.json` | Apple seller identities for the engine apps |
| `docs/` | the full operational breakdown (23 sections) and the public thread script |

## The evidence, section by section

### 1. The shared address: 511 Mondell St, Thermopolis, WY 82443
Every store's footer carries the same street address — a **real rented house**, not a business. That one
string is how the thread's OP recovered the 91-site fleet, and it fingerprints every new store instantly.
The network has *noticeably never changed it*, across three engine generations and four registrars.

### 2. One support desk for 33 "brands"
Every store, regardless of engine, carries the same 4 emails
(`service@orderlookup.live`, `service@orderfollow.live`, `service@trackorderzone.com`,
`support@trackingspaces.com`), the same two hotlines `+1 (501) 242 0006` / `+1 (404) 857 3486`, the same
support hours, and title pages of "Shopping Online" / "Home page".

### 3. One economic operator: ownerId reuse
`ownerId` is the platform's billing key. Distinct storefronts reuse owner accounts:

```
68216f12656ff70009795d49  → mavon-usa.com, velo-usa.com, farley-usa.com
684d4931ba66920009586543  → brightenlystore.com, nuvazastore.com
6882fb5d196b6300091d69a3  → iveronshopus.com, urbanstepusa.com
694918076a7c1d00098de79e  → zoticgear.com, techsparqus.com  (premex engine)
```

### 4. Three engines, one codebase
`system` in each store's config: **droptitan** (17 stores, Platinum), **dcomcy** (10, STANDARD/DIAMOND),
**premex** (2, PREMEX-STANDARD). All three are the same theme engine born 2021-09-24 with 64Hydro defaults
("© 2021 64Hydro | All Rights Reserved") — label lineage 64Hydro → LatteHub → Droptitan/Dcomcy/Premex.
**62 of 86 Next.js chunk hashes are shared across ≥2 stores** — the same chunk ships on 18 domains. No
independent micro-business ships byte-identical javascript.

- Template `storeId`s — the clone masters: `67fa158590242800095cdf9a` (x14), `689d97627b621900094d0806` (x9), `693d0eeca9a75d0008c76e7c` (x2).
- All stores serve product media from `minio.lattehub.com`; droptitan also maps `minio.droptitan.io`
  into the same Cloudflare pool as `api.droptitan.io` / `admin.droptitan.io` (104.26.4.87 / 104.26.5.87 / 172.67.71.212).

### 5. The money stack: 9 shared gateways + PingPong + AlphaX
Every store exposes the **same 9 payment IDs** through the `lattehub` payment bus:

| # | gateway | paymentId |
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

Plus the rails behind the payouts and the newest mover:
- **PingPong** — BettaMax's own TOS ("connect with PingPong to receive payments to [Vietnamese] domestic bank account") is how the operator drains funds to Viet Nam without a US/EU bank account.
- **AlphaX** (AlphaX Technologies Limited) — the **10th** gateway: SEA cross-border card acceptance + **stablecoin (USDC/USDT Swap) on/off-ramp**; checkout loads `alphax-payment-sdk.b-cdn.net/main/payment-sdk.js` + getkollo SDK, prod API `public-api.alphax.asia`. A crypto-accepting rail = processor diversification after account burn.

Config exposes the fraud posture directly: **`automaticFulfillEvenHighRiskFraud:true` + `automaticFulfillLineItem:true`** on stores — auto-fulfillment even when the processor flags the order as high-risk fraud. Chargeback losses are a budget line, not a bug.

The storefront payment contract (`/_next/static/chunks/1725-*.js`): cart create/update/delete/coupon,
`/api/orders/public/abcheckoutcart/`, `payments/v6/capture-order`, `payments/v8/create-order` +
`v8/capture-order`, per-gateway `-save-order` endpoints. **Cards never touch the store domain's DB** — each
store proxies a session-scoped capture call into the lattehub bus, which fans out across the shared
merchants. The storefronts are dumb fronts; the wallet is the lattehub/storedfilezone cluster.

### 6. The ad-account farm is the real business
- Every store's page embeds an ad-account manifest (`facebookPixel[]` + `googleAdsConversionIds`).
- 20-store crawl → **195 distinct Google Ads `AW-` IDs**; then a 7-store subset (which exposes config
  server-side) → **477 distinct `AW-` IDs**: urbanstepusa.com **144**, freshgreenshopus.com **118**,
  brightenlystore.com **93**, melozishop.com **90**, nuvazastore.com 13, shopexponow.com 11, techsparqus.com 8.
- Shared FB pixels are the operator's permanent identity: **1657324268639685** in ≥6 stores,
  **1659891016905410** in ≥3. AW IDs are largely unique per store — burned accounts get replaced, the
  pixel battery survives. Fleet-scale estimate: **≈400–900 Google Ads customer accounts** across 91 domains.

That tooling is a documented Vietnamese SaaS niche — **SMIT** (agency.smit.vn): "Managing tens of
thousands of Ad Accounts has never been this simple", **100,000+ ad accounts managed**,
**Ads Check Smit** extension to check account balance/threshold/hidden admins, and **SMIT GATE**
(iOS) whose very description is "an ad-account-farm manager with AI anti-detection posture".

### 7. The vendors (public record)
- **TRYDROPHUB PTE. LTD.** (Singapore) — Apple developer (`artistId 6787036512`): Droptitan
  (`com.droptitan.app`), Hmaxx, Vnecomy, Drixx WinWin, and the dcomcy engine's Dcomcy+ (`com.dcomcy.app`).
- **tran hoang Hoang Hiep** (individual, VN) — publishes the "seller app" variants Hmaxx V2 (`com.hmaxx.sellerapp`), Drix Win (`com.drix.win.sellerapp`). Corporate shell vs. individual split.
- **BettaMax** — Sky Global JSC (CÔNG TY CỔ PHẦN KHOA HỌC CÔNG NGHỆ SKY GLOBAL, 10F CMC Building, 11 Duy Tan, Cau Giay, Hanoi) + **BETTAMAX PTE. LTD. UEN 202519745Z** (SG, reg 2025-05-07), seller app by **Dan Do Anh**. Their own copy names PayPal ACDC + **PingPong** payouts + 20,000+ sellers.
- **techplus jsc** → AdsCheckSpeed; **Bach Phuong** → SMIT GATE; **Nguyen Trung Kien** → Bearhubs; **Trung Nguyen** → Onepage.
- The admin console `admin.droptitan.io` (Vue SPA, public) exposes the whole reseller back office:
  login-by-OTP, buy-domain, **assign-payment**, store proxies/rotation, pixel config, deposits, payout,
  fees, subscriptions/approve-to-store. Read-only listing — **no login, no capture**.

### 8. One telemetry brain
Shared chunk `3710-*.js` builds a session event (domain, session_id, user_agent, OS/device, page_url,
referer, timestamp) and `navigator.sendBeacon`s it to:

```
https://co5a2w1yba.execute-api.us-east-2.amazonaws.com/production/ad9d2b91
```

A single AWS API-Gateway in **us-east-2 — the same region as the store servers**. Every visitor event
from every storefront beams to the same AWS account. Plus a **paid ip-api.com Pro key leaked in the same
chunk** (`key=q7KkbUtfMHcdquS`, fields=61439) used to geo-score shoppers. The key is public here only to
flag the leak; it was **not consumed**.

### 9. The CDN layer churns too
Certificate transparency for `storedfilezone.com` shows `cdn.storedfilezone.com` re-issued over **seven
burner CDN front-domains** in 13 months: `cdn.xgglecdn.media`, `cdn.xcdnshopify.com`,
`cdn.myproedgecache.com`, `cdn.proedgecache.com`, `cdn.cachetigeredge.com`, `cdn.cachebearedge.com`, then
straight `cdn.storedfilezone.com`. "Ghost-host the front, ghost-host the cache" — a complete
anti-enforcement stack.

### 10. Still alive
Registrations continue into 2026: movarynshop (2026-01-05), techsparqus (01-20), freshgreenshopus
(02-22), savefueltech (05-05), farley-usa (05-14), ballhight (05-11), faashionn (07-11),
**zebinshop (2026-08-17)** — the newest store, ~3 weeks before this write-up. Not a shutdown; a
slowly-rotating fleet. `*.droptitan.io` certificates have been re-issued continuously since **2025-03-10**.

---

## Methodology

1. **Anchor:** start from the thread's address dork (`"511 Mondell St"` + `shopping online` etc.) and the
   BH search-indexed domain list.
2. **Fingerprint:** fetch candidate homepages; require (a) the Mondell footer, (b) one of the 4 shared
   emails, (c) a shared hotline, (d) Next.js `storedfilezone` infra, (e) generic "Shopping Online"
   titles. Reject anything that lacks these.
3. **Config extraction:** decode the escaped JSON config blob inside each homepage
   (`<script>…\"…\"…</script>`) → `ownerId`, `system`, `subscription.tier`, `storeId`, gateway list,
   `paymentChoice`, `proxyGroup`, order-processing booleans.
4. **Plumbing calls:** probe chunk hashes for overlap; RDAP each domain; CT-log the infra/cdn domains;
   look up the app bundles on the public iTunes Lookup API; walk the soft-404 admin/API paths read-only.
5. **Ad graph:** pull `facebookPixel[]` + `googleAdsConversionIds` per store; count distinct IDs.

Everything is saved under `data/`. Re-runnable probe scripts referenced in `docs/`.

## Integrity / safety note

- This is an illegal-fraud operation (hacked ad accounts, victim-billed ad spend, MRR rebilling). The
  investigation stays **read-only**: no purchases, no account access, no takedowns, no credentials.
- The ip-api Pro key is disclosed **as a leak to be rotated**, not for use.
- Personal identities are limited to public business records (App Store seller names, corporate
  registrations, official vendor channels). No private individuals' residences, socials, or earnings are
  published.
- Reporting these findings to Meta/Google/Apple/processors is encouraged; use the receipts in `data/`.

## References

- Source thread — BlackHatWorld: `blackhatworld.com/seo/found-a-very-weird-ecom-method-from-a-sketchy-ad-on-youtube-some-help.1770124`
- Droptitan on App Store: `apps.apple.com/us/app/droptitan/id6746070773`
- Hmaxx V2 (tran hoang Hoang Hiep): `apps.apple.com/us/app/hmaxx-v2/id6749109695`
- BettaMax platform: `bettamax.com` (TOS: PayPal ACDC + PingPong payouts)
- SMIT ad-account farm: `smit.vn` / `agency.smit.vn`
- Platform infra: `admin.droptitan.io`, `lattehub.com`, `storedfilezone.com`