# PEAK Threat Hunting Framework

## Overview

The **PEAK Threat Hunting Framework** is a vendor-agnostic methodology developed by Splunk's SURGe security research team to structure proactive threat hunting as a repeatable, measurable, and knowledge-driven operational process.

**PEAK** stands for:

- **Prepare**
- **Execute**
- **Act with Knowledge**

The framework connects threat hunting with detection engineering, security operations, threat intelligence, and continuous defensive improvement.

## Core Lifecycle

| Phase | Objective | Typical Activities | Key Outputs |
|---|---|---|---|
| **Prepare** | Define and design the hunt | Establish scope, identify intelligence requirements, formulate hypotheses, verify telemetry, define success criteria | Hunt plan, data requirements, assumptions, ATT&CK mappings |
| **Execute** | Perform the hunt and evaluate evidence | Query and correlate telemetry, investigate anomalies, test hypotheses, refine analytical logic | Findings, evidence, investigative notes, validated or rejected hypotheses |
| **Act** | Operationalize the knowledge gained | Escalate confirmed threats, improve controls, create or tune detections, document lessons learned | Detection rules, playbooks, response actions, intelligence updates, coverage improvements |
| **Knowledge** | Preserve and reuse learning across the lifecycle | Capture analytical methods, failed approaches, environmental context, reusable queries, and adversary insights | Organizational knowledge base and improved future hunts |

## Hunt Types

### 1. Hypothesis-Driven Hunting

Begins with a testable statement derived from threat intelligence, observed adversary behavior, incident findings, or MITRE ATT&CK techniques.

**Example:**

> An adversary may be abusing OAuth application consent to establish persistent access.

The hunter identifies required telemetry, expected behaviors, alternative explanations, and evidence that would support or reject the hypothesis.

### 2. Baseline Hunting

Establishes normal behavior for users, systems, applications, or network activity and investigates meaningful deviations from that baseline.

Examples include:

- Unusual PowerShell execution patterns
- Authentication from rare locations or devices
- Abnormal service-account activity
- Unexpected parent-child process relationships
- Rare DNS, proxy, or cloud API behavior

### 3. Model-Assisted Threat Hunting

Uses statistical methods, machine learning, clustering, anomaly detection, or behavioral analytics to generate investigative leads. Models support analyst judgment rather than replace it.

Model-assisted hunts should document:

- Features and data sources
- Model assumptions and limitations
- Thresholds and confidence levels
- Expected false positives
- Validation procedures
- Human review requirements

## Integration with Detection Engineering

A PEAK hunt should produce reusable defensive value even when no active compromise is confirmed. Potential outputs include:

- Sigma rules
- SIEM correlation rules
- Splunk SPL, KQL, AQL, or other platform queries
- YARA or YARA-L rules
- Threat intelligence enrichment
- MITRE ATT&CK coverage updates
- SOC investigation playbooks
- Telemetry and logging improvements
- Detection tuning and false-positive reduction
- New threat-hunting hypotheses

## Recommended Hunt Record

Each hunt should document at least:

| Field | Description |
|---|---|
| Hunt title and identifier | Unique name and tracking reference |
| Hunt owner | Responsible analyst or team |
| Objective | Defensive question the hunt is intended to answer |
| Hypothesis or analytical premise | Testable statement or baseline assumption |
| Threat context | Relevant adversary, campaign, vulnerability, or intelligence |
| ATT&CK mapping | Applicable tactics, techniques, and sub-techniques |
| Data sources | Required logs, telemetry, enrichment, and retention period |
| Queries and analytical methods | Reproducible search and correlation logic |
| Assumptions and limitations | Known gaps, blind spots, and dependencies |
| Findings | Evidence, anomalies, and investigative conclusions |
| Confidence | Analytical confidence and supporting rationale |
| Actions | Escalation, containment, detection, tuning, or logging changes |
| Lessons learned | Knowledge to preserve for future hunts |

## Suggested Metrics

PEAK metrics should emphasize defensive outcomes rather than only the number of hunts completed.

- Hunts completed and validated
- Confirmed threats or previously unknown activity identified
- Detection rules created or improved
- Percentage of hunts producing reusable detection content
- ATT&CK technique and data-source coverage gained
- Telemetry gaps identified and remediated
- False-positive reduction after tuning
- Mean time from hunt finding to operational detection
- Knowledge artifacts created or reused
- Response or control improvements resulting from hunts

## Relationship to Other Frameworks

| Framework or Knowledge Base | Relationship to PEAK |
|---|---|
| **MITRE ATT&CK** | Identifies adversary behaviors and helps define what to hunt |
| **MITRE D3FEND** | Supports selection of defensive techniques and countermeasures |
| **Diamond Model** | Structures analysis of adversary, capability, infrastructure, and victim relationships |
| **TaHiTI** | Provides a threat-intelligence-led hunting methodology |
| **Sigma** | Enables portable expression of detection logic generated from hunts |
| **Detection Engineering** | Converts validated hunt findings into sustainable production detections |

A practical distinction is:

> **MITRE ATT&CK helps define what to hunt, while PEAK structures how to prepare, execute, operationalize, and learn from the hunt.**

## Example Workflow

1. **Prepare:** Select an ATT&CK technique, define a hypothesis, confirm telemetry, and establish success criteria.
2. **Execute:** Run queries, correlate endpoint, identity, network, and cloud evidence, and investigate anomalies.
3. **Act:** Escalate confirmed activity, create or tune detections, improve telemetry, and update response procedures.
4. **Preserve knowledge:** Store queries, assumptions, results, limitations, and lessons learned for future reuse.

## References

Bianco, D. (2023, April 18). *Introducing the PEAK threat hunting framework*. Splunk. https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html

Splunk SURGe. (2023). *The PEAK threat hunting framework*. Splunk. https://www.splunk.com/en_us/form/the-peak-threat-hunting-framework.html

Splunk SURGe. (2023). *Model-assisted threat hunting with the PEAK framework*. Splunk. https://www.splunk.com/en_us/blog/security/peak-framework-math-model-assisted-threat-hunting.html

## Defensive Use Notice

This material is intended for lawful, authorized, defensive security operations, threat hunting, detection engineering, incident response, education, and research. Organizations should ensure that data collection and investigative activities comply with applicable law, privacy requirements, internal policy, and authorization boundaries.
