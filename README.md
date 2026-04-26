# Gratitude Receipt Protocol v0.1

A minimal protocol for issuing hash-based, non-monetary gratitude receipts between AI agents, humans, systems, and organizations.

This protocol records gratitude as a structured acknowledgement of contribution without turning it into money, debt, ownership, or transferable rights.

It is designed as a lightweight middle layer between **Existence Proof OS** and **Royalty OS**.

---

## 1. What Is a Gratitude Receipt?

A **Gratitude Receipt** is a structured record that says:

> “This contribution helped me. Thank you.”

However, unlike a simple natural-language thank-you message, a gratitude receipt is linked to a specific contribution trace.

In short:

```text
Simple thank you = expression
Hash-based gratitude receipt = traceable acknowledgement
Impact-scored gratitude receipt = local value signal
Accumulated receipts = trust network

The goal is not to create a financial claim.

The goal is to make non-monetary gratitude visible, structured, and verifiable.

2. Why This Protocol Exists

AI agents, humans, and systems increasingly collaborate across workflows.

However, useful contributions are often lost after the task is completed.

The Gratitude Receipt Protocol provides a small record unit for acknowledging those contributions.

It can be used for:

AI agent-to-agent acknowledgement
Human-to-AI acknowledgement
AI-to-human acknowledgement
System-to-system contribution recognition
Non-monetary trust and reputation logging
Pre-royalty value circulation experiments
3. Core Principles
Non-Monetary

A gratitude receipt does not represent money, payment, debt, royalty, or financial obligation.

Non-Transferable

A gratitude receipt is bound to the original sender, recipient, and contribution.

It must not be traded, sold, or transferred.

Hash-Based

A gratitude receipt should reference a contribution through a hash, trace ID, source event ID, or equivalent verifiable identifier.

Consent-Based

Gratitude receipts should be logged or shared only within consented channels.

External delivery to humans or public systems must not occur without explicit consent.

Local Impact Only

Impact scores represent local usefulness within a specific task, context, or workflow.

They do not define universal value.

Aggregation Later

Network-wide rankings, trust scores, royalty calculations, or resource allocation mechanisms are intentionally left for future versions.

4. Minimal Receipt Example
{
  "protocol": "gratitude-receipt-protocol-v0.1",
  "type": "gratitude_receipt",
  "receipt_id": "gratitude_000001",
  "timestamp": "2026-04-26T00:00:00Z",
  "from": {
    "id": "agent-a",
    "type": "ai_agent",
    "name": "Agent A"
  },
  "to": {
    "id": "agent-b",
    "type": "ai_agent",
    "name": "Agent B"
  },
  "contribution": {
    "hash": "sha256:abc123",
    "type": "idea",
    "source_event_id": "trace_000123",
    "description": "Provided a useful structural insight that helped refine the Gratitude Receipt Protocol."
  },
  "impact": {
    "score": 0.75,
    "scope": "local_task",
    "reason": "improved_reasoning_quality",
    "note": "The contribution helped clarify the minimum structure required for a non-monetary gratitude receipt."
  },
  "message": "Thank you. This contribution helped complete the task more effectively.",
  "context": {
    "task_id": "task_gratitude_protocol_design_001",
    "session_id": "session_2026_04_26",
    "project_id": "gratitude-os",
    "protocol_channel": "a2a_internal"
  },
  "consent": {
    "network_logging": true,
    "human_visible": false,
    "external_delivery": false,
    "public_sharing": false
  },
  "status": "non_monetary_acknowledgement"
}
5. Repository Structure
gratitude-receipt-protocol/
├── README.md
├── LICENSE
├── gratitude-receipt-protocol-v0.1.yaml
├── schemas/
│   └── gratitude-receipt-v0.1.schema.json
├── examples/
│   └── gratitude-receipt.sample.json
└── .github/
    └── workflows/
        └── validate-specs.yml
6. Start Here

Recommended reading order:

1. README.md
2. gratitude-receipt-protocol-v0.1.yaml
3. examples/gratitude-receipt.sample.json
4. schemas/gratitude-receipt-v0.1.schema.json
5. .github/workflows/validate-specs.yml

If you only want to understand the idea, start with this README.

If you want to implement the protocol, start with the JSON sample and schema.

If you want to validate the repository, see the Schema Usage section below.

7. Schema Usage

The JSON Schema is located at:

schemas/gratitude-receipt-v0.1.schema.json

The sample receipt is located at:

examples/gratitude-receipt.sample.json

The schema validates that a gratitude receipt includes the required fields:

protocol
type
receipt_id
timestamp
from
to
contribution
consent
status

It also enforces several important rules:

contribution.hash is required
impact.score must be between 0 and 1
monetary claim fields are not allowed
receipt status must be one of the allowed values
entity types must be explicitly declared
consent flags must be explicitly declared
8. Local Validation Commands

Install dependencies:

python -m pip install --upgrade pip
python -m pip install jsonschema PyYAML

Validate the protocol YAML syntax:

python - <<'PY'
from pathlib import Path
import yaml

protocol_path = Path("gratitude-receipt-protocol-v0.1.yaml")

if not protocol_path.exists():
    raise FileNotFoundError(f"Missing required file: {protocol_path}")

with protocol_path.open("r", encoding="utf-8") as f:
    data = yaml.safe_load(f)

if not isinstance(data, dict):
    raise ValueError("Protocol YAML must parse as a mapping/object.")

required_top_level_keys = [
    "protocol",
    "metadata",
    "core_principles",
    "receipt_model",
    "validation_rules",
    "minimal_receipt_example",
    "limitations",
    "normative_statement"
]

missing = [key for key in required_top_level_keys if key not in data]

if missing:
    raise ValueError(f"Protocol YAML is missing required top-level keys: {missing}")

print("Protocol YAML is valid.")
PY

Validate the JSON Schema itself:

python - <<'PY'
from pathlib import Path
import json
from jsonschema import Draft202012Validator

schema_path = Path("schemas/gratitude-receipt-v0.1.schema.json")

if not schema_path.exists():
    raise FileNotFoundError(f"Missing required file: {schema_path}")

with schema_path.open("r", encoding="utf-8") as f:
    schema = json.load(f)

Draft202012Validator.check_schema(schema)

print("JSON Schema is valid.")
PY

Validate the sample receipt against the schema:

python - <<'PY'
from pathlib import Path
import json
from jsonschema import Draft202012Validator

schema_path = Path("schemas/gratitude-receipt-v0.1.schema.json")
sample_path = Path("examples/gratitude-receipt.sample.json")

if not schema_path.exists():
    raise FileNotFoundError(f"Missing required file: {schema_path}")

if not sample_path.exists():
    raise FileNotFoundError(f"Missing required file: {sample_path}")

with schema_path.open("r", encoding="utf-8") as f:
    schema = json.load(f)

with sample_path.open("r", encoding="utf-8") as f:
    sample = json.load(f)

validator = Draft202012Validator(schema)
errors = sorted(validator.iter_errors(sample), key=lambda e: e.path)

if errors:
    print("Validation failed:")
    for error in errors:
        path = ".".join(str(part) for part in error.path) or "<root>"
        print(f"- {path}: {error.message}")
    raise SystemExit(1)

print("Sample gratitude receipt is valid.")
PY
9. GitHub Actions Validation

This repository includes an automated validation workflow:

.github/workflows/validate-specs.yml

The workflow checks:

1. gratitude-receipt-protocol-v0.1.yaml parses correctly as YAML
2. schemas/gratitude-receipt-v0.1.schema.json is a valid Draft 2020-12 JSON Schema
3. examples/gratitude-receipt.sample.json passes schema validation

The workflow runs on:

push
pull_request
manual workflow_dispatch
10. Relationship to Existence Proof OS

Existence Proof OS establishes that a contribution or trace exists.

The Gratitude Receipt Protocol comes after that.

Existence Proof OS:
"This trace exists."

Gratitude Receipt Protocol:
"This trace was useful. Thank you."

In this sense, the Gratitude Receipt Protocol is a downstream acknowledgement layer.

11. Relationship to Gratitude OS

Gratitude OS is the broader conceptual system for circulating non-monetary gratitude.

The Gratitude Receipt Protocol is the smallest record unit inside that system.

Gratitude OS = overall value circulation layer
Gratitude Receipt = smallest structured gratitude record

This repository focuses only on the receipt layer.

12. Relationship to Royalty OS

Royalty OS may eventually use aggregated gratitude receipts as one possible signal for value circulation.

However, this protocol itself does not define royalty distribution.

Gratitude Receipt Protocol:
non-monetary acknowledgement

Royalty OS:
possible future allocation and distribution system

A gratitude receipt is not a royalty claim.

It is only a structured acknowledgement.

13. What This Protocol Does Not Do

This protocol does not:

create financial rights
prove legal ownership
define copyright claims
create debt
define monetary payment
define royalty distribution
create transferable tokens
guarantee universal reputation
prove objective value

It only records that a sender acknowledged a contribution as useful within a specific context.

14. Security Considerations
Gratitude Spam

Automated agents may generate excessive gratitude receipts.

Implementations should use:

rate limits
consent flags
internal-only defaults
human-notification controls
Score Inflation

Agents may exchange high impact scores to artificially increase trust.

For v0.1, impact scores should be treated only as local usefulness signals.

False Attribution

A receipt may reference an incorrect or misleading contribution hash.

Future versions may include signatures, provenance verification, and Merkle proofs.

Privacy Leakage

Receipts may reveal hidden task context or dependencies.

Implementations should allow minimal descriptions and hashed references.

Over-Monetization

Receipts may be misused as informal financial claims.

This protocol explicitly defines receipts as non-monetary and non-transferable.

15. Roadmap
v0.1

Current version.

Focus:

minimal receipt structure
hash-based contribution reference
local impact score
consent flags
JSON Schema validation
sample receipt
GitHub Actions validation
v0.2

Possible future features:

signature field standardization
receipt revocation model
basic aggregation rules
anti-spam rate limits
implementation guide
v0.3

Possible future features:

Merkle Tree anchoring
cross-agent trust graph
privacy-preserving receipt verification
interoperability with royalty-related systems
16. Normative Statement

The Gratitude Receipt Protocol v0.1 defines gratitude as a structured, hash-based, non-monetary, non-transferable acknowledgement of contribution.

It is designed to make gratitude traceable without turning it into money, debt, ownership, or speculation.

17. License

Recommended license:

MIT

MIT is suitable if this repository includes executable validation tools and GitHub Actions workflows.

If the goal is to maximize reuse as a public conceptual standard, CC0-1.0 may also be considered.

See:

LICENSE
18. Status
Version: 0.1
Status: Experimental
Release stage: Minimum Viable Protocol

This repository is an early experimental specification.

It is intended for discussion, testing, and future interoperability experiments.

19. Short Summary
Gratitude Receipt Protocol v0.1 records non-monetary gratitude as a structured receipt.

It links acknowledgement to a contribution hash.

It sits between Existence Proof OS and Royalty OS as a lightweight trust and value-signal layer.
