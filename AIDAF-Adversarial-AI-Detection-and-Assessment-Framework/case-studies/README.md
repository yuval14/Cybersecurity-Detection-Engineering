# Covert AI-Enabled Attack Case Studies

## Purpose

These case studies translate provider-reported and publicly documented activity into defender-oriented lessons, evidence requirements, detection opportunities, and analytic cautions.

The cases should not be treated as proof that every similar behavior is AI-enabled. Each assessment requires AIDAF evidence scoring and consideration of conventional scripts, automation, developer activity, and human operation.

## Case 1: HONESTCUE and Just-in-Time C# Execution

### Reported behavior

Google Threat Intelligence Group described HONESTCUE as a downloader or launcher that obtained C# source through a model interaction, compiled the returned source using legitimate .NET capabilities, and could execute the resulting assembly without requiring a complete fixed payload in the original sample.

### Defensive significance

- Operational functionality may be absent from the initial file.
- The model request can appear similar to legitimate programming assistance.
- Runtime compilation uses trusted operating-system or framework components.
- In-memory execution can reduce disk evidence.
- Payload changes may not require a new loader hash.

### Detection opportunities

- Unexpected process connects to a model endpoint.
- Source code is written to a temporary or user-writable directory.
- `csc.exe`, Roslyn, `MSBuild`, or `CSharpCodeProvider` is launched by a non-development parent.
- A newly compiled assembly is loaded or executed immediately.
- The chain occurs on a server or endpoint without an approved development use case.

### AIDAF evidence

Runtime model interaction, source artifacts, compiler execution, and generated assembly evidence can support E3 or E4 when correlated. Runtime compilation alone is not proof of AI use.

## Case 2: COINBAIT and Trusted BaaS Infrastructure

### Reported behavior

Google Threat Intelligence Group reported a cryptocurrency-themed phishing kit associated with AI-assisted web development and legitimate infrastructure, including modern front-end components, Cloudflare, and Backend-as-a-Service capabilities.

### Defensive significance

- AI-assisted development reduces the skill required to produce polished phishing sites.
- Trusted hosting, CDN, and BaaS services complicate domain-based blocking.
- The malicious front end and credential-collection backend may use different trust reputations.
- A global block on common development platforms is generally impractical.

### Detection opportunities

- New or low-reputation domain impersonates a known brand.
- The page submits credential-like fields to a newly observed Supabase, Firebase, or similar project.
- Referrer, brand, user population, and backend project do not match an approved business application.
- A phishing email, advertisement, social message, or compromised account precedes the visit.

### AIDAF evidence

BaaS use and polished content are weak indicators of AI involvement. Provider-side development records, recovered prompts, or project artifacts are required for stronger attribution.

## Case 3: Public AI Share-Link ClickFix

### Reported behavior

Threat reporting described campaigns in which attackers used publicly shared AI conversations to host troubleshooting-style instructions. Victims were directed to open a terminal or system dialog and execute a command that installed malware or performed another unauthorized action.

### Defensive significance

- The landing page is hosted on a trusted AI-provider domain.
- The victim executes the command, reducing reliance on an exploit.
- Traditional URL-reputation controls may trust the hosting domain.
- The command can use built-in operating-system tools.

### Detection opportunities

- Browser opens a public AI conversation or share-link path.
- Terminal, PowerShell, `cmd.exe`, or Run dialog starts shortly afterward.
- The command is encoded, downloads remote content, invokes a scripting interpreter, or creates persistence.
- Similar sequences occur across several users after the same message or advertisement.

### AIDAF evidence

The share-link and browser-to-shell sequence establish AI infrastructure involvement, but not necessarily AI generation of the malicious command. The assessment should distinguish hosting from generation and orchestration.

## Case 4: Runtime AI Malware Families

### PROMPTSTEAL

Reported to use a model through Hugging Face to generate commands for system and document collection. This is a strong example of model functionality embedded in an operational malware workflow.

### PROMPTFLUX

Reported as an experimental VBScript family designed to request code rewriting or obfuscation and to create changing variants. The defensive lesson is to prioritize behavior and self-modification over static strings.

### QUIETVAULT

Reported to steal developer or package-registry credentials and then invoke locally available AI tools to identify additional secrets. This demonstrates living off the AI-enabled development environment.

### PROMPTLOCK

Reported as a research or prototype ransomware capability using a local model to generate Lua scripts. The case demonstrates feasibility but should not be represented as evidence of widespread operational deployment.

### Cross-case detection lessons

- Runtime model calls are stronger evidence than generated-code style.
- Model use may be cloud-based, proxied, or local.
- Generated code may be interpreted, compiled, or executed in memory.
- AI tools already installed on the host may become post-compromise utilities.
- Repeated model-to-tool loops are more probative than a single request.

## Case 5: AI-Orchestrated Multi-Stage Intrusion

### Reported behavior

Anthropic reported disrupting campaigns in which coding-agent or agentic capabilities supported scanning, exploitation, credential use, data discovery, analysis, and other tactical actions. Strategic targeting and control may remain human, while tactical execution is highly automated.

### Detection opportunities

- Repeated, short-interval tool invocations linked to one agent session.
- Machine-speed transition from scan results to exploit or credential actions.
- Structured handoffs between model output and tool input.
- Tool switching immediately after authentication, parser, or access errors.
- Common trace, service identity, container, process group, or working directory across several ATT&CK stages.

### AIDAF evidence

Assess autonomy by attack stage. Reconnaissance may be agentic while target selection, deployment, and escalation remain human-controlled.

## Analytic Guardrails

1. Record the source's exact claim and confidence.
2. Separate confirmed runtime integration from behavioral similarity.
3. Preserve failed actions and correction loops.
4. Test human and conventional automation alternatives.
5. Avoid attributing AI use based only on quality, speed, or code comments.
6. Treat individual Sigma matches as investigative evidence, not final attribution.

## Related Resources

- [Covert AI-Enabled Offensive Cyber Operations](../Covert-AI-Enabled-Offensive-Cyber-Operations.md)
- [AI-Enabled Detection and Hunting Catalog](../AI-Enabled-Detection-and-Hunting-Catalog.md)
- [AIDAF Framework](../README.md)
- [AIDAF Sigma Detection Pack](../sigma/README.md)