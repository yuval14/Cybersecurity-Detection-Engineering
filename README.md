# Cybersecurity Detection Engineering

## About

A practical, threat-informed repository for designing, validating, governing, and maintaining cybersecurity detections across their full lifecycle. It brings together detection engineering frameworks, Sigma rules, correlation logic, threat-hunting methodologies, log-source confidence assessment, and AI-enabled approaches for identifying conventional, AI-assisted, agentic, and autonomous adversary activity.

The repository extends beyond individual detection rules and includes:

- Detection Engineering Frameworks
- Sigma Rules
- Correlation Logic
- Detection Lifecycle Methods
- Threat Hunting Methodologies
- AI-Enabled Detection Frameworks
- Autonomous Cyber Agent Detection
- Detection Governance and Validation

---

## 🧠 Detection Philosophy

- **Behavior over indicators**
- **Threat-informed**, aligned with adversary TTPs
- **Context-aware**, leveraging correlation and enrichment
- **Defender-centric**, written for operational security teams
- **Lifecycle-managed**, with documented reasoning, validation, ownership, tuning, and retirement criteria
- **Evidence-based AI assessment**, separating observed behavior from claims of AI or autonomous-agent involvement

Where applicable, detections include:

- MITRE ATT&CK mappings
- Tuning guidance
- Known evasion considerations
- Data-source and schema assumptions
- Test evidence and lifecycle records
- AI involvement evidence level and alternative explanations
- Agent identity, containment, policy, and tool-call telemetry where available

---

## 🧪 Rule Quality Standards

Each detection rule should document:

- Detection purpose
- Required data sources
- Detection logic
- Expected false positives and tuning notes
- Suggested severity and confidence level
- ATT&CK mapping and coverage assumptions
- Required fields and schema dependencies
- Test cases and validation evidence
- Rule owner, approver, and review date
- Known blind spots, evasion paths, and retirement criteria

---

## 📚 Detection Engineering Resources

- [Confidence Scoring for Log Sources](./Log-Source-Confidence-Scoring/README.md)
- [AIDAF - Adversarial AI Detection and Assessment Framework](./AIDAF-Adversarial-AI-Detection-and-Assessment-Framework/README.md)
- [Offensive Cyber Agent Detection-in-Depth](./AIDAF-Adversarial-AI-Detection-and-Assessment-Framework/Offensive-Cyber-Agent-Detection-in-Depth.md)
- [Covert AI-Enabled Offensive Cyber Operations](./AIDAF-Adversarial-AI-Detection-and-Assessment-Framework/Covert-AI-Enabled-Offensive-Cyber-Operations.md)
- [AI-Enabled Detection and Hunting Catalog](./AIDAF-Adversarial-AI-Detection-and-Assessment-Framework/AI-Enabled-Detection-and-Hunting-Catalog.md)
- [AI-Enabled Attack Case Studies](./AIDAF-Adversarial-AI-Detection-and-Assessment-Framework/case-studies/README.md)
- [AI Attack Correlation Catalog](./AIDAF-Adversarial-AI-Detection-and-Assessment-Framework/sigma/AI-ATTACK-CORRELATION-CATALOG.md)
- [Agentic Threat Hunting and Detection Engineering Frameworks - ATHF and ADEF](./Threat-Hunting-Frameworks/Agentic-Threat-Hunting-and-Detection-Engineering-Frameworks.md)
- [PEAK Threat Hunting Framework](./Threat-Hunting-Frameworks/PEAK-Threat-Hunting-Framework.md)
- [Sqrrl Threat Hunting Reference Model](./Threat-Hunting-Frameworks/Sqrrl-Threat-Hunting-Reference-Model.md)

AIDAF provides a dedicated assessment layer for determining whether and how an adversary may have used AI during a cyber operation. It evaluates AI operational roles, attack-stage involvement, observable evidence, operational contribution, confidence, and alternative explanations.

The offensive cyber agent detection-in-depth page translates 2026 IAPS research into an ecosystem-oriented detection model covering agent identifiers, agent honeypots, AI-assisted alert triage, standardized agentic alerts, cross-provider information exchange, containment events, and autonomous-agent threat assessment.

The covert AI-enabled operations research page examines threat evolution, concealment patterns, runtime model integration, agentic orchestration, trusted-infrastructure abuse, and defender evidence requirements.

The AI-enabled detection and hunting catalog now contains twenty operational scenarios, including sandbox escape, continued autonomous action after stop or revocation, agent deception engagement, autonomous resource acquisition, cross-target campaign sequencing, and agent-identity anomalies.

The ATHF and ADEF page presents a complementary workflow from a structured threat hunt to a governed production detection. ATHF uses the LOCK lifecycle for threat hunting, while ADEF uses the FORGE lifecycle to preserve detection reasoning, validation, governance, tuning, and review history.

---

## 🤖 Agentic Detection Engineering Principles

AI agents may assist with hypothesis development, repository cataloging, schema checking, rule drafting, test generation, ATT&CK mapping, overlap analysis, and lifecycle recommendations. They should not independently deploy, disable, suppress, or retire production detections.

Required safeguards include:

- Read-only access by default
- Human approval for material rule changes
- Prompt-injection defenses for external intelligence and untrusted content
- Complete provenance for prompts, tool calls, generated logic, evidence, and approvals
- Semantic, schema, and test validation beyond syntax checking
- Separation of duties and peer review
- Branch protection, signed changes, rollback, and kill-switch procedures
- Infrastructure-enforced egress and target boundaries for offensive-cyber model evaluations
- Independent monitoring of stop, deny, revocation, containment, and sandbox-boundary events

---

## ⚙️ Supported Platforms & Formats

- **YARA / YARA-L / YARA-L 2.0**
- **Sigma** (generic, SIEM-agnostic format)
- **Splunk SPL**
- **Elastic (KQL)**
- **Microsoft Sentinel (KQL)**
- **IBM QRadar (AQL)**
- Network telemetry (DNS, proxy, NetFlow)
- AI gateway, agent trace, sandbox, deception, policy-engine, and workload-identity telemetry

Sigma rules may be used as the **authoritative source**, with platform-specific rules derived from them where applicable.

---

## 🔗 Repository Location

Canonical repository:

`https://github.com/yuval14/Cybersecurity-Detection-Engineering`

For existing local clones, update the remote with:

```bash
git remote set-url origin https://github.com/yuval14/Cybersecurity-Detection-Engineering.git
git remote -v
```

---

## 📄 License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

By using, modifying, or distributing this software, you agree to the following key terms:

- You may use the software for any purpose, including commercial use
- If you distribute modified versions, you **must** make the source code available
- Derivative works **must** be licensed under GPL-3.0
- You may not impose additional legal or technical restrictions

See the `LICENSE` file for the full license text.

---

## 🚨 Disclaimer

This repository is provided **as-is** for defensive and research purposes.

- Rules may require environment-specific tuning
- Not all detections are production-ready by default
- AI-generated or AI-assisted rules require independent validation
- ATT&CK mappings do not by themselves demonstrate effective coverage
- Agentic or autonomous behavior does not by itself prove that AI caused the activity
- No warranty is provided, express or implied

---

## 🤝 Contributions

Contributions are welcome.

By submitting a contribution, you agree that your work will be licensed under **GPL-3.0**, consistent with this project.

Please:

- Follow the repository structure
- Document assumptions and limitations
- Avoid environment-specific hardcoding
- Include MITRE ATT&CK mappings where relevant
- Include test evidence and required data fields where feasible
- Preserve the rationale for material tuning and lifecycle changes

---

## 🎯 Intended Audience

- SOC Analysts
- Detection Engineers
- Threat Hunters
- DFIR Practitioners
- Security Researchers
- AI Security and Agent Security Teams

---

**Open, shareable detection logic strengthens collective defense - GPL ensures it stays that way.**
