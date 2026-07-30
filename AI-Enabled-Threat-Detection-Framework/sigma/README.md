# Sigma Coverage for AI-Enabled Threat Detection

These rules generate investigative signals for the AI-Enabled Threat Detection Framework. They do not independently prove that a threat actor used AI.

## Internal or Victim-Side AI Visibility

| Rule | Primary telemetry | Purpose |
|---|---|---|
| `ai_sdk_or_agent_framework_execution.yml` | Process creation | Detect suspicious execution referencing common AI SDKs or agent frameworks. |
| `local_ai_model_runtime_from_suspicious_parent.yml` | Process creation | Detect a local model runtime launched from an unusual or attack-associated parent. |
| `ai_api_key_access_by_suspicious_process.yml` | Process creation | Detect suspicious attempts to read or expose AI API credentials. |
| `scan_output_followed_by_ai_client_execution.yml` | Process creation | Detect a possible handoff from reconnaissance output to an AI client or agent script. |
| `ai_generated_script_immediate_execution.yml` | Process creation | Detect rapid execution of scripts created in temporary or writable locations. |

## External Adversary AI Signals

These rules are designed for cases in which inference, prompts, and agent state remain on attacker-controlled infrastructure.

| Rule | Primary telemetry | Purpose |
|---|---|---|
| `external_rapid_web_probe_variation.yml` | WAF, reverse proxy, web server | Detect varied external reconnaissance and exploitation probes for later temporal correlation. |
| `external_authentication_strategy_variation.yml` | Authentication | Detect failed external authentication activity whose strategy may change across accounts, services, or methods. |
| `external_dynamic_command_injection_variants.yml` | WAF, reverse proxy, web server | Detect multiple command-injection payload families for response-conditioned correlation. |

## Required Correlation

Use [`CORRELATION.md`](./CORRELATION.md) to implement the high-value sequences that Sigma atomic rules cannot express portably, including:

- External probe variation conditioned on HTTP responses.
- Authentication strategy changes conditioned on failure reasons.
- Command-injection payload changes conditioned on WAF or application responses.
- Internal tool-to-inference-to-tool loops.
- Local-model execution combined with attack tooling.

## Validation Requirements

Before production deployment:

1. Validate YAML and Sigma syntax with the organization's selected Sigma tooling.
2. Map generic fields to the local schema.
3. Confirm required log sources retain source identity, target, response, timing, and relevant payload data.
4. Allowlist authorized scanners, penetration tests, red teams, research systems, and approved AI workloads.
5. Test against conventional automation, human-led activity, AI-assisted operations, and agentic operations.
6. Measure alert volume, false positives, and analyst agreement.
7. Record every match as a signal or lead, not as proof of AI involvement.

## Coverage Limitations

- External adversaries may use AI without producing any AI-specific indicator inside the victim environment.
- NAT, proxies, botnets, and distributed infrastructure can obscure source clustering.
- Conventional scanners and decision trees can resemble AI-driven adaptation.
- Stylometric or code-generation indicators are not represented as standalone Sigma rules because they are not sufficiently reliable.
- Semantic adaptation generally requires SIEM correlation or analytic review beyond a single event.