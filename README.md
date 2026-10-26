# Alianza

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Alianza is a cloud communications software company founded in 2009 and headquartered in Utah,
serving more than 1,000 communications service providers across 81 countries. Its Intelligent
Communications Fabric — an experience layer, an orchestration layer and an infrastructure layer —
is packaged as Alianza One (multi-tenant cloud voice), Alianza Core (physical, virtual and
containerized network functions) and Alianza Fusion (hosted core communications systems).

- Website: https://www.alianza.com/
- Developer portal: https://developer.alianza.com/home
- API reference: https://developer.alianza.com/provisioning-api
- GitHub: https://github.com/alianza-dev

## API

The **Alianza Public API** is a JSON REST API over the Alianza One platform — partitions,
accounts, end users, devices and device lines, business lines, SIP trunks, telephone numbers and
porting, calling plans, voicemail, virtual fax, SSO, CDRs and reporting.

- Production: `https://api.alianza.com` · Beta: `https://api.b2.alianza.com`
- Auth: `apiKey` on the `X-AUTH-TOKEN` header, obtained from `POST /v2/authorize`
- OpenAPI 3.0.3: https://developer.alianza.com/openapi.yaml — **315 paths, 471 operations, 57 tags, 259 schemas**
- Legacy Swagger 2.0 (provider-deprecated): https://api.alianza.com/v2/apidocs/swagger.json

## Artifacts

| Directory | Contents |
|---|---|
| `openapi/` | Harvested OpenAPI 3.0.3 and the legacy Swagger 2.0 |
| `overlays/` | API Evangelist enhancement overlay |
| `authentication/` | Derived auth profile |
| `conventions/` | Cross-cutting semantics (auth, pagination, errors, async, versioning) |
| `errors/` | Problem/error catalog and the `BrandError` code registry |
| `lifecycle/` | Versioning, 50 deprecated operations, status page |
| `data-model/` | 215 entities, 307 relationships derived from the spec |
| `conformance/` | Standards conformance assertions with evidence |
| `sandbox/` | Environments, datafeeds and the API certification gate |
| `packages/` | Registry sweep — no first-party API client SDK exists |
| `mcp/` | Candidate MCP tool set bound to real operationIds, plus the crosswalk |
| `skills/` | Five packaged Agent Skills for the marquee provisioning flows |
| `well-known/` | `/.well-known/` probe results (no documents published) |
| `security/` | TLS/HSTS/DNSSEC/CAA/SPF/DMARC probe |
| `agentic-access/` | Per-operation agentic execution contracts |
| `llms/` | Generated `llms.txt` |

Profiled by the API Evangelist enrichment pipeline, 2026-08-02.
