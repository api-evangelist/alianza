---
name: Search CDRs and export a usage report
description: Search call detail records for an account and drive Alianza's asynchronous CSV report jobs to completion.
api: openapi/alianza-openapi-original.yml
operations: [cdrSearch, telephoneNumberSummaryReport, userSummaryReport, sipSummaryReport, deviceSummaryReport, vFaxSummaryReport, userCSVReport3, deviceCSVReport3, accountCSVReport3, telephoneNumberCSVReport2, getJobById, getJobOutput, getOutputUrl]
---

# Search CDRs and export a usage report

Alianza splits reporting in two: synchronous summary/search endpoints that answer immediately,
and CSV report endpoints that **initiate a job** you then poll.

## Synchronous — search and summaries

- `cdrSearch` — `GET /v2/partition/{partitionId}/account/{accountId}/cdrsearch` returns call
  detail records for one account. Note the `month` / `year` and `startDate` / `endDate`
  parameters; a `412` response means an invalid month or year was supplied.
- Partition-level summaries answer directly: `telephoneNumberSummaryReport`
  (`GET /v2/partition/{partitionId}/report/tn`), `userSummaryReport` (`/report/user`),
  `sipSummaryReport` (`/report/sip`), `deviceSummaryReport` (`/report/device`),
  `vFaxSummaryReport` (`/report/vfax`).

## Asynchronous — CSV exports

1. **Initiate.** Call the CSV variant — for example `userCSVReport3`
   (`GET /v2/partition/{partitionId}/report/user/csv_3`), `deviceCSVReport3`
   (`/report/device/csv_3`), `accountCSVReport3` (`/report/account/csv_3`), or
   `telephoneNumberCSVReport2` (`/report/tn/csv_2`). These return **202 Accepted** with a job
   reference rather than the file.

2. **Poll the job.** `getJobById` — `GET /v2/job/{jobId}`. Back off between polls; no rate-limit
   headers are published, so use a conservative interval (start at 5s, back off to 30s).

3. **Collect the output.** `getJobOutput` — `GET /v2/job/{jobId}/output`, or `getOutputUrl`
   (`GET /v2/job/{jobId}/output-url`) for a downloadable link.

## Bulk CDR delivery

For continuous consumption, do **not** poll the API — Alianza delivers CDRs as SFTP datafeeds:
`sftp://datafeed.alianza.com` on port **10022** (Beta: `sftp://datafeed.b2.alianza.com:10022`).
Credentials come from Alianza.

## Rules

- Always use the **highest-numbered** CSV variant. `accountCSVReport` and `accountCSVReport2` are
  marked `deprecated: true`; `accountCSVReport3` is current. The same pattern applies across the
  Report tag.
- Read-only throughout — safe for an agent to run unattended, subject to the data-sensitivity of
  call records.
- Errors are `PublicApiException` (`{status, messages[], data}`).
