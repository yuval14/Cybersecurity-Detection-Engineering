# AIDAF Sigma Detection Pack

## Purpose

These experimental Sigma rules provide investigation signals related to adversarial AI integration. They do not independently prove that an attacker used AI.

## Rule Catalog

| Rule | Primary signal | AIDAF dimension |
|---|---|---|
| `suspicious_process_connection_to_ai_model_service.yml` | Script or administration process connects to an external model service | Access / Integration |
| `ai_client_spawns_script_interpreter.yml` | AI client or agent framework launches a shell or interpreter | Integration |
| `local_model_runtime_from_suspicious_parent.yml` | Local model runtime starts from an unusual parent | Access / Integration |
| `generated_script_execution_from_temporary_directory.yml` | Script interpreter executes code from a temporary or user-writable path | Dynamic Behavior / Output |

## Required Telemetry

- Process creation with parent process and command line
- Network connections with process image and destination hostname
- DNS and proxy telemetry where endpoint destination fields are unavailable
- File creation telemetry for correlation with generated script execution
- AI gateway, API gateway, CASB, or model-provider audit logs where available

## Validation

Before deployment:

1. Validate field mappings against the target SIEM schema.
2. Establish approved AI tools, service domains, runtimes, and development hosts.
3. Tune authorized automation, data-science, security-testing, and software-installation activity.
4. Test the rules using benign simulations and controlled purple-team exercises.
5. Correlate multiple rules before raising AIDAF evidence above E1.

## Limitations

- Attackers may use private gateways, self-hosted models, proxies, or renamed binaries.
- Legitimate AI development may resemble malicious integration.
- External attackers may use AI entirely outside victim visibility.
- Process names and destination domains are mutable and require local enrichment.
- Sigma does not reliably express all required temporal and cross-source relationships.

## Analytic Use

Treat each match as a supporting observation. Increase confidence only when the signal aligns with attack-stage activity, a coherent timeline, direct artifacts, threat intelligence, or provider-side evidence.