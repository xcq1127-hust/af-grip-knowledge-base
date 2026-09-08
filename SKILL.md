---
name: af-grip-knowledge-base
description: Query and interpret versioned AF-GRIP Knowledge Base releases for AF gene prioritization, gene comparisons, and 10q22 fine-mapping/V2G evidence.
---

# AF-GRIP Knowledge Base

Use this skill for requests to look up, compare, summarize, or prepare outputs
from AF-GRIP Knowledge Base v1.0 or v1.1. It can also retrieve the independent,
post-ranking AF-GRIP Literature Context v1.0 as an explanatory layer.

Before querying, locate unpacked releases using these environment variables:

```text
AF_GRIP_RELEASE_DIR
AF_GRIP_LITERATURE_CONTEXT_DIR
```

If they are not configured, use paths supplied by the user. The public release
archives are available at DOI `10.5281/zenodo.22089524` (Knowledge Base) and
DOI `10.5281/zenodo.22089535` (Literature Context). Do not assume the source
downloads or any human-study materials are present in either archive.

Use the release query utility for ordinary lookup:

```bash
python "$AF_GRIP_RELEASE_DIR/workflows/query_afgrip.py" \
  --release-dir "$AF_GRIP_RELEASE_DIR" \
  --literature-release-dir "$AF_GRIP_LITERATURE_CONTEXT_DIR" \
  --gene MYOZ1 --include-literature
```

For comparison, pass `--compare MYOZ1 SYNPO2L`. For locus-level records, use
`--gene <symbol> --include-v2g`. Read [the query and interpretation guide](references/query_and_interpretation.md)
before interpreting score or V2G output. Read [the schema reference](references/schema.md)
when a request requires fields, joins, database work, bulk export, or API/MCP design.
Read [the literature-context guide](references/literature_context.md) before
interpreting retrieved literature. Indexed historical symbols are recognized;
for example, `MOG1` resolves to current HGNC symbol `RANGRF`.

## Required boundaries

- Describe AF-GRIP scores as candidate-prioritization outputs. Do not call them
  causal probabilities, clinical risk scores, drug-target probabilities, or
  human intervention effects.
- For v1.0, treat the missing language view as missing evidence, never as negative evidence. For v1.1, language-derived literature context is excluded from the score.
- Treat Literature Context as post-ranking, non-scoring retrieval context. It
  must never alter, reweight, or be added to an AF-GRIP rank or score.
- Do not call a retrieved article a validated gene-AF relation. Report its PMID,
  source sentence, matching scope, and `unreviewed` curation state.
- Do not infer a unique 10q22 effector gene. MYOZ1 and SYNPO2L have shared
  fine-mapped regulatory evidence; formal AF-QTL colocalization was not done.
- Keep Nielsen 2018 (GRCh37) separate from FinnGen/GTEx locus records (GRCh38)
  unless a separate coordinate and allele harmonization analysis is supplied.
- Do not relabel raw FinnGen `cs`, `cs2`, ... annotations as one 95% credible set.

## Response content

For an individual gene, report its rank, score, nonmissing modalities, leading
evidence components, source/missingness context, and the release version.
For a gene comparison, report differences in evidence composition rather than
only the rank difference. For V2G interpretation, distinguish observed
association/QTL evidence from causal hypotheses and quote the formal-coloc
limitation. Link the user to original sources in `provenance/source_citations.tsv`
when they need raw data or source licensing.

When literature is requested, return it in a separate `Post-ranking literature
context` section after the frozen AF-GRIP result. State whether the gene was
queried by Literature Context v1.0; `not_queried` does not mean no literature.

Do not edit the frozen release for an ordinary query. Build a new versioned
release only when the user asks to incorporate new data or re-run the analysis.
