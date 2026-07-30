# AIDAF Correlation Use Cases

Portable Sigma rules identify individual events. AIDAF assessments require temporal and cross-source correlation in the target SIEM.

## 1. Runtime Model Call Followed by Generated Script Execution

Correlate, on the same host and user within 10 minutes:

1. A scripting or unusual process connects to an AI model endpoint.
2. A new script is written to a temporary or user-writable directory.
3. A shell or interpreter executes the script.
4. The script performs discovery, credential access, persistence, or network communication.

Suggested AIDAF interpretation:

- One signal: E1
- Model connection plus generated script execution: E2
- Direct runtime, agent state, prompt, or provider evidence: E3–E4

## 2. Agentic Tool-Use Loop

Correlate repeated sequences from a common process tree, user, container, API token, or execution context:

`reconnaissance -> result collection -> model or agent activity -> command execution -> validation`

Raise priority when:

- The sequence repeats three or more times in 30 minutes.
- Commands change after errors or blocked actions.
- Several ATT&CK tactics are traversed with very short decision intervals.
- Structured JSON or tool-call objects are present in logs.

## 3. Local Model Runtime Used During Intrusion Activity

Correlate:

- Local model runtime start from a suspicious parent.
- New listening port or localhost model API.
- Script interpreter or administration tool connecting to that port.
- Subsequent payload generation or command execution.

## 4. External Adaptive Exploitation

For web, WAF, API gateway, and application telemetry, group by source infrastructure and target endpoint. Detect:

- Multiple payload families after distinct server errors.
- Encoding, syntax, or exploit-strategy changes after blocks.
- Rapid transition from enumeration to exploitation.
- Target-specific payload values derived from previous responses.

This is behavioral evidence only and should generally remain E1–E2 without external corroboration.

## 5. AI-Assisted Identity and Access Operation

Correlate identity and endpoint signals:

- Geographic, device, language, or working-hour inconsistencies.
- New remote-management or input-emulation software.
- Unusual source-control, collaboration, and cloud activity.
- Access to AI writing, translation, image, or coding services.
- Multiple identities sharing infrastructure, devices, or behavioral patterns.

## Suppression and Tuning

Maintain allowlists for:

- Approved AI applications and model gateways
- Data-science and development workstations
- Authorized agent frameworks and MCP servers
- Security testing and purple-team systems
- Software deployment and notebook environments

Suppress only after validating the full process tree, user, destination, asset role, and change record. Avoid global exclusions based solely on a binary name or AI service domain.