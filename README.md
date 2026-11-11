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
| contact | 79 | 70 | email 81%, id 79%, last_name 76%, first_name 74%, phone 64%, job_title 54%, created_at 51%, custom_fields 51% | contacts 63, contact 18, people 10, persons 4 |
| company | 57 | 53 | id 81%, name 81%, created_at 68% | accounts 27, companies 26, organizations 12, account 11 |
| deal | 53 | 46 | id 83%, name 76%, created_at 65%, updated_at 52% | opportunities 32, deals 16, opportunity 7, deal 4 |
| lead | 31 | 27 | id 82%, status 59%, created_at 56%, source 52% | leads 30, lead 4 |
| activity | 40 | 32 | type 69%, id 66%, created_at 53% | activities 31, activity 8, engagements 2 |
| task | 49 | 42 | id 88%, subject 74%, due_at 67%, created_at 60%, description 50%, status 50% | tasks 44, task 7, todos 1 |
| note | 42 | 35 | id 100%, created_at 71%, content 63% | notes 40, note 4 |
| pipeline | 26 | 22 | id 86%, name 77% | pipelines 22, pipeline 6 |

Cohort: 177 providers with harvested OpenAPI, 112 crosswalks. Of the providers exposing a contact: 92% create, 90% list, 89% fetch, 75% update, 63% delete. Generated 2026-11-10 by build.py.
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
- **Properties are snake_case.** It began as a near-tie in this cohort; across CRM, help
  desk and commerce snake_case now leads (200 providers to 180), and references follow
  (`_id` 165, `Id` 162). camelCase providers map through the crosswalk without loss.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
