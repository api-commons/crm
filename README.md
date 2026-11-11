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
| contact | 63 | 58 | email 79%, last_name 76%, first_name 72%, id 72%, phone 64%, job_title 57%, created_at 53%, custom_fields 50% | contacts 54, contact 15, people 8, persons 4 |
| company | 56 | 54 | name 78%, id 76%, created_at 68%, updated_at 52%, website 50% | accounts 33, companies 20, organizations 13 |
| deal | 44 | 38 | name 82%, id 79%, created_at 74%, updated_at 63% | opportunities 26, deals 14, opportunity 5 |
| lead | 26 | 23 | id 70%, created_at 65%, source 52% | leads 26 |
| activity | 38 | 32 | type 69%, id 62%, created_at 53% | activities 29, activity 8 |
| task | 38 | 34 | id 88%, subject 79%, created_at 68%, due_at 68%, status 53%, description 50% | tasks 35 |
| note | 34 | 30 | id 90%, created_at 70%, content 63%, updated_at 57% | notes 33 |
| pipeline | 20 | 20 | id 75%, name 75% | pipelines 17 |

Operations are just as settled: 95% of providers exposing contacts let you create one,
87% list and fetch, 73% update, 59% delete. The extended tier and the long tail of every
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

## Open questions

- **deal or opportunity? company or account?** The market says `opportunities` (31
  providers) more than `deals` (18), and `accounts` (46) more than `companies` (20). Both
  counts are inflated by estates (three Microsoft Dynamics listings) and "account" also
  means a user or billing account, so the concept names stay `deal` and `company` for now --
  a decision to confirm, not a measurement.
- **`accounts` is ambiguous.** Some providers' `/accounts` are login or billing accounts,
  not customer companies. Those are in the company census today; the fix is a per-provider
  exclusion in concepts.yml, after review.
- **Property case is a near-tie** (snake 44, camel 41). The commons is snake_case; the
  crosswalk carries camelCase providers without loss.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
