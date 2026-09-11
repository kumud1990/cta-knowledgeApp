# GreenLeaf Gardens — Mini CTA Scenario (Experience Cloud Focus)

## Background

GreenLeaf Gardens is a fast-growing retailer of plants, gardening supplies, and landscaping
services operating across the US and Canada. Historically GreenLeaf sold exclusively through
brick-and-mortar stores, but the company now wants to move a large part of its business online
and build direct digital relationships with three distinct audiences: retail customers,
independent landscaping professionals who resell GreenLeaf products, and a network of nurseries
that supply plants to GreenLeaf.

GreenLeaf already uses Salesforce Sales Cloud and Service Cloud internally. The CIO has hired a
consulting firm to design a set of Experience Cloud sites (formerly Communities) that will serve
these external audiences, and has provided the following requirements. The internal CRM, ERP,
and inventory systems already exist and should be integrated with, not replaced.

## User Requirements

1. Anonymous visitors will browse a public catalog of products, care guides, and store
   locations without logging in, and this content must be indexed by search engines.
2. Retail customers will self-register, log in, place and track orders, submit support cases,
   and view their loyalty points balance and order history.
3. Retail customers may choose to register or log in using their existing Google or Apple
   account rather than creating a new username and password.
4. Landscaping professionals ("Pros") will log in to a separate branded experience where they
   can view wholesale pricing, place bulk orders, manage the leads GreenLeaf refers to them,
   and register their own end-customers for warranty tracking.
5. Pros must only see the leads, orders, and accounts that belong to their own company, and
   never those of other Pros.
6. Supplier nurseries will log in to submit available plant inventory, respond to purchase
   orders, and upload compliance certificates. Nurseries are a low volume, high-trust audience.
7. GreenLeaf's internal merchandising team must be able to author and publish catalog content,
   promotions, and care guides across all sites without a developer deployment.
8. GreenLeaf's support agents (internal, in Service Cloud) must be able to see and respond to
   cases submitted by customers and Pros from a single console.

## Access and Identity Requirements

1. Retail customers may number in the millions and are mostly infrequent, low-touch users.
2. Pros and their staff will actively transact and manage records inside the platform.
3. Nurseries need CRM object access but are few in number.
4. GreenLeaf wants a consistent login experience: a returning customer who created an account
   on the website should be able to use the same credentials in the mobile app.
5. Self-registration for retail customers must be automated; Pro and nursery accounts require
   GreenLeaf approval before access is granted.

## Content and Personalization Requirements

1. Each audience must see a distinctly branded site (logo, colors, navigation), but GreenLeaf
   wants to minimize duplicated build effort where possible.
2. Product content, care guides, and promotional banners should be authored once and reused
   across the public catalog, the customer site, and the Pro site.
3. Returning logged-in customers should see product recommendations and promotions tailored to
   their purchase history.
4. All sites must be fully usable on mobile devices.

## Data Visibility Requirements

1. Retail customers should only ever see their own orders, cases, and loyalty data.
2. Pros should see their company's shared data but not competitors' data.
3. Guest (unauthenticated) visitors must never be able to query customer or order records.
4. Sensitive fields — such as customer payment tokens and internal cost/margin data — must never
   be exposed on any external-facing site.

## Moderation and Trust Requirements

1. Customers and Pros can post product reviews and questions in a community discussion area.
2. GreenLeaf wants inappropriate content flagged and, where possible, blocked automatically.
3. GreenLeaf's brand team must be able to review and remove flagged content.

## Integration Requirements

1. Product catalog, pricing, and inventory data live in GreenLeaf's ERP and must be surfaced on
   the sites in near real time.
2. Orders placed on any site must flow to the ERP for fulfillment.
3. Loyalty point balances are calculated in a separate loyalty platform and must be displayed to
   the customer.

## Scalability Requirements

1. GreenLeaf expects to grow to 3M registered retail customers over the next 2 years.
2. Peak traffic occurs during spring and holiday promotions, when concurrent public browsing
   can spike to 10x normal levels.
3. There are approximately 8,000 Pro users across 1,500 Pro companies.
4. There are roughly 200 supplier nurseries.

## Governance and Delivery Requirements

1. The CIO wants to know how many separate sites are recommended and why.
2. The CIO wants a recommendation on the license type to use for each audience and the rationale
   behind each choice.
3. The CIO wants a recommendation on how to brand multiple sites while reusing content and
   components efficiently.
4. The CIO would like a high-level environment and release management approach for delivering the
   sites and promoting changes safely.

## Candidate System Landscape

1. Sales Cloud + Service Cloud (existing internal CRM)
2. Experience Cloud sites (public catalog, customer site, partner/Pro site, supplier site)
3. Salesforce CMS (shared content authoring and delivery)
4. Salesforce Identity / External Identity (customer and partner login, SSO, social login)
5. ERP (product, pricing, inventory, order fulfillment)
6. Loyalty platform (point balances)
7. Integration middleware (e.g., MuleSoft) for near-real-time ERP surfacing
8. Mobile access via responsive Experience Cloud sites

## Assumptions

- Public catalog and SEO requirements imply guest-user access with tightly controlled sharing.
- Millions of low-touch retail users point toward External Identity / customer login licensing;
  transacting Pros with CRM record ownership point toward partner-type licensing; the small,
  CRM-heavy nursery audience may warrant a different license again.
- Near-real-time ERP data on external sites implies an integration/middleware layer rather than
  direct point-to-point calls.
- Branding many sites while reusing content points toward shared CMS content and reusable
  Lightning components/templates.
