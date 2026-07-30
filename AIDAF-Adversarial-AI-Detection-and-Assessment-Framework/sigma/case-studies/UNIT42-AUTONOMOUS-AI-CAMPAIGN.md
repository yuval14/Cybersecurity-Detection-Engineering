# Unit 42 Autonomous AI Campaign — Detection Mapping

Source: Palo Alto Networks Unit 42, "Chinese-Speaking Threat Actor Harnesses AI Models for Autonomous Cyberattacks" (2026).

## Observed workflow

The campaign used Hermes Agent with DeepSeek as a reasoning engine and combined:

- FOFA-based internet asset enumeration
- autonomous CVE research and target reprioritization
- GitHub PoC discovery and download
- multi-target scanning with Python exploit tooling
- direct access to AI model APIs and access through a third-party proxy
- Telegram-based orchestration
- permissive AI tool configurations and reduced local logging
- local agent skills and MCP-based reconnaissance tooling
- an accidentally exposed Python HTTP file server started from the actor home directory

## Detection implications

Victim-side detections should focus on observable exploitation, adaptive probing and repeated target-specific variation. Operator-infrastructure and compromised-host detections can additionally detect model endpoints, agent runtimes, exploit acquisition, unsafe permission settings, agent traces and orchestration artifacts.

## Rules added from this case

- AI coding or agent tools connecting through suspicious model proxy infrastructure
- Hermes or agent framework connecting to model APIs and immediately spawning offensive tools
- FOFA or cyberspace-search activity followed by Python scanner execution
- GitHub PoC clone followed by exploit execution
- Python HTTP server launched from a user home directory
- permissive AI coding-agent execution settings
- Telegram-controlled agent process with shell or network-tool children
- autonomous scan-download-exploit sequence correlation

## Interpretation

These rules are investigative signals. A direct model endpoint or agent framework artifact can support stronger AIDAF evidence than behavioral variation alone, but each alert still requires validation against legitimate AI development, security research, red-team and vulnerability-management activity.
