# CRM

The common Customer Relationship Management vocabulary, distilled from the APIs that
already exist. Part of [Schema Commons](../schema-commons), published by the
[API Commons](https://apicommons.org).

Built from **134 providers** tagged CRM in [APIs.io](https://apis.io) that publish their own
OpenAPI (1,155 harvested files). Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| contact | 62 | 57 | email 81%, last_name 77%, first_name 74%, id 74%, phone 65%, job_title 58%, created_at 54%, custom_fields 51% | contacts 54, contact 15, people 8, persons 3 |
| company | 49 | 48 | id 79%, name 79%, created_at 73%, updated_at 56%, website 52% | accounts 27, companies 20, organizations 13, account 11 |
| deal | 43 | 38 | name 82%, id 79%, created_at 74%, updated_at 63% | opportunities 25, deals 14, opportunity 5, deal 4 |
| lead | 26 | 23 | id 70%, created_at 65%, source 52% | leads 26 |
| activity | 38 | 32 | type 69%, id 62%, created_at 53% | activities 29, activity 8 |
| task | 38 | 34 | id 88%, subject 79%, created_at 68%, due_at 68%, status 53%, description 50% | tasks 35 |
| note | 34 | 30 | id 90%, created_at 70%, content 63%, updated_at 57% | notes 33 |
| pipeline | 20 | 20 | id 75%, name 75% | pipelines 17 |

Operations are just as settled: 95% of providers exposing contacts let you create one,
89% list and fetch, 74% update, 60% delete. The extended tier and the long tail of every
concept are in [profile.yml](profile.yml).

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](../schema-commons/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- 91 providers, common name -> their name, with the harvested
  spec and operations each mapping was read from. Pipedrive, for example: contacts are
  `persons`, `created_at` is `add_time`, `company_id` is `org_id`, `tags` is `label_ids`.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments, and
  which spellings mean the same field. Review it; everything else is counted.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- **The concepts are `deal` and `company`, not `opportunity` and `account`.** The market
  says `opportunities` (30 providers) more than `deals` (18), and `accounts` (38) more than
  `companies` (20). But both counts lean on enterprise estates (three Microsoft Dynamics
  listings, Siebel, PeopleSoft), and `account` also means a login, billing or ledger
  account, which makes it the worse common word. The crosswalk records every provider's
  own segment, so nothing is lost.
- **`/accounts` that are not customer companies are excluded.** Every provider counted only
  through an ambiguous segment (accounts, people, opportunities) was reviewed by hand; nine
  matches were something else (the API user's own account, social-channel accounts, a
  QuickBooks ledger) and are listed with the reason under `exclude:` in
  [concepts.yml](concepts.yml). They stay in [census/](census), out of everything built
  from it.
- **Properties are snake_case.** Multi-word property names are a near-tie in this cohort
  (snake_case 44 providers, camelCase 41). The commons picks one; camelCase providers map
  through the crosswalk without loss.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
