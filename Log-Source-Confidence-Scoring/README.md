# Confidence Scoring for Log Sources

![Confidence Scoring for Log Sources](./Confidence-Scoring-for-Log-Sources.png)

## Overview

This entry documents a practical, context-driven method for assessing how much confidence a log source provides for detection engineering. The model is based on six scoring dimensions: fidelity, coverage, context, noise, timeliness, and robustness. Each dimension is scored from 1 to 5, where higher scores indicate stronger confidence that the telemetry can distinguish malicious activity from legitimate use.

The example in the figure compares three sources for detecting PowerShell usage:

| Log source | Main detection value | Key limitation |
|---|---|---|
| Windows Event Log ID 4688 | Basic process creation visibility | Limited context and weaker resilience |
| Sysmon Event ID 1 | Better process creation telemetry | Requires deployment and tuning |
| EDR telemetry | Rich process tree, script content, network, and behavioral context | Cost, coverage, and operational dependency |

## Detection engineering use

Use this scoring method to:

1. Prioritize high-confidence log sources for detection logic.
2. Identify telemetry blind spots before writing or deploying rules.
3. Reduce false positives by favoring high-context and lower-noise data.
4. Justify investments in logging, EDR, enrichment, and tamper-resistant telemetry.
5. Separate the question “Do we have the log?” from “How much confidence does this log give us?”

## Scoring dimensions

| Dimension | Meaning | Engineering question |
|---|---|---|
| Fidelity | Accuracy and trustworthiness of the data | Can we trust that the data is correct? |
| Coverage | Breadth across assets, users, and techniques | Are there blind spots? |
| Context | Richness of surrounding event information | Do we know who, what, when, where, how, and why? |
| Noise | Benign activity that resembles malicious behavior | How many false positives should we expect? |
| Timeliness | Speed of availability to detection and response systems | Can we respond in time? |
| Robustness | Resistance to evasion, tampering, or deletion | Can an attacker disable or manipulate this logging? |

## APA 7 references

MITRE Center for Threat-Informed Defense. (2026, February 17). *Ambiguous techniques*. https://ctid.mitre.org/projects/ambiguous-techniques/

Cunningham, M., & Feffer, A. (2026, February 19). *Context to confidence: The next phase of ambiguous techniques research*. MITRE Center for Threat-Informed Defense. https://ctid.mitre.org/blog/2026/02/19/ambiguous-techniques-extension/
