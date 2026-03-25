---
title: "First-Party Data: Complete PPC Specialist Guide"
version: "1.1"
updated: "2026-03-24"
audience: "PPC specialists"

tracks:
  platform:
    - id: google
      label: "Google Ads"
    - id: meta
      label: "Meta Ads"
  business_model:
    - id: ecommerce
      label: "E-commerce"
    - id: leadgen
      label: "Lead Generation"

modules:
  - id: foundations
    title: "Foundations"
    order: 1
    tracks: [google, meta]
    business_models: [ecommerce, leadgen]
  - id: google_implementation
    title: "Google Ads Implementation"
    order: 2
    tracks: [google]
    business_models: [ecommerce, leadgen]
  - id: meta_implementation
    title: "Meta Ads Implementation"
    order: 3
    tracks: [meta]
    business_models: [ecommerce, leadgen]
  - id: measurement
    title: "Measurement & Incrementality"
    order: 4
    tracks: [google, meta]
    business_models: [ecommerce, leadgen]
  - id: best_practices
    title: "Best Practices & Strategy"
    order: 5
    tracks: [google, meta]
    business_models: [ecommerce, leadgen]

quick_reference_sections:
  - data_taxonomy_table
  - match_rate_benchmarks
  - crm_schema
  - emq_scoring
  - lookalike_reference
  - google_campaign_architecture
  - google_file_format
  - kpi_evolution
  - meta_custom_audience_requirements
  - strategy_decision_matrix
  - audience_sizing_reference
  - targeting_mode_guide

checklist_sections:
  - data_hygiene_checklist
  - google_postlaunch_checklist
  - meta_postlaunch_checklist
  - google_apex_roadmap
  - meta_apex_roadmap

apex_sections:
  - google_apex_roadmap
  - meta_apex_roadmap
---

# First-Party Data: Complete PPC Specialist Guide

Signal loss is not a future risk — it is the current operating environment. Apple's App Tracking Transparency wiped visibility on ~65% of iOS ad interactions. Cookie restrictions affect 50–60% of internet users. GDPR and CCPA now mandate consent as a prerequisite for data collection. The competitive advantage in 2026 belongs to advertisers who have built a robust first-party data infrastructure — not those who are still planning to.

> ⚠️ BCG research shows companies using first-party data effectively achieve up to **2.9x higher revenue** from single customer interactions. McKinsey estimates that brands failing to adapt to signal loss will see a 10–15% decline in addressable reach within 24 months.

This guide covers everything a PPC specialist needs: foundations, platform-specific implementation (Google Ads + Meta), and measurement — with actionable checklists and a progress tracker for reaching full first-party data potential.

---

<!-- module: foundations -->

## Module 1: Foundations

<!-- section: data_taxonomy_table | quick_reference: true -->

### 1.1 The Data Landscape

| Type | Source | Examples | Control Level | Privacy Risk | PPC Viability |
|---|---|---|---|---|---|
| **Zero-party** | Voluntarily shared by user | Quiz answers, preference centers, surveys | Full | None | High — explicit intent |
| **First-party** | Collected from your own properties | CRM, purchase history, website behavior, app data | Full | Low | High — durable, consent-based |
| **Second-party** | Partner's first-party data shared directly | Retail media network data, data partnerships | Shared | Medium | Medium — quality varies |
| **Third-party** | Aggregated by data brokers | Interest segments, demographic overlays | None | High | Declining — regulatory & browser pressure |

> 🎯 First-party data is the only tier you fully control, that scales with your business, and that regulators will continue to permit. Build here.

---

<!-- section: why_fpd_matters -->

### 1.2 Why First-Party Data Matters Now

**Signal loss — mobile:** Apple's App Tracking Transparency (ATT) framework, introduced in iOS 14.5, requires opt-in consent for cross-app tracking. Industry opt-in rates hover at 25–35%, meaning ~65% of iOS user behavior is invisible to ad platforms. You are not losing signal in the future — it is already gone.

**Signal loss — web:** ITP (Safari), ETP (Firefox), and the eventual Chrome Privacy Sandbox changes affect 50–60% of internet users. Cookie-based audiences, attribution windows, and frequency capping are all degraded. Third-party cookie deprecation is an ongoing erosion, not a single event.

**Regulation:** GDPR (EU/EEA) and CCPA/CPRA (California, now joined by 15+ US states) require informed, explicit consent before collecting personal data for advertising. Legitimate interest is no longer a reliable legal basis for ad targeting in most EEA jurisdictions. Compliance is the floor, not the ceiling.

---

<!-- section: consent_mode_v2 -->

### 1.3 Google Consent Mode v2

Required for all advertisers serving EEA traffic. Without it, Customer Match and remarketing audiences cannot use user data from non-consenting users.

| Parameter | What It Controls | Impact When Denied |
|---|---|---|
| `analytics_storage` | GA4 measurement cookies | Analytics data gaps for that user |
| `ad_storage` | Ad platform measurement cookies | Attribution loss |
| `ad_user_data` | Use of personal data for advertising | **Blocks Customer Match from using that user** |
| `ad_personalization` | Remarketing and personalized ads | User excluded from all audience targeting |

> ⚠️ As of **July 21, 2025**, Google blocks data collection from sites without a valid Consent Mode v2 implementation for EEA traffic. Without `ad_user_data` consent, Google cannot use that user's data in Customer Match. Implement Advanced Consent Mode v2 (not Basic) to preserve cookieless measurement pings when consent is declined.

**Two implementation modes:**
- **Basic Consent Mode v2:** Tags blocked until consent given. Clean but causes measurement gaps.
- **Advanced Consent Mode v2:** Tags load in restricted state and send cookieless pings. Restore full functionality upon consent. Recommended for all advertisers.

---

<!-- section: crm_schema | quick_reference: true -->

### 1.4 CRM Minimum Viable Schema

The minimum fields required before a CRM list is useful for audience uploads. Anything less produces poor match rates or unusable segments.

<!-- track: ecommerce -->
**E-commerce:**

| Field | Required | Notes |
|---|---|---|
| `email` | Yes | Primary match key |
| `phone` | Recommended | Boosts match rate significantly |
| `first_name` | Recommended | Hash before upload |
| `last_name` | Recommended | Hash before upload |
| `zip` | Optional | Additive signal |
| `country` | Yes (if phone) | ISO 3166-1 alpha-2 |
| `lifetime_value` | Yes | Required for LTV bidding and RFM |
| `purchase_count` | Yes | Required for RFM frequency |
| `last_purchase_date` | Yes | Required for RFM recency |
| `product_category` | Recommended | Enables category-level segmentation |

<!-- track: leadgen -->
**Lead Generation:**

| Field | Required | Notes |
|---|---|---|
| `email` | Yes | Primary match key |
| `phone` | Recommended | Boosts match rate |
| `first_name` | Recommended | Hash before upload |
| `last_name` | Recommended | Hash before upload |
| `company` | Recommended | Required for B2B segmentation |
| `lead_source` | Yes | Attributes pipeline to channel |
| `lead_stage` | Yes | MQL/SQL/Opportunity/Closed |
| `lead_score` | Recommended | Enables score-based bidding |
| `converted` | Yes | Boolean — essential for suppression |
| `conversion_date` | Yes | Required for time-based segments |
| `gclid` | Yes | Required for Google Ads OCI |
| `fbclid` | Yes | Required for Meta attribution |

---

<!-- section: data_hygiene_checklist | checklist: true -->

### 1.5 Data Normalization and Hygiene

SHA-256 hashing is required before uploading to ad platforms via API. Platform UI uploaders hash on your behalf — do not pre-hash when using the UI. Pre-hash when calling any API directly (Google Data Manager API, Meta CAPI).

**Normalization rules:**
- Email: lowercase, strip whitespace, remove dots before `@gmail.com`
- Phone: remove all non-numeric characters, format to E.164 (`+12125551234`)
- Names: lowercase, strip accents/special characters
- Zip: correct format per country (5 digits US, alphanumeric UK, etc.)

**Pre-upload hygiene checklist:**

- [ ] Strip all leading/trailing whitespace from every field
- [ ] Lowercase all email addresses
- [ ] Remove all non-numeric characters from phone numbers
- [ ] Format all phone numbers to E.164 format (`+[country_code][number]`)
- [ ] SHA-256 hash: email, phone, first_name, last_name (API uploads only)
- [ ] Do NOT pre-hash when using platform UI upload tools
- [ ] Validate zip codes are correctly formatted per country
- [ ] Deduplicate rows on email (remove exact duplicates)
- [ ] Remove unsubscribed and opted-out contacts
- [ ] Verify country field uses ISO 3166-1 alpha-2 codes (`US`, `GB`, `DE`)
- [ ] Remove rows with missing email AND missing phone (unparseable)
- [ ] Validate no plaintext PII in any API payload

---

<!-- section: match_rate_benchmarks | quick_reference: true -->

### 1.6 Match Rate Benchmarks

| Platform | Identifier | Typical Match Rate | Notes |
|---|---|---|---|
| Google | Email (hashed) | 29–62% | Gmail addresses match significantly higher |
| Google | Phone | 20–45% | Lower outside the US |
| Google | Name + Zip | Additive | Used in combination, not standalone |
| Meta | Email | 60–80% | Strongest single identifier |
| Meta | Phone | 40–70% | Strong in mobile-heavy markets |
| Meta | Name + DOB + ZIP | Additive | Boosts EMQ when combined with email/phone |
| Meta | External ID | Additive | Required for CAPI deduplication |

> 🎯 If your Google match rate is below 29%, audit your email list quality. Gmail addresses match at significantly higher rates than other domains. Supplement with phone numbers for all US contacts.

> ⚠️ Match rate is a ceiling, not a guarantee. A 60% match rate on a 10,000-row list yields ~6,000 matched users. Minimum audience sizes for activation: Google Search/Shopping = 1,000 users; Google YouTube = 100 users; Meta targeting = 100 matched users.

---

<!-- section: segmentation_models -->

### 1.7 Segmentation Models

<!-- subsection: rfm_model | track: ecommerce -->
**RFM Model — E-commerce**

RFM scores each customer on three dimensions: Recency (days since last purchase), Frequency (number of orders), Monetary (total spend). High scores on all three = your best customers.

| Segment | RFM Profile | Audience Use |
|---|---|---|
| Champions | High R, High F, High M | Seed for Lookalike/LAL, seed for NCA goal, exclude from acquisition |
| Loyal Customers | Medium-High R, High F, Medium M | Retention campaigns, cross-sell, bid uplift |
| Big Spenders | Any R, Low F, High M | Win-back, upsell to frequency |
| At-Risk High Value | Low R, High F, High M | Win-back urgency campaigns, retention bidding |
| New Customers | High R, Low F, Low M | Nurture sequence, cross-sell |
| Promising | Medium R, Low F, Low M | Mid-funnel engagement |
| Hibernating | Low R, Low F, Any M | Suppression or re-engagement test |
| Lost | Very Low R, Very Low F, Low M | Suppression only |

<!-- subsection: funnel_stage_model | track: leadgen -->
**Funnel Stage Model — Lead Generation**

| Stage | Definition | Audience Use |
|---|---|---|
| Raw Lead | Form submitted, unqualified | Exclude from top-of-funnel; prevent re-acquisition |
| MQL | Marketing qualified by score/criteria | Mid-funnel nurture; observe on search |
| SQL | Sales accepted | Suppress from paid; hand off to sales |
| Opportunity | Active sales conversation | Suppress entirely; CRM-owned |
| Closed-Won | Became customer | Seed for Lookalike, upsell campaigns |
| Closed-Lost | Did not convert after SQL | Re-engagement campaigns OR LAL exclusion |

---

<!-- module: google_implementation | track: google -->

## Module 2: Google Ads Implementation

<!-- section: customer_match_eligibility -->

### 2.1 Customer Match — Eligibility and Rules

**Prerequisites:**
- Google Ads account with $50,000+ lifetime spend (required for full Customer Match features)
- Account must be in good standing (no active policy violations)
- Customer Match is available in: Search, Shopping, Display, YouTube, Gmail, Demand Gen, Performance Max

**Key rules that catch specialists off guard:**

> ⚠️ **540-day membership duration** is enforced as of April 7, 2025. Lists without a `membership_duration` field default to 540 days. You cannot set durations longer than 540 days. Users not refreshed within this window automatically age out. Plan your list refresh cadence and suppression logic around this hard ceiling.

> ⚠️ **Google Ads API deprecation for Customer Match — April 1, 2026.** All new Customer Match uploads must use the **Data Manager API** (GA December 9, 2025). Existing Google Ads API implementations lose access if the developer token hasn't touched a Customer Match upload since October 2025. Migrate now.

**Available campaigns:**

| Campaign Type | Customer Match Support |
|---|---|
| Search | Yes — RLSA bid modifiers |
| Shopping | Yes — NCA suppression, LTV bidding |
| Display | Yes — targeting and exclusions |
| YouTube / Demand Gen | Yes — remarketing and suppression |
| Performance Max | Yes — as audience signal (not hard targeting) |

---

<!-- section: google_file_format | quick_reference: true -->

### 2.2 File Format Specification

Required format for Customer Match CSV uploads.

| Column | Required | Format | Notes |
|---|---|---|---|
| `Email` | Recommended | Lowercase, SHA-256 hashed | Most impactful field |
| `Phone` | Optional | E.164 (`+12125551234`), SHA-256 hashed | Boosts match rate |
| `First Name` | Optional | Lowercase, SHA-256 hashed | Additive match signal |
| `Last Name` | Optional | Lowercase, SHA-256 hashed | Additive match signal |
| `Country` | Required if phone present | ISO 3166-1 alpha-2 (`US`, `GB`) | Unhashed |
| `Zip` | Optional | Country-appropriate format | Unhashed, additive |

> 🎯 The Data Manager API (recommended from April 2026) accepts multiple identifier types and handles hashing validation. Use it for all new implementations. The UI uploader hashes for you — never upload plaintext PII via any API.

---

<!-- section: data_manager_api -->

### 2.3 Google Ads Data Manager

Data Manager is Google's centralized first-party data hub, generally available as of December 9, 2025. It replaces the need to manage multiple API endpoints for different data types.

**What it handles:**
- Customer Match list uploads and scheduling
- Offline Conversion Import (OCI)
- Enhanced Conversions for Leads (ECL)
- PAIR data (Publisher Advertiser Identity Reconciliation)
- Mobile device ID matching

**Core concepts:**
- **Data Source:** Connected system (BigQuery, HubSpot, Salesforce, Amazon S3, sFTP, manual CSV)
- **Connection:** A data import created from a source
- **Destination:** Where data activates (Customer Match audience, Conversion action, etc.)

**Supported integrations:** BigQuery, Google Cloud Storage, Amazon S3, sFTP, HubSpot (native), Salesforce (native), Zapier

> 🎯 Treat Data Manager as the standard for all new first-party data workflows in Google Ads. It centralizes governance, eliminates duplicate integrations, and provides a point-and-click interface that doesn't require managing API credentials per workflow.

---

<!-- section: ga4_audience_publishing -->

### 2.4 GA4 Audience Publishing

How to configure GA4 audiences so they appear and activate in Google Ads.

1. Link GA4 property to Google Ads (GA4 Admin → Google Ads Links → enable "Personalized advertising")
2. In GA4 Admin → Data Settings → enable "Modeling contributions and Business insights"
3. Create audience in GA4 (Audiences section) with appropriate conditions
4. Set membership duration (max 540 days; align with your business cycle — 30d for cart abandonment, 180d for purchase suppression)
5. Toggle "Publish to linked Google Ads accounts"
6. In Google Ads, verify audience appears in Tools → Audience Manager → Sources shows GA4

> ⚠️ GA4 audiences take 24–48 hours to populate in Google Ads after initial publishing. Do not assume an audience is working until you verify membership counts in Audience Manager.

**GA4 Predictive Audiences (2025 update):**

Previously required 1,000+ purchase conversions in 90 days. Threshold now lowered to **200+ conversions**. Predictive audiences (Likely 7-day purchasers, Likely 7-day churners, Predicted 28-day top spenders) outperform manually defined lookalikes by 34–42% in conversion rate. Enable these before building manual audiences.

---

<!-- section: targeting_vs_observation -->

### 2.5 Targeting vs. Observation Mode

| Mode | What It Does | When to Use |
|---|---|---|
| **Targeting** | Restricts ad delivery exclusively to this audience | Remarketing, suppression, retention-only campaigns |
| **Observation** | Adds audience for bid adjustment and reporting without restricting reach | Any campaign where you want audience data without limiting scale |

> ⚠️ Start all new audiences in **Observation** mode on existing campaigns to gather performance data before committing to Targeting. Never launch a new campaign in Targeting mode with an untested audience — you may starve the campaign of impression volume and the algorithm will struggle to optimize.

Rule of thumb: use Targeting only when you explicitly want to exclude non-audience users (remarketing, win-back, customer-only campaigns). Use Observation for everything else during learning.

---

<!-- section: smart_bidding_compatibility -->

### 2.6 Smart Bidding and Customer Match Compatibility

> ⚠️ **Customer Match signals are only applied by Smart Bidding strategies.** If you are running Manual CPC or Enhanced CPC, Customer Match lists are attached but signals are completely ignored. Migrate to a Smart Bidding strategy before expecting Customer Match to influence performance.

Compatible Smart Bidding strategies:

| Strategy | Use Case |
|---|---|
| Target CPA | Lead gen with stable cost-per-lead targets |
| Target ROAS | E-commerce with known revenue targets |
| Maximize Conversions | Volume-first, budget-constrained campaigns |
| Maximize Conversion Value | Revenue-maximization with LTV signals |

Customer Match with Observation mode lets Smart Bidding automatically apply bid adjustments for matched users — without you needing to set manual bid modifiers.

---

<!-- section: ltv_conversion_value_rules | track: ecommerce -->

### 2.7 LTV-Based Conversion Value Rules

Value rules override the reported conversion value sent to Smart Bidding for specific audience members. Use this to teach Target ROAS that a Champions-segment customer is worth more than a first-time buyer.

**How it works:** When a matched Customer Match user converts, their conversion value is multiplied by your configured rule multiplier before Smart Bidding receives it. This shifts bidding toward higher-LTV prospects.

**Setup checklist:**

- [ ] Identify LTV multipliers for each audience segment (e.g., Champions = 2.5x, Loyal = 1.8x, New = 1.0x)
- [ ] Build Customer Match lists for each segment
- [ ] Navigate to: Tools → Conversions → Conversion Value Rules
- [ ] Create rules: condition = "Audience is [segment]" → multiply value by [multiplier]
- [ ] Apply rules to all relevant conversion actions (Purchase, Lead, etc.)
- [ ] Apply rules at campaign level (preferred) or account level
- [ ] Allow 2–4 weeks of Smart Bidding recalibration before evaluating impact

---

<!-- section: oci_lead_gen | track: leadgen -->

### 2.8 Offline Conversion Import (OCI)

OCI closes the loop between ad clicks and CRM outcomes. Essential for lead gen where the value event (MRR, deal closed) happens days or weeks after the form submission.

**How it works:** You capture `gclid` at form submission, store it in your CRM, then upload outcome data (SQL, Closed-Won) back to Google Ads so Smart Bidding optimizes toward real business outcomes — not just form fills.

> ⚠️ **`conversion_environment` field required from September 2025.** All OCI uploads must include this field (`WEB` or `APP`). Uploads without it will be rejected or attributed incorrectly.

> ⚠️ **`gclid` expires after 90 days.** An offline conversion upload for a click older than 90 days will be rejected. Ensure your sales cycle is shorter than 90 days, or implement Enhanced Conversions for Leads as a complement.

**Recommended upload method:** Google Ads **Data Manager** (replaces manual Google Ads API OCI workflows for new implementations).

**Setup steps:**
1. Add hidden `gclid` field to all lead capture forms (auto-populated via URL parameter)
2. Store `gclid` in your CRM alongside every lead record
3. In Google Ads: Tools → Data Manager → Create Conversion Import connection
4. Map CRM fields: `gclid`, `conversion_name`, `conversion_time`, `conversion_value`, `conversion_environment`
5. Configure upload schedule (daily recommended; within 24h of CRM stage change)
6. Verify imports: Conversions → Conversions table → "OCI" source shows non-zero counts within 3–5 days

**Conversion action hierarchy (high to low value):**

| Action | Signal Value | Upload Priority |
|---|---|---|
| Closed-Won / Customer | Highest | Upload same day |
| SQL (Sales Qualified Lead) | High | Upload within 24h |
| MQL (Marketing Qualified Lead) | Medium | Upload within 24h |
| Form Submit | Micro-conversion | Tag-based, no OCI needed |

---

<!-- section: ecl | track: leadgen -->

### 2.9 Enhanced Conversions for Leads (ECL)

ECL complements OCI. It captures hashed first-party data at form submission and stores it so Google can match a future conversion back to the original click — even when the `gclid` is absent or cookies are blocked.

**When to use:** Alongside OCI (not instead of). ECL covers attribution gaps when users clear cookies or switch devices between click and conversion.

> ⚠️ **Account-level migration:** Google began automatically upgrading Enhanced Conversions to account-level configuration from October 2025. If you haven't configured this manually, check your account — Google may have already applied settings.

**Setup checklist:**

- [ ] Enable Enhanced Conversions in Google Ads: Goals → Conversions → Enhanced conversions → Leads
- [ ] Implement ECL tag in Google Tag Manager (map form fields to ECL variables)
- [ ] Map: email → `email`, phone → `phone_number`, name fields → `first_name`/`last_name`
- [ ] Do NOT pre-hash for ECL — the tag handles hashing
- [ ] Test with Tag Assistant: verify `enhanced_conversion_data` payload is populated on form submit
- [ ] Confirm ECL is enabled on the same conversion action as OCI
- [ ] Monitor "Enhanced conversions" column in Google Ads reporting for coverage rate

---

<!-- section: nca_goal | track: ecommerce -->

### 2.10 New Customer Acquisition (NCA) Goal

NCA instructs Smart Bidding to prioritize bids for users not in your existing customer lists.

**Two modes:**
- **New customers only** (PMax, Shopping): Hard suppression — ads shown only to non-matched users
- **New customer value** (Search, Demand Gen): Bid multiplier — existing customers can still convert, but new customers receive higher bids

**Setup checklist:**

- [ ] Build an "All Existing Customers" Customer Match list from your full CRM email + phone export
- [ ] Set refresh cadence to weekly minimum (list must stay current for accurate suppression)
- [ ] In campaign settings: enable "New customer acquisition" goal
- [ ] Select mode: "New customers only" (PMax/Shopping) or "New customer value" (Search)
- [ ] Set new customer value uplift: start at 1.5x–2x average order value
- [ ] Attach the "All Existing Customers" list to the campaign
- [ ] After 2+ weeks: verify "New customer" segment in audience reporting
- [ ] Note: NCA for Demand Gen campaigns added in 2025 — check campaign type availability

---

<!-- section: performance_max_signals -->

### 2.11 Performance Max and First-Party Data Signals

PMax uses first-party data as a **signal** to guide Google's AI — not as hard targeting or exclusion (except for Customer Match used as NCA suppression list).

> 🎯 **Use 1–2 high-quality Customer Match lists as signals, not everything.** Google's 2025 guidance explicitly recommends providing your strongest signals rather than attaching all available audiences. Poor signal quality degrades PMax performance regardless of budget or creative.

**Recommended signal hierarchy:**
1. Champions / high-LTV customers (Customer Match)
2. Website purchasers / converters (GA4 audience)
3. (Optional) Cart abandoners if volume is sufficient

**What to avoid:**
- Attaching generic "all visitors" as a signal (too noisy)
- Using PMax as a remarketing-only vehicle (it needs prospecting volume)
- Assuming first-party signals restrict delivery — they inform, not restrict

**Customer Lifecycle Optimization (CLO) — new in 2026:**
Available via Data Manager: Lifecycle Segmentation + Predictive Audiences via AI Max. Automatically segments customers into lifecycle stages and predicts churn/expansion likelihood for use as campaign signals.

---

<!-- section: similar_audiences_replacement -->

### 2.12 Similar Audiences — Replacement Guide

Similar Audiences were fully removed from all campaigns in **August 2023**. They are not coming back. Use these replacements:

| Replacement | Available In | What It Does | Best For |
|---|---|---|---|
| **Optimized Targeting** | Display, Discovery, Video (conversion goal) | Finds users similar to your converters; AI-driven | Conversion-focused prospecting |
| **Audience Expansion** | Video campaigns (awareness/reach goal) | Expands reach beyond defined audience | Awareness and brand campaigns |
| **Lookalike Segments** | Demand Gen only | First-party-data-seeded lookalike | Demand Gen prospecting |

> ⚠️ None of these are observable audience segments like Similar Audiences were. You cannot see who is in them. They are algorithmic signals, not defined lists. Test each before scaling spend.

---

<!-- section: google_campaign_architecture | quick_reference: true -->

### 2.13 Campaign Architecture by Type

| Campaign Type | Audience Role | Recommended Mode | Key Notes |
|---|---|---|---|
| **Search** | RLSA bid modifiers, observation | Observation first | Bid up for high-value segments; exclude converted leads |
| **Shopping** | NCA suppression, LTV value rules | Targeting for suppression | Attach all-customers list for NCA |
| **Display** | Remarketing, exclusions | Targeting | Exclude converted users from prospecting campaigns |
| **YouTube / Demand Gen** | Remarketing, Lookalike seed | Targeting or Observation | Minimum 100 users for activation |
| **Performance Max** | Audience signal input, NCA suppression | Signal (not targeting) | 1–2 high-quality lists only; NCA for suppression |

---

<!-- section: google_apex_roadmap | checklist: true | apex: true -->

### 2.14 Google Ads — Apex Progress Roadmap

Track your progress toward full first-party data potential on Google Ads. Complete each stage before moving to the next.

---

**Stage 1 — Foundation** *(Must-haves before running any first-party targeting)*

- [ ] Google Ads account meets $50K lifetime spend threshold for Customer Match
- [ ] Account is in good standing (no policy violations)
- [ ] Consent Mode v2 (Advanced) implemented and verified in Tag Assistant for all EEA traffic
- [ ] CRM contains minimum viable schema: email, phone, first/last name per contact
- [ ] Data normalization pipeline in place: lowercase email, E.164 phone, ISO country codes
- [ ] First Customer Match list uploaded via Data Manager (not legacy Google Ads API)
- [ ] List shows "Active" status with 1,000+ matched users in Audience Manager
- [ ] All active campaigns are using Smart Bidding (not Manual CPC or Enhanced CPC)

---

**Stage 2 — Intermediate** *(Core best practices — correct configuration of all major features)*

- [ ] GA4 property linked to Google Ads with Personalized Advertising enabled
- [ ] GA4 audiences published to Google Ads and confirmed in Audience Manager (check Sources tab)
- [ ] Customer Match lists applied to campaigns in correct mode (Observation on prospecting, Targeting on remarketing)
- [ ] Existing customer suppression list applied to all acquisition/prospecting campaigns
- [ ] Customer Match list refresh cadence set (weekly minimum — respects 540-day rule)
- [ ] GA4 Predictive Audiences enabled (requires 200+ conversions; targets: Likely 7-day purchasers, Predicted top spenders)
- [ ] Campaign type-specific audiences configured per architecture table (2.13)
- [ ] No Similar Audiences references remain in campaign targeting (fully migrated to Optimized Targeting or Lookalike Segments)

---

**Stage 3 — Advanced** *(Full-funnel integration — LTV signals, OCI/ECL, segmentation)*

<!-- track: ecommerce -->
*E-commerce:*
- [ ] RFM segmentation built in CRM and exported as separate Customer Match lists per segment
- [ ] LTV-based Conversion Value Rules created and applied to Smart Bidding campaigns
- [ ] New Customer Acquisition (NCA) goal configured on primary acquisition campaigns
- [ ] NCA mode selected: "New customers only" for PMax/Shopping, "New customer value" for Search
- [ ] NCA new customer value uplift set at 1.5x–2x AOV
- [ ] Cart abandoner and product viewer remarketing audiences active with appropriate durations
- [ ] Remarketing campaign for Champions/high-LTV customers running with separate creative

<!-- track: leadgen -->
*Lead Generation:*
- [ ] `gclid` capture implemented and verified in CRM (check 100 recent leads have `gclid` populated)
- [ ] Offline Conversion Import (OCI) configured via Data Manager with `conversion_environment` field
- [ ] OCI upload confirmed: first batch imported without errors, conversions visible in Google Ads
- [ ] Conversion action hierarchy established: Closed-Won → SQL → MQL → Form Submit
- [ ] Enhanced Conversions for Leads (ECL) tag implemented and verified in Tag Assistant
- [ ] ECL and OCI both enabled on the same conversion action
- [ ] Funnel-stage audience lists (MQL, SQL, Closed-Won) applied in Observation mode for data gathering
- [ ] Converted leads (Closed-Won) suppressed from all top-of-funnel campaigns

---

**Stage 4 — Apex** *(Maximum potential — incrementality, CLO, continuous optimization loop)*

- [ ] Customer Lifecycle Optimization (CLO) enabled via Data Manager (if available on account)
- [ ] GA4 Predictive Audiences outperforming manually defined audiences — validated in experiments
- [ ] Incrementality experiment run (geo-holdout or Google Campaign Experiment) to validate Customer Match lift
- [ ] LTV data flowing into Smart Bidding — Target ROAS accounts for predicted lifetime value, not just transaction value
- [ ] First-party data feeding Media Mix Model (historical CRM revenue data available by channel and cohort)
- [ ] Customer Match refresh fully automated (Data Manager scheduled connection — no manual uploads)
- [ ] Win-back campaigns actively recovering At-Risk / Hibernating segments with measured incremental ROAS
- [ ] PMax audience signal quality reviewed quarterly — Champions list remains the primary signal
- [ ] OCI upload coverage rate monitored — >80% of offline conversions successfully matched to gclid (lead gen)
- [ ] Annual audit: remove stale segments, refresh RFM tiers, update LTV value rules to reflect current AOV

---

<!-- section: google_postlaunch_checklist | checklist: true -->

### 2.15 Google Ads Post-Launch Checklist

**Universal:**
- [ ] Customer Match lists show "Active" status in Audience Manager
- [ ] Membership counts above minimums: 1,000 for Search/Shopping, 100 for YouTube
- [ ] Consent Mode v2 tags verified firing in Tag Assistant (check `ad_user_data` and `ad_personalization`)
- [ ] All campaigns using Customer Match are on Smart Bidding strategies
- [ ] Audience mode (Targeting vs. Observation) verified per campaign intent
- [ ] GA4 audiences confirmed publishing (Audience Manager → Sources → GA4 listed)
- [ ] No existing customer lists accidentally omitted from prospecting campaign exclusions

**E-commerce specific:**
- [ ] LTV conversion value rules created and applied to all relevant conversion actions
- [ ] NCA goal configured and new customer value uplift set
- [ ] RFM-based suppression lists applied to prospecting campaigns
- [ ] All-customers list refreshed on recurring weekly schedule
- [ ] Remarketing lists for cart abandoners and product viewers are active

**Lead gen specific:**
- [ ] `gclid` capture verified on all lead forms (check CRM for populated gclid field on recent leads)
- [ ] OCI upload scheduled with `conversion_environment` field included
- [ ] First OCI batch confirmed imported without errors
- [ ] ECL tag confirmed firing in Tag Assistant
- [ ] Converted leads suppressed from top-of-funnel campaigns
- [ ] Funnel-stage lists applied in Observation mode

---

<!-- module: meta_implementation | track: meta -->

## Module 3: Meta Ads Implementation

<!-- section: meta_custom_audience_requirements | quick_reference: true -->

### 3.1 Custom Audience Upload Requirements

**Access requirements:** Ad account must accept Meta's Custom Audience Terms of Service before creating any Customer List audience.

**Identifier priority (upload as many as you have):**

| Identifier | Priority | Format | Notes |
|---|---|---|---|
| Email | P1 | Lowercase, SHA-256 hashed | Single strongest signal |
| Phone | P1 | E.164, SHA-256 hashed | Critical for mobile-heavy markets |
| First Name | P2 | Lowercase, SHA-256 hashed | Combine with Last Name |
| Last Name | P2 | Lowercase, SHA-256 hashed | |
| Date of Birth | P2 | `YYYYMMDD`, SHA-256 hashed | Boosts EMQ significantly |
| City | P2 | Lowercase, SHA-256 hashed | Geographic signal |
| State/Province | P2 | 2-char code, SHA-256 hashed | US: `CA`, `NY` |
| Zip/Postal Code | P2 | Normalized, SHA-256 hashed | |
| Country | P2 | ISO 3166-1 alpha-2 | |
| External ID | P3 | Your CRM ID, SHA-256 hashed | Required for CAPI deduplication |
| MADID | P3 | Raw (do NOT hash) | Mobile Advertiser ID |

**Minimum sizes:**

| Use Case | Minimum Matched Users |
|---|---|
| Custom Audience (targeting) | 100 |
| Lookalike seed | 100 (1,000–50,000 optimal) |

> ⚠️ **Custom Audience restrictions (2025):** Data suggesting health conditions or financial status blocked from audiences as of September 2, 2025. Housing, employment, and credit/financial campaigns restricted from using Customer List audiences (US) since January 2025. Verify your industry is not in a restricted category before relying on Customer List audiences.

---

<!-- section: pixel_capi_architecture -->

### 3.2 Pixel + CAPI Architecture

Browser Pixel and server-side CAPI are **complementary, not competing**. Both are required for reliable tracking. The Pixel fires from the browser; CAPI fires from your server. Used together with deduplication, they provide redundancy against ad blockers, ITP, and browser privacy restrictions.

**Performance impact (2025 benchmarks):** Brands running hybrid Pixel + CAPI report 13–20% lower cost per result and up to 20% increase in reported conversions vs. Pixel-only.

**Deduplication — critical rule:**

> ⚠️ Running CAPI without deduplication will double-count conversions and corrupt your Smart Bidding signals. Deduplication requires an identical `event_id` in both the Pixel and CAPI payloads for the same user action.

Implementation:
1. Generate a UUID at the moment of the user action (e.g., checkout complete)
2. Pass it to browser Pixel: `fbq('track', 'Purchase', data, {eventID: 'uuid-here'})`
3. Pass the same UUID to CAPI payload: `"event_id": "uuid-here"`
4. If these values differ, Meta counts two conversions

**CAPI v2 (July 2025):** Enhanced event matching capabilities, improved data handling. Recommended for all new implementations.

> ⚠️ **Offline Conversions API (OCAPI) deprecated May 2025.** All offline conversion tracking must migrate to CAPI. If you are still using OCAPI for any event type, migrate immediately.

---

<!-- section: capi_setup_options -->

### 3.3 CAPI Setup Options

| Option | Technical Effort | Coverage | Best For |
|---|---|---|---|
| **Conversions API Gateway** | Low (Meta-hosted) | Good | Teams without engineering resources |
| **GTM Server-Side** | Medium | Good | Teams already using GTM |
| **Native Platform Integration** | Low (Shopify, WooCommerce, etc.) | Good for platform events | E-commerce on supported platforms |
| **Direct API Integration** | High | Best — full control | Engineering teams, complex custom data |

**Required CAPI payload fields per event:**
- `event_name` (e.g., `Purchase`, `Lead`, `ViewContent`)
- `event_time` (Unix timestamp)
- `event_id` (for deduplication with Pixel)
- `action_source` (`website`, `app`, `crm`, `email`, etc.)
- `user_data` (hashed identifiers — email, phone, external_id)
- `custom_data` (for Purchase: `value`, `currency`, `content_ids`)

> ⚠️ The `action_source` field is required. Missing it degrades EMQ. For web events use `website`; for CRM-originating events (Conversion Leads) use `crm`.

---

<!-- section: emq_scoring | quick_reference: true -->

### 3.4 Event Match Quality (EMQ)

EMQ measures how reliably Meta can match an event to a user. Higher EMQ = better attribution, better optimization, lower CPA.

| EMQ Score | Label | Action |
|---|---|---|
| 0–4 | Poor | Critical: audit hashing, add more identifiers, implement CAPI |
| 5–6 | Limited | Below threshold for reliable optimization; improve before scaling |
| 7–8 | Reliable | Acceptable; push toward 8+ before scaling |
| 9–10 | Optimal | Maintain; do not sacrifice data quality for marginal score gains |

**2025 event-specific benchmarks:**
- PageView / top-funnel events: 4.5–7 (normal range)
- AddToCart / mid-funnel: 6–8 (aim for upper range)
- **Purchase events: 7.5–9 (ideal 8.8–9.3)**

> 🎯 Target EMQ **8+** on Purchase/Lead events before scaling spend. Below 7, Meta cannot reliably match events to users — attribution and optimization both suffer. Do not chase EMQ above 8.5 at the expense of data volume; returns diminish sharply.

**EMQ optimization checklist:**

- [ ] Email is passed with every conversion event (not just lead/purchase — all events)
- [ ] Phone number passed at checkout and form submission
- [ ] First name and last name included in user_data
- [ ] Date of birth included if available (loyalty data, quiz data)
- [ ] External ID (your CRM/user ID, SHA-256 hashed) included for deduplication
- [ ] `fbclid` URL parameter captured and passed (preserves click-to-event matching)
- [ ] CAPI implemented with deduplication (event_id matches Pixel)
- [ ] Hashing verified: SHA-256 on all fields except MADID and external_id
- [ ] Check EMQ in Events Manager → Data Sources → [Pixel] → Event Match Quality tab

---

<!-- section: meta_ecommerce_funnel | track: ecommerce -->

### 3.5 E-commerce Funnel Architecture

Full-funnel structure using first-party data audiences.

| Stage | Campaign Goal | Audience | Exclusions |
|---|---|---|---|
| **ACQUISITION** | Conversions (Purchase) | LAL from Champions 1–3% | All existing customers, recent purchasers (7–14d) |
| **MID-FUNNEL** | Conversions (Add to Cart / Initiate Checkout) | Website visitors 30d, video viewers 25% | Recent purchasers (30d) |
| **RETENTION** | Conversions (Purchase) or Catalog Sales | Existing customers segmented by RFM | None — these are your customers |
| **SUPPRESSION** | N/A — not a campaign | Recent purchasers (7–14d), active subscriptions | Applied as exclusions on Acquisition and Mid-Funnel |

> ⚠️ Suppression is not a campaign — it is an exclusion audience applied to Acquisition and Mid-Funnel. Failure to suppress recent purchasers from acquisition campaigns wastes budget and inflates reported CAC.

**CAPI events for e-commerce:**

| Event | Trigger | Key Parameters |
|---|---|---|
| `ViewContent` | Product page view | `content_ids`, `content_type`, `value` |
| `AddToCart` | Cart addition | `content_ids`, `value`, `currency` |
| `InitiateCheckout` | Checkout start | `value`, `currency`, `num_items` |
| `Purchase` | Order confirmed | `value`, `currency`, `order_id`, `content_ids` |

**Advantage+ Sales Campaigns (ASC) — 2025 context:**

> 🎯 ASC replaced Advantage+ Shopping Campaigns and saw 70% YoY growth in Q4 2025. First-party data is described by Meta as "essential" to getting the most out of ASC. Feed your CAPI events and Customer Lists into ASC — the algorithm uses these to identify high-value prospects.

---

<!-- section: lookalike_reference | quick_reference: true -->

### 3.6 Lookalike Audience Reference

**Seed size guide:**

| Matched Users in Seed | Quality |
|---|---|
| 100–999 | Minimum viable; quality suffers |
| 1,000–9,999 | Acceptable |
| 10,000–50,000 | Optimal range |
| 50,000+ | Diminishing returns; seed quality matters more than size |

**LAL percentage selection:**

| LAL % | Size | Precision | Use Case |
|---|---|---|---|
| 1–3% | Smallest | Highest | Prospecting toward high-intent users |
| 3–5% | Moderate | Moderate | Scaling acquisition |
| 5–10% | Largest | Lower | Awareness, lower-CPM reach objectives |

> ⚠️ **2025 strategic shift:** Traditional Lookalike Audiences are becoming less relevant. Meta's **Advantage+ Audience** (algorithmic expansion) now outperforms manually defined LALs in most campaign types. Provide your Customer List as an "audience suggestion" rather than a hard LAL definition — Meta's AI dynamically expands to find similar high-value users. Reserve explicit LAL usage for campaigns requiring precise audience control (lead gen, new market entry).

**Best seeds by business model:**
- E-commerce: Champions list (highest LTV purchasers, 1,000+ users)
- Lead gen: Closed-Won customers (not raw leads — quality signal matters)

---

<!-- section: meta_conversion_leads | track: leadgen -->

### 3.7 Lead Gen — Conversion Leads Optimization

Conversion Leads optimization sends CRM qualification signals back to Meta so the algorithm learns to find users who convert to qualified leads — not just anyone who fills out a form.

**Requirements:**
- CAPI must be fully implemented before Conversion Leads optimization can be enabled
- CRM integration must be established (native: HubSpot, Salesforce, Pipedrive; third-party: Zapier, LeadsBridge)
- **Meta Lead ID** (15–16 digit numeric ID) must be stored in your CRM alongside every lead

**Two-part integration:**
1. **Lead Sync (Meta → CRM):** Lead form submissions sync to CRM in real time via native integration or webhook
2. **CRM Feedback Loop (CRM → Meta via CAPI):** When a lead reaches MQL/SQL/Closed-Won status in CRM, send a CAPI event with `action_source: crm` so Meta knows this lead was qualified

**Performance benchmark:** Up to **15% lower cost-per-quality-lead** when CRM signals are consistently fed back to Meta over 30+ days.

**Setup checklist:**

- [ ] CAPI implementation verified with deduplication and EMQ 7+
- [ ] CRM native integration connected OR CAPI webhook from CRM events configured
- [ ] Meta Lead ID (15–16 digit) captured and stored in CRM for every new lead
- [ ] CRM qualification events configured: fire CAPI event when lead reaches MQL, SQL, or Closed-Won
- [ ] `action_source` set to `crm` on all CRM-originating CAPI events
- [ ] CAPI event name for qualified lead defined and consistent (e.g., `Lead_MQL`, `Lead_Won`)
- [ ] Campaign optimization goal changed to "Conversion Leads" (requires CRM connection to be active)
- [ ] Monitor lead quality in CRM (not just volume in Meta Ads Manager) for 30+ days
- [ ] Do not switch to Conversion Leads without at least 2 weeks of historical CRM signal data in Meta

> ⚠️ `fbclid` must be captured in your lead form URL and stored in CRM. Without it, Meta cannot attribute the CRM feedback event to the original ad click. Add `fbclid` as a hidden field to all lead capture pages.

---

<!-- section: meta_apex_roadmap | checklist: true | apex: true -->

### 3.8 Meta Ads — Apex Progress Roadmap

Track your progress toward full first-party data potential on Meta Ads.

---

**Stage 1 — Foundation** *(Must-haves before running any first-party targeting)*

- [ ] Meta Pixel installed on all key pages (PageView on every page, Purchase/Lead on conversion pages)
- [ ] Pixel events verified firing in Events Manager (no duplicate or missing events)
- [ ] Custom Audience Terms of Service accepted on ad account
- [ ] CRM contains minimum viable schema: email + phone for all contacts
- [ ] First Customer List Custom Audience uploaded with 100+ matched users
- [ ] CAPI implemented (Gateway, GTM server-side, native platform, or direct API)
- [ ] CAPI events confirmed in Events Manager ("Received" column shows server events)
- [ ] Deduplication confirmed: `event_id` present and matching in both Pixel and CAPI for all conversion events
- [ ] OCAPI fully migrated to CAPI (if you had any offline conversion tracking via old API)

---

**Stage 2 — Intermediate** *(Core best practices — correct configuration)*

- [ ] EMQ score 7+ on all conversion events (check Events Manager → Event Match Quality tab)
- [ ] EMQ score 8+ on Purchase / Lead events specifically
- [ ] All hashed identifiers verified: SHA-256 on email, phone, name, DOB; raw MADID
- [ ] `fbclid` URL parameter captured and passed in all conversion events
- [ ] Custom Audiences refreshed on weekly schedule (respects 180-day retention)
- [ ] Acquisition campaigns have recent-purchaser / recent-converter exclusion applied
- [ ] Suppression audiences built and applied as exclusions on prospecting campaigns
- [ ] Lookalike Audiences built from high-quality seed (Champions list / Closed-Won, 1,000+ users)

---

**Stage 3 — Advanced** *(Full-funnel integration and segmentation)*

<!-- track: ecommerce -->
*E-commerce:*
- [ ] Full-funnel campaign architecture implemented: ACQUISITION → MID-FUNNEL → RETENTION → SUPPRESSION (see 3.5)
- [ ] CAPI events enriched with product data (`content_ids`, `value`, `currency`) on all commerce events
- [ ] RFM-based Custom Audiences built and applied: Champions, At-Risk, New Customers, Hibernating
- [ ] Retention campaign running with RFM-segmented audiences and segment-specific creative
- [ ] Dynamic product ads (Catalog ads) configured using CAPI purchase/view events
- [ ] Advantage+ Sales Campaigns (ASC) running with Customer List as audience signal input
- [ ] LAL or Advantage+ Audience seeded from Champions list on Acquisition campaigns

<!-- track: leadgen -->
*Lead Generation:*
- [ ] `fbclid` stored in CRM for every lead (verify last 100 leads have fbclid populated)
- [ ] CRM → Meta CAPI feedback loop live: qualification events firing when lead status changes
- [ ] Meta Lead ID stored in CRM for every lead from Meta Lead Ads
- [ ] Conversion Leads optimization enabled on primary lead gen campaigns
- [ ] Funnel-stage Custom Audiences active: MQL, SQL, Closed-Won segments
- [ ] Converted leads (Closed-Won) suppressed from all acquisition campaigns
- [ ] LAL from Closed-Won customers used as seed for Acquisition campaigns

---

**Stage 4 — Apex** *(Maximum potential — incrementality, clean room, continuous optimization loop)*

- [ ] Meta Conversion Lift study run to validate first-party data incremental impact on primary campaign
- [ ] EMQ monitoring automated: alert if any event drops below 7 (use Events Manager API or dashboard)
- [ ] CRM feedback loop delivering 30+ days of signal — Conversion Leads optimization past learning phase
- [ ] Advantage+ Audience tested vs. explicit Lookalike — winner applied based on CPA/ROAS data
- [ ] Meta Advanced Analytics (clean room) accessed for first-party segmentation insights
- [ ] Creative strategy linked to audience segments: Champions see retention/upsell, Hibernating see win-back
- [ ] CAPI refresh rate matches business velocity: purchase events fire in real time (not batched)
- [ ] Cross-platform suppression: Google Ads and Meta share same exclusion audience lists (same CRM export)
- [ ] Annual review: validate EMQ, refresh Custom Audience seeds, audit CAPI payload completeness

---

<!-- section: meta_postlaunch_checklist | checklist: true -->

### 3.9 Meta Ads Post-Launch Checklist

**Universal:**
- [ ] Pixel firing on all key events (PageView, ViewContent, AddToCart, Purchase/Lead)
- [ ] CAPI events confirmed in Events Manager (check "Received" column shows server events)
- [ ] Deduplication confirmed: `event_id` matching in Pixel and CAPI payloads
- [ ] EMQ score 7+ on all conversion events (target 8+)
- [ ] Custom Audiences show "Active" status with 100+ matched users
- [ ] Suppression audiences applied to Acquisition campaigns
- [ ] Custom Audience Terms of Service accepted

**E-commerce specific:**
- [ ] Catalog connected and synced (product feed updated daily minimum)
- [ ] Dynamic product ad audiences active (ViewContent, AddToCart, Purchase events feeding retargeting)
- [ ] LAL or Advantage+ Audience seeded from high-LTV customers on Acquisition
- [ ] Retention campaigns live for existing customer segments
- [ ] Recent-purchaser suppression window set (7–14 days minimum)

**Lead gen specific:**
- [ ] Lead form connected to CRM via native integration or webhook
- [ ] `fbclid` captured in lead form hidden field and stored in CRM
- [ ] Meta Lead ID (15–16 digit) stored in CRM for all Meta Lead Ads leads
- [ ] CRM qualification events firing to Meta via CAPI
- [ ] Converted leads suppressed from acquisition campaigns
- [ ] LAL from Closed-Won customers active on Acquisition

---

<!-- module: measurement -->

## Module 4: Measurement & Incrementality

<!-- section: kpi_evolution | quick_reference: true -->

### 4.1 KPI Evolution — Measurement Maturity

| Maturity Level | Primary KPIs | Core Limitation |
|---|---|---|
| **Superficial** | CTR, Impressions, Reach | No connection to business outcomes |
| **Platform-attributed** | Platform ROAS, CPA (as reported by Google/Meta) | Attribution overlap, view-through inflation, signal loss distortion |
| **Blended** | MER (Marketing Efficiency Ratio = Total Revenue / Total Spend), Blended CAC | Ignores incrementality; channels get credit they don't deserve |
| **Deep-funnel** | SQL rate, CAC by channel (CRM-sourced), LTV:CAC ratio | Requires CRM integration and OCI/CAPI to function |
| **Incremental** | iROAS, incremental CAC, revenue lift vs. holdout | Requires testing infrastructure; most accurate |

> 🎯 Move your reporting from Platform-attributed to Blended as a minimum. Deep-funnel and Incremental measurement are the only frameworks that will remain accurate as signal loss continues to degrade platform-reported metrics.

**Marketing Efficiency Ratio (MER):**
`MER = Total Revenue / Total Ad Spend`
Use this as your primary north-star when platform attribution is unreliable. It captures all revenue, including cross-device and view-through conversions that platforms would separately claim.

---

<!-- section: incrementality_testing -->

### 4.2 Incrementality Testing

Incrementality testing answers the only question that matters: *Would this conversion have happened without the ad?*

**Method 1: Geo Holdout Test**
Divide geographic markets into treatment (exposed) and holdout (suppressed or paused) groups. Measure conversion rate difference between groups.

*Requirements:* Sufficient conversion volume per geo (~100+ conversions per geo per week); geographically separable markets; minimum 2–4 week runtime.

*Best for:* Channel-level or campaign-level incrementality; cross-platform measurement.

**Method 2: Meta Conversion Lift Study**
Meta's native holdout tool. Creates a randomized holdout group inside Meta's delivery system. More statistically rigorous for Meta-specific measurement.

*Requirements:* Access via Meta Business Suite → Measure & Report → Conversion Lift; minimum budget and volume thresholds apply.

*Best for:* Meta-specific campaign or audience incrementality.

**Method 3: Google Campaign Experiments**
Google's native A/B test framework at the campaign level. Split traffic between control and experiment.

*Best for:* Testing Customer Match bidding impact; testing new audience configurations against baseline.

| Method | Platform | Setup Effort | Statistical Rigor | Best For |
|---|---|---|---|---|
| Geo Holdout | Platform-agnostic | High | High (if well-designed) | Cross-channel, cross-platform |
| Meta Conversion Lift | Meta | Low (native) | High | Meta-specific measurement |
| Google Campaign Experiments | Google | Low (native) | Moderate | Google campaign A/B testing |

---

<!-- section: media_mix_modeling -->

### 4.3 Media Mix Modeling (MMM)

MMM uses statistical modeling to attribute revenue to marketing inputs across channels, independent of platform-reported data. It is the most reliable measurement approach at scale when attribution models disagree.

**When MMM becomes necessary:**
- Platform ROAS numbers don't reconcile with actual revenue
- Multiple channels are competing for the same conversion attribution
- Privacy restrictions make user-level attribution unreliable at scale

**First-party data inputs required for MMM:**
- CRM revenue data by week/month (not platform-reported revenue)
- Offline conversion data by channel
- Customer cohort LTV by acquisition channel
- Promotional calendar (sales, discounts, seasonality events)

> 🎯 MMM and incrementality testing are complements, not substitutes. MMM tells you budget allocation at a macro level over time. Lift tests validate specific decisions (does Customer Match actually help?) in the short term. Use both.

**Practical note for PPC specialists:** You don't need to run MMM yourself. Your job is to ensure the inputs exist: clean CRM revenue data, OCI/CAPI-backed offline conversions, and a promotional calendar. Without clean first-party data flowing into the business, MMM inputs are unreliable.

---

<!-- module: best_practices -->

## Module 5: Best Practices & Strategy

> 🎯 This module is written for someone who has never worked with first-party data before. It builds from zero — explaining what a segment actually is, why it matters, and how to think strategically before touching a single platform setting. Experienced practitioners: skip to sections 5.5 onwards.

---

<!-- section: what_is_segment -->

### 5.1 What Is a First-Party Data Segment? (Start Here)

**The simplest possible explanation:**

A first-party data segment is a named group of real people from your own customer database, uploaded to an ad platform so you can show ads specifically to them — or specifically avoid them.

That's it. You take a list of real people (emails, phone numbers, names), upload it to Google Ads or Meta Ads, the platform matches them to its own user profiles, and you now have a targetable audience made of *your actual customers*.

---

**The coffee shop analogy:**

Imagine you own a coffee shop. After years in business, you know things:
- Maria always orders espresso, comes in at 8am, and has spent €1,200 with you this year
- Tom ordered once six months ago and never came back
- Lisa just made her second purchase last week

Now imagine you could show ads *only to Maria* (to upsell her on your new coffee subscription) and *only to Lisa* (to nudge her toward a third purchase). And you could specifically *not* waste money showing acquisition ads to Tom — he already knows you exist.

That's exactly what first-party data segments let you do in Google Ads and Meta Ads.

**The key insight:** You are not buying a list of strangers from a data broker. You are reconnecting with people who have already raised their hand and said "I trust this business."

---

**Why this is more powerful than interest targeting:**

When you target "people interested in coffee" on Meta, you are reaching:
- People who scrolled past a coffee article once
- People whose algorithm thinks they like coffee based on vague signals
- Your competitors' customers
- People who will never buy from you

When you target your Champions segment (top 500 customers by LTV), you are reaching:
- People who have already proven they pay, return, and recommend you
- The exact people most likely to buy again

Research from BCG: companies using first-party data achieve **2.9x revenue uplift** and **1.5x cost savings** compared to those relying on third-party targeting. First-party data is 2.3x more likely to help brands exceed revenue targets.

---

<!-- section: how_matching_works -->

### 5.2 How Platform Matching Works

You upload a list. The platform hashes and compares your identifiers against its own user database. When there's a match, that person enters your segment. Here's what happens inside each platform:

**Google Ads — Customer Match:**

```
Your CRM Export          →    Google hashes        →    Google User Profile
email: jane@email.com         SHA-256 both               Signed-in Google account
phone: +12025551234           Compares hashes            YouTube, Gmail, Search
first_name: Jane              If match → segment         Tagged for your campaign
```

- Google can only match signed-in Google account users
- Match rate: typically 30–60% depending on list quality and identifier richness
- Higher match rates with Gmail addresses + phone numbers

**Meta Ads — Custom Audiences:**

```
Your CRM Export          →    Meta hashes         →    Meta User Profile
email: jane@email.com         SHA-256 both              Facebook/Instagram account
phone: +12025551234           Compares hashes           Behavioral graph
first_name + DOB              If match → segment        Tagged for your campaign
```

- Meta can match across Facebook, Instagram, WhatsApp, and Messenger
- Match rate: typically 40–80% — stronger than Google due to broader signed-in base
- More identifiers = higher EMQ = better optimization signal

> ⚠️ A 60% match rate on 10,000 people = 6,000 matched users. Plan your audience sizes accordingly. Not all of your list will be reachable on any given platform.

---

<!-- section: three_core_uses | quick_reference: true -->

### 5.3 The Three Core Uses of First-Party Segments

Every single thing you do with first-party data on any ad platform comes down to three fundamental uses. Learn these three and you understand 90% of the strategy.

| Use | What It Does | When to Use It |
|---|---|---|
| **1. Remarketing** | Show ads specifically TO people in your segment | Existing customers, cart abandoners, past leads, lapsed buyers |
| **2. Suppression** | Block your ads from showing TO people in your segment | Exclude current customers from acquisition campaigns; exclude converted leads |
| **3. Lookalike Seeding** | Use your segment as a *model* to find similar new people | Find new prospects who look like your best customers |

**These three uses interact.** The same "Champions" list can simultaneously:
- Be a remarketing audience in a retention campaign (USE 1)
- Be an exclusion on all prospecting campaigns so you don't pay to re-acquire people who already buy (USE 2)
- Be the seed list for a Lookalike/Similar audience that finds new high-LTV customers (USE 3)

---

**Use 1 in practice — Remarketing:**

You already know these people. Your messaging can be completely different from what you show strangers.

- Stranger: "Discover our coffee subscription — try your first bag free"
- Existing customer: "You've been with us 6 months — upgrade to our premium roast and save 20%"

Better message + known audience = higher conversion rate at lower cost.

**Use 2 in practice — Suppression:**

This is the most underused and highest-ROI application. Every time an existing customer clicks your acquisition ad, you:
1. Pay for the click
2. Report a "new customer" that was actually a repeat purchase
3. Inflate your CAC and mislead your algorithm

Suppression stops this. Apply your "All Existing Customers" list as an exclusion on all acquisition campaigns. This reduces wasted spend immediately.

> 🎯 Suppression is not optional. It is the first thing you should implement. You are literally spending money to reach people who already buy from you — and calling it acquisition.

**Use 3 in practice — Lookalike Seeding:**

Your best customers have a pattern. Demographics, behaviors, interests — there's a profile that unites them. The platform's AI can find people who match that profile if you give it a high-quality seed.

- Bad seed: "all form submissions" (10,000 mixed-quality leads)
- Good seed: "Closed-Won customers with LTV > €500" (800 verified buyers)

Smaller, higher-quality seeds outperform larger, diluted ones every time. Quality > quantity.

---

<!-- section: targeting_mode_guide | quick_reference: true -->

### 5.4 Targeting vs. Observation vs. Exclusion

This is the most commonly confused topic among beginners. There are three ways to apply an audience to a campaign — and choosing the wrong one is a very common mistake.

| Mode | Reach Effect | Bid Effect | Best For |
|---|---|---|---|
| **Targeting** | Restricts delivery ONLY to people in this audience | No automatic adjustment | Remarketing, retention, win-back — when you only want to reach this group |
| **Observation** | No restriction — campaign reaches everyone as normal | Smart Bidding adjusts bids UP/DOWN based on audience signal | Learning phase, prospecting campaigns where you want data without limiting scale |
| **Exclusion** | Blocks delivery to people in this audience — they never see the ad | Removes them entirely | Suppression of existing customers, converted leads, unqualified segments |

**The single most important rule:**

> ⚠️ **Start new audiences in Observation mode on existing campaigns.** If you immediately put an untested audience in Targeting mode on a new campaign, you may strangle delivery and starve Smart Bidding of the conversion volume it needs to learn. Always observe first, evaluate performance data, then decide if Targeting is warranted.

**Decision flow:**

```
Do you want to reach ONLY this group of people?
├── YES → Use TARGETING
│         (remarketing campaign, retention, win-back)
│
├── NO, you want everyone but want data on this group
│   → Use OBSERVATION
│     (adds audience as a signal, no reach restriction)
│
└── NO, you want to PREVENT this group from seeing your ad
    → Use EXCLUSION
      (suppression, block existing customers from acquisition)
```

---

<!-- section: audience_sizing_reference | quick_reference: true -->

### 5.5 Audience Sizing Guide

Size matters — both too small and too large cause problems.

| Platform | Use Case | Minimum | Sweet Spot | Too Large Problem |
|---|---|---|---|---|
| Google — Search/Shopping | Targeting / Observation | 1,000 matched users | 5,000–100,000 | Signal dilution, loss of relevance |
| Google — YouTube / Display | Targeting | 100 matched users | 1,000–50,000 | Broad reach, lower precision |
| Google — Customer Match (LAL seed) | Lookalike seeding | 1,000 | 5,000–50,000 | Rare issue; quality matters more |
| Meta — Custom Audience | Targeting | 100 matched users | 500–50,000 | Fine if segmented well |
| Meta — Lookalike seed | Lookalike / Advantage+ input | 100 | 1,000–50,000 | Over 50K gives diminishing returns |
| Meta — Lookalike output | Prospecting campaign | Automatic (1–10% of country) | 1–3% LAL | 5–10% reduces precision significantly |

**Freshness rules:**
- Google: 540-day max membership duration (enforced since April 2025). Lists not refreshed within 540 days lose members as they age out.
- Meta: 180-day max Custom Audience retention window. A customer not seen in any event for 180 days is removed from the audience.
- Both platforms: refresh your lists at minimum monthly, weekly for high-velocity businesses.

> ⚠️ A stale list is worse than no list. An "Existing Customers" exclusion list that hasn't been updated for 6 months will miss all new customers — and you will pay to re-acquire people who just bought from you.

---

<!-- section: audience_layering -->

### 5.6 Audience Layering Strategy

Audience layering means applying multiple audiences to a single campaign — some in Targeting, some in Observation, some as Exclusions. This is how sophisticated advertisers extract maximum signal from first-party data.

**Basic stack (any campaign):**

```
Campaign
├── EXCLUSIONS (always on)
│   ├── All Existing Customers
│   └── Recent Purchasers (7–30 days)
│
├── OBSERVATION (signals only)
│   ├── High-LTV customers (Champions)
│   ├── Cart abandoners
│   └── GA4: Predicted Purchasers
│
└── Smart Bidding reads observation signals
    → Automatically bids higher for users matching
      high-value audience profiles
```

**Advanced stack (value-based bidding):**

```
Campaign (Target ROAS)
├── EXCLUSIONS
│   └── All Existing Customers
│
├── OBSERVATION + VALUE RULES
│   ├── Champions segment → 2.5x conversion value multiplier
│   ├── Loyal Customers   → 1.8x multiplier
│   ├── New Visitors      → 1.0x (baseline)
│   └── Dormant users     → 0.7x multiplier (bid down)
│
└── Smart Bidding now bids proportionally to predicted LTV
    → Not all conversions are worth the same to the algorithm
```

> 🎯 Value rules are the single highest-leverage move in Google Ads first-party data. They teach Smart Bidding that a Champions customer converting is worth 2.5x more than an anonymous visitor. Without them, the algorithm treats a €5 purchase and a €5,000 lifetime customer exactly the same.

---

<!-- section: google_best_practices | quick_reference: false -->

### 5.7 Google Ads — Best Practices by Campaign Type

#### Search (RLSA)

RLSA (Remarketing Lists for Search Ads) applies your Customer Match lists to Search campaigns via Observation mode. Smart Bidding adjusts bids automatically — you do not need to set manual bid modifiers.

**What to do:**
- Apply Champions, Loyal Customers, and High-Intent Visitors lists in Observation on all core Search campaigns
- Let Smart Bidding auto-adjust — do NOT manually set bid modifiers when using Target CPA or Target ROAS
- Apply "All Existing Customers" as an exclusion on top-of-funnel/branded acquisition Search campaigns (unless you want upsell from search)

**Beginner mistake:** Adding a remarketing list in Targeting mode to an existing Search campaign. This immediately cuts reach to only that audience — your Search campaign stops reaching anyone who hasn't been to your site. Start with Observation.

#### Shopping & Performance Max

- Attach your all-customers list as an NCA (New Customer Acquisition) exclusion to prevent re-acquiring existing buyers
- Use your Champions/High-LTV list as PMax's audience signal (not a restriction — a suggestion)
- Apply LTV Conversion Value Rules so Smart Bidding knows which conversions are worth more
- Keep PMax signals to 1–2 high-quality lists; more lists do not improve performance

#### Display & YouTube

- Use Targeting mode for remarketing (these audiences are smaller and you specifically want to reach them)
- Exclude existing customers from awareness Display campaigns — they already know you
- Use Custom Intent or Lookalike (Demand Gen) seeded from Champions for prospecting Display campaigns
- Minimum 100 matched users for YouTube; minimum 1,000 for Display campaigns

#### Demand Gen

- The only Google campaign type where Lookalike Segments (seeded from first-party data) are available as of 2025
- Seed lookalikes from Champions or Closed-Won customers
- Use Observation + Smart Bidding rather than manual Lookalike Targeting for prospecting reach

---

<!-- section: meta_best_practices | quick_reference: false -->

### 5.8 Meta Ads — Best Practices by Campaign Type

#### Acquisition Campaigns

The goal: find new people who look like your best customers, without wasting budget on people who already buy from you.

**Audience setup:**
1. Create a Custom Audience from your Champions / Closed-Won list
2. Use it as the seed for a Lookalike Audience (1–3%) OR as an Advantage+ Audience suggestion
3. Apply your "All Existing Customers" Custom Audience as an exclusion
4. Apply "Recent Purchasers (14 days)" as an exclusion

**Advantage+ Audience vs. Manual Lookalike (2025):**
- For most campaigns: use Advantage+ Audience with your Champions list as a "suggestion" — Meta's AI outperforms manually defined LALs in most A/B tests
- For lead gen requiring precise control, or testing new markets: use explicit Lookalike (1–3%)
- Test both in a split before committing

#### Remarketing Campaigns

The goal: bring back people who showed interest but didn't convert, or upsell people who already bought.

**Segment-specific messaging rules:**

| Segment | Mindset | Message Angle | CTA |
|---|---|---|---|
| Cart Abandoners (last 7d) | Forgot / hesitating | Reminder + social proof or urgency | "Complete your order" |
| Product viewers (last 14d) | Considering | Benefits, comparison, reviews | "See why others chose us" |
| Past customers (30–90d) | Satisfied but idle | New arrivals, seasonal, loyalty | "New for you" / "You'll love this" |
| At-Risk (90–180d) | Drifting | Win-back offer, "we miss you" | "Come back, here's 15% off" |
| Lapsed (180d+) | Gone | Test with strong offer OR suppress entirely | Use sparingly |

> 🎯 **Creative rule:** The message must match the segment's relationship with your brand. Showing a "Discover us for the first time" ad to a customer who spent €800 with you last year is not just wasted budget — it actively signals that you don't know or value them.

#### Lead Gen Campaigns

<!-- track: leadgen -->
- Apply all Closed-Won customers as exclusions on lead acquisition campaigns
- Apply all current open Opportunities as exclusions (they're in the sales funnel — ads will only confuse them)
- Apply MQL/SQL lists as exclusions from top-of-funnel TOFU campaigns
- Use Closed-Won customers (not raw leads) as your Lookalike seed
- Enable Conversion Leads optimization only after CAPI is running and 30+ days of CRM signal data exists

<!-- /track -->

---

<!-- section: strategy_decision_matrix | quick_reference: true -->

### 5.9 Strategy Decision Matrix

Use this table to decide what to do with any segment.

| Segment | Google Action | Meta Action | Priority |
|---|---|---|---|
| **Champions** (top LTV buyers) | Observation + LTV value rule | Upsell/retention campaign | 🔴 High |
| **All Existing Customers** | Exclude from all acquisition | Exclude from all acquisition | 🔴 High |
| **Recent Purchasers (7–14d)** | Exclude from remarketing too | Exclude — they just bought | 🔴 High |
| **Cart Abandoners** | RLSA observation on Search + Display | Dynamic product ad retargeting | 🔴 High |
| **Loyal Customers** | Observation + mild value rule | Retention campaign + upsell | 🟠 Medium |
| **At-Risk / Lapsed** | Win-back campaign (Targeting) | Win-back ad set with offer | 🟠 Medium |
| **Closed-Won (Lead Gen)** | Exclude from prospecting; OCI seed | Exclude; Lookalike seed | 🔴 High |
| **Raw Leads / Form Submits** | Exclude from TOFU; NEVER use as seed | Exclude from top-of-funnel | 🟠 Medium |
| **MQL / SQL (Lead Gen)** | Observation on nurture campaigns | Mid-funnel nurture ad set | 🟡 Low |
| **Lookalike seed (best customers)** | Use for Demand Gen prospecting | Advantage+ suggestion or 1–3% LAL | 🟠 Medium |

---

<!-- section: common_mistakes -->

### 5.10 Top 12 Mistakes Beginners Make

Learning from these mistakes before making them will save months of wasted budget and corrupted algorithm training.

---

**Mistake 1: Using your full contact list as one giant audience**

Uploading all 50,000 CRM contacts as a single "Customers" audience mixes Champions (spending €2,000/year) with people who clicked your site once two years ago. The algorithm can't differentiate — and your bidding, creative, and suppression all fail together.

*Fix: Segment before uploading. Minimum: Champions, Loyal, At-Risk, Lapsed.*

---

**Mistake 2: Forgetting suppression entirely**

The most common and expensive beginner mistake. Running acquisition campaigns with no existing-customer exclusion means you are paying to convert people who already buy from you — and calling it growth.

*Fix: Build an "All Existing Customers" exclusion list and apply it to every single acquisition campaign. Refresh it weekly.*

---

**Mistake 3: Using raw form submissions as a Lookalike seed**

Your form submissions include bots, unqualified leads, tire-kickers, competitors researching you, and students. Seeding a lookalike from this pool tells the algorithm to find more people like... all of them.

*Fix: Use only Closed-Won customers or Champions as lookalike seeds. Quality of seed > size of seed.*

---

**Mistake 4: Applying Targeting mode immediately on untested audiences**

Switching a live campaign from broad reach to Targeting mode (restricts to only your audience) instantly chops your impression volume. If the audience is too small, Smart Bidding enters learning mode and CPA spikes.

*Fix: Always start in Observation mode. Gather 2–4 weeks of data. Only switch to Targeting when you have evidence the audience performs better.*

---

**Mistake 5: Never refreshing lists**

A customer list uploaded once and never updated:
- Misses all new customers (they won't be suppressed)
- Loses members as they hit the 540-day (Google) or 180-day (Meta) expiration window
- Shrinks below the activation minimum and silently stops working

*Fix: Set up automated weekly exports from your CRM. Google Data Manager and Meta CAPI both support scheduled refreshes.*

---

**Mistake 6: Using Customer Match lists with Manual CPC bidding**

This is silent and frustrating. If you're on Manual CPC or legacy Enhanced CPC, Customer Match lists are attached to the campaign — but Google completely ignores them. No signal is applied. No bid adjustment happens.

*Fix: Migrate to a Smart Bidding strategy (Target CPA, Target ROAS, Maximize Conversions) before attaching any Customer Match audiences.*

---

**Mistake 7: No deduplication on Meta (CAPI + Pixel)**

If you run both Meta Pixel and CAPI (which you should), and you don't implement deduplication via matching `event_id` values, every conversion is counted twice. Your reported ROAS looks 2x better than reality. Smart Bidding then chases a fictional target and overspends.

*Fix: Generate a UUID at the moment of conversion, pass it to both Pixel and CAPI as `event_id`. They cancel each other out if they match.*

---

**Mistake 8: Bidding against yourself (audience overlap)**

Running a remarketing campaign and a prospecting campaign with overlapping audiences means you are competing against yourself in the auction. Your remarketing campaign bids for past visitors while your prospecting campaign ALSO reaches them (because you forgot to exclude them). You pay twice.

*Fix: Every prospecting campaign must exclude all remarketing audiences. Every acquisition campaign must exclude all existing customers. No overlap.*

---

**Mistake 9: Wrong business model targeting**

<!-- track: ecommerce -->
*E-commerce:* Optimizing for "Purchase" is correct. But if you haven't applied LTV-based value rules, Smart Bidding treats a €10 sale and a €500 sale identically. Your spend concentrates on cheap, low-value conversions.
<!-- /track -->

<!-- track: leadgen -->
*Lead Gen:* Optimizing for "Form Submit" without feeding CRM quality signals back to the platform means Smart Bidding maximizes volume of form fills — not quality of leads. You get 200 leads, sales converts 3 of them.
<!-- /track -->

*Fix: E-commerce → implement Conversion Value Rules + LTV bidding. Lead Gen → implement OCI or CAPI Conversion Leads with CRM stage feedback.*

---

**Mistake 10: Trusting platform-reported match rates blindly**

Google shows match rates in buckets (not exact numbers). Meta shows you matched audience sizes but not why a third of your list didn't match. Platforms have strong incentives to appear to be working. Low match rates could mean bad normalization, stale emails, or wrong identifier format — none of which the platform will proactively tell you.

*Fix: Audit your lists periodically. Run a small test — upload 1,000 known-good email addresses and check the match count. Use phone numbers as a second identifier.*

---

**Mistake 11: Treating all audiences as permanent**

Audience membership changes. Champions this quarter might be At-Risk next quarter. A customer who was active 6 months ago is now lapsed. If you never update your segments, you are showing win-back ads to active buyers and upsell ads to people who churned.

*Fix: Tie your audience exports to CRM segment calculations that run on a schedule. Segments should reflect current state, not historical state.*

---

**Mistake 12: Skipping the consent layer**

Uploading customer data without verifying consent status is not just a compliance risk — it is an active legal exposure in GDPR jurisdictions. The "I'll deal with consent later" approach has resulted in significant fines and platform bans.

*Fix: Before any CRM export, filter out contacts who have not given explicit consent for ad targeting. Store consent timestamps. Make this a mandatory step in your export pipeline.*

---

<!-- section: mental_models -->

### 5.11 The Mental Models That Make Everything Click

If you take only three things from this module, make it these:

---

**Mental Model 1: "You're managing relationships, not buying eyeballs"**

Traditional advertising buys attention from strangers. First-party data marketing manages relationships with known people at different stages:
- Strangers → Prospects → First-time buyers → Loyal customers → Champions → Advocates

Your job is to move each person one step forward — and use the right message and bid for where they currently are. A Champion doesn't need a discount. A lapsed customer does. An unqualified lead shouldn't receive another ad at all.

---

**Mental Model 2: "The algorithm is only as smart as the signals you give it"**

Smart Bidding on Google and Advantage+ on Meta are genuinely powerful optimization engines. But they optimize for the signal you give them. If you give them "form submits," they find form-fillers. If you give them "Closed-Won €500+ LTV customers," they find people like those.

Garbage signal → garbage optimization, no matter how much budget you put behind it.

First-party data is how you make the algorithm smarter than your competitors' algorithms.

---

**Mental Model 3: "Every audience has three potential uses — don't default to only one"**

When you build a new segment, ask all three questions before activating:

1. Should I TARGET this group with specific ads and messaging? (remarketing)
2. Should I EXCLUDE this group from other campaigns? (suppression)
3. Should I use this group as a MODEL to find new similar people? (lookalike seeding)

The answer is often yes to all three simultaneously. Your Champions list should be: an upsell remarketing audience, excluded from acquisition campaigns, and the seed for your best Lookalike.

---

## Appendix: Quick Policy & Date Reference

| Date | Platform | Event |
|---|---|---|
| August 2023 | Google | Similar Audiences fully removed from all campaigns |
| March 2024 | Google | Consent Mode v2 enforcement begins for EEA (UCP) |
| April 7, 2025 | Google | 540-day Customer Match membership duration enforced |
| May 2025 | Meta | Offline Conversions API (OCAPI) deprecated |
| July 21, 2025 | Google | Sites without Consent Mode v2 blocked from EEA data collection |
| July 2025 | Meta | CAPI v2 released |
| September 2025 | Google | `conversion_environment` required in all OCI uploads |
| September 2, 2025 | Meta | Custom Audience restrictions on health/financial status data |
| October 2025 | Google | Enhanced Conversions account-level auto-migration |
| December 9, 2025 | Google | Data Manager API generally available |
| April 1, 2026 | Google | Google Ads API for Customer Match disabled — Data Manager required |

---

## Regulatory Consent Summary

| Regulation | Jurisdiction | Consent Requirement | Ad Implication |
|---|---|---|---|
| GDPR | EU / EEA | Explicit opt-in for any personal data use | Must gate all PII collection; consent required before any audience matching |
| CCPA/CPRA | California (US) | Opt-out (right to say no); "sharing" for cross-context advertising requires disclosure | Honor Global Privacy Control signals; maintain opt-out mechanism |
| State privacy laws | 15+ US states | Varying; most follow CPRA model | Treat all US traffic with opt-out model as baseline |

> ⚠️ "Legitimate interest" is not a valid legal basis for advertising data processing under GDPR in most EEA member states as interpreted by current DPA enforcement. Use explicit consent for all ad-related data collection if serving EEA users.
