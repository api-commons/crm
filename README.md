# CRM

The common Customer Relationship Management vocabulary, distilled from the APIs that
already exist. Part of [Schema Commons](../schema-commons), published by the
[API Commons](https://apicommons.org).

Built from the providers tagged CRM in [APIs.io](https://apis.io) that publish their own
OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| contact | 80 | 71 | email 80%, id 80%, last_name 76%, first_name 75%, phone 63%, job_title 55%, created_at 52%, custom_fields 51% | contacts 64, contact 18, people 10, persons 4 |
| company | 58 | 54 | id 83%, name 82%, created_at 67% | accounts 27, companies 26, organizations 12, account 12 |
| deal | 53 | 46 | id 83%, name 76%, created_at 65%, updated_at 52% | opportunities 32, deals 16, opportunity 7, deal 4 |
| lead | 31 | 27 | id 82%, status 59%, created_at 56%, source 52% | leads 30, lead 4 |
| activity | 41 | 33 | type 73%, id 70%, created_at 55% | activities 31, activity 9, engagements 2 |
| task | 48 | 41 | id 90%, subject 76%, due_at 68%, created_at 61% | tasks 43, task 7, todos 1 |
| note | 41 | 34 | id 100%, created_at 74%, content 65%, updated_at 50% | notes 39, note 4 |
| pipeline | 25 | 21 | id 90%, name 86% | pipelines 22, pipeline 5 |

Cohort: 177 providers with harvested OpenAPI, 112 crosswalks. Of the providers exposing a contact: 92% create, 90% list, 90% fetch, 75% update, 64% delete. Generated 2026-11-11 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](../schema-commons/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from. Pipedrive, for example: contacts are
  `persons`, `created_at` is `add_time`, `company_id` is `org_id`, `tags` is `label_ids`.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments, and
  which spellings mean the same field. Review it; everything else is counted.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- **The concepts are `deal` and `company`, not `opportunity` and `account`.** The market
  says `opportunity` (38 providers) more than `deal` (20). `account` (39) edges
  `company` (31). Opportunity leans on enterprise suites (Dynamics, Siebel, PeopleSoft),
  and `account` also means a login, billing or ledger account, which makes it the worse
  common word. The crosswalk records every provider's
  own segment, so nothing is lost.
- **Matches that are not CRM records are excluded.** Every provider counted only through an
  ambiguous segment (accounts, people, opportunities) is reviewed by hand. The ones that are
  something else -- the API user's own account, social-channel accounts, a QuickBooks
  ledger, a recruiting pipeline's "opportunities" -- are listed with the reason under
  `exclude:` in [concepts.yml](concepts.yml). They stay in [census/](census), out of
  everything built from it.
- **One company counts once.** Where the catalog lists a company more than once (Keap and
  Infusionsoft, three Microsoft Dynamics listings), the listings are merged under
  `duplicates:` rather than dropped, so no evidence is lost.
- **Properties are snake_case** -- a decision made across all contexts; see
  [Schema Commons](https://github.com/api-commons/schema-commons#decisions-that-cross-contexts).

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
