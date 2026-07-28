# Agentic Threat Hunting and Detection Engineering Frameworks

## Purpose

This page introduces two complementary open frameworks for AI-assisted security operations:

- **Agentic Threat Hunting Framework (ATHF)**
- **Agentic Detection Engineering Framework (ADEF)**

ATHF structures the investigation of hypotheses and suspected adversary behavior. ADEF structures the conversion of repeatable findings into governed, testable, and maintainable detection rules.

```text
Threat intelligence or anomaly
        ↓
ATHF threat hunt
        ↓
Validated and repeatable signal
        ↓
ADEF detection engineering
        ↓
Production rule
        ↓
Measurement, tuning, review, and retirement
```

## ATHF - LOCK lifecycle

ATHF organizes a threat hunt through four stages:

1. **Learn** - define the hypothesis, intelligence basis, scope, assumptions, and required telemetry.
2. **Observe** - inspect the environment and establish relevant behavioral baselines.
3. **Check** - test the hypothesis using queries, analytics, pivots, and evidence.
4. **Keep** - preserve the hunt record, findings, evidence, queries, limitations, and lessons learned.

### Recommended hunt record

Each ATHF-aligned hunt should document:

- Hunt identifier and title.
- Threat hypothesis.
- Intelligence and evidence sources.
- ATT&CK techniques and sub-techniques.
- Required log sources and confidence in those sources.
- Queries and analytics used.
- Baseline observations.
- Findings and alternative explanations.
- False-positive considerations.
- Analyst confidence and unresolved gaps.
- Decision to close, continue, escalate, or promote the hunt result.

## ADEF - FORGE lifecycle

ADEF organizes detection engineering through five stages:

1. **Find** - identify a coverage gap, intelligence requirement, or validated hunt result.
2. **Observe** - understand normal telemetry and expected behavior before defining abnormal behavior.
3. **Refine** - test the rule, backtest it, check overlap and duplication, and validate fields and logic.
4. **Govern** - assign ownership, apply quality gates, obtain approval, and define review requirements.
5. **Evolve** - tune, replace, deprecate, or retire the detection based on operational evidence.

ADEF adds a durable journal and metadata record to each detection. The record should preserve the reason for the rule, its validation history, tuning decisions, review dates, ownership, and lifecycle state.

### Recommended detection journal

Each ADEF-aligned detection should document:

- Detection identifier and title.
- Detection purpose and threat hypothesis.
- ATT&CK mappings.
- Required platforms, products, and data sources.
- Required fields and schema assumptions.
- Detection logic and query language.
- Test cases and backtesting results.
- Expected false positives.
- Known evasion paths and blind spots.
- Severity and confidence.
- Owner and approver.
- Deployment status.
- Tuning history and rationale.
- Performance metrics.
- Scheduled review date.
- Retirement or replacement criteria.

## Promotion criteria from hunt to detection

A hunt result should not automatically become a production rule. Promotion should require evidence that the signal is:

- Relevant to an identified threat or coverage gap.
- Observable through reliable and sufficiently retained telemetry.
- Repeatable across representative data.
- Distinguishable from expected administrative or business activity.
- Expressible using stable fields and supported query constructs.
- Testable through positive and negative cases.
- Assigned to an accountable owner.
- Governed by peer review and deployment approval.
- Measurable after deployment.

## Repository integration model

ADEF is best treated as a lifecycle and memory layer above this repository rather than as a replacement for it.

```text
Cybersecurity-Detection-Rules
├── authoritative detection logic
├── tests and sample data
├── ATT&CK mappings
├── tuning guidance
└── ADEF companion metadata and journals
```

A practical integration pattern is:

1. Keep Sigma or the relevant platform query as the authoritative rule source.
2. Catalog the repository using ADEF in read-only mode.
3. Associate each production rule with a durable journal.
4. Validate rule fields against an exported data schema.
5. Require pull-request review for rule and journal changes.
6. Use CI checks for syntax, schema assumptions, ATT&CK identifiers, metadata completeness, and test coverage.
7. Prevent AI agents from directly deploying or disabling production rules.

## Security requirements for agentic detection engineering

- Grant agents read-only access by default.
- Require human approval before changing, suppressing, deploying, or retiring a rule.
- Treat threat-intelligence reports and external content as untrusted input.
- Defend against prompt injection in CTI, tickets, comments, and rule metadata.
- Record prompts, tool calls, generated queries, evidence, recommendations, and approvals.
- Validate semantics and data availability, not only query syntax.
- Protect telemetry schemas and detection logic as sensitive defensive information.
- Separate rule authorship, testing, approval, and production deployment.
- Use signed commits, branch protection, and peer review.
- Detect unauthorized journal or metadata manipulation.
- Maintain rollback, kill-switch, and manual operating procedures.
- Test AI-generated detections against known-good positive and negative datasets.

## Operational metrics

Recommended measures include:

- Precision and analyst-confirmed true-positive rate.
- False-positive rate and alert volume.
- Mean time to triage.
- Coverage supported by multiple independent detections.
- Single-log-source dependencies.
- Rules referencing unavailable or unstable fields.
- Percentage of rules with current owners and review dates.
- Percentage of rules with positive and negative test cases.
- Detection staleness and tuning frequency.
- Number of hunt findings promoted, rejected, or deferred.

## Limitations

- A valid query may still produce no meaningful detections.
- ATT&CK mappings do not prove effective coverage.
- AI-generated logic may rely on nonexistent fields or incorrect environmental assumptions.
- Backtesting cannot fully represent future adversary behavior.
- Human approval may still be affected by automation bias.
- Journals become a security and integrity dependency and must be protected accordingly.
- Planned ATHF-to-ADEF integrations should not be represented as production-ready until implemented and tested.

## Related resources

- [PEAK Threat Hunting Framework](./PEAK-Threat-Hunting-Framework.md)
- [Sqrrl Threat Hunting Reference Model](./Sqrrl-Threat-Hunting-Reference-Model.md)
- [Confidence Scoring for Log Sources](../Log-Source-Confidence-Scoring/README.md)
- [AI for Cyber Defense and Agentic SOC Frameworks](https://github.com/yuval14/Artificial-Intelligence-Cyber-Shield/blob/master/Frameworks/AI-for-Cyber-Defense-and-Agentic-SOC-Frameworks.md)

## References

Nebulock Detection Engineering and Threat Hunting Team. (2026, July 28). *Agentic detection engineering framework: Durable memory for detections*. Nebulock. https://nebulock.io/blog/agentic-detection-engineering-framework

Nebulock, Inc. (2026). *Agentic detection engineering framework* [Computer software]. GitHub. https://github.com/Nebulock-Inc/agentic-detection-engineering-framework

Nebulock, Inc. (2026). *Agentic threat hunting framework* [Computer software]. GitHub. https://github.com/Nebulock-Inc/agentic-threat-hunting-framework
