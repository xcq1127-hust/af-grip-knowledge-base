# AF-GRIP Knowledge Base Skill

Codex skill for evidence-aware queries of versioned AF-GRIP Knowledge Base releases and its
independent Literature Context v1.0.

## Data releases

- Knowledge Base v1.0: https://doi.org/10.5281/zenodo.22089524
- Knowledge Base v1.1: https://doi.org/10.5281/zenodo.22652196 (four-view score and tie-aware GO handling).
- Literature Context v1.0: https://doi.org/10.5281/zenodo.22089535

Download and unpack both archives. Configure their paths before using the
skill:

```bash
export AF_GRIP_RELEASE_DIR=/path/to/AF-GRIP-Knowledge-Base-v1.0
export AF_GRIP_LITERATURE_CONTEXT_DIR=/path/to/AF-GRIP-Literature-Context-v1.0
```

Install this directory as a Codex skill, then ask for a gene query, comparison,
or 10q22 V2G interpretation. The Literature Context is optional and is always
post-ranking and non-scoring.

## Citation

Use the Zenodo version DOI for a fixed release or the corresponding concept DOI
when citing the resource family. See each archive for full provenance and
interpretation boundaries.

## License

CC BY 4.0. See https://creativecommons.org/licenses/by/4.0/.
