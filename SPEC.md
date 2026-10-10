# Property Portal — Project Specification

## 1. Product overview

Build an English-language property marketplace for people buying, selling, or renting property in Yangon and Mandalay, Myanmar. The first version supports web, API, and mobile clients.

## 2. Goals

- Let visitors find and compare properties for sale or rent in Yangon and Mandalay.
- Let property owners or authorized agents publish and manage listings.
- Make listing price, location, transaction type, and contact path clear.
- Provide consistent data and behavior through the API to web and mobile clients.
- Keep the first release English-only and MMK-only.

## 3. First-release scope

### In scope

- Property transactions: **Buy**, **Sell**, and **Rent**. Buy and Rent are discovery journeys; Sell is the journey for creating and managing a sale listing.
- Cities: **Yangon** and **Mandalay** only.
- English interface text only.
- Myanmar kyat (MMK) for all listing prices. No foreign-currency display or conversion.
- Responsive web client, REST API, and Expo-based mobile client.
- Public listing search and detail pages.
- Account registration and sign-in for listing owners/agents.
- Listing creation, editing, submission, and removal by its owner; administrators can review and manage listings.
- Listing inquiries using the contact information or contact action provided by the listing owner.

### Out of scope for the first release

- Cities or regions other than Yangon and Mandalay.
- Burmese or other translated interface catalogs.
- Non-MMK prices, currency conversion, payments, deposits, escrow, or rental billing.
- Map-based search, live chat, agent subscriptions, promoted listings, analytics dashboards, and recommendation algorithms.
- A guaranteed property transaction, ownership verification, or legal/valuation advice.

## 4. Users and permissions

- **Visitor:** browse, search, filter, and view active listings and their public contact options.
- **Registered owner/agent:** all visitor capabilities, plus create, edit, submit, and manage only their own listings.
- **Administrator:** review submitted listings, publish or reject them, and remove listings that violate marketplace rules.

Identity, permissions, and listing state must be enforced consistently across clients.

## 5. Core user journeys

### Find a property to buy or rent

1. Choose Buy or Rent.
2. Choose Yangon or Mandalay, with optional township and other available filters.
3. Browse matching active listings and refine or clear filters.
4. Open a listing to review its photos, price, location, property facts, description, and contact options.
5. Contact the owner/agent using the available contact action.

### Sell or offer a rental property

1. Register or sign in.
2. Create a listing, selecting Sale or Rent, city, and required property information.
3. Save a draft or submit it for administrator review.
4. See its current status and edit it when permitted.
5. View the listing publicly after it is approved and active.

### Moderate listings

1. An administrator opens the review queue.
2. The administrator reviews listing details and photos.
3. The administrator publishes or rejects the submission, with a reason for rejection.
4. The owner can see the resulting status and, if rejected, correct and resubmit the listing.

## 6. Listing requirements

Each listing should support:

- A title and descriptive text.
- Transaction type: `SALE` or `RENT`.
- Property category, such as apartment/condo, house, land, or commercial property.
- City (`YANGON` or `MANDALAY`) and optional township/address details.
- Asking price as a positive whole-number MMK amount. Store the amount as an integer; do not store formatted commas or a currency-converted value.
- Property facts appropriate to its category, such as bedrooms, bathrooms, floor area, land area, and furnishing status when applicable.
- One or more property photos, with an explicit primary image and stable ordering.
- Owner/agent contact method(s) and listing ownership.
- Creation and update timestamps, and a moderation/publication status.

Do not require residential-only facts for land or commercial listings. Public views must not expose private account credentials or internal moderation notes.

Suggested lifecycle: `DRAFT` → `PENDING_REVIEW` → `ACTIVE` or `REJECTED`. An owner may edit drafts and rejected listings and resubmit them. Only active listings appear in public search. Administrators may deactivate or remove active listings. The API must enforce valid transitions and permissions.

## 7. Functional requirements

### Public discovery

- Display a home/search entry point for Buy and Rent and a clear Sell entry point.
- Search active listings by keyword and filter by transaction type, city, township where available, property category, and price range.
- Provide useful empty, loading, and error states, and allow users to clear filters.
- Paginate or progressively load results so a large inventory does not require one oversized response.
- Sort results consistently, with newest listings available as the default ordering.

### Listing details

- Show the complete public listing information and ordered photo gallery.
- Format prices for readability as whole MMK amounts while retaining MMK as the only currency.
- Make city and available township information easy to find.
- Provide an actionable contact path without exposing non-public account data.

### Accounts and listing management

- Provide registration, sign-in, sign-out, and authenticated session handling.
- Provide an owner area for creating, editing, submitting, and reviewing the owner's own listings.
- Clearly distinguish draft, pending review, active, rejected, and removed/unavailable states.
- Protect owner operations from changes to listings belonging to another account.

### Administration

- Provide a protected review queue for pending submissions.
- Allow administrators to inspect the full listing and its photos, approve it, reject it with a reason, or deactivate/remove it.
- Record who performed a moderation action and when.

### English and MMK presentation

- All product interface labels, validation messages, and system-generated messages are in English in v1.
- Listing titles and descriptions are user-authored content and are displayed as entered; they are not machine-translated.
- Show `MMK` or `Ks` consistently next to prices. Do not show `$`, exchange rates, or converted equivalents.
- Accept whole-kyat amounts only; reject zero, negative, fractional, malformed, or out-of-range amounts with clear validation.

## 8. Quality requirements

- Layouts work on common mobile and desktop viewport sizes.
- Core flows are keyboard-operable and use semantic controls, visible focus, labels, and sufficient contrast.
- Photos have useful alternative text where meaningful; decorative imagery is hidden from assistive technology.
- Search and listing pages expose understandable loading, empty, validation, and failure states.
- API authorization is checked for each protected operation, including attempts to access another user's listing by changing an identifier.
- Store and transmit prices as whole MMK values without floating-point currency arithmetic.
- Do not seed or publish fabricated claims that a listing has been verified or guarantees a transaction.

## 9. Acceptance criteria

The first release is acceptable when:

1. A visitor can browse, search, and filter sale and rental listings in Yangon and Mandalay on web and mobile.
2. Listing cards and details show prices only in MMK and do not offer currency conversion.
3. A visitor can open a listing and use its available owner/agent contact method.
4. A registered owner/agent can create a draft, submit it, and track its moderation status.
5. An administrator can approve or reject a submission; approved listings become publicly discoverable, and rejected listings are not public.
6. Users cannot create, edit, or moderate listings beyond their permissions, even by calling the API directly.
7. All v1 interface/system copy is English, while user-entered listing text remains as authored.
## 10. Decisions to settle during implementation

The product scope does not yet specify exact contact methods, township catalogs, required photo limits, account recovery, or moderation policy details. Choose simple implementations for v1, document any change to user-visible behavior here, and do not add payment or currency-conversion flows without a revised product decision.
