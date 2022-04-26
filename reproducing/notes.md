- Get Cell Ranger

original analysis was done with Cell Ranger 2.2.0

Current is Cell Ranger 6.1.2

Mouse reference dataset

10X_41 a.k.a. brain1_str: <https://www.ncbi.nlm.nih.gov/sra?term=SRX8627776>

Create a directory

    mkdir reproducing
    cd reproducing

Create a Conda environment

    mamba create -n trex sra-tools
    conda activate trex

Download FASTQ

    mkdir fastq
    cd fastq
    fastq-dump --gzip --split-3 --defline-qual '+' --defline-seq '@$ac.$sn' SRR12103475 SRR12103476
    mv SRR12103475_1.fastq.gz SRR12103475_S1_L001_R1_001.fastq.gz
    mv SRR12103475_2.fastq.gz SRR12103475_S1_L001_R2_001.fastq.gz
    mv SRR12103476_1.fastq.gz SRR12103476_S1_L001_R1_001.fastq.gz
    mv SRR12103476_2.fastq.gz SRR12103476_S1_L001_R2_001.fastq.gz
    cd ..

Make custom reference (using 10X refdata for GRCm38/mm10). `mkref` takes approx. 1 hour.

    cat refdata-gex-mm10-2020-A/fasta/genome.fa ../references/chrH2B-EGFP-N.fa > mm10_H2B-EGFP-30N_genome.fa
    cat refdata-gex-mm10-2020-A/genes/genes.gtf ../references/chrH2B-EGFP-N.gtf > mm10_H2B-EGFP-30N_genes.gtf
    cellranger mkref --genome=mm10_H2B-EGFP-30N --fasta=mm10_H2B-EGFP-30N_genome.fa --genes=mm10_H2B-EGFP-30N_genes.gtf > mkref_mm10_H2B-EGFP-30N.out

Run `cellranger count`

    cellranger count --transcriptome=mm10_H2B-EGFP-30N --id=brain1_str --fastqs=fastq/ --sample=SRR12103475 --sample=SRR12103476 --expect-cells=2299


    /sw/apps/bioinfo/Chromium-cellranger/2.2.0/rackham/cellranger-cs/2.2.0/bin/count --id=10x_41_LV516_P11_striatum_transcriptome_NovaSeq_expect-cells-2299 --transcriptome=/proj/uppstore2018019/private/references/mm10_H2B-EGFP-30N/mm10_H2B-EGFP-30N --fastqs=/proj/uppstore2018019/private/raw/P11703/P11703_1003/02-FASTQ/181025_A00187_0084_AH3J5JDRXX --sample=P11703_1003 --expect-cells=2299
