# Sqrrl Threat Hunting Reference Model

## Overview

The **Sqrrl Threat Hunting Reference Model** is an influential threat hunting methodology introduced in 2015 and formalized in a 2016 white paper. It provides a structured approach for developing a proactive threat hunting capability through three connected components:

1. The **Hunting Maturity Model**
2. The **Hunting Loop**
3. The **Hunt Matrix**

The framework assumes that determined adversaries may already be present in the environment and that analysts should proactively search for behaviors, relationships, and patterns that automated controls have not yet detected.

---

## 1. Hunting Maturity Model

The Hunting Maturity Model assesses how effectively an organization can collect data, conduct hunts, use analytics, and operationalize findings.

| Level | Name | Characteristics |
|---|---|---|
| **HM0** | Initial | Primarily relies on automated alerting and performs little or no proactive hunting. |
| **HM1** | Minimal | Collects threat intelligence indicators and conducts basic indicator searches. |
| **HM2** | Procedural | Uses repeatable hunting procedures developed by others and maintains sufficient telemetry for structured investigations. |
| **HM3** | Innovative | Creates new hypotheses and hunting procedures based on adversary behaviors, organizational context, and prior findings. |
| **HM4** | Leading | Automates successful hunting procedures and continuously improves analytics while analysts focus on new and complex hypotheses. |

Maturity should not be measured only by the number of tools deployed. It also depends on telemetry quality, analyst expertise, repeatability, hypothesis development, and the ability to convert discoveries into automated analytics.

---

## 2. The Hunting Loop

The Sqrrl model defines threat hunting as an iterative four-stage loop.

### Stage 1: Create a Hypothesis

Develop a testable statement about possible adversary activity based on:

- Threat intelligence
- MITRE ATT&CK techniques
- Known organizational risks
- Recent incidents
- Vulnerability exposure
- Analyst intuition and experience
- Changes in the environment

A useful hypothesis should identify the suspected behavior, relevant assets or identities, expected evidence, required telemetry, and conditions that would support or reject the hypothesis.

### Stage 2: Investigate Using Tools and Techniques

Query and correlate relevant telemetry to test the hypothesis. Investigation may include:

- SIEM searches
- EDR investigation
- Identity and authentication analysis
- Network traffic analysis
- DNS and proxy analysis
- Cloud audit-log review
- Graph or linked-data analysis
- Timeline reconstruction
- Entity and relationship analysis

The objective is not only to find known indicators, but also to reveal relationships and behavioral evidence that automated detections may have missed.

### Stage 3: Uncover New Patterns and TTPs

Analyze the evidence to identify previously unknown or insufficiently understood:

- Adversary techniques, tactics, and procedures
- Behavioral patterns
- Infrastructure relationships
- Identity or privilege misuse
- Lateral movement paths
- Persistence mechanisms
- Data access or exfiltration patterns

Findings may confirm the hypothesis, reject it, or generate new hypotheses for subsequent hunting cycles.

### Stage 4: Inform and Enrich Automated Analytics

Convert validated findings into reusable defensive capabilities, such as:

- SIEM correlation rules
- Sigma rules
- EDR detections
- YARA or YARA-L rules
- Behavioral analytics
- Watchlists and enrichment logic
- Threat intelligence updates
- SOC playbooks
- Incident-response procedures
- Telemetry and logging improvements

This stage closes the loop by transforming analyst knowledge into scalable detection capability.

---

## 3. The Hunt Matrix

The Hunt Matrix combines the Hunting Maturity Model with the four stages of the Hunting Loop. It illustrates how organizations at different maturity levels perform each hunting activity.

For example:

- An **HM1** organization may create hypotheses primarily from indicators of compromise and conduct manual searches.
- An **HM2** organization may use documented hunt procedures and repeatable queries.
- An **HM3** organization may create original behavior-based hypotheses and develop new analytics.
- An **HM4** organization may automate proven hunt procedures while analysts investigate emerging and complex behaviors.

The matrix can support gap assessment, capability planning, staffing decisions, telemetry investment, and threat hunting program roadmaps.

---

## Recommended Hunt Record

Each hunt should document:

| Field | Description |
|---|---|
| Hunt ID | Unique identifier |
| Hypothesis | Testable statement describing suspected activity |
| Rationale | Threat intelligence, risk, incident, or observation that initiated the hunt |
| Scope | Systems, identities, networks, cloud services, and time period |
| ATT&CK mapping | Relevant tactics, techniques, and sub-techniques |
| Required telemetry | Logs and data sources needed to test the hypothesis |
| Queries and methods | Search logic, tools, pivots, and analytical techniques |
| Evidence | Supporting and contradictory observations |
| Assessment | Confirmed, partially supported, rejected, or inconclusive |
| New patterns or TTPs | Newly identified adversary behaviors or relationships |
| Defensive outputs | Rules, playbooks, enrichment, logging changes, or response actions |
| Follow-up hypotheses | Questions generated during the investigation |
| Limitations | Visibility gaps, data-quality issues, and analytical uncertainty |

---

## Integration with Detection Engineering

Sqrrl establishes a direct feedback loop between human-led investigation and automated detection:

```text
Threat Intelligence / Risk / Observation
                  ↓
          Create Hypothesis
                  ↓
       Investigate Telemetry
                  ↓
     Identify Patterns and TTPs
                  ↓
  Build or Improve Automated Analytics
                  ↓
       Generate New Hypotheses
```

A hunt is therefore valuable even when no active compromise is found, provided it improves organizational knowledge, telemetry, detection coverage, or investigative procedures.

---

## Suggested Metrics

Organizations may measure:

- Percentage of hunts based on behavior rather than static indicators
- Number of hypotheses tested
- Percentage of hunts producing reusable detections
- Number of new patterns or TTPs identified
- ATT&CK coverage improved through hunting
- Telemetry gaps discovered and resolved
- Detection rules created or tuned
- False-positive reduction resulting from hunt findings
- Time required to complete a hunt
- Hunting maturity progression from HM0 through HM4

Metrics should emphasize defensive improvement and learning rather than only counting discovered incidents.

---

## Relationship to PEAK

The Sqrrl model established the influential hypothesis-driven Hunting Loop and Hunting Maturity Model. The later **PEAK Threat Hunting Framework** expands the operational lifecycle by emphasizing preparation, execution, action, knowledge management, multiple hunt types, and measurable outcomes.

Sqrrl remains useful for:

- Building an initial threat hunting program
- Assessing organizational hunting maturity
- Structuring hypothesis-driven hunts
- Connecting analyst discoveries to detection engineering
- Establishing an iterative human-to-automation feedback loop

---

## Defensive and Ethical Use

This material is intended for authorized defensive security, threat hunting, detection engineering, incident response, research, and educational use. Hunting activities should comply with applicable law, organizational policy, privacy requirements, and approved rules of engagement.

---

## References

Sqrrl Data, Inc. (2016). *A framework for cyber threat hunting*.

Splunk. (2024). *What is threat hunting?* https://www.splunk.com/en_us/blog/learn/threat-hunting.html
