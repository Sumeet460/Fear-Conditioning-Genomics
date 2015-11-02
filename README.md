Fear-Conditioning-Genomics

TABLE OF CONTENTS

1. ALIGNMENT: STAR
2. RNASEQ ANALYSIS, TRAPSEQ:DIFFERENTIAL EXPRESSION:CUFFDIFF
3. RNASEQ ANALYSIS, TRAPSEQ:DIFFERENTIAL EXPRESSION:DESEQ2
4. RNASEQ ANALYSIS, TRAPSEQ:DIFFERENTIAL SPLICING:DESEQ2/DEXSEQ
5. 5HMC ANALYSIS:PEAK CALLING:MACS
6. PARCE OUT REGIONS OF 5HMC, EXPRESSION, AND ISOFORM REGULATION:BEDTOOLS
7. ANNOTATION ANALYSIS:HOMER
8. MOTIF ANALYSIS:HOMER:MOTIF ID AND LOCALIZATION
9. 5HMC ENRICHMENT ANALYSIS:HOMER:HISTOGRAMS AND SCATTERPLOTS: MOTIFS, PROMOTERS, EXONS, INTRONS, EXONS-INTRONS, CTCF, OTHER GENOMIC REGIONS (CPG ISLANDS, EXONS, LNCRNA, PSEUDOGENES, LOW COMPLEXITY REGIONS, SIMPLE REPEATS, SINES, SATELLITES
5HMC AND ALTERNATIVE SPLICING
10. FAST ANALYSIS: ANNOTATING REGIONS OF THE GENOME BASED ON DISEASE RELEVANT SNP CONTENT
11. Homology ANALYSIS:PHASTCONS


###1. ALIGNMENT: STAR

**WholeGenomeFasta Files:**
- ensembl
	- Mus_musculus.GRCm38.dna.toplevel.fa
- UCSC
	- mm10.fa.align
	- mm9 -> genome.fa

**GTF Files:**
- ensembl
	- Mus_musculus.GRCm38.79.gtf (genes)
- UCSC
	- mm10_UCSC_RepeatMasked.gtf (downloaded from UCSC)
	- 
	

Generate STAR Genome Index using repeat masked gtf file from UCSC, cat with a normal gtf file  - in order to capture the repetive elements in RepBase in the index
```
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --runMode genomeGenerate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --genomeFastaFiles /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Sequence/WholeGenomeFasta/genome.fa --runThreadN 25 --sjdbGTFfile /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10_UCSC_RepeatMasked_Genes.gtf --sjdbOverhang 100 --limitSjdbInsertNsj 5000000
```

Alignment Runs:
Automated - didn't implement this, but would've save a lot of time...
```
for f in `cat files`; do STAR --genomeDir ../STAR/ENSEMBL.homo_sapiens.release-75 \
--readFilesIn fastq/$f\_1.fastq fastq/$f\_2.fastq \
--runThreadN 12 --outFileNamePrefix aligned/$f.; done
```
19_Homecage
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 19_Homecage
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/19_Homecage
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/19_Homecage_ACAGTG_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/19_Homecage/19_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 
```
20_Homecage
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/
mkdir 20_Homecage
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/20_Homecage
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/20_Homecage_GCCAAT_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/20_Homecage/20_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 
```
21_Homecage
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 21_Homecage
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/21_Homecage
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/21_Homecage_CAGATC_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/21_Homecage/21_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 
```
22_Homecage
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 22_Homecage
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/22_Homecage
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/22_Homecage_CTTGTA_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/22_Homecage/22_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 
```
23_Homecage
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 23_Homecage
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/23_Homecage
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/23_Homecage_AGTCAA_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/23_Homecage/23_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 
```
24_Homecage
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 24_Homecage
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/24_Homecage
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/24_Homecage_AGTTCC_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/24_Homecage/24_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```
10_FC-Alone
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 10_FC-Alone
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/10_FC-Alone
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/10_FC-Alone_AGTTCC_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/10_FC-Alone/10_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
 ```
11_FC-Alone
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 11_FC-Alone
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/11_FC-Alone
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/11_FC-Alone_CGATGT_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/11_FC-Alone/11_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```

12_FC-Alone
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 12_FC-Alone
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/12_FC-Alone
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/12_FC-Alone_TGACCA_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/12_FC-Alone/12_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```
9_FC-Alone
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 9_FC-Alone
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/9_FC-Alone
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/9_FC-Alone_AGTCAA_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/9_FC-Alone/9_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
 ```
8_FC-Alone
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 8_FC-Alone
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/8_FC-Alone
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/8_FC-Alone_CTTGTA_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/8_FC-Alone/8_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```

1_IMMO-FC
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker
mkdir 1_IMMO-FC
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/1_IMMO-FC
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/1_IMMO-FC_CGATGT_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/1_IMMO-FC/1_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```
2_IMMO-FC
```
mkdir 2_IMMO-FC
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/2_IMMO-FC
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/2_IMMO-FC_TGACCA_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/2_IMMO-FC/2_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```
3_IMMO-FC
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/
mkdir 3_IMMO-FC
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/3_IMMO-FC
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/3_IMMO-FC_ACAGTG_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/3_IMMO-FC/3_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```
4_IMMO-FC
```
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/
mkdir 4_IMMO-FC
cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/4_IMMO-FC
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/4_IMMO-FC_GCCAAT_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/4_IMMO-FC/4_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
```

###2. RNASEQ ANALYSIS:DIFFERENTIAL EXPRESSION:CUFFDIFF

CUFFDIFF 

IMMO-FC Differential expression analysis: using only repeat masked gtf
```
cuffdiff -o ./AmygHC_vs_AmygIMMO-FC_RepeatMasked_SMALL -L HC,IMMO-FC -p 25 -u /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10_UCSC_RepeatMasked.gtf /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/19_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/20_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/21_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/22_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/23_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/24_Homecage_Aligned.sortedByCoord.out.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/1_IMMO-FC_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/2_IMMO-FC_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/3_IMMO-FC_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/4_IMMO-FC_Aligned.sortedByCoord.out.bam 
```

FC Differential expression analysis: using only repeat masked gtf
```
cuffdiff -o ./AmygFC_vs_AmygHC_RepeatMasked_SMALL -L HC,FC -p 25 -u /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10_UCSC_RepeatMasked.gtf /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/19_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/20_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/21_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/22_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/23_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/24_Homecage_Aligned.sortedByCoord.out.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/10_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/11_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/12_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/8_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/9_FC-Alone_Aligned.sortedByCoord.out.bam &
```

###3. RNASEQ ANALYSIS:DIFFERENTIAL EXPRESSION:DESEQ2

se is the RangedSummarizedExperiment object containing counts and information about the samples. In this example design we control for batch and condition, which should be columns of colData(se)
```{r}
dds <- DESeqDataSet(se, design = ~ batch + condition)
dds <- DESeq(dds)
res <- results(dds, contrast=c("condition","trt","con"))
```

Combine counts from geneCounts function of STAR to generate count matrices:

Read the Reads/Gene table 
```{r}
FC10Alone=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/10_FC-Alone/10_FC-Alone_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

FC11Alone=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/11_FC-Alone/11_FC-Alone_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

FC12Alone=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/12_FC-Alone/12_FC-Alone_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

Homecage19=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/19_Homecage/19_Homecage_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

Homecage20=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/20_Homecage/20_Homecage_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

Homecage21=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/21_Homecage/21_Homecage_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

Homecage22=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/22_Homecage/22_Homecage_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

Homecage23=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/23_Homecage/23_Homecage_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

Homecage24=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/24_Homecage/24_Homecage_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

FC8Alone=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/8_FC-Alone/8_FC-Alone_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

FC9Alone=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/9_FC-Alone/9_FC-Alone_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

HC1r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/1-935-HC/1-935-HC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

FC3r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/3-940-FC/3-940-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")
 
FC5r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/5-938-FC/5-938-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")
  
HC7r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/7-947-HC/7-947-HC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

HC2r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/2-935-HC/2-935-HC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")
  
FC4r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/4-940-FC/4-940-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "") 
 
FC6r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/6-938-FC/6-938-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")  

HC8r=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Amygdala_RNASeq_Replication/STARAlignedBAMs_ens/8-947-HC/8-947-HC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

IMMOFC3=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/3_IMMO-FC/3_IMMO-FC_ReadsPerGene.out.tab", header = F, sep ="\t", fill =T, comment.char = "")

IMMOFC1=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/1_IMMO-FC/1_IMMO-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

IMMOFC2=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/2_IMMO-FC/2_IMMO-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")

IMMOFC4=read.table("/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/4_IMMO-FC/4_IMMO-FC_ReadsPerGene.out.tab", header =F, sep ="\t", fill =T, comment.char = "")
```

Combine column 1, with column 2 from every dataframe, add a header with the name of the sample that column came from
```{r}
HCvsFC_gene_counts = data.frame(FC10Alone$V2, FC11Alone$V2, FC12Alone$V2, Homecage19$V2, Homecage20$V2, Homecage21$V2, Homecage22$V2, Homecage23$V2, Homecage24$V2, FC8Alone$V2, FC9Alone$V2, row.names = Homecage22$V1) 
colnames(HCvsFC_rep_gene_counts) = c("FC10Alone", "FC11Alone", "FC12Alone", "Homecage19", "Homecage20", "Homecage21", "Homecage22", "Homecage23","Homecage24", "FC8Alone", "FC9Alone")

HCvsIMMOFC_gene_counts = data.frame(Homecage19$V2, Homecage20$V2, Homecage21$V2, Homecage22$V2, Homecage23$V2, Homecage24$V2, IMMOFC3$V2, IMMOFC1$V2, IMMOFC2$V2, IMMOFC4$V2, row.names = Homecage22$V1)
colnames(HCvsFC_rep_gene_counts) = c("Homecage19", "Homecage20", "Homecage21", "Homecage22", "Homecage23", "Homecage24", "IMMOFC3", "IMMOFC1", "IMMOFC2", "IMMOFC4")

FCvsIMMOFC_genecounts = data.frame(FC10Alone$V2, FC11Alone$V2, FC12Alone$V2, FC8Alone$V2, FC9Alone$V2, IMMOFC3$V2, IMMOFC1$V2, IMMOFC2$V2, IMMOFC4$V2, row.names = Homecage22$V1)
colnames(HCvsFC_rep_gene_counts) = c("FC10Alone", "FC11Alone", "FC12Alone", "FC8Alone", "FC9Alone", "IMMOFC3", "IMMOFC1", "IMMOFC2", "IMMOFC4")

HCvsFC_ALL = data.frame(HC1r$V2, HC7r$V2, HC2r$V2, HC8r$V2, Homecage19$V2, Homecage20$V2, Homecage21$V2, Homecage22$V2, Homecage23$V2, Homecage24$V2, FC3r$V2, FC5r$V2, FC4r$V2, FC6r$V2, FC10Alone$V2, FC11Alone$V2, FC12Alone$V2, FC8Alone$V2, FC9Alone$V2, row.names = Homecage22$V1)
colnames(HCvsFC_rep_gene_counts) = c("HC1r", "HC7r", "HC2r", "HC8r", "Homecage19", "Homecage20", "Homecage21", "Homecage22", "Homecage23", "Homecage24", "FC3r", "FC5r", "FC4r", "FC6r", "FC10Alone", "FC11Alone", "FC12Alone", "FC8Alone", "FC9Alone")

HCvsFC_rep_gene_counts = data.frame(HC1r$V2, FC3r$V2, FC5r$V2, HC7r$V2, HC2r$V2, FC4r$V2, FC6r$V2, HC8r$V2, row.names = HC1r$V1) 
colnames(HCvsFC_rep_gene_counts) = c("HC1r", "FC3r", "FC5r", "HC7r", "HC2r", "FC4r", "FC6r", "HC8r")
```
Read in data about each sample
```
colData = read.table("/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_RNASeq_DESeq2/colData_HCvsFC_ALL", header =T, sep ="\t")


```

DESeq library:
```
library(DESeq2)
```

se is the RangedSummarizedExperiment object containing counts and information about the samples. In this example design we control for batch and condition, which should be columns of colData(se)
```{r}
dds <- DESeqDataSetFromMatrix(countData = HCvsFC_rep_gene_counts,
                              colData = colData,
                              design = ~ Condition)
```

Relevel so that HomeCage is the reference
```{r}
dds$Condition = relevel(dds$Condition, "HomeCage")
```

DE Analysis:
```{r}
library(BiocParallel)
register(MulticoreParam(20))
dds = DESeq(dds, parallel=TRUE)
res = results(dds)
```
Manipulation of results dataframe
```{r}
resOrdered = res[order(res$padj),]
summary(res)
```

Export Results
```{r}
resOrdered_subset = subset(resOrdered, padj <0.3)
write.csv(as.data.frame(resOrdered), file="HCvsFC_RNASeq_Replicate_Results.csv")
```

PCA
```{r}
pdf("1")
plotPCA(rld, intgroup=c("Condition"))
dev.off()
```


###4. RNASEQ ANALYSIS:DIFFERENTIAL SPLICING:DESEQ2/DEXSEQ & CUFFDIFF


###5. 5HMC ANALYSIS:PEAK CALLING:MACS                                                                                    

MACS + peaksplitter to call subpeaks within complex regions
MACS/FC1
```
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_FC1_5hmC_3 --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC1.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &
```
MACS/FC2
```
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_FC2_5hmC_MACS --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC2.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &
```
MACS/HC1
```
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_HC1_5hmC_MACS --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC1.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &
```
MACS/HC2
```
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_HC2_5hmC_MACS --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC2.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &
```

###6. PARCE OUT REGIONS OF 5HMC, EXPRESSION, AND ISOFORM REGULATION:BEDTOOLS

Calling DhMRs common to both sequencing experiments. Save the sequence JUST where two peaks overlap (no extra peak space unique to one sample or the other). Also create a "Peakheight" file that contains the entries that are overlapped; allows downstream analysis of relative 5hmC changes

Note: needed to remove the header line -- bedtools was unable to recognize it: Chromosome      Start   End     Height  SummitPosition

Common Homecage peaks
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC1_5hmC_MACS/fear_conditioning_HC1_5hmC_MACS_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC2_5hmC_MACS/fear_conditioning_HC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >HC1_HC2_BTIntersect.peaks.subpeaks.bed
```
Common homecage peaks; retain all the fields, so that peak height can be kept for downstream analyses
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC1_5hmC_MACS/fear_conditioning_HC1_5hmC_MACS_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC2_5hmC_MACS/fear_conditioning_HC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -wa -wb -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed
```
Common fear conditioning peaks
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC1_5hmC_MACS/fear_conditioning_FC1_5hmC_3_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC2_5hmC_MACS/fear_conditioning_FC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC1_FC2_BTIntersect.peaks.subpeaks.bed
```
Common fear conditioning peaks; retain all the fields, so that peak height can be kept for downstream analyses
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC1_5hmC_MACS/fear_conditioning_FC1_5hmC_3_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC2_5hmC_MACS/fear_conditioning_FC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -wa -wb -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC1_FC2_BTIntersect.peaks.subpeaks_PeakHeight.bed
```

R Script to take the average of columns 4 and 9 from the HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed file and adds it as a new column to the HC1_HC2_BTIntersect.peaks.subpeaks.bed file (ditto for FC files). End up with a BED file containing DhMRs + the average peak height from both datasets at this locus.
```{r}
FC_peaks = read.table("FC1_FC2_BTIntersect.peaks.subpeaks.bed")
FC_heights = read.table("FC1_FC2_BTIntersect.peaks.subpeaks_PeakHeight.bed")
FC_avg_heights = (FC_heights$V4+FC_heights$V9)/2
FC_peaks$FC_avg_heights = FC_avg_heights
write.table(FC_peaks, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed")

HC_peaks = read.table("HC1_HC2_BTIntersect.peaks.subpeaks.bed")
HC_heights = read.table("HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed")
HC_avg_heights = (HC_heights$V4+HC_heights$V9)/2
HC_peaks$HC_avg_heights = HC_avg_heights
write.table(HC_peaks, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed")
```

FC_5hmC_Inc: Peaks that appear in fear conditioning; no peak at this loci in HC
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -v -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_5hmC_Inc.bed
```
FC_5hmC_Dec: Peaks disappear in fear conditioning; peak at this locus in HC dissapears in FC
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed  -v -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_5hmC_Dec.bed
```
Remove tmp files

```
rm FC1_FC2_BTIntersect.peaks.subpeaks.bed FC1_FC2_BTIntersect.peaks.subpeaks_PeakHeight.bed FC_5hmC_Dec_2.bed HC1_HC2_BTIntersect.peaks.subpeaks.bed FC_5hmC_Inc_2.bed HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed
```

FC_5hmC_common_Inc: Peak is present in both HC and FC samples; but peak is smaller in HC than FC (cutoff = ??)
bedtools/FC_HC_Common.bed

```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_HC_Common.bed
```

bedtools/FC_HC_Common_PeakHeights.bed

```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -bed -sorted -wa -wb -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_HC_Common_PeakHeights.bed
```

Concatenate files (to include intermediate changes in peak height)
 -5hmC increased during FC 
 -5hmC decreased during FC
 -all dynamic 5hmC loci during FC
 
```
cat FC_HC_Common_FCgreaterHC.bed FC_5hmC_Inc.bed > FC_5hmC_Inc_increment.bed
cat FC_HC_Common_HCgreaterFC.bed FC_5hmC_Dec.bed > FC_5hmC_Dec_increment.bed
cat HCvsFC_5hmC_Inc_increment.bed HCvsFC_5hmC_Dec_increment.bed > HCvsFC_AllDynamic5hmC.bed
```

FC greater than HC and FC less than HC, cutoff - peak height difference > or < 25 

```{r}
FC_HC_common_peaks = read.table("FC_HC_Common.bed")
FC_HC_common_heights = read.table("FC_HC_Common_PeakHeights.bed")
FC_HC_common_peaks$FCminusHC <- FC_HC_common_heights$V4 - FC_HC_common_heights$V10
FC_HC_common_peaks$HCminusFC <- FC_HC_common_heights$V10 - FC_HC_common_heights$V4
FC_HC_Common_FCgreaterHC = subset(FC_HC_common_peaks, FCminusHC >=25)
write.table(FC_HC_Common_FCgreaterHC, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_HC_Common_FCgreaterHC.bed")
FC_HC_Common_HCgreaterFC = subset(FC_HC_common_peaks, HCminusFC >=25)
write.table(FC_HC_Common_HCgreaterFC, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_HC_Common_HCgreaterFC.bed")
```

Sorting Bedfiles
```
bedtools sort -chrThenSizeA -i HCvsFC_RNAseq_wholegene_ALL.bed >HCvsFC_RNAseq_wholegene_ALL_sorted.bed
5hmC-seq/all
bedtools sort -chrThenSizeA -i HCvsFC_AllDynamic5hmC.bed >HCvsFC_AllDynamic5hmC_sorted.bed
5hmC-seq/upregulated
bedtools sort -chrThenSizeA -i HCvsFC_5hmC_Inc_increment.bed HCvsFC_5hmC_Inc_increment_sorted.bed
5hmC-seq/downregulated
bedtools sort -chrThenSizeA -i HCvsFC_5hmC_Dec_increment.bed >HCvsFC_5hmC_Dec_increment_sorted.bed
```

MISCELLANEOUS

bedtools/RNAseq_wholegene/5hmC_ALL
-wb option --> write out the entirety of the 5hmC peak that overlaps with a 5hmC region; find any motifs even if they aren't precisely within body of gene
```
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
#bedtools/RNAseq_wholegene/5hmC_Inc
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
#bedtools/RNAseq_wholegene/5hmC_Dec
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
#bedtools/RNAseq_wholegene_upregulated/5hmC_Dec
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
#bedtools/RNAseq_wholegene_downregulated/5hmC_Dec
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
#bedtools/RNAseq_wholegene/5hmC_Dec
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
#bedtools/RNAseq_wholegene/5hmC_Dec
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_ALL_sorted.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_AllDynamic5hmC_sorted.bed -v -bed -wb >HCvsFC_RNAseq_wholegene_5hmC_all.bed
```
```
MISCELLANEOUS IDEAS
 -bedtools/RNAseq_all/5hmCseq_genes_TSS_all
 -bedtools/RNAseq_all/5hmCseq_genes_TSS_all
 -bedtools/RNAseq_all/5hmCseq_genes_TSS_all
 -bedtools/RNAseq_splicing_all/5hmCseq_exons_all
 -bedtools/RNAseq_splicing_all/5hmCseq_introns_all
 -bedtools/RNAseq_splicing_all/5hmCseq_exon-intron_all
 -bedtools/RNAseq_promoters/5hmC_Inc
 -bedtools/RNAseq_promoters/5hmC_Dec
 -bedtools/RNAseq_promoters_upregulated/5hmC_Inc
 -bedtools/RNAseq_promoters_downregulated/5hmC_Dec
 -bedtools/CTCFbindingsites/5hmC_all
 -bedtools/CTCFbindingsites/5hmC_inc
 -bedtools/CTCFbindingsites/5hmC_dec
 -bedtools/CTCFbindingsites/5hmC_Inc/annotation
 -bedtools/lncRNAs_all/5hmc_all
 -bedtools/lncRNAs_/5hmc_all
 -CTCF binding sites with dynamic 5hmC in FC
 -whole genome CTCF binding sites from Corces Lab
 -Enhancers with dynamic 5hmC in FC
 -FANTOM identified enhancers
 -Neural specific enhancers
 -Other Datasets? Hi-seq? TADs or mini-TADs, or CTFCF binding sites influential at these positions..proxy for conformational change

Other interesting coordinate sets to generate
 -RNAseq and lncRNA
 -RNAseq and microRNAs
 -RNAseq and TRAPSeq
 -5hmCseq and enhancers
 -5hmCseq and insulators
```

###7. ANNOTATION ANALYSIS:HOMER
```
 -HCvsFC_5hmCseq_Inc
 -HCvsFC_5hmCseq_Dec
 -HCvsFC_5hmCseq_ALL
 -HCvsFC RNAseq
 -HCvsFC 5hmC & RNAseq overlap
 -HCvsFC TRAPseq: unique and distinct from regular RNAseq
```

ORDER OF ANNOTATION ENRICHMENT CHANGE (HC > FC)
intron>exon>intergenic>LTR~>promoter

Python Script for Homer Annotations
```
import os
import fnmatch
import pybedtools
 
 
InputGeneNames = open('FXTAS.SCA7.WT.common.DhMR.Homer.txt','r')
GeneFileGTF = open('/home/byao/homer/data/accession/mouse2gene.tsv','r')
GeneList = {}
AccessionNumbers = {}
 
for line in GeneFileGTF:
	line = line.strip()
	try:
		RefSeqID, Unknown, HsID, Accession, EnsemblID, Unknown2, GeneID = line.split('\t')
		GeneList[str(RefSeqID)] = GeneID
	except:
		pass
 
 
 
 
for line in InputGeneNames:
    line = line.strip()
    PeakID, Chr, Start, End, Strand, PeakScore, Focus, Ratio_Region, Remainder = line.split('\t',8)
    try:
        Region, Remainder = Ratio_Region.split('(')
        try:
            Accession, Remainder = Remainder.split(',')
            OutputLine = str(Chr) + '\t' + str(Start) + '\t' + str(End) + '\t' + str(Ratio_Region)
            AccessionNumbers[str(Accession)] = OutputLine
        except:
            Accession, Remainder = Remainder.split(')')
            OutputLine = str(Chr) + '\t' + str(Start) + '\t' + str(End) + '\t' + str(Ratio_Region)
            AccessionNumbers[str(Accession)] = OutputLine
    except:
        pass
 
 
for AccessionNumber in AccessionNumbers.keys():
    try:
        print str(AccessionNumbers[str(AccessionNumber)]) + '\t' + str(GeneList[str(AccessionNumber)])
    except:
        print str(AccessionNumber) + ' is missing'


```


###8. MOTIF ANALYSIS:HOMER:MOTIF ID AND LOCALIZATION

*Background* Files are critical - they are how homer decides which TF sites are overrepresented
wholegene - mm10 all genes
exons - mm10 all exons
introns - mm10 all introns
5hmC regions - random sequences from the genome

Testing out different -size parameters (avg size of the DNA fragments in the BED file - input into the findmotifs script)
size 2000
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_2k/ -size 2000 -mis 1 -S 10 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &
```
size 100000
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_100k/ -size 100000 -mis 1 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &
```
size 10000
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_10k/ -size 10000 -mis 1 -S 10 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &
```
size 50000
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_50k/ -size 50000 -mis 1 -S 10 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &
```
*Conclusion about -size parameter:* having a size closer to the real average length of the regions in the bedfile makes for more statistically significant results, but much longer run time. 100k size failed to finish.

Awk script to determine average size of regions in a bed file
```
cat CTCFPeaksClustered.bed | awk -F'\t' 'BEGIN{SUM=0}{ SUM+=$3-$2 }END{print SUM}'
```
```
_Average size of each region in bed file:_
 -RNAseq Introns: 15000 (avg was 8800)
 -RNAseq exons: 3000 (avg 325)
 -CTCFPeaksClustered = 500 (avg 362)
Overshooting may be better than under
```
*Motif Finding:*

_RNAseq, whole gene, upregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_upregulated.bed mm10 HCvsFC_RNAseq_wholegene_upregulated_motifs/ -size 50000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &
```
_RNAseq, whole gene, downregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_downregulated.bed mm10 HCvsFC_RNAseq_wholegene_downregulated_motifs/ -size 50000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &
```
_RNAseq, promoters, all differentially expressed (DE)_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_promoters_ALL.bed mm10 HCvsFC_RNAseq_promoters_motifs/ -size 2000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allpromoters_-2000_background.bed -h &
```
_RNAseq, promoters, upregulated (DE)_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_promoter_upregulated.bed mm10 HCvsFC_RNAseq_promoter_upregulated_motifs/ -size 2000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allpromoters_-2000_background.bed -h &
```
_RNAseq, promoters, downregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_promoters_downregulated.bed mm10 HCvsFC_RNAseq_promoter_downregulated_motifs/ -size 2000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allpromoters_-2000_background.bed -h &
```
_RNAseq, introns, all differentially expressed (DE)_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_introns_ALL.bed mm10 HCvsFC_RNAseq_intron_motifs/ -size 15000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allintrons_background.bed -h &
```
_RNAseq, introns, upregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_introns_upregulated.bed mm10 HCvsFC_RNAseq_intron_upregulated_motifs/ -size 15000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allintrons_background.bed -h &
```
_RNAseq, introns, downregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_introns_downregulated.bed mm10 HCvsFC_RNAseq_intron_downregulated_motifs/ -size 15000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allintrons_background.bed -h &
```
_RNAseq, exons, all DE_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_exons_ALL.bed mm10 HCvsFC_RNAseq_exon_motifs/ -size 3000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allexons_background.bed -h &
```
_RNAseq, exons, upregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_exons_upregulated.bed mm10 HCvsFC_RNAseq_exon_upregulated_motifs/ -size 3000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allexons_background.bed -h &
```
_RNAseq, exons, downregulated_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_exons_downregulated.bed mm10 HCvsFC_RNAseq_exon_downregulated_motifs/ -size 3000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allexons_background.bed -h &
```
_5hmC peaks, all DhMRs_
```_
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HCvsFC_AllDHMR.bed mm10 HCvsFC_5hmC_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &
```
_5hmC peaks, 5hmC Increased_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HCvsFC_5hmC_Inc_increment.bed mm10 HCvsFC_5hmC_Inc_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &
```
_5hmC peaks, 5hmC Decreased_
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HCvsFC_5hmC_Dec_increment.bed mm10 HCvsFC_5hmC_Dec_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &
```
CTCFPeaksClustered: dataset from Victor Corces or all CTCF binding sites genome wide
```
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/CTCFPeaksClustered.bed mm10 CTCFPeaksClustered_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &
```
```
Other Analyses
 -FindMotifs/5hmC-Seq/FC_5hmC_Inc
 -FindMotifs/5hmC-Seq/FC_5hmC_Dec
 -FindMotifs/5hmC-Seq/FC_5hmC_Common_Inc
 -FindMotifs/5hmC-Seq/FC_5hmC_Common_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Dec
 -FindMotifs/5hmC-Seq_Enhancers/5hmC_Enhancers_overlap
 -FindMotifs/5hmC-Seq_CTCF/5hmC_CTCF_overlap
 -FindMotifs/CTCF Clustered Peaks

More Other analyses:
 -FindMotifs/5hmC-Seq/FC_5hmC_Inc
 -FindMotifs/5hmC-Seq/FC_5hmC_Dec
 -FindMotifs/5hmC-Seq/FC_5hmC_Common_Inc
 -FindMotifs/5hmC-Seq/FC_5hmC_Common_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Dec
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Inc
 -FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Dec
 -FindMotifs/5hmC-Seq_Enhancers/5hmC_Enhancers_overlap
 -FindMotifs/5hmC-Seq_CTCF/5hmC_CTCF_overlap
```

###9. 5HMC ENRICHMENT ANALYSIS:HOMER:HISTOGRAMS AND SCATTERPLOTS: MOTIFS, PROMOTERS, EXONS, INTRONS, EXONS-INTRONS, CTCF, OTHER GENOMIC REGIONS (CPG ISLANDS, EXONS, LNCRNA, PSEUDOGENES, LOW COMPLEXITY REGIONS, SIMPLE REPEATS, SINES, SATELLITES

***Workflow***
A) Create tag directories -> containers with all of the bam file reads (from all replicates) in one container - used to pileup all the reads from one condition in order to get a comprehensive picture of the density of reads in particular loci
B) Create a .motif file using seq2profile, or download the .motif file from http://homer.salk.edu/homer/custom.motifs
C) Predict sites where the TF will bind in a set of coordinates using annotatepeaks -m -mbed 
D) Find intensity of epigenetic marks at predetermined set of motif binding sites (as from #2) using annotatepeaks -hist -d to feed in tag directories. Can do this with any predetermined set of coordinates (i.e. RNAseq exon boundaries, promoters, etc)

**A) Create tag directories -> containers with all of the bam file reads (from all replicates) in one container - used to pileup all the reads from one condition in order to get a comprehensive picture of the density of reads in particular loci**
Tag Directories of 5hmC BAM files to calculate enrichment of 5hmC at loci specified in BED format
makeTagDirectory <Output Dir name> [options] <alignment file1> <"2> <etc>

HC
```
makeTagDirectory HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC1.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC2.bam
```
FC
```
makeTagDirectory FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC1.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC2.bam &
```
Input
```
makeTagDirectory Input_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &
```
**B) Create a .motif file using seq2profile, or download the .motif file from http://homer.salk.edu/homer/custom.motifs**

Obtaining .motif files: contain the consensus sequence and the frequency distribution of each nucleotide in the consensue
Two methods:
 -use seq2profile to create .motif file form within Homer
 -download pre-made consensus motif from Homer: http://homer.salk.edu/homer/custom.motifs 
  -may be best option - seq2profile doesn't appear to produce the same profile, as is displayed on URL; also the .motif profiles form seq2profile appear much less complex
  
TFs of interest and their consensus sequences:

Gene | Consensus Motif
--------|------------
HIF-1b	| RTACGTGC
c-Myc |	VVCCACGTGG
n-Myc | VRCCACGTGG
Maz | GGGGGGGG
Stat3 | CTTCCGGGAA
HRE (HSF) | TTCTAGAABNTTCTA
PRDM1/Blimp1 | ACTTTCACTTTC


seq2motif: creating .motif files - least stringent: *2* mismatches allowed
```
seq2profile.pl RTACGTGC 2 HIF-1b > HIF-1b.motif
seq2profile.pl VVCCACGTGG 2 c-Myc > c-Myc.motif
seq2profile.pl VRCCACGTGG 2 n-Myc > n-Myc.motif
seq2profile.pl GGGGGGGG 2 Maz > Maz_2.motif
seq2profile.pl CTTCCGGGAA 2 Stat3 > Stat3.motif
seq2profile.pl TTCTAGAABNTTCTA 2 HRE > HRE.motif
seq2profile.pl ACTTTCACTTTC 2 PRDM1 > PRDM1_2.motif
```
creating .motif files -  medium stringent: *1* mismatches allowed
```
seq2profile.pl ACTTTCACTTTC 1 PRDM1 > PRDM1_1.motif
```
creating .motif files - most stringent: *0* mismatches allowed
```
seq2profile.pl ACTTTCACTTTC 0 PRDM1 > PRDM1_0.motif
```

**C) Predict sites where the TF will bind in a set of coordinates using annotatepeaks -m -mbed**

5hmC decreases/HC unique peaks

HIF-1b
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m HIF-1b.motif -mbed HIF-1b_HC_unique.bed -hist 10 > HIF_motifs_HC_unique.txt
```
c-Myc 
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m c-Myc.motif -mbed c-Myc_HC_unique.bed > c-Myc_motifs_HC_unique.txt &
```
n-Myc
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m n-Myc.motif -mbed n-Myc_HC_unique.bed > n-Myc_motifs_HC_unique.txt &
```
Maz
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m Maz.motif -mbed Maz_HC_unique.bed > Maz_motifs_HC_unique.txt &
```
Stat3
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m Stat3.motif -mbed Stat3_HC_unique.bed > Stat3_motifs_HC_unique.txt &
```

_5hmC increases/FC unique peaks_
HIF-1b
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m HIF-1b.motif -mbed HIF-1b_FC_unique.bed -hist 10 > HIF_motifs_FC_unique.txt
```
c-Myc
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m c-Myc.motif -mbed c-Myc_FC_unique.bed > c-Myc_motifs_FC_unique.txt &
```
n-Myc
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m n-Myc.motif -mbed n-Myc_FC_unique.bed > n-Myc_motifs_FC_unique.txt &
```
Maz
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m Maz.motif -mbed Maz_FC_unique.bed > Maz_motifs_FC_unique.txt &
```
Stat3
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m Stat3.motif -mbed Stat3.motif_FC_unique.bed > Stat3_motifs_FC_unique.txt &
```
All mm10 annotated promoters (UCSC)

Stat3
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_allpromoters_-2000_background.bed mm10 -size 1000 -m Stat3.motif -mbed Stat3.motif_mm10_promoters.bed > Stat3_motifs_mm10_promoters.txt &
```
HRE (control)
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_allpromoters_-2000_background.bed mm10 -size 1000 -m HRE.motif -mbed HRE.motif_mm10_promoters.bed > HRE_motifs_mm10_promoters.txt &
```

RNAseq promoters

Maz - Using different .motif file
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_promoters.bed mm10 -size 1000 -m Maz.motif -mbed Maz_RNAseq_promoters.bed > Maz_motifs_RNAseq_promoters_2.txt &
```
PRDM1
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_promoters.bed mm10 -size 2000 -m PRDM1.motif -mbed PRDM1_RNAseq_promoters.bed > PRDM1_motifs_RNAseq_promoters.txt &
```
RNAseq promoters - upregulated

PRDM1
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_promoters_upregulated.bed mm10 -size 2000 -m PRDM1.motif -mbed PRDM1_RNAseq_promoters_upreg.bed > PRDM1_motifs_RNAseq_promoters_upreg.txt &
```

Concatenate all binding sites in both HC_unique and FC_unique peaks
Alternatively, could've searched for motifs in concatenated HC anf FC peaks first
```
cat HIF-1b_HC_unique.bed HIF-1b_FC_unique.bed > HIF_motifs_all.bed
cat c-Myc_HC_unique.bed c-Myc_FC_unique.bed > c-Myc_motifs_all.bed
cat n-Myc_HC_unique.bed n-Myc_FC_unique.bed > n-Myc_motifs_all.bed
cat Maz_HC_unique.bed Maz_FC_unique.bed > Maz_motifs_all.bed
cat Stat3_motifs_HC_unique.txt Stat3_motifs_FC_unique.txt > Stat3_motifs_all.bed
```

**D) Find intensity of epigenetic marks at predetermined set of motif binding sites (as from #2) using annotatepeaks -hist -d to feed in tag directories. Can do this with any predetermined set of coordinates (i.e. RNAseq exon boundaries, promoters, etc)**

*5hmC distribution comparison:*
*Histograms*

HIF1b binding sites in ALL DhMRs
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/HIF_motifs_all.bed mm10 -size 1000 -m HIF-1b.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > HIF_motifs_hist_HC_FC.txt
```
c-Myc binding sites within ALL DhMRs
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/c-Myc_motifs_all.bed mm10 -size 1000 -m c-Myc.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > c-Myc_motifs_hist_HC_FC.txt &
```
n-Myc binding sites within ALL DhMRs
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/n-Myc_motifs_all.bed mm10 -size 1000 -m n-Myc.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > n-Myc_motifs_hist_HC_FC.txt &
```
Maz binding sites within ALL DhMRs
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/Maz_motifs_all.bed mm10 -size 1000 -m Maz.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > Maz_motifs_hist_HC_FC.txt &
```
Maz binding sites within ALL RNAseq DE promoters
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/Maz_RNAseq_promoters.bed mm10 -size 10 -m Maz.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/  >Maz_motifs_RNAseq_promoters_hist_HC_FC_Input_10Window.txt &
```
Control - all mm10 genes, truncated
```
/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/ mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >trunc_allgenes.bed_hist_HC_FC.txt &
```
Stat3 binding sites within ALL mm10 promoters (UCSC)
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/Stat3.motif_mm10_promoters.bed mm10 -size 1000 -m Stat3.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Stat3_motifs_hist_mm10proms.txt &
```
HRE (HSF) binding sites within all mm10 promoters
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/HRE.motif_mm10_promoters.bed mm10 -size 1000 -m HRE.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >HRE_motifs_hist_mm10proms.txt &
```
PRDM1 binding sites within all mm10 promoters - 2 mismatches
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1_RNAseq_promoters_upreg.bed mm10 -size 1000 -m /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >PRDM1_motifs_hist_mm10proms.txt &
 ```
PRDM1 binding sites within all mm10 promoters - 0 mismatches
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1_RNAseq_promoters_upreg.bed mm10 -size 1000 -m /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1_0.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >PRDM1_0_motifs_hist_mm10proms.txt &
```
DE gene exon-boundaries (+/- 15 bp around boundary)
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_exonboundaries.bed mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >RNAseq_exon_boundaries_hist_HC_FC.txt &
```
Upregulated Promoters DE promoters
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_upregulatedgenes_promoters.bed mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >RNAseq_upregulated_promoters_hist_HC_FC_Input.txt &
```
Downregulated Promoters DE promoters
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_downregulatedgenes_promoters.bed mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >RNAseq_downregulated_promoters_hist_HC_FC_Input.txt &
```
All DE gene promoters
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_promoters.bed mm10 -size 1000 -log -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_promoters_ComprehensiveComparison 
```
DE gene exons
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_exons.bed mm10 -size 1000 -fragLength 150 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/  >Homer_FearConditioning_5hmC_FC_RNAseq_exons_ComprehensiveComparison_2 &
```
DE gene introns
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_introns.bed mm10 -size 1000 -p 10 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_introns_ComprehensiveComparison_2 &
```
CTCF binding sites
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/CTCFPeaksClustered.bed mm10 -size 1000 -fragLength 150 -annStats Homer_CTCFPeaksClustered_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_CTCFPeaksClustered_ComprehensiveComparison_2 & 
```
lncRNAs
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_lncRNA.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_lncRNA_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_lncRNA_ComprehensiveComparison_2 &
```
enhancers: cell based
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_primarycell_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_primarycell_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_primarycell_based_ComprehensiveComparison_2 &
```
enhancers: tissue based
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_tissue_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_tissue_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_tissue_based_ComprehensiveComparison_2 &
```
exon boundaries: +/- 50 bp from exons
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_exonboundaries.bed mm10 -size 1000 -fragLength 150 -annStats Homer_mm10_exonboundaries_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_mm10_exonboundaries_ComprehensiveComparison_2 &
```

*Scatterplots*

HC vs HC, FC, HC_unique, FC_unique
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC.bed mm10 -size 1000 -fragLength 150 -annStats Homer_HC_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_HC_ComprehensiveComparison &
```
FC vs HC, FC, HC_unique, FC_unique
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_FC_ComprehensiveComparison &
```
HC_unique vs HC, FC, HC_unique, FC_unique
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -fragLength 150 -annStats Homer_HC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_HC_unique_ComprehensiveComparison &
```
FC_unique vs HC, FC, HC_unique, FC_unique
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_FC_unique_ComprehensiveComparison &
```
RNAseq promoters vs 
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_promoters.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_promoters_ComprehensiveComparison_2 &
```
RNAseq exons
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_exons.bed mm10 -size 1000 -fragLength 150 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/  >Homer_FearConditioning_5hmC_FC_RNAseq_exons_ComprehensiveComparison_2 &
```
RNAseq introns
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_introns.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_introns_ComprehensiveComparison_2 &
```
CTCF binding sites
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/CTCFPeaksClustered.bed mm10 -size 1000 -fragLength 150 -annStats Homer_CTCFPeaksClustered_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_CTCFPeaksClustered_ComprehensiveComparison_2 & 
```
lncRNAs
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_lncRNA.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_lncRNA_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_lncRNA_ComprehensiveComparison_2 &
```
enhancers: cell based
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_primarycell_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_primarycell_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_primarycell_based_ComprehensiveComparison_2 &
```
enhancers: tissue based
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_tissue_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_tissue_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_tissue_based_ComprehensiveComparison_2 &
```
exon boundaries: +/- 50 bp from exons
```
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_exonboundaries.bed mm10 -size 1000 -fragLength 150 -annStats Homer_mm10_exonboundaries_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_mm10_exonboundaries_ComprehensiveComparison_2 &
```
###10. FAST ANALYSIS: ANNOTATING REGIONS OF THE GENOME BASED ON DISEASE RELEVANT SNP CONTENT
###11. Homology ANALYSIS:PHASTCONS





