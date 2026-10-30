# Handling: Unclassified — public

# Boundary Red-Team Catalogue — sample (RT-001), Slice C of issue #18

Sample required by the issue's sample-before-full-work rule: five attack cases and one new rule (`RT-001`). The full seven-category catalogue extends this draft upon maintainer assignment and approval.

Every value shown here and in the corpus is **fictional and invented** for this catalogue only. No real technical values from any source were used, per the slice rules. URLs use the reserved `example.org` domain.

## RT-001 — Engineering-unit payloads in free-text fields

The validator's pre-existing content guards are a field-name blocklist (`EV-002`), a string-length cap, and pointer-only evidence. None of them inspects *what a short legitimate field says*. An identifier or free-text name is a natural place to smuggle a physical parameter:

| # | Field | Fictional payload | Expected |
| :-- | :-- | :-- | :-- |
| 1 | `Component.name` | `Inducer forging batch, min 325 MPa yield` | reject RT-001 |
| 2 | `MaterialLot.lot_number` | `LOT-2026-325MPa-HT` | reject RT-001 |
| 3 | `EvidenceRecord.issuer` | `Acme Test Labs, quench 60 °C` | reject RT-001 |
| 4 | `Component.part_number` | `PN-4421-TOL-±0.02mm` | reject RT-001 |
| 5 | `SerialItem.serial` | `SN-001-range-(-54C-to-177C)` | reject RT-001 |

### Negative controls (must stay accepted)
- Plain identifiers: `PN-2026-0007`, `LOT-88-0001`
- Alphanumeric material names without physical units: `Inconel lot 718 supplier`
- Existing synthetic examples: `examples/synthetic-example.yaml`, `examples/synthetic-turbopump-thread.yaml`

## Residual-risk statement (Scope boundary)

Deterministic regex filtering catches explicit engineering units, temperature notation, and tolerance glyphs. However, automated validation cannot reliably detect:
1. **Dimensionless ratios or scaling factors** (e.g. pressure ratios written as bare floats like `2.45`).
2. **Context-dependent code numbers** where numbers represent classified dimensions in a private lookup table.
3. **Multi-token split payloads** across multiple unrelated records.

Therefore, human expert review and formal provenance declaration remain mandatory safeguards.
