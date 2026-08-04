# AI-Enabled Detection and Hunting Catalog

**Author:** Yuval Sinay  
**Version:** 1.0  
**Date:** 2026-08-05

## Purpose

This catalog translates publicly reported AI-enabled offensive tradecraft into defensive detection-engineering and threat-hunting scenarios.

The scenarios are investigation signals. No individual match proves adversarial AI use. Each scenario should be correlated with AIDAF evidence levels, the complete attack timeline, conventional automation alternatives, and relevant provider or threat-intelligence reporting.

## Catalog

| ID | Detection or Hunt Scenario | Detection Logic | Primary Telemetry | Initial Response |
|---|---|---|---|---|
| AIH-001 | Unexpected Process Calls an LLM | Detect an unapproved script interpreter, service, or binary communicating with a model API, local model endpoint, or inference gateway. | EDR, DNS, proxy, TLS, AI gateway | Identify process ancestry, revoke exposed AI credentials, preserve generated artifacts, isolate if unauthorized. |
| AIH-002 | AI Response Followed by Code Execution | Correlate a model response with file creation, runtime compilation, shell execution, or a new child process in the same host and session. | AI gateway, EDR, file events, command lines | Suspend the agent or session, stop unauthorized execution, quarantine generated files, review downstream activity. |
| AIH-003 | Runtime Compilation Outside Development | Detect unusual Roslyn, `csc.exe`, `MSBuild`, `CSharpCodeProvider`, `javac`, or equivalent compiler use outside approved developer and build assets. | EDR, module loads, file creation, CI/CD | Collect source and output, validate the parent process, block unauthorized compiler execution. |
| AIH-004 | AI Agent or CLI Accesses Secrets | Detect coding agents, AI CLIs, or local inference tools reading credential files, wallets, tokens, cloud configuration, or secret stores. | File access, DLP, agent audit, secret manager | Suspend the tool, revoke accessed secrets, review prompts and tool calls, search for follow-on access. |
| AIH-005 | AI Share Link Followed by Shell Execution | Correlate access to a public AI conversation with Terminal, PowerShell, `cmd.exe`, Run dialog, or script execution. | Secure web gateway, browser, EDR, shell logs | Block or terminate the command, collect browser artifacts, reset exposed credentials, identify the lure. |
| AIH-006 | New-Domain BaaS Credential Collection | Detect a new or low-reputation site sending credential-like data to Supabase, Firebase, or a similar Backend-as-a-Service endpoint. | DNS, proxy, TLS, DLP, browser | Block the source and project endpoint, reset submitted credentials, search for related lures. |
| AIH-007 | Unusual AI Traffic from a Server | Detect production servers, domain controllers, appliances, or workloads without an approved AI use case communicating with model endpoints. | Firewall, proxy, DNS, flow logs, EDR | Restrict egress, identify the initiating process, revoke workload credentials, inspect for persistence. |
| AIH-008 | Self-Modification after Model Interaction | Detect a process receiving model output and modifying its source, executable, persistence configuration, startup entry, or scheduled task. | EDR, file integrity, registry, AI API | Quarantine modified files, preserve pre-change and post-change versions, inspect persistence and outbound traffic. |
| AIH-009 | Abnormal GPU or Local Inference Activity | Detect non-ML endpoints loading large model weights, starting inference runtimes, opening local model ports, or showing sustained unexpected GPU use. | Process, GPU, file inventory, containers | Validate the business purpose, isolate unauthorized runtimes, preserve model files and accessed data. |
| AIH-010 | Multi-Provider AI Usage in a Short Sequence | Detect the same user, host, gateway identity, or workload accessing several AI providers in an unusual sequence or volume. | AI gateway, proxy, identity, SaaS logs | Review the workflow and accessed data, restrict unapproved providers, rotate exposed keys. |
| AIH-011 | Service Account Performs Agentic Actions | Detect a service identity using shell, browser automation, Git, cloud administration, scanners, or remote-management tools outside an approved workflow. | IAM, cloud audit, agent tool logs, EDR, Git | Disable the identity or specific tool permission, revoke tokens, preserve agent traces and memory. |
| AIH-012 | Excessive Dead or AI-Generated Decoy Code | Identify files with unusually high volumes of unreachable functions, repetitive administrative logic, generated comments, or code unrelated to observed behavior. | Static analysis, call graph, sandbox, repository history | Escalate for reverse engineering, identify reachable malicious logic, compare variants and build history. |
| AIH-013 | Lateral Movement after Agent Activity | Correlate coding-agent or orchestrator activity with scanning, credential access, SSH, WinRM, RDP, SMB, remote administration, or cloud role switching. | EDR, NDR, authentication, cloud, agent logs | Suspend the agent and account, contain the source, reset credentials, scope all reached assets. |
| AIH-014 | Frequent API-Key Rotation or Pooling | Detect many AI API keys for one workload, repeated quota failures, rapid key changes, inconsistent geographies, or changing user agents. | API management, provider usage, secret manager, identity | Revoke keys, rotate related secrets, identify exposed repositories, review provider and downstream activity. |

## Priority Correlation Patterns

### Runtime AI-generated execution

```text
Suspicious process
  -> model API or local inference
  -> source or script created
  -> compiler or interpreter launched
  -> child process or in-memory execution
```

### AI tool used for credential access

```text
AI agent or CLI
  -> protected credential path or secret store
  -> Git, cloud, wallet, or package-registry action
  -> new external destination or repository
```

### Public AI share-link ClickFix

```text
Browser opens public AI conversation
  -> user launches shell
  -> encoded or download-and-execute command
  -> new file, persistence, or credential access
```

### Agentic lateral movement

```text
Agent session
  -> host or service discovery
  -> credential access
  -> remote protocol or role switch
  -> rapid result evaluation and tool change
```

## Required Telemetry Fields

Minimum useful fields include:

- Timestamp and correlation ID
- User, service account, and workload identity
- Host, container, and cloud resource
- Process name, parent, command line, signer, and process GUID
- Destination domain, URL, IP, TLS metadata, and bytes transferred
- Model provider, model name, API-key identifier, session, and tool call
- File path, hash, access type, source, and output
- Git repository, workflow, package, commit, and token identity
- Cloud action, target resource, role, and authorization result
- Agent memory, trace, policy decision, and approval record where available

## False-Positive Controls

Common legitimate activity includes:

- Approved coding assistants and developer workstations
- CI/CD and build systems
- Security testing and red-team exercises
- Enterprise AI applications and analytics workloads
- Local AI tools approved for research or accessibility
- Administrative automation and SOAR
- Legitimate Supabase, Firebase, GitHub, and cloud applications

Tuning should use asset roles, approved tools, signed binaries, expected parent-child relationships, authorized projects, workload identities, change records, and known test windows.

## Validation Guidance

1. Validate generic fields against the target SIEM schema.
2. Test human-only, scripted, AI-assisted, and agentic control groups.
3. Preserve failed attempts, retries, and intermediate generated artifacts.
4. Confirm that benign developer and administrative activity is represented in the test set.
5. Use cross-source SIEM correlation before raising the AIDAF evidence level.
6. Record the analytic owner, approval, test evidence, tuning decisions, and review date.

## Workbook

A companion Excel workbook is maintained in the [AI-Security-Tooling repository](https://github.com/yuval14/AI-Security-Tooling/tree/main/AI_Enabled_Threat_Detection_and_Hunting_Workbook).

## Related Resources

- [Covert AI-Enabled Offensive Cyber Operations](./Covert-AI-Enabled-Offensive-Cyber-Operations.md)
- [AIDAF Framework](./README.md)
- [AIDAF Sigma Detection Pack](./sigma/README.md)
- [Lessons Learned from AI-Enabled Cyber Attacks and Experiments](./Lessons-Learned-from-AI-Enabled-Cyber-Attacks-and-Experiments.md)