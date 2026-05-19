---
status: non-authoritative source material
snapshot-date: 2026-05-18
ingested-on: 2026-05-19
origin: founder-supplied early context (pre-foundation)
authority: NONE — see ../README.md and ../../CLAUDE.md
---

> **Non-authority notice.** This file is raw source material, frozen as a dated snapshot. The source body below this notice is preserved as the ingested snapshot; it is not edited in place. It is **not** project authority, **not** an implementation spec, **not** a feature backlog, **not** an ADR, and **not** a milestone or work-package plan. It must not be used to derive tasks, code, schemas, or commitments.
>
> Promotion path: insights are synthesised into `docs/05-product-context.md`, `docs/06-feature-blueprint.md`, `docs/07-technical-direction.md`, ADRs, or milestone work-package task files **only through an explicit planning conversation** with the product owner. Presence here does not promote anything.
>
> Reading instruction for AI sessions: read for grounding only. Do not act on, cite as authority, or convert into deliverables without explicit direction. Legal, privacy, and GDPR statements inside this document are early product intent only and require separate review before promotion.
>
> Note on encoding: this snapshot was originally filed (2026-05-19) with UTF-8/Latin-1 mojibake in the supplied source text (apostrophes and the diagram arrows in particular); the body was re-decoded the same day in a follow-up refinement pass to restore typographic punctuation (em-dash, curly quotes, apostrophes, ellipsis, arrows). Textual content was not otherwise altered. The original mojibake version remains in Git history.

> **Tech-proposal-specific warning.** This document names concrete frameworks and providers (Flutter, Next.js, NestJS, PostgreSQL, OpenSearch, Redis / BullMQ, FCM, RevenueCat / Stripe, PostHog, separate AI services). These choices align with each repo's currently *proposed* direction, but **alignment is not approval**. None of these are adopted until validated through the normal `01-foundation` research and ADR process in each repo (framework-validation ADR, data/ORM ADR, search-strategy ADR, auth ADR, hosting/deployment research, etc.). The "modular monolith first" and "clients never talk to OpenSearch directly" architectural claims in this document are similarly unadopted until promoted via ADR.

---

# Proposed Technology Stack Decision Document

**Project:** Migrant-focused AI news, alerts, audio, and information platform  
**Document type:** Engineering proposal / primary technology decision record  
**Language:** English  
**Date:** 2026-05-18  
**Status:** Proposed stack, not final implementation specification

---

## 1. Purpose of This Document

This document summarizes the primary technology decisions discussed so far for the product. It is intended as a handoff document for engineers and developers who have no prior context from the conversation.

The goal is not to define every package, vendor, authentication method, payment flow, or deployment detail. Instead, the document defines the proposed core stack, the reasoning behind each choice, relevant alternatives, and the main concerns that should be considered during implementation.

The product is expected to support:

- Mobile apps on Android and iOS
- A web app/browser-accessible application
- Potential desktop apps on macOS, Windows, and Linux
- A public SEO/AEO website
- A robust backend serving mobile, web, agents, subscriptions, comments, notifications, and user data
- AI-generated and AI-processed multilingual content
- Search over thousands, then potentially millions, of articles, documents, summaries, and legal/practical information
- Audio and possibly video-related public/premium content
- Strong privacy and security expectations for users in Germany and the EU

---

## 2. High-Level Proposed Architecture

```text
Flutter App
- Android
- iOS
- Web app
- Later desktop if needed

Next.js Website
- Public SEO/AEO website
- Landing pages
- Public articles/brief pages
- Podcast/video index pages
- Multilingual public routes

NestJS Backend
- Core product API
- Auth/business logic
- User management
- Subscriptions/entitlements
- Feed/content APIs
- Comments/contributions
- Notification orchestration
- Agent ingestion API

PostgreSQL
- Main source-of-truth database

OpenSearch
- Search and retrieval layer
- Full-text + semantic/hybrid search

Object Storage + CDN
- Audio files
- Images
- Generated media
- Potential public media assets

Redis / Queue Layer
- Cache
- Rate limiting
- Background jobs
- Notification jobs
- Indexing jobs

External Providers
- Push notifications
- Payments/subscriptions
- Analytics/A-B testing
- Email delivery
```

Core principle:

> **PostgreSQL is the source of truth. NestJS owns business logic. OpenSearch owns search/retrieval. Next.js renders the public website. Flutter powers the application experience.**

---

## 3. Frontend Application: Flutter

### Proposed decision

Use **Flutter** for the application layer.

### Target surfaces

- Android
- iOS
- Browser-accessible app
- Later: macOS, Windows, Linux if justified by user demand

### Why Flutter fits this product

Flutter is a good match because the product needs to ship across multiple platforms while keeping the app experience consistent. The application will likely include:

- Personalized feed
- Audio player
- Saved/bookmarked items
- Search UI
- User preferences
- Notifications
- Premium access
- Potential lock-screen/widget-related experiences
- Future desktop availability

Flutter's value here is mainly speed of cross-platform product delivery from a shared codebase.

The official Flutter site describes Flutter as an open-source framework for building natively compiled, multi-platform applications from a single codebase, including mobile, web, desktop, and embedded experiences.  
Reference: https://flutter.dev/

### Why not separate native apps first

Separate native Android and iOS apps may offer maximum platform control, but would slow down the initial product significantly. For this project, time-to-market and consistent feature delivery across platforms matter more than native-only optimization at the beginning.

### Key concern

Flutter should be used for the **application**, not for the SEO-heavy public website. Flutter Web can serve the app experience, but the public website should be rendered separately for SEO/AEO.

---

## 4. Public Website Frontend: Next.js

### Proposed decision

Use **Next.js** for the public website.

### Website role

The website is not the main application and should not become the main backend. It should serve:

- Homepage
- Public landing pages
- SEO/AEO content pages
- Public article/brief pages
- Category pages
- City/language pages
- Public podcast pages
- YouTube/video index pages
- App-install and app-open CTAs

The website does not need to handle complex logged-in user behavior at the beginning. It should consume public content from the backend and render it in an indexable, fast, structured format.

### Why Next.js fits this product

Next.js is a good fit because the public website needs:

- Strong SEO metadata support
- Static and server-rendered pages
- Dynamic routing
- Multilingual routing
- Incremental Static Regeneration or similar update workflows
- Backend/API-driven page generation
- Ability to render many public pages without rebuilding the whole site every time

Next.js documentation states that its Metadata APIs can define application metadata for improved SEO and web shareability.  
Reference: https://nextjs.org/docs/app/getting-started/metadata-and-og-images

Next.js ISR enables updating static content without rebuilding the entire site, reducing server load and helping handle large numbers of content pages.  
Reference: https://nextjs.org/docs/pages/guides/incremental-static-regeneration

### Why not WordPress

WordPress was explicitly set aside. The product's content pipeline is expected to become automated through backend APIs and AI agents rather than manual editorial publishing through a traditional CMS.

### Why not Flutter Web for the public website

Flutter Web may be acceptable for the logged-in app, but it is not the preferred option for SEO/AEO-driven public pages. The website needs clean HTML rendering, metadata control, structured pages, and search-engine-friendly behavior.

### Alternative considered: Astro

Astro is a strong alternative for mostly static, content-driven sites. It may be simpler and faster for a purely static blog/landing site. However, the product's public site may become a dynamic product surface with language/city/category routes, API-triggered updates, audio previews, content previews, and structured SEO/AEO pages. For that reason, Next.js is preferred.

---

## 5. Core Backend: NestJS

### Proposed decision

Use **NestJS** as the core backend framework.

### Backend role

The backend is the core of the product. It should serve:

- Flutter app APIs
- Next.js public website APIs
- Agent ingestion APIs
- User authentication and account flows
- User preferences
- Subscriptions and entitlements
- Feed personalization
- Bookmarks
- Comments
- User contributions
- Moderation support
- Notification orchestration
- Analytics/event ingestion support
- Content visibility rules
- Access control for audio/premium content
- Search API mediation
- Public/private content boundaries

### Why NestJS fits this product

NestJS is a strong fit because the system needs a structured, scalable, API-first backend that is not too bare-bones but also not a rigid platform. It aligns well with:

- TypeScript
- Next.js ecosystem
- Modular backend architecture
- AI-assisted development
- API-first development for Flutter and public website
- Future maintainability
- Team readability

NestJS official documentation describes it as a framework for building efficient, scalable Node.js server-side applications, built with and fully supporting TypeScript.  
Reference: https://docs.nestjs.com/

The NestJS site also emphasizes modular architecture, flexibility, and scalable server-side applications.  
Reference: https://nestjs.com/

### Preferred backend shape

Start as a **modular monolith**, not microservices.

This means one main NestJS backend with clear modules, such as:

- Auth
- Users
- Content
- Feed
- Audio entitlement
- Subscription
- Notification
- Comments
- Contributions
- Analytics events
- Agent ingestion
- Public website API
- Search gateway

This avoids premature microservice complexity while keeping the codebase structured enough to split services later if needed.

### Why not Express-only

Express is too unstructured for the expected size of this backend. It is flexible, but the product needs guardrails, module boundaries, dependency injection patterns, validation structure, testing structure, and long-term maintainability.

### Alternatives considered

#### Django / Django REST Framework

Django is a serious alternative. It is mature, secure, batteries-included, and has a strong admin system. It is especially strong if internal admin/moderation/content-review workflows become a major early requirement.

Django official site describes Django as a high-level Python web framework that encourages rapid development and clean pragmatic design, taking care of much of the hassle of web development.  
Reference: https://www.djangoproject.com/

Why not primary choice now:

- Less aligned with the TypeScript/Next.js stack
- May feel heavier for an API-first mobile product
- The built-in admin is valuable, but the current direction is not CMS-first
- AI/Hermes services can still use Python separately

#### FastAPI

FastAPI is strong for AI/data-facing APIs and internal ML services. It is modern, high-performance, and Python-native.

FastAPI is described as a modern, fast web framework for building APIs with Python based on standard type hints.  
Reference: https://github.com/fastapi/fastapi

Why not primary core backend:

- Less batteries-included for full product backend needs
- Auth, permissions, billing, notification orchestration, moderation, and large product structure require more assembly
- Better suited for Hermes/AI services than the core product backend

#### Laravel

Laravel is a strong practical backend framework with mature auth, queues, notifications, billing ecosystem, and productivity. It is a serious option if the team prioritizes speed and batteries-included backend features.

Laravel has official documentation for authentication, queues, and notifications.  
References:  
https://laravel.com/docs/13.x/authentication  
https://laravel.com/docs/13.x/queues  
https://laravel.com/docs/13.x/notifications

Why not primary choice now:

- PHP stack is less aligned with Next.js/TypeScript
- AI-assisted development and shared TypeScript types are less straightforward
- Strong option, but less consistent with the selected frontend/web direction

#### AdonisJS

AdonisJS is a TypeScript backend framework with a more batteries-included style than NestJS.

AdonisJS describes itself as a batteries-included TypeScript framework with authentication, ORM, validation, mail, queues, cache, and testing working together.  
Reference: https://adonisjs.com/

Why not primary choice now:

- Smaller ecosystem and adoption compared with NestJS
- NestJS has broader community, enterprise usage, and architecture flexibility
- Still a legitimate fallback if the team wants a more Laravel-like TypeScript backend

#### Go frameworks

Go is excellent for high-throughput services, workers, notification dispatchers, crawlers, or performance-critical components. However, it is usually more bare-bones for a feature-heavy product backend and would likely slow down MVP development.

Why not primary choice now:

- More boilerplate for product features
- Fewer out-of-the-box product/backend conveniences
- Better reserved for future specialized services if needed

---

## 6. Main Database: PostgreSQL

### Proposed decision

Use **PostgreSQL** as the primary database and source of truth.

### Why PostgreSQL fits this product

The product data is highly relational:

- Users
- Preferences
- Languages
- Countries/states/cities
- Categories
- Articles
- Sources
- Translations
- Audio assets
- Subscriptions
- Comments
- Reports
- Bookmarks
- Notifications
- Content visibility
- Entitlements

PostgreSQL is a mature, reliable relational database with strong indexing, transactions, constraints, joins, and long-term operational trust.

PostgreSQL describes itself as a powerful, open-source object-relational database system with over 35 years of active development and a strong reputation for reliability, feature robustness, and performance.  
Reference: https://www.postgresql.org/

### Why not NoSQL as primary database

NoSQL was considered conceptually but is not preferred because the product has many relational entities and access-control rules. A document or key-value database could complicate consistency, permissions, reporting, analytics, and future migrations.

### ORM/data-access note

Use of an ORM is likely, but final selection is not locked. Prisma is a strong candidate because it is TypeScript-native and has official NestJS guidance.

Prisma's NestJS guide covers Prisma ORM and Prisma Postgres usage with NestJS.  
Reference: https://www.prisma.io/docs/guides/frameworks/nestjs

NestJS also has an official Prisma recipe.  
Reference: https://docs.nestjs.com/recipes/prisma

---

## 7. Search and Retrieval Layer: OpenSearch

### Proposed decision

Use **OpenSearch** as the long-term search and retrieval engine.

### Why search is a separate architectural concern

PostgreSQL should remain the source of truth, but it should not be expected to power the full long-term search experience by itself.

The app will eventually need to search across:

- Thousands to millions of articles
- Legal documents
- Practical guides
- Summaries
- Translations
- Source metadata
- City/state/category-specific information
- User-facing knowledge base entries
- Potential RAG/AI answer retrieval

The search experience should support:

- Filters by legal/financial/tax/transport/immigration topics
- Language-aware search
- City/state/category filtering
- Keyword search
- Semantic search
- Hybrid search
- Ranking and relevance tuning
- Retrieval even when the user does not know the exact keyword

### Why OpenSearch fits

OpenSearch supports semantic and hybrid search patterns that combine keyword and semantic search. Its documentation describes hybrid search as combining keyword and semantic search to improve relevance.  
Reference: https://docs.opensearch.org/latest/vector-search/ai-search/hybrid-search/index/

OpenSearch documentation also describes semantic search using text embedding models and vector indexes.  
Reference: https://docs.opensearch.org/latest/vector-search/ai-search/semantic-search/

OpenSearch vector search documentation includes raw vector search, semantic, hybrid, multimodal, neural sparse search, and RAG-related use cases.  
Reference: https://docs.opensearch.org/latest/vector-search/

### Proposed search architecture

```text
PostgreSQL
- Source of truth

Indexing Worker
- Reads published/indexable content
- Builds search documents
- Adds metadata and embeddings

OpenSearch
- Search index
- Keyword + semantic/hybrid retrieval

NestJS
- Search API
- Permission checks
- User-language filters
- Premium/public access rules
- Result shaping

Flutter / Next.js
- Calls NestJS search API only
```

The app should not talk directly to OpenSearch. All requests should go through NestJS for security, personalization, filtering, and access control.

### Alternatives considered

#### Meilisearch

Meilisearch is attractive for MVP because it is simple, fast, developer-friendly, and has strong out-of-the-box relevance. It also supports hybrid/AI retrieval features.

Meilisearch describes itself as a flexible, user-focused search engine for websites and applications.  
Reference: https://www.meilisearch.com/

Why not primary long-term choice:

- Simpler than OpenSearch, but possibly less flexible for large-scale legal/document-heavy retrieval
- May be excellent for MVP, but the product vision suggests a more advanced retrieval layer

#### Typesense

Typesense is also strong for fast product/app search and supports vector search and semantic/hybrid scenarios.

Typesense documentation describes vector search and nearest-neighbor search using embeddings.  
Reference: https://typesense.org/docs/30.2/api/vector-search.html

Why not primary long-term choice:

- Strong and simple, but OpenSearch offers a broader advanced-search and retrieval ecosystem
- Better as a lightweight alternative than the default long-term choice for this product vision

#### PostgreSQL full-text search only

PostgreSQL full-text search may be acceptable for an early prototype, but it is not enough for the intended semantic/hybrid, multilingual, document-heavy search experience.

### Recommendation nuance

If the team wants fastest MVP with low operational overhead, Meilisearch can be used temporarily. However, to avoid later migration pain and because search is strategically important, the proposal favors designing around OpenSearch from the beginning.

---

## 8. Cache and Background Jobs: Redis + BullMQ or Equivalent

### Proposed decision

Use **Redis** for cache/rate-limit/session-like fast data and **BullMQ** or an equivalent queue system for background jobs in the Node/NestJS ecosystem.

### Why this layer is needed

The backend will need asynchronous and background processing for:

- Notifications
- Search indexing
- Audio metadata updates
- Agent ingestion processing
- Content publication workflows
- Email jobs
- Subscription webhook processing
- Analytics/event batching
- Cache invalidation

Redis is widely used as a fast in-memory cache/data structure store.  
Reference: https://redis.io/docs/latest/develop/get-started/data-store/

BullMQ is a Node.js library implementing a queue system built on Redis.  
Reference: https://docs.bullmq.io/

NestJS documentation also covers queues and notes that Bull/BullMQ use Redis to persist job data and can support distributed queue architecture.  
Reference: https://docs.nestjs.com/techniques/queues

### Alternative

Temporal or another workflow engine could be considered later if workflows become complex, long-running, stateful, and business-critical. For the first backend, Redis + BullMQ is sufficient as a pragmatic default.

---

## 9. Media Storage and Delivery: Object Storage + CDN

### Proposed decision

Use object storage plus CDN for audio, images, and generated media.

Examples:

- AWS S3
- Cloudflare R2
- S3-compatible storage
- CDN in front of media assets

### Why not serve media directly from NestJS

The backend should not stream large audio/video files directly at scale. Its role should be:

- Check permissions
- Generate signed URLs
- Return media metadata
- Track play/access events
- Control entitlement

The files themselves should be served by object storage and CDN.

Cloudflare R2 supports an S3-compatible API, allowing applications to use S3 SDKs/tools with R2.  
References:  
https://developers.cloudflare.com/r2/api/s3/  
https://developers.cloudflare.com/r2/api/s3/api/

### Audio/video note

For audio, object storage + CDN is enough initially. For video, especially multiple resolutions/qualities, a dedicated transcoding and streaming approach may be required later. If public videos are hosted on YouTube, the website can index/embed them and avoid handling video delivery itself.

---

## 10. Push Notifications: External Push Provider, Likely FCM

### Proposed decision

Use an external push notification provider, likely **Firebase Cloud Messaging (FCM)**, with NestJS orchestrating notification logic.

### Why this fits

FCM is a cross-platform messaging solution for reliably sending messages.  
Reference: https://firebase.google.com/docs/cloud-messaging

The backend should own:

- User notification preferences
- Device tokens
- Notification rules
- Job scheduling
- Segmentation
- Notification event tracking
- Business rules

FCM should deliver messages to devices.

### Important boundary

Using FCM for push does not mean Firebase becomes the core backend. It is only an infrastructure provider for notifications.

---

## 11. Payments and Subscriptions: External Providers

### Proposed direction

Use established payment/subscription providers rather than building payment infrastructure directly.

Likely direction:

- RevenueCat for mobile in-app purchases/subscription management
- Stripe for web subscription/payment flows

### Why RevenueCat is relevant

RevenueCat supports in-app purchases and subscription management across mobile ecosystems and has Flutter SDK support.

Reference: https://www.revenuecat.com/  
Flutter SDK reference: https://www.revenuecat.com/docs/getting-started/installation/flutter

### Why Stripe is relevant

Stripe supports subscription billing and provides SCA-ready products/APIs for European payment authentication requirements.

References:  
https://docs.stripe.com/api/subscriptions  
https://docs.stripe.com/strong-customer-authentication

### Backend role

NestJS should not process cards directly. It should:

- Create payment/subscription sessions where needed
- Receive provider webhooks
- Update internal entitlements
- Expose subscription status to the app
- Enforce premium access rules

Final payment/provider selection is not part of this decision document.

---

## 12. Analytics, Feature Flags, and A/B Testing

### Proposed direction

Use an external product analytics and experimentation tool, with **PostHog** as a strong candidate.

### Why this is needed

The product will need to test and measure:

- Feature adoption
- Feed engagement
- Audio listen-through
- Search behavior
- Notification opt-in and opens
- Premium conversion
- User retention
- A/B tests on onboarding, paywall, article preview, and CTA flows

PostHog supports feature flags, safe rollouts, A/B testing, and remote configuration.  
Reference: https://posthog.com/docs/feature-flags

PostHog has Flutter documentation and feature flag support for Flutter.  
References:  
https://posthog.com/docs/libraries/flutter  
https://posthog.com/docs/feature-flags/installation/flutter

### Backend role

NestJS should store and process critical business events, but not attempt to replace a full analytics/experimentation platform in the beginning.

---

## 13. Security, Privacy, and EU/Germany Context

### Core concern

Because the product targets users in Germany and Europe and may process personal data, user preferences, locations/cities, subscription data, comments, and potentially sensitive migrant-related interactions, privacy and security must be treated as architecture-level concerns.

### Proposed principle

Adopt **privacy by design and by default** from the beginning.

The European Commission states that under EU data-protection law, data protection has to be built into the early stages of product design.  
Reference: https://commission.europa.eu/law/law-topic/data-protection/rules-business-and-organisations/obligations/what-does-data-protection-design-and-default-mean_en

GDPR Article 25 is specifically about data protection by design and by default.  
Reference: https://gdpr-info.eu/art-25-gdpr/

### Backend implications

The backend architecture should support:

- Minimal personal data collection
- Strong authentication
- Access control
- Role-based internal permissions
- Audit logs
- Secure password reset/email verification flows
- Encryption in transit
- Secure secrets management
- Data deletion/export workflows
- No unnecessary personal data in logs
- Clear separation of public/app-only/premium data
- Rate limiting and abuse protection
- Moderation tools for comments and user reports

### Framework note

NestJS can support security patterns, but GDPR/security compliance is not provided automatically by any framework. It must be implemented through architecture, process, hosting, data handling, and review.

---

## 14. AI/Hermes Agent Services

### Proposed decision

Keep AI generation/processing services separate from the core product backend.

### Role of AI/Hermes services

Hermes or similar agent systems may handle:

- Source monitoring
- Content extraction
- Fact structuring
- Summarization
- Translation
- Audio generation requests
- Categorization
- Prioritization
- Search-document enrichment
- Embedding generation

### How agents should interact with the backend

Agents should call controlled NestJS ingestion APIs.

```text
Hermes / AI Agent
→ NestJS Ingestion API
→ PostgreSQL source-of-truth storage
→ Queue/indexing jobs
→ OpenSearch indexing
→ App/website consumption
```

This keeps the AI pipeline separate from product/user/security logic.

### Possible AI-service framework

Python/FastAPI remains a strong option for internal AI services because of Python's ML/AI ecosystem. It is not selected as the core product backend, but may be selected for AI processing services.

---

## 15. What Was Explicitly Not Selected

### WordPress

Not selected for the website or backend. The project is expected to become automated/API-driven rather than manual CMS-driven.

### Firebase/Supabase/Appwrite as core backend

These were explicitly deprioritized or excluded for the core backend decision. They may still be used as infrastructure components where appropriate, such as FCM for push notifications, but not as the main backend/source of truth.

### Flutter Web for SEO website

Not selected for SEO/AEO public website rendering. Flutter remains selected for the application layer.

### PostgreSQL-only search

Not selected for long-term search. PostgreSQL remains the source of truth, but search/retrieval should be delegated to OpenSearch or another search engine.

### Microservices from day one

Not selected. A modular monolith is preferred first, with the option to split services later.

---

## 16. Proposed Stack Summary

| Layer | Proposed choice | Reason |
|---|---|---|
| Mobile/web/desktop app | Flutter | Cross-platform app delivery from one codebase |
| Public website | Next.js | SEO/AEO, static/server rendering, multilingual public pages, ISR |
| Core backend | NestJS | TypeScript, scalable API-first modular backend |
| Main database | PostgreSQL | Reliable relational source of truth |
| Search | OpenSearch | Full-text + semantic/hybrid search for large-scale retrieval |
| Cache/jobs | Redis + BullMQ/equivalent | Caching, queues, background processing |
| Media storage | S3/R2-compatible object storage + CDN | Scalable audio/image/media delivery |
| Push | FCM or equivalent | Cross-platform push delivery; backend keeps business logic |
| Payments | RevenueCat / Stripe direction | Avoid building payment/subscription infrastructure from scratch |
| Analytics/A-B | PostHog direction | Product analytics, feature flags, experiments |
| AI processing | Separate Hermes/FastAPI-style services | Keep AI pipeline separate from product backend |

---

## 17. Final Recommendation

The proposed stack is:

```text
Flutter
+ Next.js
+ NestJS
+ PostgreSQL
+ OpenSearch
+ Redis/BullMQ
+ Object Storage/CDN
+ External providers for push, payments, analytics, and experimentation
+ Separate AI/Hermes services
```

This is a balanced architecture for the product vision:

- Not too temporary
- Not too over-engineered
- Strong enough for long-term growth
- Clear separation of responsibilities
- Friendly to AI-assisted development
- Suitable for multilingual, content-heavy, user-account-based applications
- Suitable for future search-heavy and document-heavy use cases
- Compatible with strong privacy/security expectations in Germany/EU

The most important architectural decision is to avoid mixing responsibilities:

> **The backend owns business logic and security. PostgreSQL owns truth. OpenSearch owns retrieval. Next.js owns public SEO rendering. Flutter owns app UX. AI agents generate and enrich content through controlled APIs.**

---

## 18. Engineering Notes for Next Review

The next engineering decision document should define:

1. Backend module boundaries
2. API style: REST vs GraphQL vs hybrid
3. Auth approach
4. Subscription entitlement model
5. Search indexing schema
6. Public/private content visibility model
7. User privacy and GDPR data map
8. Hosting/deployment strategy
9. CI/CD and environment strategy
10. Monitoring/logging/observability stack

These are intentionally not finalized in this document.

---

## 19. References

### Flutter
- https://flutter.dev/

### Next.js
- https://nextjs.org/docs/app/getting-started/metadata-and-og-images
- https://nextjs.org/docs/pages/guides/incremental-static-regeneration

### NestJS
- https://docs.nestjs.com/
- https://nestjs.com/

### PostgreSQL
- https://www.postgresql.org/
- https://www.postgresql.org/docs/current/intro-whatis.html

### Prisma / NestJS
- https://www.prisma.io/docs/guides/frameworks/nestjs
- https://docs.nestjs.com/recipes/prisma

### OpenSearch
- https://docs.opensearch.org/latest/vector-search/
- https://docs.opensearch.org/latest/vector-search/ai-search/hybrid-search/index/
- https://docs.opensearch.org/latest/vector-search/ai-search/semantic-search/

### Meilisearch
- https://www.meilisearch.com/
- https://meilisearch.com/docs/capabilities/hybrid_search/overview

### Typesense
- https://typesense.org/docs/30.2/api/vector-search.html

### Redis / BullMQ
- https://redis.io/docs/latest/develop/get-started/data-store/
- https://docs.bullmq.io/
- https://docs.nestjs.com/techniques/queues

### Object Storage / Cloudflare R2
- https://developers.cloudflare.com/r2/api/s3/
- https://developers.cloudflare.com/r2/api/s3/api/

### Push Notifications
- https://firebase.google.com/docs/cloud-messaging

### Payments / Subscriptions
- https://www.revenuecat.com/
- https://www.revenuecat.com/docs/getting-started/installation/flutter
- https://docs.stripe.com/api/subscriptions
- https://docs.stripe.com/strong-customer-authentication

### Analytics / A-B Testing
- https://posthog.com/docs/feature-flags
- https://posthog.com/docs/libraries/flutter
- https://posthog.com/docs/feature-flags/installation/flutter

### Alternative Frameworks
- Django: https://www.djangoproject.com/
- FastAPI: https://github.com/fastapi/fastapi
- Laravel Authentication: https://laravel.com/docs/13.x/authentication
- Laravel Queues: https://laravel.com/docs/13.x/queues
- Laravel Notifications: https://laravel.com/docs/13.x/notifications
- AdonisJS: https://adonisjs.com/

### Privacy / GDPR
- European Commission, Data protection by design and by default: https://commission.europa.eu/law/law-topic/data-protection/rules-business-and-organisations/obligations/what-does-data-protection-design-and-default-mean_en
- GDPR Article 25: https://gdpr-info.eu/art-25-gdpr/

---

# End of Document
