# HONESTCUE: LLM-Generated C# and Runtime Execution

## Summary

Google Threat Intelligence Group reported HONESTCUE as a downloader or launcher that requested C# code from Gemini, compiled the returned source with legitimate .NET capabilities, and could execute the resulting assembly in memory.

The sample demonstrates a just-in-time malware pattern in which the initial artifact does not contain the complete operational payload.

## AIDAF Assessment

| Field | Assessment |
|---|---|
| AI role | Content Generator and runtime capability provider |
| Likely involvement level | AI-2 to AI-3 depending on deployment and decision logic |
| Strong evidence | Model API calls, prompt or response artifacts, C# source, compiler invocation, generated assembly |
| Alternative explanation | Conventional download-and-compile framework without AI involvement |
| Public evidence status | Technically validated; public reporting indicated limited or developmental operational use |

## Detection Opportunities

1. Model API access by a non-development process.
2. AI response followed by `csc.exe`, Roslyn, `CSharpCodeProvider`, or another runtime compiler.
3. Source creation in temporary or user-writable locations.
4. In-memory assembly loading by a process that is not an approved build or application host.
5. Short correlation windows linking model traffic, compilation, and execution.

## Suggested Correlation

```text
Suspicious Process
  -> Gemini or Model-Compatible Endpoint
  -> C# Source or Structured Code Response
  -> Runtime Compiler
  -> Assembly Load or Child Process
```

## False Positives

- Approved coding assistants
- Interactive development
- CI/CD workers
- Deployment and installation tools
- Security testing in an authorized laboratory

## Response Guidance

- Preserve the process tree, model endpoint, source file, assembly, and memory artifacts.
- Revoke the relevant model API key or service credential.
- Determine whether the model request contained victim-specific data.
- Review persistence, child processes, and subsequent network connections.
- Treat model-provider evidence as supporting AIDAF assessment, not actor attribution.

## Source

Google Threat Intelligence Group. *GTIG AI Threat Tracker: Distillation, experimentation, and continued integration of AI for adversarial use*.

https://cloud.google.com/blog/topics/threat-intelligence/distillation-experimentation-integration-ai-adversarial-use