# AI-Enabled Threat Detection Framework

## Purpose

The AI-Enabled Threat Detection Framework (AIDF) helps organizations assess whether a cyber threat actor used artificial intelligence during an intrusion. It supports SOC, threat hunting, DFIR, cyber threat intelligence, cloud security, legal, and risk-management teams.

The framework is designed to answer five questions:

1. Was AI probably involved?
2. What operational role did AI perform?
3. At which attack stages was it used?
4. What evidence supports the assessment?
5. Did AI materially change the speed, scale, adaptability, or impact of the intrusion?

AIDF does not treat polished phishing text, high-quality code, rapid activity, or multilingual output as proof of AI use. These indicators may support an assessment but must be combined with technical, forensic, behavioral, or provider-side evidence.

## Core Principles

- Evidence fusion over single indicators.
- Behavior and integration over stylistic intuition.
- AI-role attribution before model attribution.
- Explicit consideration of alternative explanations.
- Separate confidence from analytic judgment.
- Preserve evidence before disrupting suspected AI infrastructure.

## AI Involvement Levels

| Level | Classification | Description |
|---:|---|---|
| 0 | No identified AI use | No material evidence of AI involvement was found. |
| 1 | AI-assisted | A human used AI for research, translation, advice, content, or code generation. |
| 2 | AI-augmented | AI was integrated into a defined attack workflow and accelerated selected tasks. |
| 3 | AI-orchestrated | AI interpreted results, selected tools, and coordinated multiple attack steps. |
| 4 | Semi-autonomous | AI completed a substantial portion of the operation under human supervision. |
| 5 | Autonomous | AI planned and executed a multi-step operation with limited human intervention. |

Levels 4 and 5 require evidence of a recurring operational loop:

```text
Observe -> Interpret -> Plan -> Select Tool -> Execute -> Evaluate -> Adapt
```

## AI Operational Roles

| Role | Description |
|---|---|
| Advisor | Provides recommendations, explanations, or troubleshooting assistance. |
| Content generator | Produces phishing text, scripts, code, images, audio, or other artifacts. |
| Translator | Localizes content and adapts language or cultural context. |
| Analyst | Analyzes scan results, logs, documents, credentials, or stolen data. |
| Tool operator | Selects a tool, creates a command, or triggers an action. |
| Orchestrator | Coordinates multiple tools and attack stages. |
| Autonomous agent | Pursues a goal through repeated planning, execution, and adaptation. |

## Detection Dimensions

### A - Access Evidence

Determines whether the attacker accessed an AI model, API, inference service, or supporting infrastructure.

Examples:

- Connections to an AI API or inference endpoint.
- API keys, tokens, model names, or provider endpoints.
- Local model servers or model files.
- AI gateway, vector database, embedding, or reranking services.
- Cloud audit events involving model endpoints, GPU instances, or inference workloads.

Primary telemetry:

- DNS and proxy logs.
- Firewall and NetFlow.
- TLS metadata.
- Cloud audit logs.
- Secret-manager access.
- EDR network and process telemetry.

### I - Integration Evidence

Determines whether AI was integrated into the attack code or operational workflow.

Examples:

- AI SDK imports or dependencies.
- Prompt templates and system prompts.
- Tool-calling JSON schemas.
- Planner-executor, critic, reflection, or retry components.
- Agent state, checkpoints, task queues, or memory stores.
- Model output passed directly into shell, PowerShell, Python, or another attack tool.
- MCP clients, servers, or other agent-tool interfaces.

Potential artifacts:

```text
.env
agent.yaml
tools.json
prompt.txt
system_prompt.md
memory.db
task_state.json
requirements.txt
package.json
docker-compose.yml
```

### D - Dynamic Behavior

Determines whether observed attacker behavior indicates context-sensitive adaptation rather than only deterministic automation.

Examples:

- Rapid correction after an unexpected error.
- Selection of a different exploit after version discovery.
- Payload generation based on target configuration.
- Semantic analysis of stolen documents.
- Adaptation of phishing content to the victim's role or response.
- Repeated action-feedback-adaptation cycles.
- Parallel activity across several targets while retaining context.

Dynamic behavior is not proof by itself. A large team, conventional automation, reusable playbooks, outsourcing, or stolen tooling can produce similar patterns.

### O - Output Evidence

Examines text, code, media, and other attacker outputs for supporting indicators.

Examples:

- Prompt remnants, Markdown fences, or chatbot response fragments in source files.
- Unreplaced placeholders or hallucinated details.
- Unusually explanatory comments or unused imports.
- Many semantically similar variants generated in a short period.
- Synthetic-media provenance or watermarking.
- Abrupt style changes between code sections.

Output evidence is the weakest dimension and must not be used alone to confirm AI involvement.

## Evidence Strength

| Level | Meaning | Examples |
|---:|---|---|
| E0 | No evidence | No material indicators identified. |
| E1 | Weak indicator | AI-like wording, polished phishing, unusual comments, or rapid activity. |
| E2 | Circumstantial evidence | AI endpoint traffic, AI libraries, local runtime, or context-sensitive behavior. |
| E3 | Strong technical evidence | Tool schema, prompt template, agent state, API key with correlated use, or repeated inference-to-execution sequence. |
| E4 | Direct evidence | Full prompts, provider logs, active agent infrastructure, captured model sessions, or confirmed provider attribution. |

An assessment should be classified as confirmed only when it contains at least one E4 item or two independent E3 evidence sources.

## AI Involvement Index

Each investigation scores the four dimensions from 0 to 100:

```text
AIIS = 0.30(A) + 0.35(I) + 0.20(D) + 0.15(O)
```

| Score | Assessment |
|---:|---|
| 0-19 | Insufficient evidence |
| 20-39 | Possible AI use |
| 40-59 | Probable AI use |
| 60-79 | Highly probable AI use |
| 80-100 | Confirmed or near-confirmed AI use |

A high score does not override the evidence threshold for a confirmed assessment.

## Investigation Workflow

### 1. Initial Detection

Owner: SOC Tier 1 and Tier 2.

- Tag the event as `suspected-ai-enabled-activity`.
- Preserve endpoint, network, email, identity, and cloud telemetry.
- Identify AI endpoints, SDKs, model runtimes, and suspicious process chains.
- Do not declare AI involvement at this stage.

### 2. Technical Enrichment

Owner: SOC Tier 3 and threat hunting.

- Correlate endpoint, network, cloud, and identity events.
- Build a high-resolution timeline.
- Look for tool-output-to-inference-to-execution sequences.
- Search for agent loops, dynamic payload generation, and local inference.
- Compare behavior with previous non-AI campaigns attributed to the same actor.

### 3. Forensic Examination

Owner: DFIR.

- Capture memory when legally and operationally appropriate.
- Preserve command history, browser history, environment variables, containers, and package manifests.
- Locate prompts, tool definitions, model identifiers, API keys, state files, and response artifacts.
- Reconstruct whether model output caused an attacker action.

### 4. Intelligence Assessment

Owner: CTI.

- Map AI use to attack stages and operational roles.
- Evaluate alternative explanations.
- Assess whether AI changes existing attribution judgments.
- Record confidence separately from the assessment.
- Seek provider corroboration when justified and legally authorized.

### 5. Organizational Determination

Owner: Incident commander or designated analytic authority.

Record:

- AI involvement assessment.
- Confidence level.
- Evidence strength.
- AI operational role and involvement level.
- Attack stages affected.
- Material contribution to speed, scale, adaptability, or impact.
- Legal, privacy, regulatory, and reporting implications.

## Required Telemetry

### Endpoint

- Process creation and parent-child relationships.
- Command-line and script-block logging.
- Python, PowerShell, shell, and interpreter activity.
- DLL and library loading.
- File creation and model-file access.
- Environment variables and credential access.
- Browser and network connection telemetry.

### Network

- DNS queries and responses.
- Proxy and secure web gateway logs.
- Firewall and NetFlow.
- TLS metadata and destination categorization.
- API gateway and egress telemetry.

### Cloud

- AI-service audit events.
- Model endpoint and GPU instance creation.
- Secret-manager access.
- Container and serverless deployment events.
- Inference usage and cost anomalies.

### Email and Collaboration

- Message headers and thread history.
- Response timing and identity telemetry.
- Attachment metadata.
- Audio, image, and video provenance where available.

## Detection Use Cases

### Reconnaissance Followed by AI Inference

```text
Scanner executes
-> output file is created
-> connection to an AI endpoint
-> new script or command is created
-> generated content executes
```

### Agentic Tool Loop

```text
Tool execution
-> result captured
-> outbound inference request
-> structured response
-> next tool execution
```

Raise confidence when the loop repeats and the next action is semantically related to the previous result.

### Local Model During an Intrusion

```text
Suspicious process
-> model runtime or GPU library loaded
-> model file accessed
-> command or script generated
-> attack tool executes
```

### Dynamic Payload Generation

```text
Target or environment data collected
-> payload generated or modified
-> payload immediately executed
```

### AI-Assisted Social Engineering

Look for combinations of high-volume personalization, rapid contextual replies, multilingual adaptation, synthetic media, and current public information. None of these indicators is conclusive alone.

## Sigma Rules

Experimental Sigma rules are provided in [`sigma/`](./sigma/). They are starting points and require local validation, field mapping, allowlisting, and correlation support.

| Rule | Purpose |
|---|---|
| [`ai_sdk_or_agent_framework_execution.yml`](./sigma/ai_sdk_or_agent_framework_execution.yml) | Detects suspicious interpreter command lines referencing common AI SDKs and agent frameworks. |
| [`local_ai_model_runtime_from_suspicious_parent.yml`](./sigma/local_ai_model_runtime_from_suspicious_parent.yml) | Detects local model runtimes launched by unusual or attack-associated parent processes. |
| [`ai_api_key_access_by_suspicious_process.yml`](./sigma/ai_api_key_access_by_suspicious_process.yml) | Detects suspicious command-line access to common AI API-key environment variables. |
| [`scan_output_followed_by_ai_client_execution.yml`](./sigma/scan_output_followed_by_ai_client_execution.yml) | Detects a potential handoff from reconnaissance output to an AI client or agent script. |
| [`ai_generated_script_immediate_execution.yml`](./sigma/ai_generated_script_immediate_execution.yml) | Detects scripts created in temporary locations and quickly launched by interpreters. |

Some high-value use cases require correlation across endpoint and network datasets. Implement these in the SIEM using the generic correlation logic described in [`sigma/CORRELATION.md`](./sigma/CORRELATION.md).

## Assessment Record

Each case should include:

| Field | Required value |
|---|---|
| Incident ID | Organizational incident identifier |
| Assessment | None, possible, probable, highly probable, or confirmed |
| Confidence | Low, moderate, or high |
| AI role | Advisor, generator, translator, analyst, operator, orchestrator, or autonomous agent |
| Involvement level | 0-5 |
| Evidence level | E0-E4 |
| AIIS score | 0-100 |
| Attack stages | Stages in which AI was used |
| Direct evidence | Provider, forensic, or captured-session evidence |
| Behavioral evidence | Observed adaptive or agentic behavior |
| Alternative explanations | Automation, team size, reuse, outsourcing, or stolen tools |
| Model or provider | Known, suspected, or unknown |
| Material impact | Contribution to speed, scale, adaptability, or harm |
| Recommended actions | Detection, containment, intelligence, legal, and reporting actions |

## Example Analytic Judgment

> We assess with moderate confidence that the threat actor used an AI system to analyze reconnaissance results and produce follow-on commands. This assessment is based on a recurring sequence of scanner execution, result-file creation, communication with a model endpoint, receipt of a structured response, and execution of a semantically related tool. We lack sufficient evidence to identify the model provider or determine that the broader intrusion was autonomous.

## Governance

- SOC owns initial detection and evidence preservation.
- Threat hunting owns pattern discovery and correlation development.
- DFIR owns forensic acquisition and timeline reconstruction.
- CTI owns role assessment, confidence, alternatives, and attribution impact.
- Cloud security owns visibility into model services, gateways, secrets, and inference workloads.
- Legal and privacy teams assess provider contact, data disclosure, retention, and notification duties.
- The CISO approves thresholds, reporting requirements, and risk treatment.

## Maturity Model

| Level | Description |
|---:|---|
| 1 - Ad hoc | No common definitions, limited telemetry, intuition-driven assessments. |
| 2 - Defined | Common terminology, incident fields, basic detections, and initial training. |
| 3 - Integrated | Shared SOC, DFIR, and CTI workflow with SIEM correlation and scoring. |
| 4 - Measured | Accuracy, timeliness, false positives, and analyst consistency are measured. |
| 5 - Adaptive | Agentic behavior analytics, cross-domain correlation, exercises, and provider collaboration are institutionalized. |

## 90-Day Implementation Plan

### Days 1-30

- Approve definitions, roles, evidence levels, and confidence language.
- Inventory approved and unapproved AI services and endpoints.
- Assess DNS, proxy, EDR, cloud, identity, and email visibility.
- Add AI involvement fields to incident records.

### Days 31-60

- Deploy and tune the initial Sigma rules.
- Implement at least two cross-source correlation use cases.
- Create a forensic collection checklist and assessment template.
- Train SOC, DFIR, CTI, cloud, legal, and privacy stakeholders.

### Days 61-90

- Conduct a threat hunt and purple-team exercise.
- Simulate conventional automation, AI-assisted operation, and an agentic loop.
- Measure false positives and analyst agreement.
- Refine scoring, thresholds, allowlists, and data requirements.
- Present residual gaps and a one-year roadmap to the CISO.

## Limitations

- AI-generated content detectors are not reliable enough to serve as sole evidence.
- Encrypted traffic and private models can obscure model use.
- Conventional automation can resemble agentic behavior.
- Human editing can remove model artifacts.
- Model attribution is substantially harder than identifying an AI operational role.
- Sigma rules cannot express every required temporal or cross-source relationship consistently across SIEM platforms.

## Defensive Use Disclaimer

This framework and its rules are provided for defensive research and detection engineering. All rules require local validation and tuning before production deployment. A match indicates activity requiring investigation, not proof that a threat actor used AI.