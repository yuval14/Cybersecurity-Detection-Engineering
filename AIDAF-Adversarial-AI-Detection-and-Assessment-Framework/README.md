# AIDAF – Adversarial AI Detection and Assessment Framework

AIDAF provides a structured method for detecting, assessing, and reporting adversarial use of artificial intelligence during cyber operations.

The framework is designed for cases in which an attacker may use AI externally, internally, or across a hybrid attack workflow. It does not assume that defenders can directly observe the adversary's model, prompts, agent configuration, or supporting infrastructure.

## Purpose

AIDAF helps SOC, DFIR, threat-hunting, detection-engineering, and CTI teams answer five questions:

1. What operational role did AI appear to perform?
2. At which attack stages was AI likely used?
3. What observable evidence supports the assessment?
4. How much operational advantage did AI provide?
5. How confident is the organization in the conclusion?

AIDAF should not be used to classify activity as AI-enabled solely because it is fast, automated, adaptive, or technically sophisticated. Conventional scripts, malware frameworks, human operators, and standard automation can produce similar behavior.

## Scope

AIDAF covers:

- External attackers using commercial or private AI services
- AI-assisted reconnaissance and target selection
- AI-generated social engineering and operational content
- AI-assisted vulnerability analysis and exploitation
- Agentic tool orchestration
- Dynamic payload generation and modification
- AI-assisted credential attacks
- AI-supported command-and-control decisions
- AI-enabled lateral movement and persistence
- AI-assisted data discovery and exfiltration
- Semi-autonomous or autonomous attack workflows

## AI Operational Roles

AIDAF distinguishes the role performed by AI:

| Role | Description |
|---|---|
| Advisor | Recommends tactics, commands, targets, or next actions |
| Content Generator | Produces text, code, scripts, payloads, lures, or documents |
| Analyst | Interprets scan results, logs, credentials, errors, or target data |
| Translator | Adapts language, terminology, tone, or regional context |
| Tool Operator | Invokes individual tools or executes commands |
| Orchestrator | Coordinates multiple tools and attack steps |
| Autonomous Agent | Pursues an objective with limited human intervention |

## AI Involvement Levels

| Level | Classification | Description |
|---:|---|---|
| AI-0 | No substantiated AI use | Evidence supports conventional human or automated activity |
| AI-1 | AI-assisted | AI supports isolated tasks such as drafting, translation, or coding |
| AI-2 | AI-augmented | AI materially improves speed, scale, targeting, or adaptation |
| AI-3 | AI-orchestrated | AI coordinates multiple attack tasks or tools |
| AI-4 | Semi-autonomous | AI executes iterative attack loops with periodic human control |
| AI-5 | Autonomous | AI independently plans, executes, evaluates, and adapts toward an objective |

## Assessment Dimensions

AIDAF uses four primary dimensions.

### A – Access

Assesses whether the adversary appears to have access to AI capabilities.

Potential evidence includes:

- AI-service credentials or account artifacts
- Local model runtimes or model files
- AI SDKs, agent frameworks, or model-gateway clients
- Provider-side records, where lawfully available
- Threat-intelligence reporting linking the actor to AI-enabled tradecraft

### I – Integration

Assesses whether AI is integrated into the attack workflow rather than used as an isolated support tool.

Indicators include:

- Tool-to-model-to-tool execution chains
- Structured model outputs consumed by scripts or agents
- Repeated inference between attack steps
- Model-generated artifacts executed immediately
- AI-linked services embedded in malware or automation infrastructure

### D – Dynamic Behavior

Assesses whether the attack adapts rapidly to target responses.

Indicators include:

- Payload changes following parser, WAF, authentication, or application errors
- Reconnaissance that changes based on newly discovered services
- Fast switching between techniques after failures
- Target-specific generation of commands, scripts, or lures
- Iterative testing with short decision cycles

Dynamic behavior is supporting evidence only. It may also result from conventional automation or skilled human operation.

### O – Output

Assesses whether observable outputs are consistent with AI-assisted generation.

Examples include:

- High-volume personalized phishing content
- Rapid multilingual adaptation
- Target-specific scripts or payloads
- Consistent structured command objects
- Generated code containing target-derived values
- Large numbers of semantically varied attack artifacts

Output characteristics must not be treated as proof without corroborating evidence.

## Evidence Levels

| Level | Evidence quality | Example |
|---:|---|---|
| E0 | No evidence | Suspicion based only on speed or sophistication |
| E1 | Weak | Output patterns consistent with AI but highly ambiguous |
| E2 | Moderate | Multiple behavioral indicators and correlated telemetry |
| E3 | Strong | AI runtime, SDK, credential, endpoint, or workflow artifacts |
| E4 | Direct | Provider records, recovered prompts, agent configuration, or confirmed operator disclosure |

## AIDAF Scoring Model

An organization may calculate an Adversarial AI Involvement Score:

```text
AAIIS = (0.30 × A) + (0.35 × I) + (0.20 × D) + (0.15 × O)
```

Each dimension is scored from 0 to 100.

Suggested interpretation:

| Score | Assessment |
|---:|---|
| 0–19 | Insufficient indication |
| 20–39 | Possible AI involvement |
| 40–59 | Plausible AI involvement |
| 60–79 | Probable AI involvement |
| 80–100 | Highly probable AI involvement |

The final assessment must also include an evidence level and analytic confidence. A high score with weak evidence should not be reported as confirmed AI use.

## Confidence

Use a separate confidence statement:

- Low confidence
- Moderate confidence
- High confidence

Confidence should reflect:

- Evidence quality
- Source reliability
- Alternative explanations
- Telemetry completeness
- Reproducibility
- Intelligence corroboration

## Detection Workflow

1. Detect suspicious behavior using endpoint, network, identity, application, cloud, email, and security-control telemetry.
2. Preserve the full attack timeline, including failed actions.
3. Identify possible AI roles and relevant attack stages.
4. Score Access, Integration, Dynamic Behavior, and Output.
5. Assign an evidence level.
6. Test conventional automation and human operation as alternative explanations.
7. Correlate technical findings with CTI and provider-side data where available.
8. Assign the AI involvement level and confidence.
9. Record the operational contribution of AI.
10. Document detection gaps and required improvements.

## Detection Engineering Guidance

Sigma and platform-specific detections should generate investigation signals, not definitive AI-attribution alerts.

Priority detection patterns include:

- AI SDK or agent-framework execution on unauthorized systems
- Local-model runtime launched from suspicious parent processes
- Access to AI-service keys by unexpected processes
- Reconnaissance followed by AI-client execution
- Newly generated scripts executed immediately
- Rapid external web-probe variation
- Authentication-strategy variation following failures
- Dynamic command-injection payload variation
- Repeated tool-inference-tool loops

Temporal and cross-source correlation should be implemented in the target SIEM because many AI-relevant behaviors cannot be represented reliably in a single portable Sigma rule.

## External-Adversary Assessment

When the adversary's AI infrastructure remains outside organizational visibility, defenders should prioritize behavioral and temporal evidence:

- Response-driven adaptation
- Operational tempo changes
- High variation with target specificity
- Iterative failure and correction loops
- Cross-stage coordination
- Intelligence corroboration

In these cases, the correct conclusion may be "behavior consistent with AI-assisted activity" rather than "confirmed AI use."

## Reporting Template

```text
Incident or Campaign:
Date Range:
Threat Actor or Cluster:
Affected Assets:

Suspected AI Role:
Relevant Attack Stages:
AI Involvement Level:

Access Score:
Integration Score:
Dynamic Behavior Score:
Output Score:
AAIIS:

Evidence Level:
Confidence:

Key Evidence:
Alternative Explanations:
Operational Contribution of AI:
Detection Gaps:
Recommended Actions:
```

## Governance Principles

- Require human review for confirmed or high-confidence attribution.
- Preserve provenance for evidence, scoring, and analytic judgments.
- Separate detection from attribution.
- Record plausible non-AI explanations.
- Reassess conclusions when new evidence becomes available.
- Avoid public claims based only on generated-content characteristics.
- Validate all AI-related detection logic against benign automation and authorized testing.

## Metrics

Suggested organizational metrics include:

- Percentage of relevant incidents assessed with AIDAF
- Time required to identify possible AI involvement
- Percentage of assessments supported by E2 or stronger evidence
- False-positive rate for AI-related detection signals
- Number of validated AI-related correlation use cases
- Percentage of AI-related rules with test evidence
- Detection coverage by AI role and attack stage
- Number of assessments revised after additional evidence

## Relationship to Other Repository Content

AIDAF complements the repository's AI-Enabled Threat Detection Framework, Sigma rules, correlation use cases, threat-hunting methods, detection lifecycle practices, and governance guidance.

It is intended to provide the dedicated assessment layer for determining how AI may have contributed to adversarial cyber activity, while avoiding unsupported claims of AI attribution.

## Status

Initial framework version: 2026

Author: Yuval Sinay

This framework is intended for defensive, research, and detection-engineering use. It requires environment-specific validation and does not independently prove adversarial AI use.
