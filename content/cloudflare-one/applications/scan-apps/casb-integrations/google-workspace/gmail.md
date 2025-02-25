---
pcx_content_type: reference
title: Gmail
rss: file
weight: 2
---

# Gmail

The Gmail integration detects a variety of user security, data loss prevention, and misconfiguration risks in an integrated Google Workspace account that could leave you and your organization vulnerable.

## Integration prerequisites

- A Google Workspace account with a Business Starter, Business Standard, Business Plus or Enterprise plan
- [Super Admin privileges](https://support.google.com/a/answer/2405986) in Google Workspace

{{<render file="casb/_integration-perms.md" withParameters="Google Workspace;;google-workspace">}}

## Security findings

The Gmail integration currently scans for the following findings, or security risks.

To stay up-to-date with new CASB findings as they are added, bookmark this page or subscribe to its RSS feed.

### Gmail administrator settings

| Finding                                                   | Severity | Description                                                                                                                  |
| --------------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Google Workspace Domain SPF Record Allows Any IP Address  | High     | A Google Workspace Domain SPF record allows any email to be sent from any IP address on your behalf.                         |
| Google Workspace Domain SPF Record Not Present            | Medium   | An SPF record does not exist for a Google Workspace Domain.                                                                  |
| Google Workspace Domain DMARC Record Not Present          | Medium   | A DMARC record does not exist for a Google Workspace Domain.                                                                 |
| Google Workspace Domain DMARC Not Enforced                | Medium   | A DMARC record for a Google Workspace Domain is not enforced.                                                                |
| Google Workspace Domain DMARC Not Enforced for Subdomains | Medium   | A DMARC record for a Google Workspace Subdomain is not configured to quarantine or reject messages that fail authentication. |
| Google Workspace Domain DMARC Only Partially Enforced     | Medium   | A DMARC record for a Google Workspace Domain is not configured to quarantine or reject messages that fail authentication.    |

### Email forwarding

| Finding                                      | Severity | Description                                                                                                                      |
| -------------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Google Workspace User Delegates Email Access | High     | A user has delegated access to their inbox to another party. Delegates can read, send, and delete messages on the user's behalf. |
