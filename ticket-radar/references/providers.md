# Where the fields actually live

During first-run discovery you are hunting for four things. Everything else in this file is detail.

| What you need | Why |
|---|---|
| **Product area / component** | Decides which tickets are this PM's |
| **Customer identity** | Powers "which customers are we hearing from" |
| **Account tier or size** | Powers weighting — the difference between signal and volume |
| **Severity / priority** | Feeds the escalate/watch/noted lanes |

Two warnings that apply to every system below.

**Never assume a field named `product_area` exists.** Most teams bolted product ownership onto whatever field was handy — often tags, sometimes the assigned group, occasionally the ticket form. Discover, don't guess.

**A missing tier field is common and survivable.** Plenty of teams don't tag account size on tickets at all. When there's no tier field, fall back to the profile's `key_accounts` list plus ticket priority, and say so in the brief's coverage line so the PM knows the weighting is coarser than it could be.

---

## Zendesk

**Discovery:** `GET /api/v2/ticket_fields` lists every custom field with its id, title and dropdown values. `GET /api/v2/organization_fields` does the same for account-level fields.

- **Product area** — usually a custom dropdown in `ticket.custom_fields` (an array of `{id, value}` — you need the id from discovery to read it). Also commonly carried by `ticket.tags`, by the assigned `group_id`, or by which ticket form was used.
- **Customer** — `ticket.organization_id`, then `GET /api/v2/organizations/{id}`.
- **Tier** — almost always a custom org field, surfaced in `organization_fields` on the organization record. Look for names like plan, tier, segment, ARR band.
- **Severity** — `ticket.priority` (`urgent` / `high` / `normal` / `low`). Note this is often left at `normal` by default, so treat it as weak evidence on its own.
- **Status** — `new` / `open` / `pending` / `hold` / `solved` / `closed`. "Pending" means waiting on the customer, so exclude it from aging counts — the delay isn't the team's.

**Querying:** `GET /api/v2/search.json?query=type:ticket status<solved created>2026-08-17`. Search syntax supports `tags:`, `organization:`, and `custom_field_<id>:`.

**Aging:** compare `updated_at` against the last public agent comment, not against `updated_at` alone — automations and tag changes bump `updated_at` without anyone actually replying to the customer.

---

## Salesforce Service Cloud

**Discovery:** describe the `Case` object to list fields including custom ones (suffix `__c`). Same for `Account`.

- **Product area** — nearly always a custom field on Case (`Product__c`, `Component__c`, `Feature_Area__c` — naming varies by org). `Case.Type` and `Case.Reason` are standard picklists sometimes repurposed for this.
- **Customer** — `Case.AccountId` → `Account.Name`.
- **Tier** — `Account.Type`, `Account.AnnualRevenue`, or a custom tier/segment field on Account. Many orgs also carry it via `Entitlement` / service contract.
- **Severity** — `Case.Priority`, plus `Case.IsEscalated` which is a strong, explicit signal worth surfacing on its own.
- **Status** — `Case.Status`, values are org-configurable so read them rather than assuming.

**Querying:** SOQL, e.g. `SELECT Id, Subject, Priority, Status, Account.Name FROM Case WHERE Product__c IN (...) AND CreatedDate = LAST_N_DAYS:1`.

**Aging:** `Case.LastModifiedDate` is noisy for the same reason as Zendesk's. Prefer the most recent outbound `CaseComment` or `EmailMessage`.

---

## Jira Service Management

**Discovery:** `GET /rest/api/3/field` returns every field including `customfield_NNNNN` ids and their names.

- **Product area** — `components` is the idiomatic home and worth checking first. Otherwise `labels` or a custom field. `Request Type` (a JSM custom field) is often closer to "what kind of problem" than "which product area".
- **Customer** — JSM Organizations, or the `reporter`'s company domain.
- **Tier** — rarely native. Usually a custom field on the organization, or absent entirely.
- **Severity** — `priority`, and often a separate custom Severity field where teams wanted impact separate from urgency. If both exist, ask the PM which one their team actually maintains — the neglected one will quietly poison the lanes.

**Querying:** JQL, e.g. `project = SUP AND component in ("Checkout") AND created >= -1d`.

---

## Intercom

- **Product area** — `custom_attributes` on the conversation, or `tags`. Intercom teams lean heavily on tags.
- **Customer** — `contacts` on the conversation, then the associated `company`.
- **Tier** — `company.plan`, `company.monthly_spend`, or `company.size`. `monthly_spend` is unusually useful here since it's a real number rather than a label.
- **Severity** — `priority` (just `priority` / `not_priority`, so it's coarse). `state` is `open` / `closed` / `snoozed`.

Conversations are threads rather than tickets, so one conversation may contain several distinct issues. Cluster on the issue, not the conversation.

---

## HubSpot

- **Product area** — `hs_ticket_category`, or a custom property. Check the pipeline too: teams often model product areas as separate pipelines.
- **Customer** — association to the Company object.
- **Tier** — a Company property, often `annualrevenue` or a custom lifecycle/tier property.
- **Severity** — `hs_ticket_priority`. Status is `hs_pipeline_stage`, whose values depend on the pipeline, so read them per pipeline.

---

## Freshdesk

**Discovery:** `GET /api/v2/ticket_fields`.

- **Product area** — `product_id` if multiple products are configured, otherwise `custom_fields` or `tags`.
- **Customer** — `company_id` → Companies.
- **Tier** — a custom company field.
- **Severity** — `priority` is numeric, 1 (low) to 4 (urgent). `status` is numeric too: 2 open, 3 pending, 4 resolved, 5 closed.

---

## CSV fallback

When nothing is connected, a CSV export still gives a real brief — it just can't run on a schedule.

Ask for an export of the last 30 days with at least: ticket id, created date, last updated, status, priority, subject, product area/component, account name. Account tier and a link column are welcome additions.

Map the columns by reading the header row. Confirm with a volume sanity-check rather than a column list — "that's about 340 tickets in your areas over 30 days, roughly 11 a day, sound right?" — since a PM can judge a number instantly and can't judge a column mapping at all. Then run the identical rubric. Say clearly in the coverage line that this was a one-off from an export, so the PM doesn't expect one tomorrow.

Thirty days of export is also the fastest way to seed the History baseline — a single import can backfill enough rows that trend claims start working immediately rather than after a week.
