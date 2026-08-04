# AI Share-Link ClickFix

## Summary

Public reporting described campaigns that used publicly shared AI conversations as trusted landing pages for ClickFix-style instructions. The victim was directed to copy or manually execute a command presented as a troubleshooting or verification step.

The method launders malicious instructions through a reputable AI-service domain and shifts execution to the user, reducing dependence on a conventional phishing site or direct malware download.

## AIDAF Assessment

| Field | Assessment |
|---|---|
| AI role | Content Generator and trusted instruction host |
| Likely involvement level | AI-1 to AI-2 |
| Strong evidence | Shared-conversation URL, browser history, lure, copied command, shell execution, downloaded payload |
| Alternative explanation | Conventional technical-support scam hosted on another trusted platform |
| Public evidence status | Operationally observed in public threat-intelligence reporting |

## Detection Opportunities

1. Access to a public AI share URL from email, advertisement, social media, or messaging application.
2. PowerShell, Terminal, `cmd.exe`, Run dialog, AppleScript, or shell execution within a short period.
3. Encoded commands, download-and-execute patterns, or commands referencing temporary directories.
4. Browser-to-shell process relationships where telemetry is available.
5. Subsequent credential theft, persistence, or infostealer behavior.

## Suggested Correlation

```text
Lure
  -> Public AI Conversation
  -> Clipboard or Manual Command Transfer
  -> Shell or Script Interpreter
  -> Download, Execution, or Credential Access
```

## False Positives

- Legitimate troubleshooting
- Administrator support
- Training and security-awareness exercises
- Developer documentation using shared AI conversations

## Response Guidance

- Isolate the endpoint where malicious execution is suspected.
- Preserve the shared URL, conversation contents, browser history, clipboard artifacts where lawfully available, command line, and downloaded files.
- Reset credentials exposed after execution.
- Search proxy and email telemetry for additional recipients or visitors.
- Report the malicious shared content to the platform provider where appropriate.

## Source

Google Threat Intelligence Group. *GTIG AI Threat Tracker: Distillation, experimentation, and continued integration of AI for adversarial use*.

https://cloud.google.com/blog/topics/threat-intelligence/distillation-experimentation-integration-ai-adversarial-use