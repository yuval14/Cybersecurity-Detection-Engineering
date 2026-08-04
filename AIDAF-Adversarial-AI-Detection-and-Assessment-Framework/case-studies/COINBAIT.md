# COINBAIT: AI-Assisted Phishing on Trusted Web Infrastructure

## Summary

Google Threat Intelligence Group reported COINBAIT as a cryptocurrency-themed phishing kit assessed to have been accelerated through an AI application-development platform and deployed with legitimate web infrastructure, including modern frontend tooling, Cloudflare, and Backend-as-a-Service components.

The case illustrates how AI-assisted development can lower the cost of producing persuasive, functional phishing applications while legitimate hosting and backend platforms complicate reputation-based blocking.

## AIDAF Assessment

| Field | Assessment |
|---|---|
| AI role | Application and content generator |
| Likely involvement level | AI-1 to AI-2 |
| Strong evidence | Development-platform artifacts, application structure, backend project, credential-collection flow, provider or infrastructure records |
| Alternative explanation | Conventionally developed phishing application using the same cloud services |
| Public evidence status | Operationally observed; AI development involvement assessed by the reporting provider |

## Detection Opportunities

1. Newly registered or low-reputation domains impersonating a known brand.
2. Credential forms sending data directly to Supabase, Firebase, or another Backend-as-a-Service project.
3. Brand, domain, certificate, referrer, and backend-project mismatches.
4. Direct browser communication with a newly created backend project after an email or advertisement lure.
5. Reuse of the same project identifier, frontend assets, or analytics tokens across several domains.

## Suggested Correlation

```text
New or Low-Reputation Domain
  -> Brand-Impersonating Application
  -> Credential or Token Form
  -> BaaS Project or API Endpoint
  -> Account Takeover or Follow-On Access
```

## False Positives

- New legitimate startups
- Internal prototypes
- Authorized low-code applications
- Newly deployed SaaS portals

## Response Guidance

- Block the source domain and malicious backend project where feasible.
- Reset submitted credentials and revoke active sessions or tokens.
- Search for related domains, shared assets, project identifiers, and lure infrastructure.
- Notify the relevant hosting, CDN, and Backend-as-a-Service providers.
- Preserve the application, network requests, JavaScript, form fields, and project configuration for analysis.

## Source

Google Threat Intelligence Group. *GTIG AI Threat Tracker: Distillation, experimentation, and continued integration of AI for adversarial use*.

https://cloud.google.com/blog/topics/threat-intelligence/distillation-experimentation-integration-ai-adversarial-use