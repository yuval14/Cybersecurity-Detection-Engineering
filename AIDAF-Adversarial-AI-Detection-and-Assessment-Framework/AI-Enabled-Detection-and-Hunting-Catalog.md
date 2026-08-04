# AI-Enabled Detection and Hunting Catalog

**Author:** Yuval Sinay  
**Version:** 1.0  
**Date:** 2026-08-05

## Purpose

This catalog translates publicly documented and emerging adversarial AI tradecraft into defensive detection-engineering and threat-hunting scenarios.

The scenarios generate investigative signals. No individual match proves that an attacker used AI. Analysts should correlate endpoint, network, identity, model, agent, cloud, development, and data telemetry and consider conventional automation as an alternative explanation.

## Scenario Catalog

| ID | Scenario | Detection or hunting logic | Priority telemetry | Likely benign explanations | Initial response |
|---|---|---|---|---|---|
| AIH-001 | Unexpected Process Calls an LLM | Detect an unapproved script interpreter, service, or binary communicating with a model API, model-compatible proxy, or local inference endpoint | EDR process tree, DNS, proxy, TLS metadata, AI gateway | Approved developer tools, security testing, embedded AI applications | Validate the business purpose, preserve the process tree, review child processes, and revoke exposed model credentials |
| AIH-002 | AI Response Followed by Code Execution | Correlate model activity with file creation, runtime compilation, shell execution, or a new child process on the same host and session | AI gateway, EDR, command line, file creation, compiler telemetry | Coding assistants, CI/CD, interactive development | Suspend the session or agent, quarantine generated artifacts, inspect memory and persistence, and rotate relevant credentials |
| AIH-003 | Runtime Compilation Outside Development | Detect Roslyn, `csc.exe`, `CSharpCodeProvider`, MSBuild, `javac`, or equivalent compilers outside approved build environments | EDR, application control, software inventory, CI/CD | Software installation, administrative automation, deployment systems | Collect source and output artifacts, identify the parent process, and block unauthorized compiler execution |
| AIH-004 | AI Agent or CLI Accesses Secrets | Detect coding agents, AI CLIs, or local inference tools reading credential files, wallets, cloud configuration, tokens, or secret stores | EDR file access, DLP, agent audit logs, secret-manager logs | Approved secret scanning, troubleshooting, authorized assessment | Stop the agent, revoke accessed secrets, preserve prompts and tool traces, and review follow-on activity |
| AIH-005 | AI Share Link Followed by Shell Execution | Correlate access to a publicly shared AI conversation with PowerShell, Terminal, `cmd.exe`, Run, or script execution | Secure web gateway, browser history, EDR, PowerShell and shell logs | Legitimate troubleshooting, administrator support, training | Block or terminate the command, isolate the endpoint where appropriate, collect browser artifacts, and identify the initial lure |
| AIH-006 | New-Domain BaaS Credential Collection | Detect a newly registered or low-reputation site sending credential-like data to Supabase, Firebase, or a similar Backend-as-a-Service platform | DNS, proxy, TLS, DLP, domain intelligence, browser telemetry | Internal prototypes, sanctioned SaaS, new startups | Block the source site and destination project, reset submitted credentials, and search for related lures |
| AIH-007 | Unusual AI Traffic from a Server | Detect production servers, domain controllers, appliances, or workloads without an approved AI use case contacting external model endpoints | Firewall, DNS, proxy, workload identity, cloud flow logs, EDR | Approved analytics, observability, embedded AI features | Restrict egress, identify the initiating process, revoke workload credentials, and investigate persistence and lateral movement |
| AIH-008 | Self-Modification After Model Interaction | Detect a process receiving model output and modifying its source, executable, startup entry, scheduled task, service, or persistence configuration | EDR, file integrity, registry, scheduled tasks, AI gateway | Self-updating applications, hot reload, authorized patching | Quarantine modified files, preserve pre-change and post-change versions, and inspect persistence and outbound communication |
| AIH-009 | Abnormal GPU or Local Inference Activity | Detect non-ML endpoints loading model weights, launching inference runtimes, opening local model ports, or showing sustained unexplained GPU use | EDR, GPU monitoring, model-file inventory, containers, local ports | Video processing, approved AI applications, data science | Validate the use case, isolate unauthorized runtimes, preserve model files and prompts, and inspect accessed data |
| AIH-010 | Multi-Provider AI Usage in a Short Sequence | Detect the same identity, device, workload, or gateway accessing several AI providers in an unusual sequence or volume | AI gateway, proxy, DNS, identity, SaaS and billing logs | Researchers comparing models, approved multi-model applications | Review the session intent and sensitive data access, restrict unapproved providers, and rotate exposed keys |
| AIH-011 | Service Account Performs Agentic Actions | Detect a service identity using shell, browser automation, Git, scanners, cloud management, or remote administration without an approved workflow | IAM, cloud audit, agent tool logs, EDR, Git and CI/CD | Deployment pipelines, SOAR, authorized automation | Disable the identity or specific permission, revoke tokens, preserve agent state and traces, and review the full session |
| AIH-012 | Excessive Dead or AI-Generated Decoy Code | Identify unusually high volumes of unreachable functions, repetitive administrative logic, verbose generated comments, or code unrelated to the primary behavior | Static analysis, call graph, sandbox, repository history | Boilerplate, legacy code, tests, intellectual-property obfuscation | Escalate for reverse engineering, identify reachable malicious logic, and compare generated variants and build history |
| AIH-013 | Lateral Movement After Agent Activity | Correlate agent or orchestrator activity with scanning, credential access, SSH, WinRM, RDP, SMB, remote administration, or cloud-role switching | EDR, NDR, authentication, PAM, agent tool calls, cloud audit | Penetration testing, administration, deployment orchestration | Suspend the agent and account, contain source systems, reset credentials, and scope all reached assets |
| AIH-014 | Frequent API-Key Rotation or Pooling | Detect many keys for one workload, repeated quota errors, rapid key changes, overlapping validity, multiple countries, or inconsistent user agents | API management, model-provider logs, secret manager, identity, billing, proxy | Planned rotation, multi-region services, disaster recovery | Revoke affected keys, identify the exposure source, rotate related secrets, and review downstream model activity |

## Correlation Patterns

### Pattern 1: Runtime AI-generated execution

```text
Unexpected Process
  -> Model API or Local Inference
  -> Script or Source File Creation
  -> Compiler or Interpreter
  -> Child Process or In-Memory Execution
```

### Pattern 2: AI-assisted secret discovery and exfiltration

```text
AI Client or Coding Agent
  -> Sensitive File or Secret Store Access
  -> Repository, Cloud, or Wallet Enumeration
  -> Archive, Network Transfer, or Public Repository Creation
```

### Pattern 3: AI-hosted ClickFix

```text
Email, Advertisement, or Message
  -> Shared AI Conversation
  -> Clipboard or Manual Command Transfer
  -> PowerShell, Terminal, Run, or Script Execution
  -> Download or Credential Theft
```

### Pattern 4: Agentic lateral movement

```text
Agent Session
  -> Discovery
  -> Credential Access
  -> Remote Protocol or Cloud Role Switch
  -> New Host or Workload
  -> Repeated Tool-Inference-Tool Loop
```

### Pattern 5: Model-hopping workflow

```text
Identity or Device
  -> Provider A
  -> Sensitive File or Code Access
  -> Provider B or Model-Compatible Proxy
  -> Local Runtime or Provider C
  -> Execution or External Transfer
```

## Evidence and Confidence

The following evidence should receive greater weight:

1. Recovered prompts, agent plans, task files, memory, or traces.
2. AI-service credentials, model SDKs, runtime processes, or model files.
3. Tool calls or generated artifacts directly linked to execution.
4. Provider-side records lawfully available to the investigation.
5. Repeated inference between attack stages.
6. Correlated process, network, identity, and model timelines.

The following are supporting indicators only:

- Fast execution
- Multilingual content
- High code quality
- Generated-looking comments
- Rapid payload variation
- Sophisticated target adaptation

## Tuning Principles

- Maintain approved AI-tool and model-provider inventories by user, asset, and workload role.
- Distinguish development systems from business endpoints and production servers.
- Require correlation rather than raising high-severity incidents from single generic events.
- Preserve failed commands, retries, and model-provider refusals where available.
- Test human-only, scripted, AI-assisted, and agentic scenarios as separate control groups.
- Record detection assumptions, blind spots, owner, last test, and review date.

## Workbook

The operational workbook is maintained in the `AI-Security-Tooling` repository:

`https://github.com/yuval14/AI-Security-Tooling/tree/main/AI_Enabled_Threat_Detection_and_Hunting_Workbook`

## Related AIDAF Resources

- [AIDAF Framework](README.md)
- [Covert AI-Enabled Offensive Cyber Operations](Covert-AI-Enabled-Offensive-Cyber-Operations.md)
- [Lessons Learned from AI-Enabled Cyber Attacks and Experiments](Lessons-Learned-from-AI-Enabled-Cyber-Attacks-and-Experiments.md)
- [External vs Internal Adversary Detection](External-vs-Internal-Adversary-Detection.md)
- [Sigma Detection Pack](sigma/README.md)

## Responsible Use

This catalog is intended for lawful, ethical, authorized, defensive, and protective use. Detection logic must be validated against local architecture, privacy obligations, operational safety, and approved AI use cases.