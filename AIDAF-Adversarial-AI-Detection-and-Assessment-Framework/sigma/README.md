# AIDAF Sigma Detection Pack

## Purpose

These experimental Sigma rules provide investigation signals related to adversarial AI integration. They do not independently prove that an attacker used AI.

## Guidance and Correlation Documents

- [`DETECTION-TRACKS.md`](DETECTION-TRACKS.md) — separates external AI-enabled activity from intrusion-stage and post-compromise AI use.
- [`CORRELATION.md`](CORRELATION.md) — baseline correlation guidance for combining individual Sigma signals.
- [`AI-ATTACK-CORRELATION-CATALOG.md`](AI-ATTACK-CORRELATION-CATALOG.md) — expanded SIEM correlation patterns across reconnaissance, authentication, lateral movement, collection, defense evasion, and agentic activity.

## Root-Level Sigma Rules

| Rule | Primary signal | AIDAF dimension |
|---|---|---|
| [`suspicious_process_connection_to_ai_model_service.yml`](suspicious_process_connection_to_ai_model_service.yml) | Script or administration process connects to an external model service | Access / Integration |
| [`ai_client_spawns_script_interpreter.yml`](ai_client_spawns_script_interpreter.yml) | AI client or agent framework launches a shell or interpreter | Integration |
| [`local_model_runtime_from_suspicious_parent.yml`](local_model_runtime_from_suspicious_parent.yml) | Local model runtime starts from an unusual parent | Access / Integration |
| [`generated_script_execution_from_temporary_directory.yml`](generated_script_execution_from_temporary_directory.yml) | Script interpreter executes code from a temporary or user-writable path | Dynamic Behavior / Output |

## External AI-Enabled Adversary Rules

These rules focus on attacker behavior observed at the organizational boundary. They usually provide E1–E2 evidence and require SIEM correlation.

| Rule | Primary signal |
|---|---|
| [`external/rapid_web_payload_variation_after_errors.yml`](external/rapid_web_payload_variation_after_errors.yml) | Web payload structure changes after errors or security-control responses |
| [`external/distributed_target_specific_authentication_variation.yml`](external/distributed_target_specific_authentication_variation.yml) | Authentication strategy varies across sources against the same target |
| [`external/high_variation_phishing_from_shared_infrastructure.yml`](external/high_variation_phishing_from_shared_infrastructure.yml) | High-variation phishing content delivered through shared infrastructure |
| [`external/adaptive_directory_discovery_after_http_denials.yml`](external/adaptive_directory_discovery_after_http_denials.yml) | Requested paths change rapidly after HTTP 403 or 404 responses |
| [`external/adaptive_password_spray_strategy.yml`](external/adaptive_password_spray_strategy.yml) | Password-spray behavior changes usernames, timing, or client attributes |
| [`external/mfa_fatigue_with_variable_cadence.yml`](external/mfa_fatigue_with_variable_cadence.yml) | MFA push attempts use variable cadence or client characteristics |

## Intrusion-Stage and Post-Compromise Rules

These rules focus on AI-linked tooling, workflows, model access, and adaptive behavior on organizational assets. Correlated artifacts may support E2–E4 evidence.

| Rule | Primary signal |
|---|---|
| [`internal/ai_client_following_host_discovery.yml`](internal/ai_client_following_host_discovery.yml) | AI client or runtime activity follows host or domain discovery |
| [`internal/suspicious_ai_api_key_access.yml`](internal/suspicious_ai_api_key_access.yml) | Unexpected process accesses AI-service API-key material |
| [`internal/victim_data_sent_to_ai_service.yml`](internal/victim_data_sent_to_ai_service.yml) | Victim data is transmitted to an external AI service |
| [`internal/repeated_agent_tool_execution_loop.yml`](internal/repeated_agent_tool_execution_loop.yml) | Repeated agent or model tool-execution loop |
| [`internal/ai_linked_process_modifies_agent_policy_or_memory.yml`](internal/ai_linked_process_modifies_agent_policy_or_memory.yml) | AI-linked process modifies agent memory, policy, identity, or configuration |
| [`internal/adaptive_protocol_switch_after_remote_access_failure.yml`](internal/adaptive_protocol_switch_after_remote_access_failure.yml) | Remote-access technique switches after failed attempts |
| [`internal/semantic_sensitive_file_discovery.yml`](internal/semantic_sensitive_file_discovery.yml) | Script or agent searches for business-sensitive concepts |
| [`internal/edr_aware_tool_switching.yml`](internal/edr_aware_tool_switching.yml) | Execution method changes after security-control failure or blocking |
| [`internal/ai_agent_spawns_multiple_network_tools.yml`](internal/ai_agent_spawns_multiple_network_tools.yml) | AI agent or model client launches reconnaissance or remote-administration tools |
| [`internal/local_model_download_after_suspicious_execution.yml`](internal/local_model_download_after_suspicious_execution.yml) | Local model or runtime is downloaded following suspicious execution |
| [`internal/agent_trace_or_prompt_artifact_creation.yml`](internal/agent_trace_or_prompt_artifact_creation.yml) | Prompt, plan, tool-call, trace, memory, or agent-state artifacts are created in unusual paths |

## Required Telemetry

- Process creation with parent process and command line
- Network connections with process image and destination hostname
- DNS and proxy telemetry where endpoint destination fields are unavailable
- File creation telemetry for correlation with generated script execution
- Authentication and MFA telemetry with result, source, client, and target identity
- Web and WAF telemetry with URI, payload, status, source, and session fields
- Email telemetry with sender infrastructure, subject, recipient, URLs, and attachments
- AI gateway, API gateway, CASB, or model-provider audit logs where available
- Agent, prompt, trace, tool-call, policy, identity, and memory logs where available

## Validation

Before deployment:

1. Validate field mappings against the target SIEM schema.
2. Establish approved AI tools, service domains, runtimes, agents, and development hosts.
3. Tune authorized automation, data-science, security-testing, and software-installation activity.
4. Test the rules using benign simulations and controlled purple-team exercises.
5. Correlate multiple rules before raising AIDAF evidence above E1.
6. Preserve failed attempts, intermediate outputs, and full process or session timelines.

## Limitations

- Attackers may use private gateways, self-hosted models, proxies, renamed binaries, or external AI infrastructure invisible to the victim.
- Legitimate AI development, automation, scanners, SOAR, RPA, and red-team activity may resemble malicious integration.
- Process names, paths, destination domains, and tool identifiers are mutable and require local enrichment.
- Sigma does not reliably express all required temporal, cardinality, semantic, and cross-source relationships.

## Analytic Use

Treat each match as a supporting observation. Increase confidence only when the signal aligns with attack-stage activity, a coherent timeline, direct artifacts, threat intelligence, or provider-side evidence. External behavioral rules normally indicate behavior consistent with AI assistance, while internal runtime, prompt, trace, credential, and tool-call artifacts may support stronger assessments.
