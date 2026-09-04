# Literature & Research Log

## 2026-09-03 — Day 1

### Research question status
Which genes and proteins are shared between the axolotl limb blastema, the regenerative (distal) mouse digit tip, and the regenerative phases of human fingertip healing?
Which of these conserved candidates are absent, downregulated, or delayed in the non-regenerative mouse digit tip (proximal amputation), representing a plausible molecular barrier to regeneration?
Which biological pathways and interaction hubs emerge from this conserved gene set, and how do they relate to known regeneration mechanisms (ECM remodeling, immune modulation, blastema formation)?

### Papers reviewed
- Gerber et al. 2018 (Science) — [status: saved]
- Qu et al. 2020 (FASEB J) — [status: saved]
- Schultz et al. 2025 (npj Regen Med, published Nov 2025; supersedes the medRxiv preprint cited in the synopsis) — [status: saved]

### Dataset access confirmation
- GSE106269: confirmed; supplementary processed tables and raw-data SRA links are available.
- GSE131078: confirmed; processed gene counts file and SRA raw-data links are available.
- Schultz et al. supplementary tables: confirmed; supplementary PDF and multiple Excel/XLS datasets are available.
- GSE241132 (optional): confirmed; supplementary cell metadata and RAW archive are listed.

### Environment status
- Conda env `axolotl_regen` created, scanpy/pandas/numpy/matplotlib/seaborn/jupyterlab installed and tested: yes
- Google Colab tested: yes
- GitHub repo created and pushed: yes

### Notes / questions for supervisor
- All four planned dataset sources are currently accessible.
- No datasets have been downloaded yet.
- Need to confirm the final operational definitions of regeneration-competent versus scar-forming tissues before analysis begins.
- Need to confirm the exact research questions and cross-species comparison strategy with the supervisor before proceeding too far into analysis.

### Tomorrow's plan
- Begin deep-reading Gerber et al. 2018 in full.
- Take structured notes on: what defines a blastema cell state, marker genes used, major cell populations/clusters, experimental design, and analytical methods.
- Record any methodological details that will be needed when processing the Axolotl dataset in Week 3.
- 
## 2026-09-05 — Day 2

### Deep-read: Gerber et al. 2018 (Science) — Axolotl regeneration

#### 1. Sample/tissue preparation
- The study used FACS-sorted connective-tissue (CT) cells from the upper arm using the Prrx1:Cre-ER;Caggs:lp-Cherry mCherry reporter line.
- Multiple experimental subsets contributed to GSE106269:
  - Whole-limb reference atlas: uninjured adult upper arm, 2,379 cells, 10x Genomics.
  - Dense CT time course: 0, 3, 5, 8, 11, and 18 dpa plus regenerate samples using Fluidigm C1.
  - Late-blastema trajectory: 18, 25, and 38 dpa using 10x Genomics and Prrx1 lineage cells.
  - Col1a2-lineage subset: uninjured and 11 dpa blastema cells using SmartSeq2.
  - Developmental comparison: limb field/bud stages 28, 40, and 44 using Fluidigm C1.
- Important: the 0 dpa and later-stage comparisons involve different experimental subsets/lineage strategies, so GSM-level sample metadata must be checked before analysis.

#### 2. Sequencing platform and cell numbers
- The study used multiple platforms rather than a single sequencing technology:
  - 10x Genomics Chromium for the whole-limb atlas and 18/25/38 dpa trajectory.
  - Fluidigm C1 for the dense CT time course and developmental samples.
  - SmartSeq2 for the Col1a2-lineage subset.
- The reported datasets therefore differ in throughput and sequencing characteristics.

#### 3. QC thresholds and axolotl transcriptome handling
- Cells with fewer than 1,000 detected genes were excluded.
- Ribosomal protein genes were excluded from all datasets.
- For limb-bud samples, cells with zero Prrx1 expression (TPM = 0) were removed as an identity check.
- No mitochondrial-percentage cutoff was reported.
- Reads were mapped/pseudoaligned against a curated axolotl transcriptome rather than relying directly on the poorly annotated axolotl genome.
- Kallisto and STAR/Cell Ranger approaches were used, and isoform-level counts were summed to gene level using custom scripts.
- This transcriptome-first strategy is important because axolotl genome annotation is incomplete.

#### 4. Clustering approach
- PCA was used for dimensionality reduction, followed by t-SNE for visualization.
- Seurat was used for clustering and differential expression.
- The paper does not explicitly report a Louvain/Leiden resolution parameter.
- The core uninjured CT dataset produced eight clusters, including tenocytes, periskeletal cells, cycling cells, and five fibroblast subtypes (fCT I–V).
- Additional trajectory analyses used diffusion maps, MERLoT, and SPRING.

#### 5. Cell-type/cluster markers
- Whole-limb atlas markers included:
  - Hbb — erythrocytes
  - Krt5 — epidermal cells
  - Myl10 — muscle
  - Mmrn1 — endothelial cells
  - Lyz/Apoe/Gzmb/Trac — immune populations
  - Pbx3/Igfbp3/Mfap5/Tnmd/Col3a1/Lum/Dpt — CT populations/subtypes
- CT subtype markers included:
  - Tnmd — tenocytes
  - Col8a2 — periskeletal cells
  - Ccnb1 — cycling cells
  - Twist2/Ptgds — fCT IV/V dermal fibroblasts
  - Cnmd/Lect1 and Otos — skeletal populations
- Prrx1 was used as a pan-CT lineage marker.
- The blastema/progenitor state showed an embryonic-limb/cell-cycle-associated signature including Prdx2, Nrep, and Ccnb1.

#### 6. Convergence finding
- Uninjured connective tissue contains transcriptionally distinct CT populations.
- Following amputation, these differentiated populations lose many of their distinguishing identity and extracellular-matrix signatures and converge toward a shared progenitor-like transcriptional state resembling an embryonic limb-bud state.
- The cells subsequently re-diversify into different CT populations during later regeneration.
- The study therefore supports active dedifferentiation of differentiated cells rather than regeneration being driven exclusively by a pre-existing dedicated stem-cell population.
- Relevance to RQ3: blastema formation should be investigated as a transient change in cellular identity/heterogeneity followed by later re-diversification.

#### 7. Data/code availability
- Single-cell count matrices and FASTQ files are deposited in NCBI GEO under accession GSE106269.
- Supplementary methods, figures, and tables are provided with the paper.
- GSE106269 will therefore be the primary data source for reproducing the axolotl single-cell analysis.

### Key takeaway for FYP
Gerber et al. provides the main axolotl reference for identifying regeneration-associated CT states. The most important concept for the current project is the transition from heterogeneous differentiated CT populations toward a shared progenitor-like blastema state followed by re-diversification.

### Deep-read: Qu et al. 2020 (FASEB Journal) — Mouse digit regeneration

#### 1. Amputation levels and terminology
- The paper uses the labels **Regen (Regenerative)** and **Non-Regen (Non-Regenerative)**.
- Regen: distal amputation, with approximately 20–30% of P3 (terminal phalanx) length removed.
- Non-Regen: proximal amputation, with approximately 60–70% of P3 length removed.
- The unamputated digit 3 on the same limb was used as a control for imaging/histology but was not sequenced.
- These Regen/Non-Regen labels should be retained when organizing the mouse RNA-seq metadata.

#### 2. Sequenced timepoints
- Bulk RNA-seq was performed at 12, 14, and 21 days post-amputation (DPA).
- These timepoints represent different stages of the regenerative/healing response:
  - 12 DPA — blastema formation
  - 14 DPA — blastema differentiation
  - 21 DPA — early bone regrowth
- The imaging/histology experiments included additional timepoints (10, 12, 14, 21, 28, and 56 DPA), but these additional points were not part of the RNA-seq experiment.

#### 3. Replicate structure
- There were **4 biological replicates per condition per timepoint**.
- Conditions: Regen and Non-Regen.
- Timepoints: 12, 14, and 21 DPA.
- Total bulk RNA-seq samples: **24**.
- The replicates were independent animals rather than technical replicates.

#### 4. Differential expression method
- DESeq2 was used for differential expression analysis.
- The study applied upstream batch-correction procedures before DESeq2.
- Importantly, the main comparisons were **between timepoints within each condition**, rather than direct Regen-vs-Non-Regen comparisons at each matched timepoint.
- Comparisons included 12 vs 14, 14 vs 21, and 12 vs 21 DPA separately for Regen and Non-Regen.
- Reported significance criteria included >2-fold expression change, >1 CPM, and Benjamini-adjusted P < 0.05.
- The analysis identified 1,513 DEGs in Regen and 1,400 DEGs in Non-Regen.
- DEGs were k-means clustered into four temporal expression patterns: Upregulation, Transient Upregulation, Transient Downregulation, and Downregulation.
- GO enrichment was performed using DAVID v6.8 with Benjamini P < 0.05.

#### 5. Key genes and pathways
- Regenerative responses were associated with transient upregulation of pathways involving:
  - skeletal system development
  - embryonic skeletal system morphogenesis
  - limb morphogenesis
  - embryonic digit morphogenesis
  - collagen fibril organization
  - ECM organization
  - osteoblast differentiation
  - ossification
  - angiogenesis
  - nervous system development
- Important genes highlighted include:
  - Col3a1 — wound healing/ECM and reduced-scarring response
  - Mmp2 — extracellular-matrix remodeling
  - Hoxa11/Hoxd11 — distal limb patterning
  - Hoxa13/Hoxd13 — digit identity
  - Prrx1, Alx4, Tbx2 — limb-patterning transcription factors
  - Runx2 — osteoblast differentiation
  - Foxf2, Runx3, Sox17 — additional regeneration-associated regulatory genes
- Non-Regen/pro-fibrotic responses included Fos-family genes (Fos, Fosb, Fosl1) and elevated H19.

#### 6. Data availability
- RNA-seq data are deposited in NCBI GEO under accession **GSE131078**.
- The paper states that other relevant data are available on request.
- The paper does not clearly establish from the text alone whether GEO provides only raw sequencing data or also the fully processed/batch-corrected count matrices.
- Therefore, the actual GSE131078 supplementary files should be inspected before deciding whether preprocessing can be skipped in the Week 6 analysis.

### Key methodological takeaway for FYP
Qu et al. provides the mouse regenerative-versus-non-regenerative experimental framework and identifies important temporal regeneration-associated genes and pathways. However, their principal DE analysis was a **within-condition time-course analysis**, not a direct matched-timepoint Regen-versus-Non-Regen DESeq2 comparison.

For the FYP, this distinction must be retained. A direct Regen-vs-Non-Regen comparison may be appropriate for our research question, but it would represent a reanalysis/design different from the primary DE analysis reported by Qu et al.

- ## Qu et al. primarily performed within-condition temporal DE analysis rather than direct matched-timepoint Regen-vs-Non-Regen DE analysis. Need to confirm the final DESeq2 design for the FYP and whether direct condition comparisons, time effects, or an interaction model should be used.

- ### Skim: Schultz et al. 2025 (npj Regenerative Medicine) — Human fingertip regeneration

#### 1. Clinical phases
The paper defines four clinical phases of human fingertip regeneration:
1. Coagulation
2. Hypergranulation
3. Proliferation
4. Epithelialization

The phases were assigned using clinical observations and then supported by distinct wound-fluid proteomic signatures.

#### 2. Differentially expressed protein data
- The study analyzed wound-fluid proteomes from 36 samples:
  - Coagulation: 7
  - Hypergranulation: 8
  - Proliferation: 12
  - Epithelialization: 9
- 974 reviewed proteins were detected in at least one sample.
- 60 proteins were identified as differentially expressed between two or more regeneration phases.
- The main DEP statistical dataset is **Supplementary Data 4 — Statistics 60 Proteins**.
- Supplementary Data 1 contains total spectral counts, while Supplementary Data 3 contains the five major protein clusters.
- Supplementary Data 5–11 contain network, pathway, enrichment, upstream-regulator, and GSEA analyses.

#### 3. Divergence from regenerative animal models
Human fingertip regeneration shares features with regenerative animal models, including proliferation, immune activity, ECM remodeling, and oxidative-stress regulation. However, the timing and organization differ: in animal models, epithelialization/wound closure occurs relatively early, whereas in human fingertip regeneration mature epithelialization occurs as the final phase while substantial tissue remodeling continues. A definitive human blastema or apical epithelial cap has also not yet been demonstrated.

### Key takeaway for FYP
The human dataset should not be treated as a direct transcriptomic equivalent of the axolotl and mouse datasets. Schultz et al. provides a longitudinal **proteomic** view of human fingertip regeneration, so cross-species comparisons will need to focus on conserved genes/proteins and biological pathways rather than assuming identical cell states or regeneration timelines.

### GEO metadata recon (GEOparse)

- **GSE106269:** 1,604 GSM sample records. Metadata revealed multiple experimental subsets and scRNA-seq methods, including Fluidigm C1, SmartSeq2, and 10X Genomics. Series-level supplementary files include `GSE106269_RAW.tar` and supplementary tables `GSE106269_Table_S3.csv.gz`, `GSE106269_Table_S5.csv.gz`, `GSE106269_Table_S7.csv.gz`, `GSE106269_Table_S8.csv.gz`, and `GSE106269_Table_S9.csv.gz`. Individual 10X samples also have sample-level barcode files.

- **GSE131078:** 24 GSM sample records. The dataset contains 12 Regen and 12 Non-Regen samples across 12, 14, and 21 DPA, with 4 biological replicates per condition/timepoint. GEO provides the processed gene-count matrix `GSE131078_gene_counts.txt.gz`. No individual sample supplementary files were listed.

### Notes / open questions for supervisor

- Confirm the final statistical design for the mouse analysis. Qu et al. primarily performed within-condition temporal DE analysis rather than direct matched-timepoint Regen-vs-Non-Regen DE analysis. Need to confirm whether the FYP should use direct condition comparisons, time effects, or a condition × time interaction model.
- Confirm which specific GSE106269 axolotl subset(s) should be used for the principal regeneration analysis rather than attempting to analyze all 1,604 GSM records together.
- Determine the appropriate strategy for cross-species gene/protein mapping between axolotl, mouse, and human.
- Confirm how the human proteomic candidates will be integrated with the axolotl and mouse transcriptomic candidates.
- Verify the exact structure and gene identifiers of the mouse processed count matrix before downstream analysis.

### Tomorrow's plan

- Refine the 3 research questions in light of today's reading.
- Begin the annotated bibliography deliverable required by Phase 1 (Weeks 1–2) of the synopsis.