# Query and interpretation guide

## Commands

Use the SQLite-backed query tool in the release directory:

```bash
python workflows/query_afgrip.py --release-dir <release-dir> --gene MYOZ1
python workflows/query_afgrip.py --release-dir <release-dir> --compare MYOZ1 SYNPO2L PITX2
python workflows/query_afgrip.py --release-dir <release-dir> --gene SYNPO2L --include-v2g
python workflows/query_afgrip.py --release-dir <release-dir> --gene MOG1 --include-literature
python workflows/query_afgrip.py --release-dir <release-dir> --top 20
```

The tool returns JSON with a `release` object. Include its version and relevant
boundary statements in any scientific interpretation.

`--include-literature` adds a separate AF-GRIP Literature Context v1.0 result;
it does not alter any frozen v1.0 value. The main query tool can resolve an
indexed historical alias through the Literature Context HGNC alias table, e.g.
`MOG1` to `RANGRF`.

## Gene-priority interpretation

- `genome_wide_rank` is ascending: smaller is higher priority.
- `AF_MVG_score` integrates robust-z evidence components under frozen weights.
  It is not a probability.
- `modalities_nonmissing` is a coverage measure. It is not independent evidence
  strength by itself.
- `LanguageEvidenceScore = null` is a release-wide missing value, not evidence
  against a gene.
- A high `GeneticScore` may reflect nearest-gene aggregation and does not make a
  causal gene assignment.
- A positive/negative atrial-state component is expression association, not a
  direction of causal AF effect.
- A graph component describes network context; a perturbation component is a
  model feature, not a human intervention estimate.

## Post-ranking literature context

Use `--include-literature` only when the user asks for literature interpretation
or when a gene's computational profile needs biological context. This resource
queried the v1.0 top 1% plus RANGRF/MOG1. It is intentionally outside the
scoring model.

- `europepmc_hit_count` is the source search count; it is not a score.
- `retrieved_article_count` counts the fixed-cap records retained in this
  release; it is not evidence strength.
- Every retained article has a `matched_aliases` field and a source sentence.
  `same_sentence` indicates AF and the entity occur in the same sentence;
  `same_abstract_only` is weaker textual context.
- All v1.0 context records are `unreviewed`. Verify the original article before
  claiming a mechanistic, genetic, or functional relation.
- `not_queried` means that a gene is outside the 194-gene initial query set. It
  does not mean that the gene lacks literature evidence.

## 10q22 V2G interpretation

`v2g_evidence` combines FinnGen R13 I9_AF (GRCh38) with GTEx v8 Heart Atrial
Appendage QTL records. `pip` is a published FinnGen FINEMAP variant-level PIP,
not a gene-level posterior probability. `credible_set_membership_raw` retains
the source columns without claiming a single credible set.

For `rs199809516`, the C allele is AF-protective in FinnGen and has MYOZ1 eQTL,
SYNPO2L eQTL, and SYNPO2L sQTL evidence. This supports shared regulatory
evidence, not unique assignment to MYOZ1 or SYNPO2L. Formal colocalization was
not performed because complete matched QTL summary statistics and
ancestry-matched LD were unavailable.

## Raw data and updates

Use `provenance/source_citations.tsv` for original source URLs, citations,
access dates, and licensing notes. Do not obtain source raw data by assuming it
is redistributable from the AF-GRIP archive. When new public data are added,
preserve v1.0 and construct a new versioned release with fresh checksums and a
new model card.
