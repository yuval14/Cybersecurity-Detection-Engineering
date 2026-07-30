# External vs. Internal Adversary Detection Model

AIDAF separates two operationally different situations because the available evidence, detection logic, and confidence limits are not the same.

## Track A — External AI-Enabled Adversary

The adversary operates outside the organization and uses AI for reconnaissance, target selection, phishing, exploit development, payload variation, credential attacks, or campaign orchestration.

The victim usually cannot observe the model, prompt, agent state, or AI provider account. Detection therefore focuses on the attack outputs and their temporal relationship to target responses.

### Observable evidence

- Rapid variation of URIs, parameters, payload syntax, encodings, user agents, or authentication strategies
- Target-specific probes that change after HTTP, WAF, parser, or authentication errors
- High-volume personalized or multilingual social-engineering content
- Fast transition from public vulnerability disclosure to target-specific exploitation
- Distributed but coordinated probing across multiple source addresses
- Repeated failure-correction cycles with short decision intervals
- Infrastructure, personas, or content linked by threat intelligence to AI-assisted tradecraft

### Confidence limits

Behavioral evidence alone normally supports E1 or E2. It should be reported as behavior consistent with AI-assisted activity, not confirmed AI use. E3 or E4 requires provider records, recovered tooling, prompts, operator disclosures, or other direct artifacts.

## Track B — Intrusion-Stage or Post-Compromise AI Use

The adversary is attempting initial execution inside the organization, has established a foothold, or is conducting discovery, privilege escalation, lateral movement, persistence, collection, exfiltration, or impact.

The defender may observe endpoint processes, command lines, files, model clients, local runtimes, API calls, credentials, agent frameworks, tool invocation, and generated artifacts. This track can therefore produce stronger evidence.

### Observable evidence

- AI client, SDK, agent framework, or local model runtime on an unauthorized host
- Script interpreter or administrative tool spawned by an AI-linked process
- Reconnaissance output followed by an AI service call and a new command or script
- Repeated tool-to-model-to-tool loops
- Access to AI API keys by an unexpected process or account
- Victim data uploaded to a model service shortly before collection or exfiltration actions
- Runtime generation, modification, or obfuscation of scripts and payloads
- AI-assisted interpretation of errors followed by rapid technique changes

### Confidence limits

Direct endpoint or workflow artifacts may support E3. Recovered prompts, agent state, provider telemetry, or confirmed operator evidence may support E4. The analyst must still establish malicious context because authorized developers, data scientists, and enterprise agents can exhibit similar behavior.

## Detection Decision Matrix

| Question | Track A: External | Track B: Intrusion/Post-Compromise |
|---|---|---|
| Is the AI system normally visible? | No | Sometimes |
| Primary telemetry | WAF, proxy, email, DNS, identity, edge services | EDR, process, file, network, identity, cloud, AI gateway |
| Strongest practical signal | Response-driven adaptation across events | Model/tool integration inside the attack chain |
| Typical evidence level | E1–E2 | E2–E4 |
| Main false-positive risk | Scanners, bots, red teams, skilled humans | Authorized AI tools, automation, development activity |
| Recommended alert wording | AI-consistent external attack behavior | Suspected AI-integrated intrusion activity |

## Required Correlation Keys

### External track

Correlate by source IP, source ASN, session, cookie, JA4/JA3 fingerprint, target application, account, URI family, payload family, and time window. Where source rotation is expected, cluster events by shared infrastructure, behavioral sequence, payload semantics, and target selection.

### Internal track

Correlate by host, user, process tree, process GUID, container, cloud identity, API key, model endpoint, file hash, file path, command line, destination, and time window.

## Reporting Requirement

Every AIDAF assessment should state:

- Adversary position: external, intrusion-stage, post-compromise, or hybrid
- Which telemetry was available
- Whether the AI component was directly observed or inferred
- Evidence level and confidence
- Conventional alternative explanations
- Detection and visibility gaps
