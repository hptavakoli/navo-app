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
> Note on encoding: this snapshot was originally filed (2026-05-19) with UTF-8/Latin-1 mojibake in the supplied source text; the body was re-decoded the same day in a follow-up refinement pass to restore German umlauts (ü/ä/ö), the euro sign (€), and typographic punctuation (em-dash, curly quotes, apostrophes, ellipsis, arrows). Textual content was not otherwise altered. The original mojibake version remains in Git history.

---

# Product Scope: AI-Powered Migrant Life Intelligence Platform for Germany

**Document type:** Product feature and functionality scope  
**Language:** English  
**Prepared on:** 2026-05-18  
**Source basis:** Founder conversation, uploaded startup dossier, and product-scope decisions made in the current conversation  
**Working name / positioning:** Migrant Life Intelligence Platform for Germany  
**Initial audience:** Farsi-speaking / Iranian immigrants and foreign residents in Germany  
**Expansion audience:** Arabic-speaking, Turkish-speaking, and other non-German-speaking immigrant communities in Germany; later, immigrants in other countries  

---

## 1. Purpose of This Document

This document defines the application's product scope from a feature and functionality perspective.

It is intended to help engineers, architects, product managers, product owners, designers, content operators, and future team members understand the application without needing prior context from the founder conversations.

This document covers:

- The purpose of the application.
- The target user experience.
- The product principles and boundaries.
- All known features and functions discussed so far.
- How users interact with the app.
- The information categories the app should support.
- The AI, search, audio, calendar, bookmark, social, trust, privacy, and admin requirements.
- The automated content creation, processing, and publishing pipeline.
- The pricing and monetization model.
- Reference links relevant to product functionality.

This document intentionally does **not** include:

- MVP selection.
- Development phases.
- Technical architecture decisions.
- Competitor analysis.
- Market-size analysis.
- Detailed legal analysis.
- Detailed implementation specifications.

Those topics should be handled in separate documents.

---

## 2. Product Definition

The product is an **AI-powered migrant life intelligence platform**, initially focused on foreigners and immigrants living in Germany, starting with the Iranian / Farsi-speaking community.

The application collects practical, life-affecting information from trusted and relevant sources such as:

- Official German federal sources.
- State-level sources.
- City and municipality sources.
- Immigration and public-administration authorities.
- Transport providers.
- Public warning and alert systems.
- NGOs and migrant-support organizations.
- Newsletters and trusted practical-information sources.
- Selected credible media, where appropriate.
- Community-reported information, clearly labeled by verification status.

The system then filters, categorizes, prioritizes, summarizes, translates, stores, makes searchable, and optionally converts this information into high-quality audio.

The product is **not simply a news app for immigrants**. Its stronger positioning is:

> A personal intelligence layer for immigrant life in Germany: it tells immigrants what changed, why it matters to them, whether they need to act, and where to verify the source.

Alternative positioning statements:

- Your daily practical briefing for life in Germany, in your own language.
- Important German updates for immigrants — summarized, translated, sourced, and listenable.
- Only the German updates that matter to your life, city, status, and language.

---

## 3. Core User Problem

Immigrants and foreign residents in Germany face practical information barriers:

- Important information is scattered across many channels.
- Many relevant sources are only in German.
- News volume is overwhelming.
- Most general news is irrelevant to a specific immigrant.
- Some important updates are easy to miss.
- Administrative, legal, transport, safety, and city-level changes can directly affect daily life.
- Users may not know which authority, office, website, or organization is responsible for a specific issue.
- A user may miss transport strikes, appointment changes, regulation changes, deadlines, or local warnings until it is too late.

The app solves this by turning fragmented German-language information into a personalized, source-linked, translated, concise, actionable, and optionally audio-first information experience.

---

## 4. Product Boundaries

The product must remain focused.

It should **not** become:

- A general news app.
- A generic technology-news feed.
- An IT-news feed.
- An entertainment news app.
- A general social network.
- A broad local-business directory.
- A directory of irrelevant places such as bakeries, restaurants, or generic shops.
- A feed that overwhelms users with millions of unrelated stories.
- A legal-advice app that replaces professional advice.
- A source of truth independent from official or cited sources.

The product should stay focused on:

- Immigrants and foreigners living in Germany.
- Practical life information.
- Administrative and legal updates that affect daily life.
- Immigration, residence, citizenship, public offices, and obligations.
- Local, state, and federal changes.
- Transport disruptions and route-relevant alerts.
- Public warnings and safety notices.
- Rights, responsibilities, and deadlines.
- Short, useful, source-linked updates in the user's preferred language.
- Audio and briefings that reduce the need to manually follow German-language sources.

---

## 5. Target Users and Personalization Context

### 5.1 Initial Target User

The first target user is a Farsi-speaking Iranian immigrant or foreign resident living in Germany.

Typical traits:

- May not be fluent in German.
- May not follow German news daily.
- Needs practical information that affects life in Germany.
- Wants concise summaries rather than long articles.
- Wants information in Persian/Farsi.
- May prefer listening over reading.
- Needs source links and trust signals.
- Cares about local relevance by city and state.

### 5.2 Expansion Users

The platform should be designed so it can later support:

- Arabic-speaking immigrants in Germany.
- Turkish-speaking immigrants in Germany.
- English-speaking foreigners in Germany.
- Other language groups.
- Migrants in other countries, if the model works in Germany.

### 5.3 Personalization Inputs

Users should be able to personalize the app by selecting or defining:

- Preferred app language.
- Reading language.
- Audio language.
- City.
- State / Bundesland.
- Country, initially Germany.
- Topics and categories.
- Daily-brief timing.
- Weekly-brief preferences.
- Notification preferences.
- Transport routes or lines they often use.
- Calendar preference.
- Saved/bookmarked items and custom bookmark buckets.

The product should minimize sensitive personal-data collection. It should personalize based on preferences, not based on unnecessary private documents or sensitive immigration records.

---

## 6. High-Level User Journey

### 6.1 First-Time User Journey

1. The user opens the application.
2. The user signs in with Apple or Google.
3. The user completes a short onboarding flow.
4. The app asks for language, city, state, topics, notification preferences, and brief preferences.
5. The user lands on a personalized feed.
6. The user can read, listen, search, bookmark, comment, or ask the AI assistant questions.

### 6.2 Daily Returning User Journey

1. The user receives a daily brief notification or opens the app manually.
2. The feed shows only relevant, prioritized items.
3. The user reads short translated summaries or taps play to listen.
4. The user can open a detail page for source links, affected groups, action steps, deadlines, and original source information.
5. The user can bookmark, highlight, share, report an error, or ask AI follow-up questions.

### 6.3 Search / Question Journey

1. The user asks a natural-language question, even without exact official German wording.
2. AI Search retrieves semantically relevant information.
3. Results show source-linked items, knowledge-base entries, and historical updates.
4. The user can open results, save them, highlight sections, or ask the chatbot for an explanation.

### 6.4 Audio Journey

1. The user taps “Listen” on an item or starts the daily/weekly briefing.
2. The app plays individual audio items sequentially.
3. The app adds pre-generated connector phrases between items.
4. The result feels like a personalized podcast without generating a unique full podcast file per user.

---

## 7. Authentication, Account, and Profile Scope

### 7.1 Authentication Requirements

The app should offer simple and secure authentication through trusted identity providers.

Required sign-in options:

- Sign in with Apple.
- Sign in with Google.

Possible future sign-in options:

- Facebook.
- X / Twitter.
- Other OAuth-based providers if there is strong user demand.

The app should **not** offer traditional email/password login managed by the startup.

Reasons:

- The product should avoid storing or managing user passwords.
- Security and ease of use are more important than offering many login methods.
- One-click provider-based login reduces friction.
- The user should not need to remember another password.

### 7.2 Firebase Authentication as a Candidate Product Requirement

The founder currently expects Firebase to be used behind the scenes for authentication, while the product may also have its own backend and PostgreSQL database for product data.

Product-level requirement:

- Authentication should be handled by a proven managed identity provider or equivalent secure identity system.
- The internal product backend should map the authenticated user identity to an internal user profile.
- The app should not store user passwords.
- The final architecture decision between Firebase Auth, custom auth, or another identity provider should be handled separately in the technical architecture document.

Reference notes:

- Firebase Authentication supports federated identity providers such as Google, Apple, X, Facebook, GitHub, and others.
- Firebase Authentication can integrate with a custom backend through industry-standard identity flows.
- Firebase supports Sign in with Apple through the Apple ID OAuth flow.

### 7.3 Minimal User Profile

The user account should collect only what is necessary.

Allowed profile fields:

- Internal user ID.
- Authentication provider ID.
- Display username for comments.
- Optional profile image/avatar.
- Preferred language.
- City.
- State.
- Selected content categories.
- Notification preferences.
- Brief timing preferences.
- Calendar preference.
- Bookmark buckets.
- Subscription status.

Avoid collecting unless later clearly justified:

- Passwords.
- Exact home address.
- Passport number.
- Residence-permit number.
- Immigration-case details.
- Legal documents.
- Date of birth.
- Sensitive identity information.
- Health information.
- Financial records.

### 7.4 Username and Comment Identity

For comments and community interaction:

- Users can define a public display username.
- Users can optionally upload or select a profile image.
- The display username can be different from the legal name connected to their Apple/Google account.
- The app should not expose private identity-provider information publicly.

### 7.5 Account Controls

Users should be able to:

- Sign out.
- Change their display username.
- Change their profile image.
- Change language/city/state/topic preferences.
- Delete bookmarks and highlights.
- Delete search/chat history if stored.
- Delete their account.
- View what data is stored for personalization.

---

## 8. Onboarding and Preferences

### 8.1 Onboarding Goal

Onboarding must be short but strong enough to power personalization.

The app should learn:

- Preferred language.
- Current city.
- Current state.
- Main topics of interest.
- Whether the user wants daily briefs.
- Preferred daily brief time.
- Whether the user wants weekly summaries.
- Whether the user wants push notifications.
- Whether the user wants route-specific transport alerts.
- Preferred calendar format.

### 8.2 Example Onboarding Flow

1. Choose language: Farsi, Arabic, Turkish, English, German, etc.
2. Choose city: Berlin, Hamburg, Munich, Cologne, Düsseldorf, etc.
3. Choose state: Berlin, NRW, Bavaria, etc.
4. Choose topics: residence, citizenship, transport, job, tax, housing, health insurance, public alerts, events.
5. Choose brief time: morning, noon, evening, or custom.
6. Choose audio preference: text only, audio for important items, daily podcast-like playback.
7. Choose notifications: urgent only, daily brief, category-specific, transport-specific.

### 8.3 Preference Editing

All onboarding choices should be editable later in Settings.

---

## 9. Content Domains and Categories

The content should focus on information that matters to immigrants and foreign residents in Germany.

### 9.1 Immigration and Residence

Examples:

- Visa changes.
- Residence permits.
- Residence-title rules.
- Ausländerbehörde updates.
- Appointment delays.
- Passport-related issues for immigrants.
- Asylum or refugee-related information where relevant.
- Rules, deadlines, and process changes.

### 9.2 Citizenship and Naturalization

Examples:

- Naturalization law changes.
- Dual citizenship changes.
- Eligibility changes.
- Processing delays.
- City/state implementation updates.
- Official authority announcements.

### 9.3 National, State, and City Regulations

Examples:

- Federal law changes.
- State-level rule changes.
- City-level administrative updates.
- Local regulation changes affecting daily life.

### 9.4 Public Administration

Examples:

- Bürgeramt updates.
- Ausländerbehörde updates.
- Jobcenter information.
- Citizenship authority updates.
- Office closures.
- Appointment systems.
- New forms or process changes.
- Which organization is responsible for which issue.
- Where to call, write, or go.

### 9.5 Alerts and Incidents

Examples:

- City-level alerts.
- State-level alerts.
- Civil-protection warnings.
- Safety warnings.
- Emergency updates.
- Police warnings where relevant.
- Weather and flood warnings where relevant.

### 9.6 Transport and Mobility

Examples:

- Train cancellations.
- Strikes.
- Local transport disruptions.
- Deutsche Bahn disruptions.
- S-Bahn, U-Bahn, tram, bus alerts.
- Transit-line-specific alerts.
- Passenger-rights and compensation information.
- Warnings about routes the user often uses.

### 9.7 Rights and Obligations

Examples:

- Basic rights in Germany.
- Practical obligations and deadlines.
- Rights related to work, housing, discrimination, social benefits, and administration.
- Information users should know before dealing with authorities.

### 9.8 Daily-Life Administrative Changes

Examples:

- Health-insurance changes.
- Tax and employment changes affecting foreigners.
- Housing and rental updates.
- Social-benefit or Jobcenter updates.
- Education or certificate-recognition updates.
- Family-reunification-related practical information.

### 9.9 Community-Reported Updates

Examples:

- Local user reports about office delays.
- Reports of changed processes at public offices.
- Community experiences with appointments.
- Local issues affecting immigrants.

These must be clearly labeled as community-reported and unverified unless verified.

### 9.10 Events and Local Notices

Examples:

- Migrant-community events.
- Integration events.
- Legal-information sessions.
- NGO events.
- Cultural events relevant to target communities.
- Sponsored local notices if clearly labeled.

### 9.11 Calendar Events

Examples:

- German public holidays.
- Important community holidays.
- Local administrative deadlines.
- Migrant-related public events.
- App-generated reminders.
- User-saved event reminders.

---

## 10. Source and Content Intelligence System

### 10.1 Source Types

The system should support multiple source types:

- Official federal sources.
- Official state sources.
- Official city / municipality sources.
- Transport providers.
- Public warning systems.
- NGOs.
- Trusted newsletters.
- Selected credible media.
- Community reports.
- Internal knowledge-base entries.

### 10.2 Source Metadata

Each source should have metadata such as:

- Source name.
- Source URL.
- Source type: official, authority, transport, NGO, media, community, internal.
- Country.
- State.
- City.
- Topic categories.
- Language of original source.
- Reliability level.
- Update frequency.
- Whether source reuse requires special handling.
- Whether source has RSS/API or must be manually monitored.

### 10.3 Content Processing Functions

The backend intelligence layer should be able to:

- Collect or ingest new source items.
- Detect new and changed content.
- Identify duplicates and related stories.
- Categorize by topic.
- Detect geographic relevance: country, state, city, office, route.
- Detect urgency and priority.
- Detect whether the information is an alert, ordinary update, regulation change, administrative update, event, or community report.
- Summarize content.
- Translate content.
- Generate audio.
- Generate or assign visual assets.
- Store processed outputs.
- Link every item back to its source.
- Reuse translated/audio outputs across users.

### 10.4 Caching and Reuse

Generated outputs should be reused where possible.

Example:

- If one Persian audio file is generated for a news item, all Persian-speaking users who receive that item should use the same audio file.
- The app should not generate a new audio file per user unless the content is truly user-specific.
- This supports scalability and cost control.

---


## 11. Automated Content Creation, Processing, and Publishing Pipeline

### 11.1 Purpose and Role in the System

The application requires a dedicated **automated content creation, processing, and publishing pipeline**.

This pipeline is not just an isolated backend task. It is a core product system block that turns external information into usable, source-linked, translated, listenable, searchable, and publishable product content.

The pipeline should support the product's long-term direction:

- Start with manual or semi-manual content collection and review if needed.
- Gradually move toward automated source monitoring, scheduled ingestion, AI-assisted processing, and controlled publishing.
- Communicate with the backend, content database, search system, audio system, image generation layer, notification system, admin panel, and public publishing channels.
- Keep human review available for high-risk or sensitive categories.

The product should treat this pipeline as a first-class part of the system, because the quality, freshness, trustworthiness, and scalability of the application depend on it.

### 11.2 Content Input Sources

The pipeline should be able to ingest or monitor content from sources such as:

- Official German federal websites.
- State-level websites.
- City and municipality websites.
- Immigration and public-administration authorities.
- Transport-provider websites and service updates.
- Public alert and safety-warning systems.
- NGO and migrant-support organization websites.
- Newsletters and mailing lists.
- RSS feeds and official APIs where available.
- Selected credible media sources, where source policy allows.
- Community reports submitted by users, after moderation and verification handling.
- Internal knowledge-base updates created by the team.

The pipeline should always preserve source metadata and source links so that user-facing content remains verifiable.

### 11.3 Automation Modes

The pipeline should support multiple levels of automation:

- **Manual creation:** team members manually add, rewrite, translate, review, and publish content.
- **Semi-automated processing:** operators add or approve source items, while AI assists with summarization, translation, tagging, and formatting.
- **Scheduled automation:** cron jobs or scheduled workers periodically check sources and process new items.
- **Source monitoring:** the system detects new or changed content from configured sources.
- **Agent-assisted workflows:** AI agents can assist with discovery, extraction, classification, summarization, translation, enrichment, quality checks, and publishing preparation.

A Hermes-style AI agent can be considered as an example category for this kind of automated or semi-automated workflow, but this document does not make a final vendor or technology decision.

### 11.4 Ingestion and Normalization

For each source item, the pipeline should be able to:

- Fetch, scrape, crawl, import, or receive the content through the most appropriate allowed method.
- Extract the main text or structured content.
- Extract the source title.
- Extract the canonical source URL.
- Extract publication and update timestamps where available.
- Detect original language.
- Detect source type.
- Normalize content into the internal content format.
- Store raw-source metadata separately from generated user-facing content.
- Record ingestion status and processing errors.

The product should distinguish between raw source material, internally processed content, translated content, audio assets, visual assets, and published user-facing items.

### 11.5 Duplicate Detection, Clustering, and Change Detection

The pipeline should reduce noise and avoid publishing the same information multiple times.

It should support:

- Duplicate detection.
- Similar-story clustering.
- Detection of updates to previously processed content.
- Detection of superseded or outdated content.
- Grouping multiple sources around the same topic when useful.
- Keeping a history of important changes.

Example:

If several sources report the same transport strike, the user should not receive five separate repetitive alerts unless each one adds distinct local or route-specific value.

### 11.6 Classification, Prioritization, and Enrichment

After ingestion, each item should be classified and enriched.

The pipeline should identify:

- Topic category and subcategory.
- Country, state, city, office, route, or authority relevance.
- Affected user group.
- Urgency level.
- Whether action may be required.
- Important dates or deadlines.
- Whether the content is an alert, administrative update, legal/regulatory change, event, guide, media report, or community report.
- Source reliability and verification status.
- Whether human review is required before publishing.

This enrichment is what makes the product more valuable than a simple translated news feed.

### 11.7 Rewriting, Rephrasing, and Summarization

The pipeline should transform long or complex source material into concise, practical, user-facing content.

The system should be able to:

- Rephrase source material into original wording.
- Produce short summaries.
- Produce longer detail-page summaries when needed.
- Explain why the information matters.
- Identify who is affected.
- Identify where the update applies.
- Identify whether the user may need to act.
- Preserve key dates, deadlines, warnings, and source context.
- Avoid copying source articles verbatim.
- Avoid making unsupported claims beyond the source.

The output should be practical and action-oriented, not generic news rewriting.

### 11.8 Translation and Localization

The pipeline should translate processed content into supported user languages.

Initial priority:

- Persian / Farsi.
- English, where needed.

Future language support may include:

- Arabic.
- Turkish.
- Other immigrant-community languages.

Translation should preserve important German administrative terms when needed, especially when exact official wording matters.

Examples of terms that may require careful handling:

- Aufenthaltstitel.
- Ausländerbehörde.
- Einbürgerung.
- Bürgergeld.
- Jobcenter.
- Anmeldung.

The product should support terminology consistency across summaries, chatbot answers, knowledge-base entries, audio scripts, and public posts.

### 11.9 Reference, Metadata, and Source Attribution

Every generated content item should remain connected to its source information.

The pipeline should attach metadata such as:

- Original source URL.
- Source name.
- Source type.
- Original publication date.
- Processing date.
- Last updated date.
- Language of original source.
- Translation language.
- Reliability label.
- Verification label.
- Location relevance.
- Affected group.
- Important dates or deadlines.
- Whether the item has been manually reviewed.
- Whether the item has been corrected or superseded.

This is required for trust, user verification, search, history, correction workflows, and admin review.

### 11.10 Audio Generation

The pipeline should generate human-like AI audio from processed summaries and brief scripts.

Audio generation should support:

- Audio per individual content item.
- Daily brief audio.
- Weekly brief audio.
- Podcast-like playback using per-item audio assets.
- Multiple languages.
- Reuse of the same generated audio file across all relevant users.
- Regeneration if a summary or translation is corrected.

Audio should be treated as a product asset connected to the content item, language, version, and publication status.

### 11.11 AI-Generated Images and Visual Assets

The pipeline should support generating or assigning visual assets for content items.

Purpose:

- Make the feed and public content more visually attractive.
- Avoid relying on copyrighted images from source websites.
- Create a consistent visual identity for the product.
- Support app, website, newsletter, social, and video/podcast-channel presentation.

Visual generation should respect trust and safety:

- Images should not misrepresent real events.
- AI-generated visuals should not imply they are documentary photos when they are illustrative.
- Official logos, government marks, or institutional branding should not be used in a misleading way.
- Sensitive topics should use neutral, non-sensational visuals.

### 11.12 Publishing Destinations

The pipeline should prepare content for multiple publishing surfaces.

Internal product destinations:

- Personalized app feed.
- Detail pages.
- Daily brief.
- Weekly brief.
- Search index.
- AI chatbot / RAG knowledge layer.
- Information Center entries.
- Notification payloads.
- Calendar and event surfaces where relevant.

External/public destinations:

- Website pages.
- Newsletter.
- Telegram or WhatsApp community channels if used.
- YouTube weekly or monthly summaries.
- Spotify / podcast feeds.
- YouTube Music podcast distribution.
- Social snippets or short-form posts.

The public channel output should remain more general, while the application remains the place for personalized, daily, city/state/language-specific and premium experiences.

### 11.13 Human Review and Approval

The pipeline should allow human review before publication, especially for sensitive or high-impact categories.

Human review should be considered for:

- Immigration and residence updates.
- Citizenship and naturalization updates.
- Asylum-related information.
- Jobcenter and social-benefit information.
- Tax and employment obligations.
- Safety warnings.
- Legal or administrative deadlines.
- Highly local authority-process updates.
- User/community reports.

Review actions may include:

- Approve.
- Reject.
- Edit summary.
- Edit translation.
- Mark as requiring source verification.
- Mark as outdated.
- Request regeneration.
- Add correction note.

### 11.14 Backend and System Communication Requirements

The pipeline should communicate with other parts of the application system.

At product level, it should be able to send or expose processed content to:

- Backend content services.
- PostgreSQL or the selected product database.
- Search and semantic-search indexes.
- Translation storage.
- Audio storage.
- Image/visual asset storage.
- Notification services.
- Admin and moderation tools.
- Subscription and entitlement logic where audio or premium content is involved.
- Public publishing systems.
- Analytics and monitoring systems.

This document does not define the final technical architecture, but the product requirement is clear: the content pipeline must be integrated with the rest of the system rather than operating as a disconnected script.

### 11.15 Monitoring, Error Handling, and Operational Visibility

The pipeline should provide operational visibility for the internal team.

Operators should be able to see:

- Which sources were checked.
- When a source was last checked.
- Whether ingestion succeeded or failed.
- Which items were processed.
- Which items are waiting for review.
- Which items were published.
- Which items failed summarization, translation, audio generation, or image generation.
- Which outputs were corrected.
- Which content has become outdated or superseded.

This visibility should connect to the admin, operations, and moderation panel.

### 11.16 Product-Level Output Requirements

For each processed item, the pipeline should be able to produce or prepare:

- User-facing title.
- Short summary.
- Detail summary.
- Affected group.
- Location relevance.
- Why it matters.
- Action required.
- Important dates or deadlines.
- Original source link.
- Source and reliability labels.
- Translation.
- Audio script.
- Audio file.
- Visual asset.
- Searchable text.
- Notification text if relevant.
- Public-channel version if relevant.
- Version and correction metadata.

This ensures that a single source item can become a complete, reusable product content object across the app, website, audio experience, search system, and public acquisition channels.

---

## 12. Feed and Reading Experience

### 12.1 Personalized Feed

The app's main screen should show a personalized feed of practical updates.

Feed ranking should consider:

- User language.
- User city.
- User state.
- User selected categories.
- Urgency.
- Source reliability.
- Freshness.
- Whether action may be required.
- Whether the item affects many users.
- Whether it is route-specific or location-specific.

### 12.2 Feed Item Requirements

Each feed item should be:

- Short.
- Summarized.
- Translated into the selected language.
- Categorized.
- Prioritized.
- Source-linked.
- Easy to scan.
- Marked by urgency or action need where relevant.

### 12.3 Feed Item Template

Each item should ideally include:

1. Title.
2. Short summary.
3. Category.
4. Source type label.
5. Reliability label.
6. Country/state/city relevance.
7. Who is affected.
8. Why it matters.
9. Whether action is required.
10. Important date or deadline.
11. Original source link.
12. Last updated timestamp.
13. Listen button.
14. Bookmark button.
15. Share button.
16. Report error button.

### 12.4 Detail Page

The detail page should show:

- Full translated summary.
- Original source link.
- Original language/source metadata.
- “What changed?” explanation.
- “Who is affected?” explanation.
- “Where does it apply?” explanation.
- “Do I need to act?” section.
- Important dates or deadlines.
- Related items.
- Related knowledge-base entries.
- Listen button.
- Bookmark/highlight tools.
- Comment section if enabled.
- Error-reporting option.

### 12.5 Original Source Access

Users should always be able to open the source.

The app should make clear:

- The platform summarizes and translates information.
- The original source remains the verification point.
- High-stakes items should be checked at the source before action.

---

## 13. Daily Brief, Weekly Brief, and Summary Experience

### 13.1 Daily Brief

The daily brief is a personalized daily summary of relevant updates.

Possible schedule options:

- Morning.
- Noon.
- Evening/night.
- Custom time.

The daily brief should consider:

- User language.
- City and state.
- Selected topics.
- Urgent alerts.
- New changes since the last brief.
- Items requiring action.

### 13.2 Weekly Brief

The weekly brief is for users who do not follow updates daily.

It should summarize:

- Important weekly changes.
- Top items by category.
- Administrative and legal updates.
- Transport and public alerts.
- Items with deadlines.
- Community or event highlights.

### 13.3 Monthly Summary

Monthly summaries may be used for public channels and broader information packaging.

They should remain focused on practical migrant-life information, not general news.

---

## 14. Audio and Podcast-Like Playback

### 14.1 Audio Per Item

The system should generate high-quality audio for individual summaries in the user's selected language.

Audio can be used for:

- One article at a time.
- Daily brief playback.
- Weekly summary playback.
- Public podcast episodes.

### 14.2 Podcast-Like Daily Player

Instead of generating a unique full podcast file for each user, the app should create a podcast-like experience dynamically:

- Generate audio per item.
- Store/reuse per item and language.
- Play relevant items sequentially.
- Add pre-generated connector phrases.
- Add a daily intro.
- Optionally add a closing message.

Example intro:

> Today is Monday, [date], in Berlin. Here are the most important updates for you.

### 14.3 Connector Phrases

Connector phrases can be pre-generated in multiple languages.

Examples:

- “Next, an update about public transportation.”
- “Now, a citizenship-related change.”
- “This next item may require action.”
- “Finally, one local notice for your city.”

### 14.4 Weekly Conversational Podcast

The weekly podcast can be more conversational, similar to AI-generated explanatory podcast formats.

It should still:

- Stay source-linked.
- Focus on practical information.
- Avoid unsupported claims.
- Avoid becoming entertainment-only content.

### 14.5 Audio Monetization

Audio is expected to be a premium value driver.

Premium audio features may include:

- Audio for every item.
- Daily podcast-like playback.
- Weekly podcast inside app.
- Continuous listening queue.
- Offline audio for saved items.

---

## 15. AI Features and Support Layers

The app should have multiple AI-powered layers, all grounded in internal source-linked data.

### 15.1 AI-Powered Summarization

AI should summarize source content into short, practical, user-friendly updates.

Summaries should preserve:

- Key facts.
- Dates and deadlines.
- Affected groups.
- Geographic scope.
- Action requirements.
- Source attribution.

### 15.2 AI-Powered Translation

The system should translate content into the user's selected language.

Important languages:

- Farsi/Persian.
- Arabic.
- Turkish.
- English.
- German.
- Other languages later.

Translation requirements:

- Preserve official terms.
- Use consistent terminology.
- Allow users to view original text where possible.
- Allow users to report bad translations.
- Maintain a glossary for key terms such as Aufenthaltstitel, Einbürgerung, Bürgergeld, Jobcenter, Bürgeramt, Ausländerbehörde, etc.

### 15.3 AI Search

AI Search is a powerful semantic search layer.

It should support:

- Natural-language queries.
- Semantic matching.
- Multilingual search.
- Search even when the user does not know exact German keywords.
- Search across summaries, official references, stored items, knowledge-base entries, events, and historical changes.

Example query:

> What is the latest situation about getting a passport as an immigrant from Iran in Berlin?

Expected behavior:

- Retrieve relevant current items.
- Show recent changes.
- Show source-linked information.
- Explain if the answer is uncertain or depends on an authority.
- Offer related knowledge-base entries.

### 15.4 AI Chatbot / RAG Chat

The chatbot should answer user questions based on the platform's stored and source-linked information.

It should:

- Retrieve relevant chunks from the internal knowledge base and content archive.
- Answer in the user's preferred language.
- Cite or link sources.
- Show last-updated information.
- Avoid giving unsupported legal advice.
- Clearly say when it cannot answer confidently.
- Recommend checking the official source for high-stakes actions.

### 15.5 AI Support Agent

The AI Support Agent is a higher-level assistant experience inspired by AI support-agent systems such as Snowie.ai.

Its role in this product:

- Help users navigate app content.
- Answer questions based on internal data.
- Guide users to relevant articles, sources, calendar events, or official pages.
- Explain practical steps when supported by source-linked data.
- Potentially support voice-based interaction later.

The agent should not freely invent answers. It should operate against the platform's internal source-linked knowledge base.

### 15.6 AI Safety Layer

AI answers should include safety controls:

- Source links.
- Last updated date.
- Confidence or reliability indication.
- “This is not legal advice” notice for legal/immigration topics.
- Escalation to original source or professional support when needed.
- Refusal to answer if there is insufficient information.
- Error-reporting option.

### 15.7 AI-Generated Images

The app may generate original images for items.

Reasons:

- Avoid using copyrighted images from source websites.
- Make the feed visually attractive.
- Create a consistent visual identity.
- Support public social/channel distribution.

Image generation should:

- Avoid misleading realism for sensitive topics.
- Avoid implying official endorsement.
- Be clearly decorative when appropriate.
- Match brand style.

---

## 16. Search, Historical Knowledge, and Information Memory

### 16.1 Searchable History

The app should keep a searchable archive of processed content.

Users should be able to find:

- Current updates.
- Previous changes.
- Historical evolution of a topic.
- Related official source links.
- Practical explanations.

### 16.2 Historical Change Tracking

The system should understand that laws, office processes, transport rules, and public policies change over time.

Search results should distinguish:

- Current information.
- Old information.
- Superseded information.
- Items that may require re-checking.

### 16.3 Knowledge Graph / Structured Knowledge Potential

Over time, the app can connect:

- Authorities.
- Offices.
- Topics.
- Cities.
- Documents.
- Rights.
- Obligations.
- Deadlines.
- User questions.
- Related changes.

This supports the transition from simple feed to queryable migrant-life knowledge platform.

---

## 17. Information Center / Single Source of Truth

### 17.1 Purpose

The Information Center should help users answer stable or semi-stable questions such as:

- Which authority is responsible for this issue?
- Where should I find official information?
- Who should I call?
- What are my rights?
- Which office is relevant?
- What is the official process?
- Which documents are usually needed?

### 17.2 Content Types

The Information Center can contain structured entries for:

- Ausländerbehörde.
- Bürgeramt.
- Jobcenter.
- Citizenship authorities.
- Immigration-related organizations.
- NGOs.
- Contact points.
- Official pages.
- Rights and obligations.
- Process explainers.
- Glossary terms.
- Checklists.

### 17.3 Boundary

The Information Center must not become a generic directory of all businesses or places.

It should remain focused on migrant-life, public-administration, rights, legal-process, transport, alert, and practical-support information.

---

## 18. Bookmarks, Buckets, and Highlighting

### 18.1 Standard Bookmarking

Users should be able to bookmark:

- Feed items.
- Detail pages.
- Search results.
- Knowledge-base entries.
- AI answers if saved.
- Calendar events.
- Community reports.
- Official source explainers.

### 18.2 Bookmark Buckets

Users should be able to organize bookmarks into custom buckets/folders.

Requirements:

- Create bucket.
- Rename bucket.
- Delete bucket.
- Move bookmark between buckets.
- Add one bookmark to multiple buckets if desired.
- Use default bucket if no custom bucket is selected.

Example bucket names:

- Residence.
- Citizenship.
- Jobcenter.
- Tax.
- Transport.
- Housing.
- Read later.
- Important deadlines.

### 18.3 Highlight Bookmarking

For long articles, guides, or official content, users should be able to highlight a selected section and save only that section.

Behavior:

1. User selects text or a content area.
2. User taps “Save highlight.”
3. App stores the highlighted section and its parent article/content item.
4. When the user opens the saved highlight later, the app opens the full article and scrolls to the highlighted section.
5. The selected part appears visually highlighted.

### 18.4 Bookmark Metadata

Each bookmark/highlight should store:

- User ID.
- Parent content ID.
- Source URL if applicable.
- Timestamp.
- Bucket ID.
- Highlight text or position.
- Language viewed.
- Original language/source reference if relevant.
- Optional user note.

### 18.5 Optional User Notes

Users may later be able to add private notes to bookmarks or highlights.

Example:

> Need to check this before my appointment.

---

## 19. Calendar and Events

### 19.1 Calendar Purpose

The calendar should help users see relevant dates, events, holidays, deadlines, and reminders inside the app.

It should not become a general-purpose calendar replacement.

### 19.2 Calendar Systems

The app should support multiple calendar views:

- Gregorian / international / European calendar.
- Persian / Jalali calendar.
- Arabic / Hijri calendar.
- Chinese calendar as a possible later option.

### 19.3 Calendar Content

Calendar entries may include:

- German public holidays.
- State-specific holidays.
- City-level events.
- Migrant-community events.
- Integration events.
- NGO events.
- Legal-information sessions.
- Important administrative deadlines.
- User-saved reminders.
- Deadlines extracted from feed items.
- Weekly/monthly summary release dates.

### 19.4 Event Detail Page

Each event should include:

- Title.
- Date and time.
- Calendar equivalent in selected calendar system.
- Location or online link.
- Organizer.
- Category.
- Language.
- Description.
- Source link.
- Save/bookmark button.
- Add reminder option.
- Share option.
- Sponsored label if paid/sponsored.

### 19.5 Local Event Submissions

Later, relevant organizations or users may submit events.

Events should be reviewed or labeled based on source quality.

---

## 20. Notifications, Widgets, and Lock-Screen Experience

### 20.1 Push Notifications

The app should support notifications for:

- Daily brief.
- Weekly brief.
- Urgent alerts.
- Topic-specific updates.
- City/state-specific updates.
- Route-specific transport alerts.
- Saved-item updates.
- Calendar reminders.
- Community-report follow-ups.
- Subscription/payment events.

### 20.2 Notification Controls

Users should control:

- Enable/disable notifications.
- Urgent only.
- Daily brief time.
- Weekly brief time.
- Categories to notify about.
- Location/city/state alerts.
- Route-specific alerts.
- Sound/vibration settings where supported.

### 20.3 Lock-Screen and Home-Screen Widgets

Possible widget experiences:

- Today's top practical update.
- Daily brief play button.
- Urgent alert card.
- Saved deadline reminder.
- Upcoming calendar event.
- Transport line alert.

### 20.4 Quick Audio Playback

Users should be able to start the daily brief or continue audio playback quickly from supported surfaces.

---

## 21. Transport Alerts and Route-Specific Intelligence

### 21.1 Transport Use Cases

Users often need to know:

- Whether their train or local line is disrupted.
- Whether strikes affect their commute.
- Whether there are major delays or cancellations.
- Whether compensation or passenger rights apply.

### 21.2 User Route Preferences

Users should be able to save:

- Common train routes.
- S-Bahn/U-Bahn/tram/bus lines.
- Departure city/station.
- Arrival city/station.
- Commute days/times.

### 21.3 Transport Alert Behavior

The app should:

- Match disruptions to saved routes.
- Notify users when relevant.
- Summarize official transport updates.
- Explain practical impact.
- Link to official transport sources.
- Include passenger-rights information where relevant.

### 21.4 Scope Boundary

The app should not replace DB Navigator or local transport apps.

Instead, it should translate, summarize, and contextualize transport information for immigrant users and integrate it into their broader daily-life intelligence layer.

---

## 22. Public Alerts and Safety Warnings

### 22.1 Alert Sources

The app may use or link to official/authorized warning systems such as:

- NINA / BBK.
- KATWARN.
- DWD weather warnings where relevant.
- State/city emergency pages.
- Official police or public safety announcements where relevant.

### 22.2 Alert Presentation

Alerts should show:

- What happened.
- Where it applies.
- Severity.
- Source.
- Time issued.
- Recommended action.
- Expiry or update status.
- Original source link.

### 22.3 Safety Warning Boundary

The app should not override official emergency-warning apps.

It should act as a multilingual explanation and integration layer.

---

## 23. Community Reports and Citizen Updates

### 23.1 Purpose

Community reports allow users to contribute local practical information that may not yet appear in official sources.

Examples:

- A local office changed its process.
- Appointment delays are happening in a specific city.
- A public office is temporarily closed.
- Users are experiencing the same administrative issue.

### 23.2 Report Creation

A report submission should ask for structured information:

- City.
- State.
- Office/organization if relevant.
- Topic/category.
- Short description.
- Date observed.
- Evidence/source if available.
- Whether the user wants to stay anonymous publicly.

### 23.3 Verification Labels

Community reports must be clearly labeled.

Possible labels:

- Unverified community report.
- Confirmed by multiple users.
- Reviewed by moderation.
- Verified with official source.
- Disputed.
- Outdated.

### 23.4 Community Confirmation

Users may be able to confirm or dispute reports.

Example:

- “I experienced this too.”
- “This is no longer true.”
- “This happened in another city, not here.”

### 23.5 Moderation Requirements

Community reports create risk and require moderation.

The app should prevent or moderate:

- Personal attacks.
- Naming individual public-office staff in harmful ways.
- Defamation.
- Hate speech.
- Private personal data.
- Rumors presented as fact.
- Panic-inducing unverified claims.
- Spam or manipulation.

---

## 24. Comments and User Interaction

### 24.1 Comments on Content

Users may comment on news items, guides, reports, or events.

Comments can:

- Add local context.
- Share practical experiences.
- Help identify implications.
- Increase engagement.
- Feed future knowledge improvements.

### 24.2 Comment Identity

Comments should show:

- User display username.
- Optional avatar.
- Timestamp.
- Possibly city/state if user chooses or if relevant.

They should not expose private identity-provider information.

### 24.3 Comment Moderation

The app should support:

- Report comment.
- Hide comment.
- Delete comment.
- User blocking.
- Admin review queue.
- Spam detection.
- Abuse detection.

### 24.4 Comment Scope Boundary

Comments should support practical discussion, not turn the app into a general social network.

---

## 25. Sharing and External Distribution

### 25.1 User Sharing

Users should be able to share items through:

- Link copy.
- Telegram.
- WhatsApp.
- Instagram story/share options where supported.
- X/Twitter.
- Facebook.
- Email.
- Native mobile share sheet.

### 25.2 Shared Content Format

Shared content should be short and source-aware.

It should include:

- Title.
- Short summary.
- App link.
- Source mention where appropriate.
- Language label if relevant.

### 25.3 Public Content Channels

The product should also support public external channels for acquisition and trust-building:

- YouTube.
- YouTube Music podcasts.
- Spotify podcasts.
- Other podcast platforms.
- Instagram / short video channels.
- Telegram channel.
- Possibly WhatsApp broadcast/community.

Public channels may publish:

- Weekly summaries.
- Monthly summaries.
- Public audio podcasts.
- AI-generated videos.
- Non-personalized immigrant-life briefings.

Strategic distinction:

- Public channels are top-of-funnel and trust-building.
- The app is the place for daily, personalized, local, premium, actionable experience.

---

## 26. Privacy, Data Control, and Trust

### 26.1 Data Minimization

The app should collect the minimum data needed for personalization.

The product should not collect sensitive immigration or identity data unless a future feature clearly requires it and the user explicitly understands why.

### 26.2 User Data Controls

Users should be able to:

- See their profile/preferences.
- Edit preferences.
- Clear search history.
- Clear chat history if stored.
- Delete bookmarks/highlights.
- Delete community reports where allowed.
- Delete comments where allowed.
- Delete account.

### 26.3 Transparency

The app should clearly explain:

- What data is collected.
- Why it is collected.
- How it affects personalization.
- What is public and what is private.
- How AI is used.
- That AI summaries should be verified at the source for high-stakes actions.

### 26.4 Trust Signals

Each content item should show trust indicators:

- Official source.
- Trusted media.
- NGO source.
- Community report.
- AI-generated summary.
- Human-reviewed if applicable.
- Last updated.
- Source link.
- Verification status.

---

## 27. Error Reporting and Quality Feedback

### 27.1 Report Error Button

Every content item and AI answer should allow users to report problems.

Possible report reasons:

- Translation is wrong.
- Summary is wrong.
- Information is outdated.
- Source link is broken.
- Wrong city/state.
- Wrong category.
- This is not relevant to me.
- This looks misleading.
- Offensive or unsafe content.

### 27.2 Correction Workflow

Reported errors should flow into an admin/review queue.

The system should support:

- Review status.
- Correction notes.
- Re-generation of summary/translation/audio.
- Correction log.
- User notification if a saved item was corrected.

### 27.3 Accuracy Rating

Users may rate usefulness and accuracy:

- Useful / not useful.
- Accurate / inaccurate.
- Needs more explanation.
- Needs simpler language.

---

## 28. Language, Localization, and Accessibility

### 28.1 App Language

Users should be able to use the app in their preferred language.

Initial and future languages may include:

- Farsi/Persian.
- German.
- English.
- Arabic.
- Turkish.
- Other languages later.

### 28.2 Original and Translated View

For important items, users should be able to view:

- Translated summary.
- Original title/source text where available.
- Original source link.

### 28.3 Glossary and Term Consistency

The product should maintain a multilingual glossary for important German administrative and legal terms.

Examples:

- Aufenthaltstitel.
- Ausländerbehörde.
- Bürgeramt.
- Bürgergeld.
- Einbürgerung.
- Niederlassungserlaubnis.
- Jobcenter.
- Meldebescheinigung.

### 28.4 Accessibility

The app should support:

- Clear typography.
- Audio playback.
- Simple language where possible.
- High contrast modes where possible.
- Screen-reader compatibility.
- Avoiding overly dense text.

---

## 29. Offline and Saved Reading

### 29.1 Saved Content Access

Bookmarked content, highlights, and important knowledge-base entries should be available reliably.

Possible offline/cached behavior:

- Saved summaries available offline.
- Saved highlights available offline.
- Saved audio available offline for premium users.
- Offline access to selected guide/checklist content.

### 29.2 Sync Behavior

When the user returns online:

- Saved changes should sync.
- Outdated saved content should show an update warning.
- Deleted or corrected items should be reflected.

---

## 30. Subscription, Payments, and Entitlements

### 30.1 Free Tier

The core app should be free.

Free users may access:

- Text summaries.
- Personalized feed.
- Detail pages.
- Source links.
- Basic search.
- Basic bookmarks.
- Limited daily summaries.
- Ads on detail pages.
- Public weekly/monthly summaries outside the app.

### 30.2 Premium Tier

Premium users may access:

- Audio for every article.
- Daily podcast-like playback.
- Weekly podcast inside app.
- Premium listening experience.
- Advanced search/history.
- Personalized notifications.
- Route-specific alerts.
- High-priority alerts.
- More bookmark/highlight features.
- Offline audio.
- No ads or reduced ads.

### 30.3 Pricing Direction

The subscription should be affordable because the product targets normal people and solves practical daily-life problems.

Previously discussed price range:

- Around €2.99/month.
- Around €4.99/month.
- Possibly annual discounted plan.

Final pricing should be validated separately.

### 30.4 Entitlement Requirements

The app needs entitlement logic for:

- Free user.
- Premium user.
- Trial user.
- Expired subscription.
- Restored purchase.
- Family plan or community plan if added later.

### 30.5 Future Advertising and Sponsorship

Potential future monetization:

- Ads in details pages.
- Sponsored community events.
- Sponsored but relevant local notices.
- Partnerships with NGOs, lawyers, advisors, community centers, or migrant-service providers.
- City/topic/language-targeted promotions.

Trust rule:

- Sponsored content must be clearly labeled.
- Ads should remain relevant and not compromise trust.

---

## 31. Admin, Operations, and Moderation Panel

### 31.1 Admin Panel Purpose

The internal team needs tools to manage content quality, safety, sources, users, reports, and operations.

### 31.2 Source Management

Admins should be able to:

- Add sources.
- Edit source metadata.
- Classify source type.
- Mark source reliability.
- Enable/disable source.
- Monitor source ingestion status.

### 31.3 Content Management

Admins should be able to:

- View ingested items.
- Edit summaries.
- Approve or reject items.
- Regenerate summaries/translations/audio.
- Assign categories.
- Assign geography.
- Assign urgency.
- Add source links.
- Mark items as outdated or corrected.

### 31.4 AI Output Review

Admins should be able to review:

- Summaries.
- Translations.
- Audio quality.
- AI-generated images.
- AI search results.
- Chatbot answers flagged by users.

### 31.5 Community Moderation

Admins should be able to:

- Review community reports.
- Review comments.
- Remove harmful content.
- Mark reports as verified/disputed/outdated.
- Manage abusive users.
- Handle spam.

### 31.6 Event Management

Admins should be able to:

- Add events.
- Review submitted events.
- Label sponsored events.
- Edit calendar metadata.
- Remove outdated or irrelevant events.

### 31.7 User Support

Admins should be able to:

- View user support requests.
- Review error reports.
- Track correction workflows.
- Respond to common support needs.

---

## 32. Content Item Data Model — Product-Level View

A content item should include product-level fields such as:

- Content ID.
- Title.
- Short summary.
- Full summary.
- Original source title.
- Original source URL.
- Source name.
- Source type.
- Original language.
- Translation language.
- Category.
- Subcategory.
- Country.
- State.
- City.
- Office/authority if relevant.
- Transport line/route if relevant.
- Urgency level.
- Reliability label.
- Verification label.
- Affected group.
- Why it matters.
- Action required.
- Important date/deadline.
- Published timestamp.
- Processed timestamp.
- Pipeline processing status.
- Review status.
- Publishing destination status.
- Content cluster ID if relevant.
- Source content hash or change-detection identifier if relevant.
- Generated audio asset reference if available.
- Generated visual asset reference if available.
- Last updated timestamp.
- Outdated/superseded status.
- Related items.
- Audio asset links by language.
- Image asset.
- Comment count.
- Bookmark count.
- Error-report status.

---

## 33. User-Facing Safety and Liability Language

For sensitive categories, especially immigration, residence, citizenship, asylum, benefits, tax, and legal obligations, the app should include careful language.

Examples:

- “This is an AI-assisted summary of the linked source.”
- “For official action, always check the original source.”
- “This is not legal advice.”
- “Rules may vary by city, state, authority, or personal situation.”
- “Last updated: [date/time].”

The app should avoid presenting AI output as final legal truth.

---

## 34. Example Product Scenarios

### 34.1 Immigration Update

A federal ministry publishes a change about citizenship rules.

The system:

1. Detects the update.
2. Classifies it as citizenship/naturalization.
3. Summarizes the change.
4. Translates it into Farsi.
5. Adds who is affected.
6. Adds whether action may be required.
7. Links to the official source.
8. Sends it to users who follow citizenship topics.
9. Makes it available in search and AI chat.
10. Generates Persian audio if premium/audio output is enabled.

### 34.2 Berlin Office Update

A Berlin authority changes appointment rules.

The system:

1. Detects the city-level update.
2. Tags it as Berlin and public administration.
3. Shows it to Berlin users.
4. Does not show it prominently to users in Munich unless broadly relevant.
5. Adds “action required” if users may need to change appointment behavior.

### 34.3 Transport Strike

A transport strike is announced.

The system:

1. Detects strike information from a transport or official source.
2. Tags affected routes and cities.
3. Notifies users whose saved routes may be affected.
4. Summarizes alternatives and official links.
5. Links to passenger-rights information if relevant.

### 34.4 User Bookmark Highlight

A user reads a long guide about permanent residence.

The user:

1. Highlights a paragraph about required documents.
2. Saves it to a bucket named “Residence.”
3. Later opens the saved highlight.
4. The app opens the full guide and scrolls to the highlighted paragraph.

### 34.5 AI Chat Question

A user asks:

> Can I apply for citizenship after five years in Germany?

The chatbot:

1. Searches internal source-linked content.
2. Explains the general rule based on current sources.
3. Notes that eligibility depends on personal situation.
4. Links official sources.
5. Adds a non-legal-advice notice.

---

## 35. Product Analytics, Notifications Inbox, and Operational Feedback

### 35.1 Product Analytics

The application should include privacy-conscious product analytics so the team can understand whether the product is useful, accurate, and habit-forming.

Important product events may include:

- Daily brief opened.
- Daily brief listened to.
- Weekly brief opened.
- Audio started, paused, skipped, completed.
- Feed item opened.
- Source link opened.
- Bookmark created.
- Highlight created.
- Search performed.
- AI chat question asked.
- Error reported.
- Notification received/opened.
- Subscription started, renewed, cancelled, or expired.
- Content shared.

Analytics should not require collecting sensitive immigration, identity, or legal-status data.

### 35.2 Key Product Health Metrics

Metrics useful for internal product improvement include:

- Daily brief open rate.
- Weekly active users.
- Daily active users.
- Audio listen-through rate.
- Notification opt-in rate.
- Source-link click-through rate.
- D7 retention.
- D30 retention.
- Premium conversion rate.
- Monthly churn.
- Number of saved/bookmarked items.
- Number of successful searches.
- User rating for usefulness.
- User rating for accuracy.
- User rating for actionability.
- Percentage of AI summaries corrected.
- Time from source publication to app publication.
- Share/referral rate.
- Podcast listen-through rate.
- YouTube/Spotify conversion to app/newsletter where measurable.

A particularly important early habit metric is how many users open or listen to the brief at least four days per week.

### 35.3 In-App Notification Center

In addition to push notifications, the app should include an in-app notification center where users can review:

- Missed alerts.
- Daily brief notifications.
- Saved deadline reminders.
- Updates to bookmarked items.
- Corrections to previously viewed or saved content.
- Community report updates.
- Subscription-related messages.

### 35.4 Help, Support, and App Information

The app should include basic support and information pages:

- Help / FAQ.
- Contact support.
- Report a problem.
- Privacy policy.
- Terms of service.
- About the app.
- Explanation of how AI summaries work.
- Explanation of source labels and reliability labels.
- Explanation that sensitive legal/immigration decisions should be verified with official sources or qualified professionals.


---

## 36. Reference Links Relevant to Product Functionality

This section lists functional references that informed the product scope. These are not competitor-analysis notes and do not define implementation choices by themselves.

### 36.1 Authentication and Identity

- Firebase Authentication — Simple, multi-platform sign-in:  
  https://firebase.google.com/products/auth

- Firebase Authentication documentation:  
  https://firebase.google.com/docs/auth

- Firebase — Authenticate using Apple:  
  https://firebase.google.com/docs/auth/ios/apple

- Firebase — Authenticate with a custom authentication system:  
  https://firebase.google.com/docs/auth/flutter/custom-auth

- Apple Developer — Sign in with Apple:  
  https://developer.apple.com/documentation/signinwithapple

- Apple Support — What is Sign in with Apple?:  
  https://support.apple.com/en-us/102609

### 36.2 External Podcast and Content Channels

- YouTube Creators — Podcasting on YouTube:  
  https://www.youtube.com/creators/podcasts/

- YouTube Help — Create a podcast in YouTube Studio:  
  https://support.google.com/youtube/answer/12751636

- Spotify for Creators:  
  https://creators.spotify.com/

- Spotify for Creators — Podcast features:  
  https://creators.spotify.com/features/podcast

### 36.3 Official / Practical Information Sources Relevant to Product Content

- Deutsche Bahn — Passenger rights:  
  https://int.bahn.de/en/booking-information/passenger-rights

- Deutsche Bahn — Passenger rights FAQ:  
  https://int.bahn.de/en/booking-information/passenger-rights/faq

- BBK — Warn-App NINA:  
  https://www.bbk.bund.de/DE/Warnung-Vorsorge/Warn-App-NINA/warn-app-nina_node.html

- KATWARN — Officially connected:  
  https://www.katwarn.de/en/areas.php

- Handbook Germany:  
  https://handbookgermany.de/en

- Integreat:  
  https://integreat-app.de/en/

- European Commission — Integreat multilingual integration platform for newcomers to Germany:  
  https://home-affairs.ec.europa.eu/projects/integreat-multilingual-integration-platform-newcomers-germany_en

### 36.4 German Public and Administrative Information Sources Mentioned in the Original Dossier

- BAMF:  
  https://www.bamf.de/EN/Startseite/startseite_node.html

- BAMF — The “Ankommen” app:  
  https://www.bamf.de/EN/Themen/Integration/ZugewanderteTeilnehmende/ErsteOrientierung/AppAnkommen/app-ankommen-node.html

- Federal Foreign Office — Law on Nationality:  
  https://www.auswaertiges-amt.de/en/229970-229970

- Federal Ministry of the Interior — New law on nationality takes effect:  
  https://www.bmi.bund.de/SharedDocs/kurzmeldungen/EN/2024/06/mod-staatsangehoerigkeitsrecht.html

### 36.5 AI Support Agent Reference

- Snowie.ai:  
  https://snowie.ai/

This is referenced only as an example category for AI support-agent style behavior. It is not a final vendor decision.

---

## 37. Final Product Scope Summary

The application should become a practical, trusted, personalized migrant-life intelligence layer for Germany.

Its core value is not raw news. Its core value is helping immigrants understand:

- What changed.
- Why it matters.
- Whether it applies to them.
- Whether they need to act.
- Where to verify the information.
- How to stay informed without reading many German-language sources.

The complete product scope currently includes:

- Secure provider-based authentication.
- Minimal profile and privacy-first personalization.
- Short onboarding.
- Personalized feed.
- Practical content categories for migrant life.
- Source collection and source metadata.
- Automated content creation, processing, and publishing pipeline.
- AI summarization.
- AI translation.
- Source-linked detail pages.
- Daily and weekly briefs.
- Audio per item.
- Podcast-like playback.
- Semantic AI Search.
- RAG-based AI Chatbot.
- AI Support Agent.
- Searchable history.
- Information Center / single source of truth.
- Bookmarks.
- Bookmark buckets.
- Highlight bookmarks.
- Multi-calendar and event support.
- Push notifications.
- Widgets and lock-screen experiences.
- Transport and route-specific alerts.
- Public alerts and safety warnings.
- Community reports.
- Comments and user interaction.
- Sharing.
- Public YouTube/Spotify/podcast channels.
- Privacy and data controls.
- Report-error and quality feedback.
- Language/localization controls.
- Offline/saved reading.
- Subscription and entitlement system.
- Ads and sponsorship support with trust safeguards.
- Admin, moderation, and content-operations panel.
- Product analytics and operational feedback.
- In-app notification center.
- Help, support, privacy, terms, and app-information pages.
- AI safety and source-verification guardrails.

This document should serve as the current product-scope foundation. Each feature can later be expanded into detailed product requirements, user stories, acceptance criteria, UX flows, data models, and technical architecture documents.

---

# End of Product Scope
