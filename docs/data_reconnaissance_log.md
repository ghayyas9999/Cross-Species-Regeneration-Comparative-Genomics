
## Day 5 — Human Fingertip Proteomics Verification

### Schultz et al. (2025) supplementary data

**Supplementary Data 1**
File: `data/human_fingertip/41536_2025_441_MOESM2_ESM.xlsx`

- Sheet: `Supplementary Data 1_Total Spec`
- Spreadsheet dimensions: 1648 rows × 45 columns
- Publisher header indicates 2064 identified proteins
- 36 measurement/sample columns
- Four healing phases represented:
  - Proliferation: 7 samples
  - Epithelialization: 10 samples
  - Coagulation: 12 samples
  - Hypergranulation: 7 samples
- Metadata include UniProt accession numbers, alternate IDs/gene symbols, molecular weight, protein grouping/ambiguity, taxonomy, phase, and patient/sample information.
- The source spreadsheet contains the spelling `Hypergrnaulation`; this will be standardized to `Hypergranulation` only in derived analysis tables. The original file will remain unchanged.

**Supplementary Data 4**
File: `data/human_fingertip/41536_2025_441_MOESM5_ESM.xlsx`

- Sheet: `60 Signif DEPs`
- Dimensions: 60 rows × 12 columns
- 60 unique gene symbols
- 0 missing gene values
- 0 duplicate gene symbols
- Six pairwise healing-phase comparisons are provided.
- Number of significant proteins:
  - Coagulation vs Hypergranulation: 19
  - Coagulation vs Proliferation: 34
  - Coagulation vs Epithelialization: 48
  - Hypergranulation vs Proliferation: 30
  - Hypergranulation vs Epithelialization: 44
  - Proliferation vs Epithelialization: 33

The 60 proteins are phase-associated/significantly different proteins and should not automatically be described as regeneration-specific genes.


## Day 5 — Human Fingertip GSEA and Supplementary PDF Verification

**Supplementary Data 11**
File: `data/human_fingertip/41536_2025_441_MOESM12_ESM.xlsx`

- Contains 3 GSEA ranked gene-list sheets.
- `GSEA_ranked_gene_list_CvsH_1741`: 970 rows × 3 columns.
- `GSEA_ranked_gene_list_HvsP_1741`: 970 rows × 3 columns.
- `GSEA_ranked_gene_list_PvE_17410`: 970 rows × 3 columns.
- Columns are `NAME`, `TITLE`, and `SCORE`.
- `NAME` contains human HGNC gene symbols.
- `TITLE` contains gene/protein descriptions.
- `SCORE` provides the ranking statistic used for the GSEA ranked lists.

**Supplementary PDF**
File: `data/human_fingertip/41536_2025_441_MOESM1_ESM.pdf`

- 14 pages; successfully converted to text.
- Supplementary Fig. 4 reports average total spectrum counts per phase for selected proteins.
- Supplementary Fig. 5 reports GSEA hallmark terms for Coagulation, Hypergranulation, Proliferation, and Epithelialization.
- GSEA comparisons are described as each phase compared with the other regenerative phases.
- Reported GSEA results include pathway-level FWER p-values.
- The supplementary material confirms that the four phases are treated as regenerative/healing phases in the published analysis.
- Human GSEA results will be used as pathway/contextual evidence rather than treating all ranked genes as regeneration-specific genes.
