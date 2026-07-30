# AIDAF Alignment with Artificial Intelligence Cyber Shield Frameworks

AIDAF should be used together with the framework catalog maintained in the `Artificial-Intelligence-Cyber-Shield` repository. The two repositories address different but complementary questions:

- **Artificial Intelligence Cyber Shield:** How should organizations secure, govern, threat-model, and test AI systems and agents?
- **AIDAF:** How should defenders detect and assess whether an adversary used AI during a cyber operation?

## Framework Mapping

| AIDAF need | Relevant framework family | How it supports AIDAF |
|---|---|---|
| Model adversary behavior against enterprise systems | MITRE ATT&CK | Maps conventional intrusion stages, techniques, and defensive telemetry |
| Model attacks against AI systems and workflows | MITRE ATLAS | Maps adversarial behavior involving models, data, prompts, agents, and AI infrastructure |
| Identify AI application weaknesses | OWASP Top 10 for LLM Applications | Supports hypotheses involving prompt injection, insecure output handling, sensitive-data disclosure, excessive agency, and unsafe plugins or tools |
| Test AI-enabled and agentic workflows | OWASP AI Red Teaming Guide and CSA Agentic AI Red Teaming Guide | Provides structured tests for model, application, tool, identity, autonomy, and multi-step risks |
| Structure AI risk decisions | NIST AI RMF and GenAI Profile | Supports governance, mapping, measurement, management, evidence, and risk acceptance |
| Secure AI architecture and operations | Google Secure AI Framework | Supports secure design, supply-chain controls, detection, response, and operational monitoring |
| Threat-model AI components and trust boundaries | AI STRIDE, STRIDE, PASTA, LINDDUN | Helps identify spoofing, tampering, repudiation, disclosure, denial-of-service, privilege, business-risk, and privacy scenarios |
| Control AI agents and tool use | OWASP agent guidance, CSA agentic guidance, Microsoft agent security guidance | Defines identity, tool, permission, memory, delegation, approval, and observability controls |
| Validate cyber capability and misuse | CyberSecEval, Inspect, PyRIT, garak, Giskard, Counterfit | Supports controlled experiments and purple-team validation of hypotheses and detections |

## Dual Mapping Requirement

AIDAF investigations should map activity to both conventional cyber behavior and AI-specific behavior where applicable:

1. **ATT&CK mapping:** What cyber technique occurred?
2. **AI-role mapping:** What role did AI appear to perform?
3. **ATLAS or AI-risk mapping:** Was the AI system, agent, model, prompt, data, or tool chain itself attacked or abused?
4. **Evidence mapping:** Which observations support the assessment and at what evidence level?

Example:

| Observation | ATT&CK | AIDAF role | AI framework relevance |
|---|---|---|---|
| External payloads change after each WAF rejection | Exploit Public-Facing Application | Advisor or Content Generator | Dynamic behavior; validate against scanner and red-team baselines |
| Compromised host calls an external model after domain discovery | System Network or Domain Discovery | Analyst or Advisor | Agent/tool observability, data disclosure, model gateway monitoring |
| AI-linked parent repeatedly invokes PowerShell and Nmap | Command and Scripting Interpreter; Network Service Discovery | Orchestrator | Excessive agency, tool misuse, hidden delegation, unsafe autonomy |
| Sensitive files are sent to a model service | Exfiltration Over C2 Channel or Web Service | Analyst or Tool Operator | Information disclosure, DLP, tool governance, agent identity controls |

## Agent Security Principles Relevant to Detection

The AI Cyber Shield repository emphasizes that an agent is more than a model: it is a model plus its surrounding harness, including context, tools, identities, execution environment, orchestration, tests, policy gates, and observability. AIDAF should therefore search for the complete operational chain rather than a model process alone.

High-value detection points include:

- Agent or workload identity
- Tool invocation and parameters
- Model endpoint and API key
- Prompt, response, and trace identifiers where legally and operationally available
- Context, retrieval, and memory access
- Parent-child process relationships
- Approval or policy-gate bypass
- Repeated delegation or tool loops
- Sandbox escape or execution outside the approved environment
- Sensitive-data access followed by model or tool activity

## Agent Rule of One as a Detection Prioritization Principle

The AI Cyber Shield's Agent Rule of One recommends that an agent possess only one high-risk capability at a time:

1. Access to sensitive data.
2. Ability to execute external actions.
3. Ability to modify memory, policy, identity, or configuration.

For AIDAF, observed combination of two or more of these capabilities on an unauthorized or compromised system should increase investigation priority. It does not independently prove malicious AI use, but it indicates a larger blast radius and stronger integration.

## AI STRIDE Mapping for Post-Compromise Detection

| AI STRIDE category | Relevant post-compromise signal |
|---|---|
| Spoofing | Unapproved agent identity, forged model endpoint, reused service token |
| Tampering | Modified prompts, tools, model configuration, memory, retrieval content, or agent policy |
| Repudiation | Missing prompt, tool, approval, or trace logs during suspicious activity |
| Information disclosure | Sensitive data submitted to a model, tool, retrieval store, or agent output |
| Denial of service | Recursive tool calls, token or compute exhaustion, uncontrolled agent loops |
| Elevation of privilege | Agent or model-triggered use of privileged tools, APIs, identities, or approval bypass |

## Validation Guidance

Use the red-team and evaluation resources from Artificial Intelligence Cyber Shield to validate AIDAF detections:

- Use OWASP AI Red Teaming and CSA Agentic AI Red Teaming scenarios for multi-step tool and identity abuse.
- Use MITRE ATLAS and ATT&CK to ensure both AI-specific and enterprise attack behavior are covered.
- Use PyRIT, garak, Giskard, Inspect, CyberSecEval, Counterfit, or controlled custom agents where suitable.
- Capture full telemetry during experiments: prompts, responses, tool calls, process trees, files, network connections, identities, approvals, errors, and retries.
- Test benign automation, authorized coding assistants, enterprise agents, scanners, and human red teams as negative controls.

## Source Repository

Relevant maintained pages in `yuval14/Artificial-Intelligence-Cyber-Shield` include:

- `Frameworks/AI-Agent-Security.md`
- `Frameworks/AI-Threat-Modeling-Frameworks.md`
- `Frameworks/AI-Red-Team-Frameworks.md`
- `Frameworks/AI-Security-Frameworks.md`
- `Frameworks/AI-Governance-and-Assurance.md`
- `AI-Maturity-Model/OWASP-AI-Maturity-Assessment.md`
