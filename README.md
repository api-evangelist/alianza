# Alianza

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
