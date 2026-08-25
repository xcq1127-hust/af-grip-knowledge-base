# AF-GRIP Literature Context v1.0

The literature context release is independent of AF-GRIP Knowledge Base v1.0.
It must be unpacked locally and referenced with `AF_GRIP_LITERATURE_CONTEXT_DIR`:

```text
$AF_GRIP_LITERATURE_CONTEXT_DIR
```

Its public archive DOI is `10.5281/zenodo.22089535`.

It is a post-ranking, non-scoring retrieval layer. AF-GRIP v1.0 scores, ranks,
weights, thresholds, and nominations are unchanged by its construction or use.

## Query

```bash
python "$AF_GRIP_LITERATURE_CONTEXT_DIR/workflows/query_literature_context.py" \
  --release-dir "$AF_GRIP_LITERATURE_CONTEXT_DIR" \
  --gene MOG1 --limit 10
```

Or use the integrated AF-GRIP query tool with `--include-literature`.

## Data model

`gene_literature_summary` stores query coverage, all aliases used, source hit
count, retained entity-validated records, query URL, and status.
`literature_evidence` stores PMID/PMCID/DOI, title, matching aliases, original
abstract sentence, matching scope, source URL, query, and curation status.
`gene_aliases` maps query aliases to current HGNC symbols.

The first release queried all 193 v1.0 top-1% genes and RANGRF. It uses current
HGNC symbols plus aliases/previous symbols, with overly generic aliases
excluded. It retained at most 25 cited Europe PMC records per gene and only
records whose title or returned abstract could be locally re-matched to a query
symbol/alias.

## Required interpretation

An evidence row means only that an Europe PMC article was retrieved using AF and
gene/alias constraints. It is not a relation extraction, a causal claim, a
gene-disease association score, or an AF-GRIP feature. Do not aggregate article
counts into a score. All entries are `unreviewed`; cite and manually verify the
source article before using it as scientific support.
