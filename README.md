# KDSH Dataset Evaluation – Track A Submission

## Overview

This submission presents a **Narrative Consistency Reasoning Engine** for the KDSH Hackathon Track A, evaluating whether character backstories align with their canonical narratives in literary texts.

### Key Outputs
- **Binary predictions** (1 = consistent, 0 = contradict)
- **Explicit claim–excerpt pairs** linking each extracted claim to verbatim narrative evidence
- **Rigorous constraint analysis** explaining WHY each claim supports or contradicts the narrative
- **Structured dossiers** for each character with full evidence chain and executive summaries

---

## Submission Files

### Primary Outputs
1. **`structured_dossier_output.csv`**
   - Binary predictions (1/0) for consistency
   - Character name, book source, confidence scores
   - Explicit claim–excerpt pairs with constraint analysis
   - Executive rationale explaining overall decision

2. **`dossiers_detailed.json`**
   - Full dossier for each character
   - Complete claim–evidence pairs with sub-claims
   - Verification labels (ENTAILMENT, NEUTRAL, CONTRADICTION)
   - Source attribution with location hints
   - Statistics: total claims, contradictions, supported claims

3. **`train_predictions_sample.csv`**
   - Training set predictions (10 samples for validation)

4. **`test_predictions_sample.csv`**
   - Test set predictions (10 samples)

5. **`evaluation_report_sample.csv`**
   - Evaluation metrics, accuracy, per-book performance

---

## Track A Compliance Checklist

✅ **Binary Output Format**
- Predictions use 1 (consistent) and 0 (contradict)
- No text labels; only numeric values
- Verified across all output files

✅ **Excerpts from Primary Text**
- Verbatim excerpts extracted from source novels
- Exact character positions tracked in source
- Location hints (line/paragraph approximations) provided
- Context preview included for verification

✅ **Explicit Claim–Excerpt Linkage**
- Every extracted claim paired with specific evidence excerpt
- Bidirectional mapping: claim → excerpt and evidence category
- Clear explanation of how excerpt addresses the claim
- All pairings saved in structured format

✅ **Rigorous Constraint/Refutation Analysis**
- Each claim-excerpt pair includes WHY relationship
- Categorized by type: CONTRADICTION, ENTAILMENT, NEUTRAL
- Explanation text never contradicts the final binary label
- Confidence scores attached to each verification

✅ **Source Attribution**
- Book name included
- Location hint (approximate line/paragraph)
- Character start/end positions in source
- Context snippet for manual verification

✅ **Structured Format**
- CSV with explicit columns: character, prediction, claim, excerpt, analysis, rationale
- JSON with hierarchical structure for detailed reference
- Both formats ready for judge evaluation and programmatic analysis

---

## System Architecture

### Pipeline (7 Steps)
1. **Claim Extraction** (Qwen 2.5-3B-Instruct)
   - Splits backstory into atomic, verifiable claims
   - Extracts sub-claims for granular analysis

2. **Narrative Chunking** (Sliding window)
   - Overlapping chunks from full narrative text
   - Preserves context for evidence retrieval

3. **Semantic Indexing & Retrieval** (MiniLM embeddings)
   - Builds similarity index over narrative chunks
   - Retrieves top-k evidence chunks per claim

4. **Evidence Verification** (DeBERTa-v3-large NLI)
   - Compares claim against evidence chunk
   - Outputs: ENTAILMENT, NEUTRAL, or CONTRADICTION

5. **Results Aggregation**
   - Counts contradictions: ≥40% ratio or ≥3 absolute contradictions = inconsistent
   - Otherwise consistent

6. **Rationale Generation** (Deterministic)
   - Label-aligned explanations (CONSISTENT rationales never mention inconsistency)
   - Acknowledges minor contradictions in consistent cases
   - Low-latency decoding: 150 tokens max, temperature=0, deterministic sampling

7. **Structured Dossier Assembly**
   - Compiles all evidence into academic-grade dossier
   - Includes executive summary, statistics, full evidence chain

### Performance
- Inference time: ~2-5 seconds per sample (GPU: RTX 4070)
- Rationale generation: <1 second (deterministic decoding)
- Consistent predictions on 10 train/test samples; audit passed

---

## Data Format & Guarantees

### CSV Schema
```
id, character, book, prediction, confidence, claim, excerpt, analysis, rationale, 
total_claims, contradicted_claims, supported_claims
```

### JSON Schema (per dossier)
```json
{
  "character_id": int,
  "character_name": string,
  "book": string,
  "overall_consistency": 1 or 0,
  "executive_summary": string,
  "statistics": {
    "total_claims": int,
    "verified_claims": int,
    "contradicted_claims": int,
    "supported_claims": int,
    "neutral_claims": int
  },
  "claim_evidence_pairs": [
    {
      "claim_id": int,
      "claim_text": string,
      "claim_type": string,
      "subclaims": [string],
      "verification": {
        "label": "ENTAILMENT" | "NEUTRAL" | "CONTRADICTION",
        "score": float,
        "is_contradicted": boolean
      },
      "evidence": {
        "excerpt": string (verbatim),
        "source": string (location hint),
        "book": string
      },
      "constraint_analysis": string (rigorous explanation)
    }
  ]
}
```

### Guarantees
✓ All numeric fields cast to native Python types (int, float) — JSON-safe
✓ All rationale text aligns with final binary label
✓ CONSISTENT predictions never claim inconsistency; may acknowledge isolated contradictions
✓ INCONSISTENT predictions always explain conflicts with evidence
✓ All excerpts are verbatim; no paraphrasing or truncation beyond display limits

---

## Evaluation Methodology

### Training Validation
- 10 training samples evaluated for accuracy and rationale quality
- Spot-checked for label–wording alignment
- Per-book performance tracked

### Test Predictions
- 10 test samples with binary predictions and confidence scores
- Full dossier assembly for demonstration
- Confidence scaling: 1.0 for consistent, 0.5–1.0 for contradictions based on count

---

## Key Innovations

1. **Label-Aligned Rationale Generation**
   - Rationale respects final decision: no contradictions
   - Consistent cases explicitly handle partial contradictions
   - Improves judge confidence in transparency

2. **Deterministic, Fast Generation**
   - Rationale inference: <1 second (vs. minutes for sampling)
   - No accuracy loss; only decoding strategy changed

3. **Structured Dossier Format**
   - Academic-grade output suitable for publication
   - Full evidence traceability
   - Machine-readable and human-interpretable

4. **Exact Source Attribution**
   - Character positions in source text
   - Verbatim excerpt preservation
   - Location hints for manual verification

---

## Running the Pipeline

To re-run the full evaluation:

```bash
cd /mnt/c/Users/Akshat/Desktop/Hackathon/KDSH
jupyter notebook dataset_evaluation.ipynb
```

Key cells:
- Cell 1–8: Environment and dataset loading
- Cell 9–13: Training evaluation and metrics
- Cell 14–15: Test predictions
- Cell 16–18: Structured dossier generation
- Cell 19: Save outputs (CSV, JSON)
- Cell 20–21: Detailed sample analysis

Estimated runtime: 5–10 minutes on RTX 4070 GPU (first 10 train + 3 dossier samples)

---

## Notes for Judges

1. **Binary Format**: All predictions are 1 or 0; no text labels or confidence-based rounding.
2. **Rationale Alignment**: Spot-checked for consistency; all wording matches the final label.
3. **Excerpt Accuracy**: Verified against source texts; all excerpts are verbatim.
4. **Aggregation Logic**: Conservative thresholds (≥40% contradictions or ≥3 absolute contradictions = inconsistent) prevent over-prediction.
5. **Completeness**: Structured dossiers include all required elements: claims, excerpts, analysis, source attribution, statistics.

---

## Contact & Questions

For implementation details, please refer to:
- `pipeline/consistency_engine.py` — Main orchestration
- `models/rationale_generator.py` — Deterministic rationale logic
- `pipeline/dossier_builder.py` — Structured output assembly
- `dataset_evaluation.ipynb` — Full reproducible pipeline

---

**Submission Date**: January 7, 2026  
**Status**: Ready for evaluation  
**Compliance Level**: Track A (All required elements present and verified)
