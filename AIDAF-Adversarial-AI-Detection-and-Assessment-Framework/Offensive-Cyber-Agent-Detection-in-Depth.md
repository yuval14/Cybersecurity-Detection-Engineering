# Offensive Cyber Agent Detection-in-Depth

## Purpose

This page translates the Institute for AI Policy and Strategy (IAPS) report *Detecting Offensive Cyber Agents: A Detection-in-Depth Approach* into detection-engineering requirements for SOC, threat-hunting, DFIR, critical-infrastructure, cloud, and AI-security teams.

The central problem is that autonomous cyber agents can combine scale, adaptation, multi-step reasoning, tool use, and rapid retry cycles. Traditional detections remain necessary, but individual endpoint, network, identity, or signature controls may not reliably distinguish a human operator, conventional automation, and an autonomous cyber agent.

Detection-in-depth therefore combines multiple complementary mechanisms and correlates them with ordinary security telemetry.

## Strategic model

```text
Agent-origin evidence
        +
Enterprise behavioral telemetry
        +
Deception and canary telemetry
        +
AI-assisted alert analysis
        +
Cross-provider threat reporting
        ↓
Correlated agentic threat assessment
        ↓
Containment, attribution support, and ecosystem disruption
```

No single layer should be treated as sufficient proof that an attack is agentic.

## Five IAPS detection mechanisms

| Mechanism | Detection-engineering interpretation | Required evidence |
| --- | --- | --- |
| Agent Identifiers for Critical Infrastructure | Use persistent, cryptographically verifiable agent credentials or attestations where an ecosystem supports them | Agent identifier, signer or issuer, workload identity, target, timestamp, revocation status |
| Agent Honeypots | Deploy decoys designed to attract autonomous reconnaissance and tool use | Decoy access, honeytoken use, command sequence, tool interaction, network path, session correlation |
| AI-Automated Alert Analysis and Triage | Use AI to prioritize and interpret high-volume agentic signals while retaining deterministic controls and analyst review | Source alerts, model output, rationale, confidence, policy decision, analyst disposition |
| Agentic Security Alert Standard | Normalize reporting of suspected agentic activity across organizations and providers | Actor or agent identifier, evidence level, affected assets, observed actions, timestamps, confidence, recommended response |
| Agentic Cybersecurity Exchange (ACE) | Share provider and cloud-origin evidence to detect and disrupt malicious agent activity closer to its origin | Provider abuse evidence, cloud identity, model or agent session, infrastructure indicators, revocation and disruption status |

## Enterprise detection requirements

### 1. Preserve agent identity and provenance

Where agent identifiers or workload attestations exist, log them as first-class security entities. Correlate them with:

- Initiating user or service identity
- Model and provider
- Agent framework and version
- Tool calls and parameters
- Host, container, browser, or cloud workload
- Authorization and policy decisions
- Downstream system actions
- Network destinations and external effects

A missing or invalid agent identifier is not proof of malicious activity, but it becomes a high-value anomaly when the environment requires signed or registered agent identities.

### 2. Detect action sequences, not only isolated events

High-value patterns include:

```text
Reconnaissance -> tool selection -> action -> error/result collection -> model/agent activity -> modified action
```

```text
Stop or deny decision -> alternate tool/identity/path -> continued execution
```

```text
Agent session -> decoy resource -> honeytoken or honey tool -> follow-on action
```

```text
Agent or workload -> unexpected boundary crossing -> new Internet egress -> external target interaction
```

### 3. Treat containment state as telemetry

Security analytics should ingest:

- Sandbox boundary events
- Namespace or container escapes
- Policy denies
- Egress blocks
- Credential revocations
- Kill-switch invocation
- Approval denials
- Agent termination requests
- Retry and alternate-path behavior after control activation

Continued action after a stop, deny, revocation, or isolation event is especially important because it can indicate policy bypass, orchestration outside the expected control plane, or loss of containment.

### 4. Use deception specifically for autonomous behavior

Agent honeypots should capture more than access. Preserve the trajectory:

1. How the decoy was discovered.
2. Which artifacts were read.
3. Whether the agent interpreted synthetic instructions or metadata.
4. Which tool it selected.
5. Whether it attempted privilege escalation, credential use, lateral movement, or external communication.
6. Whether the agent adapted after a failed or deceptive result.

### 5. Correlate provider-side and victim-side evidence

Victim telemetry may reveal the intrusion but not the model or agent responsible. Provider-side evidence can materially improve confidence when it links:

- Model session
- Agent runtime
- API key or account
- Tool calls
- Cloud workload
- Abuse detection event
- External target

AIDAF should record this as stronger evidence than behavioral similarity alone.

## New hunting scenarios

The companion detection catalog includes the following IAPS-informed scenarios:

- **AIH-015** - Agent sandbox escape followed by unauthorized egress
- **AIH-016** - Continued autonomous action after stop or revocation
- **AIH-017** - Agent honeypot or canary engagement
- **AIH-018** - Autonomous resource acquisition or persistence
- **AIH-019** - Cross-target autonomous campaign sequence
- **AIH-020** - Agent identity missing, invalid, revoked, or mismatched

## Relationship to HACCAs

IAPS uses the term **Highly Autonomous Cyber-Capable Agent (HACCA)** for a future or emerging class of AI system capable of conducting multi-stage cyber campaigns at a level comparable to advanced criminal or state-affiliated operators with little meaningful human direction.

HACCA is a capability and threat-class concept. It should not replace AIDAF's AI involvement levels. In particular:

- **AIDAF AI-5** describes the assessed autonomy of observed AI involvement in a specific incident.
- **HACCA** describes the broader operational capability of an agent to plan, execute, sustain, evade, and adapt across a campaign.

A system could show AI-5 behavior in a constrained task without meeting the broader HACCA concept.

## Validation and analytic caution

- Do not infer autonomous-agent use only from speed, scale, variation, or sophisticated exploitation.
- Compare against scanners, worms, botnets, SOAR, RPA, CI/CD, red-team tooling, and skilled human operators.
- Require multiple independent evidence types before escalating confidence.
- Keep AI involvement separate from the underlying MITRE ATT&CK technique.
- Treat provider or ecosystem claims as source evidence that must be preserved with provenance.
- Use human review for high-confidence attribution or public reporting.

## References

Kraprayoon, J., Ee, S., Rosen, B., Matthew, Y., Singh, A., Covino, C., & Gershovich, A. B. (2026, March 11). *Highly autonomous cyber-capable agents: Anticipating capabilities, tactics, and strategic implications*. Institute for AI Policy and Strategy. https://www.iaps.ai/research/highly-autonomous-cyber-capable-agents

Mittelsteadt, M., Kraprayoon, J., Staes-Polet, R., Galeev, O., Wehner, J., Covino, C., & Ee, S. (2026, May 19). *Detecting offensive cyber agents: A detection-in-depth approach*. Institute for AI Policy and Strategy. https://www.iaps.ai/research/detecting-offensive-cyber-agents
