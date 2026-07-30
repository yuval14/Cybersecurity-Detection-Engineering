# AIDAF Sigma Detection Tracks

## External AI-Enabled Adversary

These rules analyze attack outputs at the organizational boundary. They cannot directly observe the adversary's AI system and should normally contribute E1–E2 evidence.

- `external/rapid_web_payload_variation_after_errors.yml`
- `external/distributed_target_specific_authentication_variation.yml`
- `external/high_variation_phishing_from_shared_infrastructure.yml`

Required SIEM correlation:

- Count distinct payloads, encodings, methods, user agents, or authentication methods.
- Correlate failures with subsequent changed attempts.
- Group rotating source addresses by session, fingerprint, ASN, infrastructure, target, and behavioral sequence.
- Compare activity with authorized scanners, red teams, and known internet noise.

## Intrusion-Stage or Post-Compromise AI Use

These rules analyze AI-linked tooling or workflow integration on organizational assets. They may support E2–E4 evidence when combined with process trees, file activity, model calls, prompt or tool-call logs, and identity telemetry.

- `internal/ai_client_following_host_discovery.yml`
- `internal/suspicious_ai_api_key_access.yml`
- `internal/victim_data_sent_to_ai_service.yml`
- `internal/repeated_agent_tool_execution_loop.yml`

Required SIEM correlation:

- Correlate discovery commands with subsequent model-client or model-runtime activity.
- Correlate file access, archive creation, and network connections to model services.
- Count repeated parent-child tool invocations within short windows.
- Link AI API-key access to the requesting user, process, host, and later model calls.
- Distinguish authorized enterprise AI agents, coding assistants, SOAR workers, and data-science systems.

## Interpretation

A single Sigma match is not proof that an attacker used AI. External-track detections are generally behavioral and probabilistic. Internal-track detections may be stronger because they can reveal model, agent, credential, process, and tool integration. Analysts must record the adversary position, available telemetry, evidence level, confidence, and non-AI alternatives.