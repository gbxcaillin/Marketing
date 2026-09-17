# Execution Systems

The playbook, gap analysis, and training supplement are *knowledge* deliverables. This reference turns that knowledge into *operating systems*: repeatable, templated workflows a company runs every time they take a product or offer to market.

The model is Zeely's pipeline (an AI marketing app for small businesses): paste a product link, generate creatives in batch, get copy variants, launch a Meta campaign from one screen with a calculated budget, pass a compliance check, sell from a generated sales page, and manage from a seven-metric dashboard. The whole loop runs in minutes with almost no decisions left to the operator. That compression is the point: every decision the operator would otherwise have to make is pre-decided by a system.

Zeely's published weaknesses are also instructive. Reviewers flag no competitor research, generic copy with weak brand voice, Meta-only channels, and no automated optimisation rules. Systems 1, 3, and 8 below exist to close those gaps, so the company gets Zeely's speed without Zeely's blind spots.

Build one system per section below. Each system is a one-page SOP with: **trigger** (when it runs), **inputs**, **steps**, **decision rules** (pre-made choices so the operator doesn't deliberate), **outputs**, and **owner**. Fill each SOP with the company's own frameworks from the playbook wherever one exists, and with the training supplement's frameworks where it doesn't.

## Connector map

Before writing the tool hooks, check which of these connectors are live in the session (`ListConnectors`, and `COMPOSIO_SEARCH_TOOLS` for apps bridged through Composio). Name the live ones in each system's hook; write the manual fallback for the rest. A connector that is installed but not authenticated counts as absent.

| Connector | What it gives the pipeline | Systems |
|---|---|---|
| **AdWhispr** | Verified competitor discovery, live ads by longevity, brief generation, Meta/Google/TikTok launch and management. Needs `connect_ad_account` before launch or performance tools work. | 1, 5, 8 |
| **Shopify** | Real product data (title, images, price, variants) instead of a scraped URL; the product page as the sales page; orders and customers for post-sale; analytics for blended ROAS. | 2, 7, 8, post-sale |
| **Canva** | Brand kits (`list-brand-kits`), on-brand static generation (`generate-design`), the three-size step (`resize-design`), export, and any existing ad assets to remix (`search-designs`). | 3 |
| **OpenArt** | Image and video generation for concepts Canva can't produce: image-to-video, avatar-style clips. | 3 |
| **Composio → HighLevel** | The CRM the summit runs on: contacts, pipelines, opportunities, tasks. Lead tracker source of truth, booking and nurture triggers. Needs an auth config set up once in the Composio dashboard. | 7, 8, nurture |
| **Composio → Meta Ads** | Account and campaign insights (`METAADS_GET_INSIGHTS`) for the Monday pull; ad and campaign objects for management. | 5, 8 |
| **Composio → Google Ads** | Campaign lookup, customer lists for brand and search campaigns. | 5 |
| **Composio → Google Sheets** | The three-tab dashboard and lead tracker: append rows, read ranges, upsert. | 8 |
| **Composio → SendGrid / Brevo** | Sends, lists, sender identities for nurture and deliverability checks. | nurture, email |
| **Composio → Firecrawl** | Product-link scrape for clients not on Shopify. | 2 |
| **Google Drive, Dropbox, SharePoint (Microsoft 365)** | Where client collateral lives for Phase 1, and where to store the four deliverables. SharePoint often holds client compliance standards that the Compliance Gate must absorb (e.g. an AFSL licensee's marketing standard for a financial-services client). | Phase 1, 6 |
| **Cloudflare / Render** | Hosting for a generated sales page or a lead-tracker database when a spreadsheet is outgrown. | 7, 8 |
| **Composio → Microsoft Clarity** | Session recordings, heatmaps, scroll depth via data export. The summit's named tool for qualitative page feedback. | 7, 8 |
| **Composio → Instagram / TikTok** | Organic publishing (`INSTAGRAM_POST_IG_USER_MEDIA_PUBLISH`, `TIKTOK_PUBLISH_VIDEO`) for the Organic Track. | 9 |
| **Composio → HeyGen** | Avatar-led UGC video, Zeely's signature format. Use for talking-head concepts OpenArt can't produce. | 3 |
| **Composio → Google Analytics** | Landing page and conversion reports from the source rather than the ad platform. | 8 |
| **Composio → Klaviyo** | Email and SMS flows and lists for Shopify clients. Nurture and post-sale automation. | nurture, post-sale |
| **Composio → TikTok Ads** | Campaign creation and management. Backup to AdWhispr for TikTok. | 5 |
| **Composio → Slack** | Monday performance summary and creative-refresh requests to the team. | 8 |
| **Motion Creative Analytics** (registry) | Creative-level Meta insights and competitor ad libraries: which hooks fatigue, which formats hold. | 1, 3, 8 |
| **Semrush** (registry) | Keyword research, competitor domains and PPC data for non-brand Google Search. | 1, 5 |
| **Airtable / Notion** (registry or Composio) | A database for the Creative Library and change log with status fields, once a spreadsheet is outgrown. | 3, 8 |

Connectors with no role here (market data, travel, crypto) are simply not mentioned in the hooks.

---

## System 1: Competitor Intelligence

*Fills Zeely's biggest gap. Runs before any creative work so the company builds from proven winners instead of guesses.*

- **Trigger:** New product, new offer, new market, or quarterly refresh.
- **Inputs:** The company's product URL or one-paragraph description; 3-5 known competitor names (optional).
- **Steps:**
  1. Identify competitors that are *actively advertising right now*. Verified advertisers only; a brand that isn't running ads teaches nothing about ads.
  2. Pull each competitor's active ads, sorted by longevity. An ad running 60+ days is paying for itself.
  3. Log each long-running ad: hook type, format (static / UGC video / avatar / carousel), offer, CTA, landing page type.
  4. Extract the top 3 recurring patterns across competitors. These become the creative brief's "proven angles."
  5. Note the angles nobody is using. These become the "differentiation" angles.
- **Decision rules:** Only ads with 30+ days of runtime count as evidence. Minimum 3 verified competitors before drawing patterns. If fewer than 3 advertise, widen to adjacent niches.
- **Outputs:** Competitor swipe file (table), pattern summary, 3 proven angles + 2 differentiation angles.
- **Tool hook:** When the AdWhispr connector is available, use `save_my_brand`, `find_competitors`, `get_brand_ads` (sortBy: longevity), and `generate_brief`. Otherwise use the Meta Ad Library manually and record the same fields.

## System 2: Product Intake

*Zeely's "Generate from Link." One structured brief that every downstream system reads from.*

- **Trigger:** Any new product, offer, or campaign.
- **Inputs:** A product or offer URL, or manual entry.
- **Steps:**
  1. Extract: product name, one-line description, price, 3-6 images, primary CTA.
  2. Attach the avatar: which customer avatar from the playbook is this for, and at what awareness level.
  3. Attach the offer stack: core outcome, vehicle, bonuses, guarantee, price anchor (from the offer creation module if the company has no offer framework).
  4. State the single conversion goal: traffic, leads, or purchase. One goal per brief.
  5. Record the three proven angles and two differentiation angles from System 1.
- **Decision rules:** One product per brief. If the URL has multiple products, make multiple briefs. If no price is present, the brief is for lead generation, not sales.
- **Outputs:** A one-page Product Brief (template below). Everything downstream references it by name.
- **Tool hook:** With Shopify, `get-product` / `search_products` pull the real product record and `get-shop-info` gives currency and market. Otherwise Composio's `FIRECRAWL_EXTRACT` on the URL, or manual entry.

```
PRODUCT BRIEF — [name]
Goal: traffic | leads | purchase
Avatar: [name] at [unaware/problem/solution/product/most aware]
Outcome (before → after):
Vehicle:
Price / anchor:
Guarantee:
CTA:
Proven angles: 1. 2. 3.
Differentiation angles: 1. 2.
Images: [links]
Compliance flags: [health / finance / income / personal attributes / none]
```

## System 3: Creative Factory

*Zeely's Creative Builder, Batch Mode, and Brand-Inspired Remix, with the company's own frameworks driving the concepts so the output isn't generic.*

- **Trigger:** A completed Product Brief.
- **Inputs:** Product Brief; the company's creative frameworks from the playbook (hook formulas, video script structures, testimonial formats).
- **Steps:**
  1. **Concept sheet.** From the 5 angles in the brief, write 10 concepts: each is one hook + one format. Spread across at least 3 formats (static, UGC-style video, avatar/talking-head, carousel, testimonial).
  2. **Batch statics.** Produce statics in a run of 10-20 per batch, each in three sizes: feed (1:1), story/reel (9:16), and landscape (1.91:1). Use the company's brand kit (fonts, colours, logo lockup) on every asset.
  3. **Remix.** Take 2-3 of the long-running competitor ads from System 1 and produce a variant "in the spirit of" each, with the company's product and voice.
  4. **Video scripts.** Every video follows Hook → Problem → Solution → CTA. Hook in the first 3 seconds. 15-30 seconds for cold, 30-60 for warm. Use the company's own script framework if the playbook has one.
  5. **Voice pass.** Rewrite every headline and script line in the company's brand voice using their tone rules and banned-word list. This step is what separates the output from Zeely's "generic copy."
- **Decision rules:** Minimum 5 creatives per ad set at launch (algorithms reward diversity). No more than 2 creatives sharing the same hook. Every batch includes at least one testimonial-format piece and one direct-offer piece.
- **Outputs:** Creative Library entry per asset: concept, format, hook type, angle, sizes, status (draft / approved / live / retired).
- **Tool hook:** With Canva, `list-brand-kits` for the brand guard, `generate-design` (with the brand kit) for each static concept, `resize-design` for the feed/story/landscape set, `search-designs` to find existing ad assets to remix, `export-design` for delivery. With OpenArt, `openart_generate_image` and `openart_generate_video` for image-to-video and avatar-style clips. Without either: Canva desktop and CapCut by hand.

## System 4: Copy Variants

*Zeely generates captions, headlines, and primary text in sets. This system does the same but from the company's copy frameworks.*

- **Trigger:** Approved creatives from System 3.
- **Inputs:** Product Brief; the company's copywriting frameworks; platform specs.
- **Steps:**
  1. Write 5 headlines (mix of how-to, question, number, testimonial, and direct).
  2. Write 3 primary text variants: short (under 125 characters), medium (3-4 lines with a line break after the hook), long (story-led, 150+ words).
  3. Write 3 CTA variants matched to the goal (traffic: "See how", leads: "Get the guide", purchase: "Shop now").
  4. For Google, write 15 RSA headlines (30 chars) and 4 descriptions (90 chars) from the same angles.
  5. Run the compliance pass (System 6) on every line before it enters the library.
- **Decision rules:** Never ship a single copy variant. Pair each creative with at least 2 primary texts. The Rule of One: one idea, one reader, one CTA per ad.
- **Outputs:** Copy set attached to each Creative Library entry.

## System 5: Campaign Launch

*Zeely's one-screen launch: goal, location, age/gender, budget. Zeely calculates budget and duration for the user. This system pre-decides everything else too.*

- **Trigger:** Creative Library has 5+ approved assets with copy for one Product Brief.
- **Inputs:** Product Brief; unit economics (max CPL / target CPA / breakeven ROAS from the budget module); audience tiers from the company's targeting framework.
- **Steps:**
  1. **Objective** comes from the brief's goal. Never change it mid-campaign.
  2. **Budget calculator.** Daily budget = (leads or sales needed per day) × (max allowable CPL or target CPA). Testing floor: enough spend to reach roughly 50 conversion events per week per ad set. Duration: 7 days minimum before any judgement.
  3. **Structure.** One campaign per objective. Ad sets split by audience temperature: cold (lookalike + interest), warm (engagers, video viewers), hot (site visitors, leads). Apply the company's naming convention.
  4. **Placement and demographics.** Automatic placements by default. Age and gender from the avatar. Location from the brief.
  5. **Pre-launch checklist.** Pixel firing, landing page loads on mobile in under 3 seconds, every link tested, compliance pass complete, UTM parameters set.
  6. Submit to platform review. Log the campaign in the tracker with launch date, budget, and hypothesis ("we expect CPL under $X from the pain-hook angle").
- **Decision rules:** Don't touch a new campaign for 72 hours. Scale a winner by no more than 20% every 3-5 days. Kill an ad set at 2× target CPA with zero conversions after 3 days. Duplicate rather than edit when a change is structural.
- **Outputs:** Live campaign, tracker row, stated hypothesis.
- **Tool hook:** With AdWhispr, `launch_meta_ad` / `launch_campaign` for Meta, `launch_search_campaign` or `launch_pmax_campaign` for Google, `launch_tiktok_campaign` for TikTok, and `update_budget`, `pause_campaign`, `resume_campaign` for management. This is where the company gets beyond Zeely's Meta-only limit. With Composio's Meta Ads and Google Ads toolkits, `METAADS_GET_AD_ACCOUNTS` / `METAADS_LIST_ADS` and `GOOGLEADS_GET_CAMPAIGN_BY_NAME` / `GOOGLEADS_CREATE_CUSTOMER_LIST` cover account checks and customer-list uploads for the data-hierarchy audiences.

## System 6: Compliance Gate

*Zeely reviews every campaign against Meta's guidelines before it goes live and checks that links work. Make that a gate every asset passes through, not a favour from support.*

- **Trigger:** Any creative or copy moving to "approved."
- **Inputs:** The asset; the ad policy module's five rejection triggers.
- **Steps:**
  1. Scan for personal-attribute callouts ("you" + a personal problem).
  2. Scan for before/after implications in images and copy.
  3. Scan for income, earnings, or specific-result claims without substantiation.
  4. Scan for sensational or fear-based phrasing.
  5. Confirm the landing page delivers exactly what the ad promises.
  6. Check category restrictions (health, finance, housing, employment, alcohol).
- **Decision rules:** Any flag sends the asset back to System 3 or 4 with the flagged line and a compliant rewrite suggested. Nothing goes live with an open flag. A rejected ad gets a manual review request before any edit.
- **Outputs:** Pass/fail with flagged lines and rewrites; account health log.
- **Client-specific layer:** Regulated clients carry their own marketing standards on top of platform policy. Search the client's SharePoint, Drive or Dropbox for anything titled "marketing standard", "compliance", "disclosure" or "licensee" during Phase 1 and fold its rules into steps 1–6 as extra checks. A financial-services client under an AFSL, for example, needs the general-advice warning and disclosure page on every landing page.

## System 7: Sales Page Generator

*Zeely builds an AI sales page with Stripe/PayPal for businesses with no store. This system produces a page spec from the Product Brief and the company's landing page frameworks.*

- **Trigger:** A Product Brief whose goal is purchase or leads and no matching page exists.
- **Inputs:** Product Brief; landing page layout and copy frameworks from the playbook or supplement.
- **Steps:**
  1. Above the fold: headline (the core outcome), subhead (the vehicle), one hero image or video, primary CTA, one trust element.
  2. Body in the company's page structure (or PASTOR: Problem, Amplify, Story, Transformation, Offer, Response).
  3. Offer stack block with line-item values, total value, price, guarantee directly below.
  4. Social proof block using the company's testimonial format.
  5. FAQ block answering the top 5 objections from the avatar.
  6. Repeat CTA every screen height. Form fields: the minimum the sales process needs.
  7. Payment or booking: Stripe/PayPal for purchases, calendar embed for calls, with the confirmation stack from the show-rate system wired up.
- **Decision rules:** One page per Product Brief. One CTA type per page. Mobile layout is designed first. Page speed under 3 seconds or it doesn't launch.
- **Outputs:** Page spec (section-by-section copy and layout), ready for a page builder or developer.
- **Tool hook:** With Shopify, the product page is the sales page: `update-product` for copy and images, `create-discount` for the offer, `create-collection` to split pages by audience. With Composio's HighLevel toolkit, the booking funnel, calendar and confirmation workflow live there (`HIGHLEVEL_CREATE_OPPORTUNITY`, `HIGHLEVEL_UPSERT_CONTACT`). With Cloudflare or Render, host a static page from the spec. Otherwise GHL or the client's page builder by hand.

## System 8: Performance Loop

*Zeely tracks seven metrics and tells users to check daily for three days, then two or three times a week, sort by ROAS, and act on one or two suggestions. This system keeps that cadence and adds the decision rules Zeely leaves out.*

- **Trigger:** Any live campaign.
- **Inputs:** Platform metrics; CRM outcomes; unit economics targets.
- **The seven metrics, and what each one diagnoses:**

| Metric | Diagnoses | Act when |
|---|---|---|
| Impressions | Delivery / audience size | Under budget pace for 2 days |
| CTR | Creative appeal | Below 1% cold, 1.5% warm |
| CPC | Audience efficiency | 50% above account average |
| Hook rate (3s views ÷ impressions) | Video opening | Below 25% |
| Conversions / CPL / CPA | Page + offer | CPA above 2× target for 3 days |
| ROAS | Revenue impact | Below breakeven for 7 days |
| Frequency | Fatigue | Above 3 on cold audiences |

- **Cadence:** Days 1-3 after launch: one check per day, no changes. Days 4-7: one check, act only on the "kill" rules. Week 2 onward: two or three checks per week. Every Monday: pull the week into the dashboard, flag any metric that moved more than 20%, decide, execute.
- **Decision rules:** Sort ads by ROAS (or CPL for lead gen) and make at most two changes per session. Diagnose top-down: if CTR is fine and CPA is bad, the problem is the page or offer, not the ad. Refresh creative when frequency passes 3 or CTR decays 30% from its peak. Feed closed-won data back to the platform weekly so optimisation targets quality, not volume.
- **Outputs:** Weekly dashboard row, change log with hypothesis and result, creative refresh requests to System 3.
- **Tool hook:** With AdWhispr, `get_account_performance` and `list_campaigns` for the pull; `update_budget` and `pause_campaign` for the actions. With Composio: `METAADS_GET_INSIGHTS` at campaign level for the Monday pull (one of `date_preset` or `time_range`, never both; KPI values arrive as strings and `actions` as arrays to normalise); `HIGHLEVEL_SEARCH_OPPORTUNITIES` for the tracker's booking → show → close columns; `GOOGLESHEETS_SPREADSHEETS_VALUES_APPEND` / `GOOGLESHEETS_VALUES_GET` to write and read the three tabs. With Shopify, `run-analytics-query` or `list-orders` for the revenue side of blended ROAS. Without them: platform exports into the sheet by hand.

## System 9: Organic Distribution

*Zeely's "Viral campaigns" schedule AI videos as Reels. Every paid creative gets a second life organically, and organic performance pre-tests hooks for paid.*

- **Trigger:** Every video approved in System 3.
- **Steps:**
  1. Post each video organically (Reels, TikTok, Shorts) 3-5 days before or alongside its paid run.
  2. Log 24-hour organic metrics: views, watch-through, saves, shares.
  3. Any organic post in the top 20% for watch-through moves to the front of the paid queue.
  4. Build engager audiences from organic viewers for warm retargeting.
- **Decision rules:** Three organic posts per week minimum. Never post a paid-only creative organically without removing the offer-heavy CTA.
- **Outputs:** Organic calendar, organic-to-paid promotion list.

---

## Mapping systems to the 24-skill framework

| System | Skills it operationalises |
|---|---|
| 1 Competitor Intelligence | Creative Diversity, Neuromarketing, Offer Positioning |
| 2 Product Intake | Customer Avatar, Offer Creation |
| 3 Creative Factory | Ad Content Creation, Creative Diversity, Video & Image Production |
| 4 Copy Variants | Copywriting, Landing Page Copy |
| 5 Campaign Launch | Campaign Structure, Ad Targeting, Google Search, Budget & Unit Economics |
| 6 Compliance Gate | Ad Policy & Compliance |
| 7 Sales Page Generator | Conversion Funnels, Landing Page Optimisation, Landing Page Copy, Offer Creation |
| 8 Performance Loop | Reporting & KPIs, Lead Quality Tracking, A/B Testing, Retargeting |
| 9 Organic Distribution | Ad Content Creation, Retargeting |

Nurture Sequences, Show Rates, CRM, Email Deliverability, and Post-Sale are downstream of these systems and are operationalised by the training supplement's exercises directly (each of those modules ends in a build task).

## Building the Execution Systems artifact

Produce one HTML artifact, `execution-systems.html`, structured as an operating manual:

1. A one-screen pipeline diagram: Competitor Intel → Intake → Creative Factory → Copy → Compliance → Launch → Sales Page → Performance Loop, with Organic Distribution as a parallel track.
2. One section per system, each as the SOP block (trigger, inputs, steps, decision rules, outputs, owner), filled with the company's own frameworks by name.
3. The Product Brief template and the seven-metric table as copy-ready blocks.
4. A "tool hooks" note per system: which of the live connectors from the connector map performs the step, and the manual equivalent if none is live.
5. A closing "first 30 days" rollout: which system to stand up each week.

Design it as a manual people operate from, not a document they read once: dense, scannable, with every decision rule visually distinct from the explanatory text. Load `artifact-design` first, and give it a fourth distinct identity so the four deliverables read as a set.
