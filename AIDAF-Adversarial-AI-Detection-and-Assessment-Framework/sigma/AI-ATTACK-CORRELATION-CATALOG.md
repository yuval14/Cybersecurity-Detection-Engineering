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

## Required Correlation Fields

- Source and destination IP
- Host and device identifiers
- User, service, and agent identity
- Process GUID, parent process GUID, and command line
- Session, trace, request, and tool-call identifiers
- URI, HTTP status, authentication result, and client fingerprint
- File path, hash, creation time, and signer
- Model endpoint, runtime, SDK, provider, and API-key identity

## Validation Requirements

- Test against conventional scanners, SOAR, RPA, CI/CD, administrative scripts, and approved AI tools.
- Preserve failed attempts and intermediate outputs.
- Require analyst review before assigning AI involvement.
- Report alternative explanations and telemetry gaps.
- Treat external behavioral patterns as lower-quality evidence than internal runtime, prompt, agent-state, or tool-call artifacts.
