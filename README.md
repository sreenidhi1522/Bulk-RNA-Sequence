# Bulk-RNA-Sequence
This project demonstrates a complete Bulk RNA-Seq analysis workflow, from raw SRA files to identification of differentially expressed genes (DEGs). Steps include preprocessing, QC, alignment, quantification, normalization, statistical testing, and visualization.
This dataset is designed to investigate how hypoxia (low oxygen conditions) alters gene expression in two distinct prostate cancer cell lines, each derived from different metastatic sites:
LNCaP (Lymph Node Carcinoma of the Prostate)
Origin: Derived from a lymph node metastasis of a human prostate adenocarcinoma.
Clinical relevance: Represents androgen-sensitive prostate cancer.
PC3 (Prostate Cancer 3)
Origin: Derived from a bone metastasis of a grade IV prostate adenocarcinoma.
Clinical relevance: Represents androgen-independent, more aggressive prostate cancer.
📌 Objectives
Learn the basic workflow of Bulk RNA-Seq analysis.
Perform quality control, alignment, quantification, and differential expression analysis (DEA).
Visualize results using plots such as PCA, heatmaps, volcano plots, and pathway enrichment analysis of DEGs.
Document the workflow in a clear and reproducible way for beginners.
📂 Dataset Information
Dataset Source: GSE106305, NCBI GEO
Reference: Guo et al., Nature Communications, 2019
This dataset investigates how hypoxia (low oxygen conditions) affects gene expression in two prostate cancer cell lines:
LNCaP (androgen-sensitive, derived from a lymph node metastasis)
PC3 (androgen-independent, derived from a bone metastasis)
Raw Data Organization
Each biological replicate is associated with a GSM ID in GEO.
However, raw sequencing data is stored in multiple technical runs (SRR IDs) in the SRA database.
Example:
LNCaP Normoxia Replicate 1 (GSM3145509) → split into 4 SRR files:
SRR7179504, SRR7179505, SRR7179506, SRR7179507
This approach is common in sequencing, where one sample is run across multiple lanes to improve sequencing depth and reduce bias.

Data Downloaded

Total SRR files downloaded: 20

These correspond to all replicates under Normoxia and Hypoxia conditions for both LNCaP and PC3 cell lines.
'''SRR7179504, SRR7179505, SRR7179506, SRR7179507
SRR7179508, SRR7179509, SRR7179510, SRR7179511
SRR7179520, SRR7179521, SRR7179522, SRR7179523
SRR7179524, SRR7179525, SRR7179526, SRR7179527
SRR7179536, SRR7179537, SRR7179540, SRR7179541'''
⚙️ Step 1: Data Download & Conversion
Installed SRA Toolkit
Installed using sudo apt install sra-toolkit.
Toolkit provides prefetch (for downloading .sra files) and fastq-dump (for converting to FASTQ).
Downloaded raw .sra files
Used prefetch with SRR IDs (e.g., prefetch SRR7179504).
.sra files were saved into the default SRA directory.
Converted .sra → .fastq.gz
Used fastq-dump with appropriate flags:

'''fastq-dump --outdir fastq --gzip --skip-technical --readids \
  --read-filter pass --dumpbase --split-3 --clip SRR7179504.sra'''

Output: compressed FASTQ files.

Options ensured removal of technical sequences, proper read IDs, and trimming of adaptors/clipped bases.
Although the dataset was single-end, we kept --split-3 to handle any paired-end cases consistently.
Automated downloads for multiple SRRs
Wrote a small Python script to loop over SRR IDs, download .sra files, and convert them into .fastq.gz.
This generated 20 FASTQ files (corresponding to 20 SRR runs).
<img width="1226" height="144" alt="image" src="https://github.com/user-attachments/assets/30fbf8f5-27df-40c8-9a09-ac250fb0102d" />
