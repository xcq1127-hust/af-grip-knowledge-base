# AF-GRIP v1.0 schema reference

The SQLite database is `database/afgrip.sqlite`. It is the primary local query
surface; compressed TSV files are for portable bulk use.

| Table | Key | Purpose |
|---|---|---|
| `gene_scores` | `gene` | Compact ranking and component summary. |
| `gene_evidence` | `gene` | Full frozen feature matrix. |
| `gene_dictionary` | `symbol` | HGNC and Ensembl identifier mapping. |
| `sensitivity_results` | `run_id` | Model/data-source/seed sensitivity records. |
| `external_validation` | `cutoff` | Held-out known-AF-gene enrichment. |
| `locus_genes` | `ensembl_gene_id` | GRCh38 10q22 gene universe. |
| `finemap_variants` | `variant_id` | Published FinnGen FINEMAP records. |
| `v2g_evidence` | no unique single-column key | Allele-harmonized AF-QTL evidence rows. |
| `metadata` | `key` | Version and inference boundaries. |

Useful joins:

```sql
SELECT s.gene, s.genome_wide_rank, d.name, d.location
FROM gene_scores AS s
LEFT JOIN gene_dictionary AS d ON d.symbol = s.gene
WHERE s.gene = 'MYOZ1';

SELECT gene, qtl_type, rsid, nes, p_value_qtl, beta, p_value_gwas, pip
FROM v2g_evidence
WHERE gene IN ('MYOZ1', 'SYNPO2L') AND rsid = 'rs199809516'
ORDER BY gene, qtl_type;
```

The full field dictionary is `schema/AF-GRIP_feature_dictionary.tsv`, and the
machine-readable table schema is `schema/AF-GRIP_schema.json`.

## Independent literature-context database

`/home/xuchengqi/myoz1/releases/AF-GRIP-Literature-Context-v1.0/database/afgrip_literature_context.sqlite`
contains `gene_literature_summary`, `literature_evidence`, `gene_aliases`, and
`metadata`. It is not joined into `afgrip.sqlite` and is never used to alter the
frozen AF-GRIP v1.0 ranking. See [literature_context.md](literature_context.md).
