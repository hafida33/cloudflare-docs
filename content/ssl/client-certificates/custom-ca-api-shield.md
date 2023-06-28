---
pcx_content_type: how-to
title: Custom CA for API Shield mTLS
weight: 6
---

# Custom CA for API Shield mTLS

This page explains how you can manage mTLS with [API Shield](/api-shield/) using client certificates that have not been issued by Cloudflare CA.

This is specially useful if you already have mTLS implemented and client certificates issued by your chosen CA are already installed on devices.

## Availability

* Currently, this process can only be completed via API.
* To be able to set this up, your account must be on an Enterprise plan.
* Each Enterprise account can upload up to five CAs. This quota does not apply to CAs uploaded through [Cloudflare Access](/cloudflare-one/identity/devices/access-integrations/mutual-tls-authentication/).

## Set up mTLS with your CA

1. Use the [Upload mTLS certificate endpoint](/api/operations/m-tls-certificate-management-upload-m-tls-certificate) to upload the CA root certificate.
2. Take note of the Certificate ID (`id`) that is returned in the API response.
3. Use the [Replace Hostname Associations endpoint](/api/operations/client-certificate-for-a-zone-put-hostname-associations) to enable mTLS in each hostname that should use the CA for mTLS validation. Use the following parameters:
  
  * `hostnames`: The hostnames that will be using the indicated CA for client certificate validation.

  {{<Aside type="warning">}}
    
  Submitting an empty array will remove all hostnames associations.
    
  {{</Aside>}}

  * `mtls_certificate_id`: The Certificate ID obtained from previous step.
  
  {{<Aside type="warning">}}
    
  If no `mtls_certificate_id` is provided, the hostnames will be associated to your active Cloudflare Managed CA.
    
  {{</Aside>}}

4. Create a custom rule to enforce client certificate validation.
You can do this [via the dashboard](/api-shield/security/mtls/configure/) or via API, using the [Create firewall rules endpoint](/api/operations/firewall-rules-create-firewall-rules).

```text
  "rule": "(http.host in \"<HOSTNAME>\" and not cf.tls_client_auth.cert_verified)", 
  "action": "block"
```

## Remove custom CA