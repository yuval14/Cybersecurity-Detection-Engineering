# Correlation Use Cases

The following detections require temporal or cross-source correlation that is not consistently portable across Sigma backends. Implement them in the target SIEM after mapping local schemas and approved AI services.

## 1. Reconnaissance to AI Inference to Execution

### Objective

Identify a potential agentic or AI-assisted loop in which reconnaissance output is sent to a model and followed by a new action.

### Logic

Within five minutes on the same host or session:

1. A scanner such as Nmap, Masscan, Nuclei, Gobuster, FFUF, or Feroxbuster executes.
2. A result file is created or modified.
3. The host connects to an unapproved AI, inference, model-gateway, or local-model endpoint.
4. A new script, command, or payload is created.
5. An interpreter or attack tool executes the new artifact.

### Required fields

- Host or device identifier.
- User and logon session identifier.
- Process image, parent image, command line, and process GUID.
- File path, hash, creation time, and creating process.
- Destination domain, IP, port, URL category, and connection time.

### Tuning

Allowlist authorized penetration-testing, vulnerability-management, purple-team, software-development, and AI-security research systems.

## 2. Repeated Tool-Inference-Tool Loop

### Objective

Identify repeated tool use in which each result is followed by inference and another semantically related tool action.

### Logic

Within 15 minutes on the same host, user, container, or agent session, identify at least three repetitions of:

```text
Tool execution -> result capture -> model request -> structured response -> next tool execution
```

Increase severity when:

- The next action changes after an error.
- The next action targets a newly discovered service or credential.
- Responses are JSON objects containing tool, command, target, action, plan, or next-step fields.
- The activity occurs on a server or user population with no approved AI use.

## 3. Local Model Runtime with Attack Tooling

### Objective

Identify local inference supporting attack activity.

### Logic

Within ten minutes:

1. A local model runtime starts or loads a model file.
2. The same process tree creates a script, shell command, or executable.
3. A reconnaissance, credential-access, lateral-movement, exploitation, or exfiltration tool executes.

Potential model artifacts include `.gguf`, `.safetensors`, `.onnx`, tokenizer files, model configuration files, and local inference caches.

## 4. Suspicious AI Credential Access and Subsequent Use

### Objective

Detect theft or misuse of AI-service credentials.

### Logic

Within 30 minutes:

1. A suspicious process reads, prints, searches for, or accesses an AI API key or secret.
2. The host or a newly observed external system connects to the corresponding service.
3. The account records inference activity inconsistent with its baseline.

Coordinate endpoint, secret-manager, identity, cloud, API-gateway, and provider-side logs where legally available.

## 5. Dynamic Payload Generation

### Objective

Detect target-specific payload generation and immediate execution.

### Logic

Within three minutes:

1. A process collects host, service, version, document, or identity information.
2. A new script or payload appears in a temporary or writable directory.
3. The payload executes before normal review, build, signing, or deployment activity.
4. The payload contains target-specific values observed during step 1.

This pattern may indicate AI generation, template-based automation, or conventional malware adaptation. Treat it as supporting evidence only.

## 6. External Adaptive Web Reconnaissance

### Objective

Identify an external source that changes its reconnaissance or exploitation strategy in response to the organization's web-service responses.

### Logic

Within ten minutes for the same source IP, infrastructure cluster, or session:

1. The source probes several unrelated endpoint families or technologies.
2. The organization returns distinguishable responses such as 200, 401, 403, 404, 405, or 500.
3. The source rapidly shifts URI, method, header, encoding, or payload family in a manner relevant to the previous response.
4. The source continues against newly identified technologies rather than replaying a fixed list.

Use `external_rapid_web_probe_variation.yml` as an atomic signal. Increase confidence when URI diversity, payload-family diversity, and response-conditioned changes all exceed the local baseline.

### Alternative explanations

- Conventional vulnerability scanners.
- Human penetration testers.
- Large distributed scanning frameworks.
- Precomputed decision trees.

## 7. External Authentication Strategy Adaptation

### Objective

Identify rapid changes in external credential-access behavior following distinct authentication failures.

### Logic

Within 15 minutes for the same source or related infrastructure:

1. Multiple authentication failures occur.
2. Failure reasons differ, such as unknown user, invalid password, MFA required, protocol rejected, or account locked.
3. The source changes usernames, protocol, authentication method, pacing, or target application.
4. A later attempt succeeds or reaches a different authentication stage.

Use `external_authentication_strategy_variation.yml` as an atomic signal. Correlate identity, VPN, SaaS, cloud, and application authentication telemetry.

### Escalation factors

- Attempts move between services while preserving target identity context.
- The strategy changes within seconds of receiving a distinct error.
- The source avoids previously triggered lockout or rate-limit conditions.
- The same infrastructure conducts tailored social engineering against the account.

## 8. External Dynamic Exploit Payload Variation

### Objective

Identify target-specific or response-conditioned command-injection payload generation from an external source.

### Logic

Within ten minutes against the same host, application, or vulnerable parameter:

1. Multiple command-injection payload families are observed.
2. Encoding, shell, command separator, or execution method changes between attempts.
3. Changes follow different HTTP status codes, WAF blocks, parser errors, or application responses.
4. A later request produces a materially different response, callback, process event, or file event.

Use `external_dynamic_command_injection_variants.yml` as an atomic signal. Correlate WAF, reverse proxy, application, EDR, DNS, and egress telemetry.

### Escalation factors

- Payloads contain values learned from earlier responses.
- The source changes between Windows and Unix command syntax after platform discovery.
- A successful callback follows several semantically related corrections.
- The campaign generates distinct payloads for different targets rather than replaying one artifact.

## External-Adversary Analytic Guardrail

For an external attacker, absence of internal AI API traffic is expected when the attacker performs inference on attacker-controlled infrastructure. Do not reduce confidence solely because the organization cannot observe prompts, model calls, or agent state.

At the same time, none of the external-facing Sigma rules proves AI use. A stronger assessment requires multiple independent signals, response-conditioned adaptation, intelligence corroboration, captured attacker infrastructure, provider evidence, or repeated behavior that is difficult to explain through conventional automation.

## Analytic Handling

A correlation match must be recorded as a lead, not proof of AI use. Analysts should:

- Preserve the complete timeline and relevant files.
- Determine whether the AI service or runtime is approved when internal execution is observed.
- Cluster external source infrastructure rather than relying only on one IP address.
- Retain server responses and failure reasons needed to test whether later actions were adaptive.
- Examine alternative explanations such as conventional automation, distributed scanning, or authorized testing.
- Assign AIDF evidence and confidence levels.
- Seek direct or strong technical evidence before making a confirmed assessment.