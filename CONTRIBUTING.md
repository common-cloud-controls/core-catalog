# Contributing to the CCC Core Catalog

## Overview

This repository holds the core catalog for the [FINOS Common Cloud Controls](https://www.finos.org/common-cloud-controls-project) project. It contains three catalog files — capabilities, threats, and controls — each conforming to the corresponding [Gemara](https://github.com/gemaraproj/gemara) schema.

Catalog-level `metadata` (title, version, publication date, etc.) is not authored here — it is populated automatically at release time by the [delivery toolkit](https://github.com/finos/common-cloud-controls/tree/main/delivery-toolkit) using the [`go-gemara`](https://github.com/gemaraproj/go-gemara) SDK.

## Repository Structure

```
capabilities.yaml   — CapabilityCatalog
threats.yaml        — ThreatCatalog
controls.yaml       — ControlCatalog
metadata.yaml       — supplementary metadata
```

---

## Capabilities (`capabilities.yaml`)

Conforms to the Gemara [`CapabilityCatalog`](https://github.com/gemaraproj/gemara) schema.

### Schema

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier in the form `CCC.Core.CP<NN>` |
| `title` | string | Short label for the capability |
| `description` | string | Detailed description of what the capability provides |

### Example

```yaml
capabilities:
  - id: CCC.Core.CP01
    title: Encryption in Transit Enabled by Default
    description: |
      The service automatically encrypts all data using industry-standard
      cryptographic protocols prior to transmission via a network interface.
```

### Adding a capability

1. Append a new entry to the `capabilities` list in `capabilities.yaml`.
2. Assign the next available `CP` number (e.g. `CCC.Core.CP14`).
3. Ensure `id`, `title`, and `description` are all present.

---

## Threats (`threats.yaml`)

Conforms to the Gemara [`ThreatCatalog`](https://github.com/gemaraproj/gemara) schema.

### Schema

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier in the form `CCC.Core.TH<NN>` |
| `title` | string | Short label for the threat |
| `description` | string | Description of the threat and its potential impact |

### Example

```yaml
threats:
  - id: CCC.Core.TH01
    title: Access is Granted to Unauthorized Users
    description: |
      Logic designed to give different permissions to different entities may be
      misconfigured or manipulated, allowing unauthorized entities to access
      restricted parts of the service.
```

### Adding a threat

1. Append a new entry to the `threats` list in `threats.yaml`.
2. Assign the next available `TH` number.
3. Ensure `id`, `title`, and `description` are all present.

---

## Controls (`controls.yaml`)

Conforms to the Gemara [`ControlCatalog`](https://github.com/gemaraproj/gemara) schema.

### Schema

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier in the form `CCC.Core.CN<NN>` |
| `title` | string | Short label for the control |
| `objective` | string | Unified statement of intent for this control |
| `assessment-requirements` | list | Requirements that confirm the control objective has been met |

### Example

```yaml
controls:
  - id: CCC.Core.CN01
    title: Prevent Unencrypted Requests
    objective: |
      Ensure that the service rejects any request that does not use an
      encrypted transport protocol.
    assessment-requirements:
      - id: CCC.Core.CN01.AR01
        text: |
          The service must reject unencrypted requests by default.
```

### Adding a control

1. Append a new entry to the `controls` list in `controls.yaml`.
2. Assign the next available `CN` number.
3. Ensure `id`, `title`, `objective`, and at least one `assessment-requirements` entry are present.

---

## Importing core entries into service catalogs

Service catalogs reference core entries by ID using the `imports` key:

```yaml
imports:
  - reference-id: CCC.Core
    entries:
      - reference-id: CCC.Core.CP01
        remarks: Encryption in Transit Enabled by Default
      - reference-id: CCC.Core.TH01
        remarks: Access is Granted to Unauthorized Users
```

The source catalog must also declare a `mapping-references` entry in its metadata pointing to the versioned release of this repo.

---

## Validation

Each file can be validated locally against the Gemara CUE schema using the [CUE CLI](https://cuelang.org/docs/install/):

```sh
cue vet --schema '#CapabilityCatalog' github.com/gemaraproj/gemara capabilities.yaml
cue vet --schema '#ThreatCatalog'     github.com/gemaraproj/gemara threats.yaml
cue vet --schema '#ControlCatalog'    github.com/gemaraproj/gemara controls.yaml
```

## References

- [Gemara schema](https://github.com/gemaraproj/gemara) — CUE schema definitions
- [go-gemara](https://github.com/gemaraproj/go-gemara) — Go SDK used by the delivery toolkit
- [gemara.openssf.org](https://gemara.openssf.org) — full model documentation
