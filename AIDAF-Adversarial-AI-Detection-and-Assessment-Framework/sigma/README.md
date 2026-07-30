# AIDAF Sigma Detection Pack

## Purpose

This directory is the single flat library for all AIDAF Sigma YAML rules. Rules are not separated into `external`, `internal`, or `case-studies` subdirectories. Operational context is preserved through file names, descriptions, references, and tags such as `aidaf.external`, `aidaf.internal`, and case-specific tags.

Every rule is an experimental investigation signal. No individual match independently proves that an attacker used AI.

## Detection Groups

### External AI-Enabled Adversary

- `adaptive_directory_discovery_after_http_denials.yml`
- `adaptive_password_spray_strategy.yml`
- `distributed_target_specific_authentication_variation.yml`
- `high_variation_phishing_from_shared_infrastructure.yml`
- `mfa_fatigue_with_variable_cadence.yml`
- `rapid_web_payload_variation_after_errors.yml`

These rules usually provide E1-E2 behavioral evidence because the adversary's model, prompts, agent, and infrastructure remain outside organizational visibility.

### Intrusion and Post-Compromise AI Use

- `adaptive_protocol_switch_after_remote_access_failure.yml`
- `agent_trace_or_prompt_artifact_creation.yml`
- `ai_agent_spawns_multiple_network_tools.yml`
- `ai_client_following_host_discovery.yml`
- `ai_linked_process_modifies_agent_policy_or_memory.yml`
- `edr_aware_tool_switching.yml`
- `local_model_download_after_suspicious_execution.yml`
- `repeated_agent_tool_execution_loop.yml`
- `semantic_sensitive_file_discovery.yml`
- `suspicious_ai_api_key_access.yml`
- `victim_data_sent_to_ai_service.yml`

These rules can support E2-E4 assessments when correlated with model endpoints, runtimes, API keys, prompts, traces, tool calls, process trees, or provider evidence.

### Unit 42 Autonomous AI Campaign-Derived Rules

- `ai_agent_model_proxy_connection.yml`
- `fofa_query_followed_by_python_scanner.yml`
- `github_poc_clone_followed_by_execution.yml`
- `permissive_ai_agent_execution_configuration.yml`
- `python_http_server_from_home_directory.yml`
- `telegram_controlled_agent_spawns_shell.yml`
- `unit42_autonomous_scan_download_exploit_correlation.yml`

These detections are derived from observable tradecraft reported by Unit 42: Hermes Agent with DeepSeek, FOFA enumeration, GitHub PoC acquisition, threaded Python scanning, model-proxy access, permissive agent settings, Telegram orchestration, and a Python HTTP server started from the operator home directory.

### GTIG Runtime AI Malware-Derived Rules

- `promptflux_vbscript_gemini_self_modification.yml`
- `promptflux_thinking_robot_log_creation.yml`
- `promptflux_vbscript_spread_to_removable_and_network_drives.yml`
- `promptsteal_huggingface_api_command_execution.yml`
- `promptsteal_programdata_info_collection.yml`
- `quietvault_ai_cli_secret_search_github_exfiltration.yml`
- `promptlock_llm_generated_lua_execution.yml`
- `gtig_runtime_ai_malware_correlation.yml`

These rules are derived from Google Threat Intelligence Group reporting on PROMPTFLUX, PROMPTSTEAL/LAMEHUG, QUIETVAULT, and PROMPTLOCK. They focus on direct runtime model integration, generated-command execution, AI-assisted secret search, dynamic script creation, persistence, collection, propagation, and exfiltration.

### Baseline AI Integration Rules

- `ai_client_spawns_script_interpreter.yml`
- `generated_script_execution_from_temporary_directory.yml`
- `local_model_runtime_from_suspicious_parent.yml`
- `suspicious_process_connection_to_ai_model_service.yml`

## Guidance

- `DETECTION-TRACKS.md` — interpretation of external versus intrusion/post-compromise activity.
- `CORRELATION.md` — baseline cross-source correlation guidance.
- `AI-ATTACK-CORRELATION-CATALOG.md` — expanded attack-stage correlation patterns.
- `case-studies/UNIT42-AUTONOMOUS-AI-CAMPAIGN.md` — supporting case analysis.

## Required Telemetry

- Process creation with parent process, command line, user, session, and process GUID
- Network, DNS, proxy, WAF, web, email, authentication, and MFA telemetry
- File creation and file access events
- AI gateway, API gateway, CASB, model-provider, prompt, trace, tool-call, memory, and agent logs where available

## Validation

1. Validate YAML and Sigma syntax against the selected Sigma tooling.
2. Map generic fields to the target SIEM schema.
3. Establish allowlists for approved AI tools, agents, gateways, development systems, scanners, red teams, and research environments.
4. Test human-only, conventional automation, AI-assisted, and agentic scenarios as separate control groups.
5. Preserve failures, retries, intermediate outputs, and complete timelines.
6. Use SIEM correlation before raising the AIDAF evidence level.

## Analytic Guardrail

Detect the workflow and observable tradecraft, not writing style or sophistication. External adaptation is generally circumstantial evidence. Direct runtime, prompt, trace, API-key, model, agent, and provider artifacts support stronger assessments.