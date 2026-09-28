# THE 33 MASKS — full thread script

Voice: Matty Mo (hook/intro/outro) + Craig Clemens (direct, money-logic middle).
Every tweet <= 280 characters. Twitter/X format. The '#' lines are MY directions to you, NOT tweets.

---

## HOOK / THE FRAME

**T1**
I found 33 stores with the same fake address, same desk emails, same phone lines, same checkout code — and ONE wallet routing every payment.

They're not 33 stores. They're 1 operator printing storefronts.

The receipts are in this thread. 🧵

# SHOW: lusishop.com homepage scrolled to footer — "511 Mondell St, Thermopolis, WY 82443" + hotline visible.
# LINK: https://lusishop.com

**T2** (YouTube origin)
Blame YouTube.

The whole machine runs on dropshipper dreams sold in "make money online" videos — the same genre that sold "100,000 ad accounts" (SMIT) and "20,000 sellers" (BettaMax).

The stores? That dream, industrialized.

# SHOW: YouTube search results screenshot for the store brands / the product names (wind spinners); also the SMIT & BettaMax tutorial-style pages = the seller-recruitment funnel.
# LINK: https://smit.vn / https://bettamax.com

---

## PART 1 — THE FINGERPRINT (impossible to fake)

**T3**
They never changed the address. Not once.

33 domains. 1 footer:

"511 Mondell St, Thermopolis, WY 82443"

Not 33 businesses. 33 fonts on one face.

# SHOW: side-by-side footers from 2-3 different stores (lusishop + urbanstepusa + farley-usa), same text highlighted.
# LINKS: https://urbanstepusa.com https://farley-usa.com

**T4**
Same 2 phone numbers on every contact page:

+1 (501) 242 0006
+1 (404) 857 3486

One desk answers every "brand."

# SHOW: contact pages of two different stores showing identical hotlines.
# LINK: pick any two stores from the manifest.

**T5**
And just 4 support emails for 33 "brands":

orderlookup.live · orderfollow.live · trackorderzone.com · trackingspaces.com

Every ticket from every "shop" lands in one inbox.

# SHOW: footer email rows on 2-3 stores.

**T6**
The checkout is the smoking gun.

Every store loads ONE payment bus ("latte") and flickers between the SAME 9 merchant accounts per order — PayPal, Stripe, Adyen, Square, Airwallex, Payoneer, DiandianPay, Authorize.net, PayPal-Pro.

Volume distributed. Merchant unchanged.

# SHOW: page-source of any store, search "paymentId" — the full 9-entry payments array highlighted.
# Also SHOW the v6/v8 capture endpoints in the checkout JS: /api/stores/public/payments/v6/capture-order + v8/create-order.

**T7**
Same compiled code. Same bytes.

62 of 86 JS chunks identical across stores; the same chunk ships on 18 domains.

No independent business sends byte-identical javascript.

# SHOW: two browser tabs on different stores + the identical chunk filename (2422-b095799e072a04ad.js) in both Network panels.

**T8**
Billing keys are reused across brands:

mavon · velo · farley → 1 ownerId
brightenly + nuvaza → 1 ownerId
iveronshopus + urbanstep → 1 ownerId
zoticgear + techsparqus → 1 ownerId

One operator. Four seats. 9 masks.

# SHOW: owner_matrix3.json table or a drawn cluster diagram of ownerId → domains.

---

## PART 2 — THE BRAIN (one control plane)

**T9**
Every store beams a session telemetry event to ONE AWS account:

co5a2w1yba.execute-api.us-east-2.amazonaws.com/production/ad9d2b91

us-east-2 — the same region as the store servers. One brain, 33 bodies.

# SHOW: find "co5a2w1yba" (ctrl+F) in the shared JS chunk 3710-*.js; + a screenshot of the 403 Forbidden on GET (endpoint is POST-only=live).

**T10**
They even shipped a paid ip-api Pro key to every visitor's browser:

key=q7KkbUtfMHcdquS

They geo-score every shopper to dodge risk. The key is a public leak on their own checkout.

# SHOW: the pro.ip-api.com string in the JS bundle, key highlighted.
# LINK: https://pro.ip-api.com/json/?fields=61439&key=q7KkbUtfMHcdquS (paste in a browser = proof of the live paid key)

**T11**
The ad closet is the flex:

477 Google Ads customer accounts wired into just 7 stores.
urbanstepusa alone? 144.

That's not marketing. That's a burner account farm.

# SHOW: the googleAdsConversionIds array in urbanstepusa page source (144 AW- IDs), scrolled so multiple AW- numbers are visible.

**T12**
And the identity that can't die:

the same Meta pixels are shared across dozens of stores.

Kill an ad account, the pixel memory survives. That's the asset they treasure.

# SHOW: page-source search "fbq" / "1657324268639685" on 2+ stores showing the same pixel ID.

---

## PART 3 — THE SHELL COMPANY (who to name)

**T13**
The engine is a real product:

Droptitan — built & published by TRYDROPHUB PTE. LTD. (Singapore). Same Apple developer account also ships Hmaxx, Vnecomy, Drixx WinWin.

One company. Four engine brands. All feeding the same farm.

# SHOW: Apple App Store page for Droptitan (seller: TRYDROPHUB PTE. LTD.).
# LINK: https://apps.apple.com/us/app/droptitan/id6746070773

**T14**
Here's the split that matters:

engine = corporate (TRYDROPHUB PTE. LTD.)
seller app = an Apple developer account named "tran hoang Hoang Hiep"

Corporate shell up front. Person behind the checkout.

# SHOW: Hmaxx V2 + Drix Win App Store pages (artistName = tran hoang Hoang Hiep).
# LINKS: https://apps.apple.com/us/app/hmaxx-v2/id6749109695
# LINKS: https://apps.apple.com/us/app/drix-win/id6751709185

**T15**
The back office is PUBLIC:

admin.droptitan.io

deposits · payout · proxy rotation · assign-payment · buy-domain · approve-to-store

The dashboard you fantasized about is one login page away.

# SHOW: admin.droptitan.io loading 200; + grep of /js/app.ccc5bdff.js showing the endpoint list.
# LINK: https://admin.droptitan.io

**T16**
They built the full anti-enforcement stack:

per-store proxy groups, rotating CDN brand fronts (xgglecdn · proedgecache · cachetigeredge · cachebearedge) all under storedfilezone.

Ghos-host the front, ghost-host the cache.

# SHOW: crt.sh for %storedfilezone.com — highlight the repeated cdn.* front-wildcard reissues.
# LINK: https://crt.sh/?q=%25.storedfilezone.com

**T17**
And they keep BUYING bodies:

9 fresh domains registered in 2026 alone.
Newest: 2026-08-17.

This farm is alive, hiring, and rotating.

# SHOW: RDAP window for zebinshop.com (created 2026-08-17, registrar NameSilo) + the manifest table.
# LINK: https://crt.sh/?q=%25.droptitan.io (also shows platform alive since 2025-03)

---

## PART 4 — THE MONEY (Clemens-style, direct)

**T18**
The money movers:

PayPal ACDC · Airwallex · Payoneer · DiandianPay · Adyen · Stripe · Square
+ BettaMax (payouts to Vietnam)
+ AlphaX (stablecoin USDC/USDT rail)

9 processors + a crypto frame. Speed after the burn.

# SHOW: BettaMax TOS/payout page + AlphaX landing (alphax.asia) side by side.
# LINKS: https://bettamax.com  https://alphax.asia  https://docs.alphax.asia/payments/sdk.md

**T19**
The nastiest line in the config:

"automaticFulfillEvenHighRiskFraud": true

They ship orders the banks flagged high-risk. On purpose. Chargeback losses are just a cost line.

# SHOW: the config JSON snippet with that toggle + automaticFulfillLineItem highlighted.

**T20**
Run the cash (estimate, not a P&L):

$36 "70%-off" gadgets, landed ~$25, CAC≈$0 (ad cards aren't theirs).
200+ live ad accounts → $500k–$1.5M+/mo gross, $200k–$600k+/mo net.

The "penthouses and supercars" math works.

# SHOW: a simple model table you make yourself (orders/day × AOV = gross column).

---

## PART 5 — THE ASK (actions, Clemens close)

**T21**
What YOU can do:

report to Meta/Google Ads risk, Apple, and every processor on that list (Stripe, PayPal, Adyen, Airwallex, Square, Authorize.net, Payoneer, DiandianPay) with the receipts.

One merchant group. One operator. One rail.

# SHOW: the 9-gateway array screenshot again + the manifest table. This is your evidence pack.

**T22** (Matty Mo outro)
33 masks. One jack-o'-lantern.

Every flag was written on their own checkout page.

Retweet so the person building store #34 knows we're watching. 👁

# No visual needed — this is the closer. Pin it after T21.

---

## MASTER SCREENSHOT CHECKLIST (capture order)

1. lusishop.com footer — Mondell address + hotline (T3)
2. Two more store footers, side-by-side (T3/T4)
3. Contact pages of 2 stores — identical hotlines (T4)
4. Footer email rows on 2-3 stores (T5)
5. Page-source: "paymentId" → 9-gateway array (T6)
6. Checkout JS: v6/v8 capture-order endpoints (T6)
7. Two tabs + identical chunk 2422-b095799e072a04ad.js in Network (T7)
8. ownerId cluster table (T8)
9. "co5a2w1yba" in chunk 3710-*.js + 403 proof (T9)
10. pro.ip-api.com key string highlighted (T10)
11. urbanstepusa googleAdsConversionIds — 144 IDs (T11)
12. Shared Meta pixel ID on 2 stores (T12)
13. Apple: Droptitan page (TRYDROPHUB PTE. LTD.) (T13)
14. Apple: Hmaxx V2 + Drix Win (tran hoang Hoang Hiep) (T14)
15. admin.droptitan.io + endpoint grep (T15)
16. crt.sh storedfilezone CDN churn (T16)
17. RDAP zebinshop 2026-08-17 + crt.sh droptitan (T17)
18. BettaMax payout page + AlphaX landing (T18)
19. automaticFulfillEvenHighRiskFraud:true config (T19)
20. Your revenue model table (T20)

## MASTER LINK SET
https://lusishop.com · https://urbanstepusa.com · https://farley-usa.com
https://admin.droptitan.io
https://apps.apple.com/us/app/droptitan/id6746070773
https://apps.apple.com/us/app/hmaxx-v2/id6749109695
https://apps.apple.com/us/app/drix-win/id6751709185
https://crt.sh/?q=%25.storedfilezone.com · https://crt.sh/?q=%25.droptitan.io
https://smit.vn · https://bettamax.com · https://alphax.asia · https://docs.alphax.asia/payments/sdk.md
https://pro.ip-api.com/json/?fields=61439&key=q7KkbUtfMHcdquS
App Store lookup proof (returns JSON, citable): https://itunes.apple.com/lookup?id=6746070773