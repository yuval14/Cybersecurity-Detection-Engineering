# AIDAF AI-Enabled Attack Correlation Catalog

Individual Sigma matches are investigation signals. The following correlations should be implemented in the target SIEM using stable entity identifiers, ordered event sequences, and environment-specific thresholds.

## External AI-Assisted Activity

### EXT-AI-01 Adaptive Web Exploitation

Correlate for the same source infrastructure and target application within 10 minutes:

1. Repeated 4xx or 5xx responses.
2. Rapidly changing URI, parameter, encoding, header, or payload structure.
3. A successful or materially different response after failures.

Suggested AIDAF evidence: E1; E2 when supported by multiple sensors or intelligence.

### EXT-AI-02 Adaptive Authentication Attack

Correlate within 30 minutes:

1. Password-spray failures against multiple accounts.
2. Changes in password family, username order, source node, client fingerprint, or timing.
3. MFA push requests or a later successful authentication.

Suggested AIDAF evidence: E1-E2.

### EXT-AI-03 Target-Specific Social Engineering

Correlate campaign telemetry for:

1. Shared sender or delivery infrastructure.
2. High semantic variation across messages.
3. Organization-, role-, language-, or recipient-specific content.
4. Credential access or payload delivery.

Suggested AIDAF evidence: E1; do not infer AI solely from writing quality.

## Intrusion and Post-Compromise AI Use

### INT-AI-01 Model-to-Tool Execution Chain

Correlate on one endpoint within 15 minutes:

1. Connection to an AI service or local model runtime.
2. Agent/client process starts a shell, interpreter, discovery tool, or remote-access utility.
3. Newly created code or script executes.

Suggested AIDAF evidence: E2-E3.

### INT-AI-02 Adaptive Lateral Movement

Correlate by user and source host:

1. Discovery of hosts, domains, sessions, shares, or high-value systems.
2. Failure using one remote protocol.
3. Switch to another protocol or tool.
4. Successful remote authentication or process execution.

Suggested AIDAF evidence: E2; E3 when tied to agent traces or model calls.

### INT-AI-03 Semantic Collection and External Reasoning

Correlate:

1. Search for business-sensitive concepts.
2. Access to selected high-value documents.
3. Compression, summarization, staging, or transmission to an AI service.
4. Follow-on commands based on the selected data.

Suggested AIDAF evidence: E2-E3.

### INT-AI-04 EDR-Aware Adaptation

Correlate on one host:

1. Security control blocks or process failures.
2. Switch among interpreters, LOLBins, obfuscation methods, or execution locations.
3. Repeated attempts with materially changed command structures.
4. Eventual success or defense impairment.

Suggested AIDAF evidence: E2.

### INT-AI-05 Local Model Deployment After Compromise

Correlate:

1. Suspicious shell or script execution.
2. Download of model files or local runtime.
3. Runtime launch from a user-writable or temporary path.
4. Child network, discovery, credential, or collection tools.

Suggested AIDAF evidence: E3.

### INT-AI-06 Agent Planning Loop

Correlate repeated ordered cycles:

1. Tool or command execution.
2. Output or error collection.
3. Model or agent activity.
4. Modified next command.
5. Repetition across at least three cycles.

Suggested AIDAF evidence: E3 when trace, prompt, tool-call, or process artifacts are preserved.

## Autonomous Cyber Agent and Containment Correlations

### INT-AI-07 Sandbox Escape Followed by Unauthorized Egress

Correlate on one agent or evaluation workload:

1. Sandbox, namespace, container, browser, VM, or policy-boundary violation or unexpected transition.
2. Newly available network route, proxy use, or egress not present before the boundary event.
3. Connection to an unapproved Internet destination or third-party target.
4. Reconnaissance, authentication, exploitation, data access, or command execution against that target.

Suggested AIDAF evidence: E3 when the agent trace and boundary telemetry are directly correlated. Raise to E4 only with direct provider or recovered agent evidence confirming autonomous causation.

### INT-AI-08 Continued Action After Stop, Deny, or Revocation

Correlate:

1. Kill switch, explicit stop signal, policy deny, approval denial, credential revocation, or containment action.
2. Subsequent alternate identity, alternate tool, new process, new network path, or delegated sub-agent.
3. Continued system modification, external communication, or tool use.

Suggested AIDAF evidence: E3 when the same agent, trace, workload, or orchestrator is linked across the control event.

### INT-AI-09 Agent Honeypot or Canary Engagement

Correlate:

1. Access to an agent-specific honeypot, honeytoken, honey tool, decoy memory object, synthetic credential, or false administrative path.
2. Tool or command selection based on the decoy content.
3. Follow-on action such as credential use, privilege escalation, lateral movement, or external communication.
4. Adaptation after a deceptive or failed result.

Suggested AIDAF evidence: E2-E3 depending on whether agent-specific traces or model interaction are available.

### INT-AI-10 Autonomous Resource Acquisition and Persistence

Correlate across identity, cloud, host, and billing telemetry:

1. Unauthorized credential harvesting, account creation, cloud-resource provisioning, cryptocurrency mining, or compute acquisition.
2. Deployment of a new runtime, workload, proxy, or persistence mechanism.
3. Continued agentic or offensive activity from the acquired resource.
4. Shutdown avoidance, provider switching, or replacement infrastructure after containment.

Suggested AIDAF evidence: E2-E3. Resource acquisition alone does not establish autonomous AI use.

### EXT-AI-04 Cross-Target Autonomous Campaign Sequence

Correlate across targets or organizations where data-sharing arrangements permit:

1. Common agent identity, provider account, service identity, trace cluster, infrastructure, or tool fingerprint.
2. Repeated reconnaissance-to-action cycles against multiple targets.
3. Short-interval adaptation after target-specific errors or defenses.
4. Common command structures, provider telemetry, or cloud-origin evidence.

Suggested AIDAF evidence: E2 from behavioral correlation; E3-E4 when provider or agent-origin telemetry links the campaign.

### INT-AI-11 Invalid or Mismatched Agent Identity

In environments that require verifiable agent identity or attestation, correlate:

1. Missing, invalid, expired, or revoked agent identifier.
2. Identifier reused by multiple inconsistent workloads or locations.
3. Claimed agent identity does not match workload identity, certificate, attestation, model gateway, or execution environment.
4. Sensitive tool use or critical-infrastructure interaction follows the mismatch.

Suggested AIDAF evidence: E2 as an agent-origin anomaly. Identity mismatch alone does not prove maliciousness.

## Required Correlation Fields

- Source and destination IP
- Host and device identifiers
- User, service, agent, and workload identity
- Process GUID, parent process GUID, and command line
- Session, trace, request, and tool-call identifiers
- URI, HTTP status, authentication result, and client fingerprint
- File path, hash, creation time, and signer
- Model endpoint, runtime, SDK, provider, and API-key identity
- Agent identifier or attestation, issuer, signature status, workload binding, and revocation status where applicable
- Sandbox or containment state, boundary events, policy decisions, approval status, stop events, and kill-switch events
- Cloud resource creation, billing or compute anomalies, and new workload identifiers

## Validation Requirements

- Test against conventional scanners, SOAR, RPA, CI/CD, administrative scripts, and approved AI tools.
- Preserve failed attempts and intermediate outputs.
- Require analyst review before assigning AI involvement.
- Report alternative explanations and telemetry gaps.
- Treat external behavioral patterns as lower-quality evidence than internal runtime, prompt, agent-state, tool-call, or provider-origin artifacts.
- Test containment and sandbox-escape scenarios only in isolated authorized environments.
- Do not treat missing agent identifiers as suspicious unless an identity or attestation requirement has been formally deployed.

## References

Bearman, T., Covino, C., Mittelsteadt, M., & O'Brien, J. (2026, July 27). *The OpenAI/Hugging Face incident: Challenges in controlling and containing cyber-capable AI systems*. Institute for AI Policy and Strategy. https://www.iaps.ai/research/the-openaihugging-face-incident-challenges-in-controlling-and-containing-cyber-capable-ai-systems

Kraprayoon, J., Ee, S., Rosen, B., Matthew, Y., Singh, A., Covino, C., & Gershovich, A. B. (2026, March 11). *Highly autonomous cyber-capable agents: Anticipating capabilities, tactics, and strategic implications*. Institute for AI Policy and Strategy. https://www.iaps.ai/research/highly-autonomous-cyber-capable-agents

Mittelsteadt, M., Kraprayoon, J., Staes-Polet, R., Galeev, O., Wehner, J., Covino, C., & Ee, S. (2026, May 19). *Detecting offensive cyber agents: A detection-in-depth approach*. Institute for AI Policy and Strategy. https://www.iaps.ai/research/detecting-offensive-cyber-agents
