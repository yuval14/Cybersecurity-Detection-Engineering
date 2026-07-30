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

## Analytic Handling

A correlation match must be recorded as a lead, not proof of AI use. Analysts should:

- Preserve the complete timeline and relevant files.
- Determine whether the AI service or runtime is approved.
- Examine alternative explanations such as conventional automation or authorized testing.
- Assign AIDF evidence and confidence levels.
- Seek direct or strong technical evidence before making a confirmed assessment.
