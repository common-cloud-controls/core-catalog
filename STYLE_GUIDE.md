# Core Catalog Style Guide

This guide defines the writing conventions for entries in the CCC core catalog. The core catalog contains capabilities, threats, and controls that apply across all cloud service types, so entries here must be written in a service-agnostic way.

---

## General Conventions

### Voice and Tone

- Write in a professional, technical tone suitable for a security-engineering audience.
- Use present tense throughout.
- Be precise and specific — avoid vague language like "properly", "appropriately", or "as needed".
- Avoid marketing language or subjective qualifiers ("best-in-class", "robust", "comprehensive").

### Title Case

All titles use Title Case. Capitalize every word except:

- Articles: a, an, the
- Short prepositions (four letters or fewer): in, of, for, to, at, by, on, via, with
- Conjunctions: and, or, but, nor

Always capitalize the first and last word regardless of part of speech. Examples: "Encryption in Transit Enabled by Default", "Alert on Key-version Changes".

### Formatting

- Use the YAML block literal (`|`) for any multi-line text field (`description`, `objective`, `text`).
- End all sentences with a period.
- Do not end titles with a period or any trailing punctuation.
- Use one blank line between paragraphs within a field, not between sentences.
- Keep entries to the minimum words needed to convey the requirement clearly.

---

## Capabilities

### Title

- Use **Title Case** (see [Title Case](#title-case) above).
- Write as a short noun phrase describing what the service provides.
- Keep to 2-8 words.

**Good:** "Encryption in Transit Enabled by Default"
**Good:** "Configurable Data Retention Period"
**Bad:** "the service encrypts data in transit" (sentence, not a title)
**Bad:** "ENCRYPTION IN TRANSIT" (all caps)

### Description

- Write 1-3 complete sentences in present tense.
- Open with what the service does: "The service..." or describe the capability directly.
- Explain what the capability provides and, where helpful, how it works at a high level.
- Stay service-agnostic — describe the capability in terms any cloud service could satisfy.

**Example:**

```yaml
description: |
  The service automatically encrypts all data using industry-standard
  cryptographic protocols prior to transmission via a network interface.
```

---

## Threats

### Title

- Use **Title Case** (see [Title Case](#title-case) above).
- Write as a descriptive phrase stating the threat condition in present tense.
- Phrase as what goes wrong, not what should be prevented.
- Keep to 4-12 words.

**Good:** "Access is Granted to Unauthorized Users"
**Good:** "Data is Replicated to Restricted Regions"
**Bad:** "Prevent Unauthorized Access" (this is a control title, not a threat)
**Bad:** "unauthorized access" (not title case, too vague)

### Description

- Write 2-4 complete sentences.
- First sentence: describe the misconfiguration, weakness, or attack vector.
- Subsequent sentences: explain the consequence and potential impact.
- Use modal verbs ("may", "can", "could") to express likelihood — threats describe what _could_ happen, not what _will_ happen.
- Stay service-agnostic.

**Example:**

```yaml
description: |
  Logic designed to give different permissions to different entities may be
  misconfigured or manipulated, allowing unauthorized entities to access
  restricted parts of the service. This can result in data exfiltration,
  modification of sensitive configurations, or denial of service to
  legitimate users.
```

---

## Controls

### Title

- Use **Title Case** (see [Title Case](#title-case) above).
- Begin with an **imperative verb** describing the protective action: Prevent, Ensure, Enforce, Restrict, Encrypt, Implement, Verify, Monitor, Alert.
- Keep to 3-10 words.

**Good:** "Prevent Unencrypted Requests"
**Good:** "Enforce Automatic Key Rotation"
**Bad:** "Unencrypted Requests are Prevented" (passive — use imperative)
**Bad:** "Encryption" (too vague, not actionable)

### Objective

- Write 1-3 complete sentences.
- Open with an imperative verb: "Ensure that...", "Prevent...", "Restrict...".
- Explain the protection goal, the mechanism, and the benefit.
- Stay service-agnostic.

**Example:**

```yaml
objective: |
  Ensure that all communications are encrypted in transit to protect data
  integrity and confidentiality.
```

### RFC 2119 Keywords

Assessment requirement `text` fields use [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) keywords to express obligation levels:

| Keyword | Meaning | When to Use |
|---|---|---|
| **MUST** | Absolute requirement | The condition is mandatory for compliance |
| **MUST NOT** | Absolute prohibition | The condition must never occur |
| **SHOULD** | Recommended | The condition is expected unless a justified reason exists |
| **MAY** | Optional | The condition is permitted but not required |

Always write these keywords in UPPERCASE. Use **MUST** by default — only reach for SHOULD or MAY when the requirement genuinely permits exceptions.

RFC 2119 keywords apply only to assessment requirement `text` fields. Do not use them in titles, objectives, descriptions, or recommendations.

### Logical Connectors in AR Text

- Use uppercase **AND** when multiple conditions must all be met in a single requirement.
- Use lowercase "or" for alternatives in prose.
- Use semicolons to separate related but distinct clauses within a single requirement.

### Assessment Requirement `text`

- Write as a single testable statement.
- Use the conditional pattern: **"When [trigger condition], [subject] MUST [requirement]."**
- The subject is typically "the service", "the system", or "an alert".
- Include specific, measurable thresholds where applicable (e.g., "within five minutes", "TLS 1.3 or higher").
- Each AR should test exactly one condition — split compound requirements into separate ARs.

**Example:**

```yaml
text: |
  When a port is exposed for non-SSH network traffic, all traffic MUST
  include a TLS handshake AND be encrypted using TLS 1.3 or higher.
```

### Recommendation

- Write as brief, actionable implementation guidance.
- Use imperative verbs: "Track", "Configure", "Review", "Implement".
- Include specific examples or tool names where helpful.
- Leave as an empty string (`""`) when the AR text is self-explanatory.

---

## Group

Every capability, threat, and control entry must have a `group` field. The group describes the security or operational domain the entry belongs to — not what service it is part of. The same groups are used across all three catalog types.

Assign the group by asking: **"What security or operational concern does this entry primarily address?"**

- Unauthorized access → `Access`
- Data exposed in transit or at rest → `Encryption`
- Loss of visibility into what happened → `Observability`
- Data lost, corrupted, or leaked → `CCC.Core.Data`
- Resources misconfigured or exhausted → `CCC.Core.Resource`

When an entry spans two domains, pick the one that best describes its _primary purpose_. For example, a control about encrypting data at rest belongs in `Encryption`, not `CCC.Core.Data`.

See the [group assignment guide](../CLAUDE.md#group-assignment-guide) for the full list of available groups with descriptions.

---

## Service-Agnostic Language

Because the core catalog is inherited by every service catalog, entries must not reference specific services, products, or cloud providers. Write in terms of generic cloud capabilities:

| Instead of | Write |
|---|---|
| "S3 bucket", "GCS bucket" | "storage bucket" or "storage resource" |
| "IAM role" | "identity or principal" |
| "CloudWatch", "Azure Monitor" | "monitoring service" |
| "AWS KMS", "Cloud KMS" | "key management service" |
| "EC2 instance", "Compute Engine VM" | "compute instance" |
