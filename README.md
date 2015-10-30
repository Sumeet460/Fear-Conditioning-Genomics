Fear-Conditioning-Genomics

TABLE OF CONTENTS

1. ALIGNMENT: STAR
2. 5HMC ANALYSIS:PEAK CALLING:MACS
3. RNASEQ ANALYSIS:DIFFERENTIAL EXPRESSION:CUFFDIFF
4. RNASEQ ANALYSIS:DIFFERENTIAL EXPRESSION:DESEQ2
5. RNASEQ ANALYSIS:DIFFERENTIAL SPLICING:CUFFDIFF
6. RNASEQ ANALYSIS:DIFFERENTIAL SPLICING:DESEQ2/DEXSEQ
7. PARCE OUT REGIONS OF 5HMC, EXPRESSION, AND ISOFORM REGULATION:BEDTOOLS
8. ANNOTATION ANALYSIS:HOMER
9. MOTIF ANALYSIS:HOMER:MOTIF ID AND LOCALIZATION
10. 5HMC ENRICHMENT ANALYSIS:HOMER:HISTOGRAMS AND SCATTERPLOTS: MOTIFS, PROMOTERS, EXONS, INTRONS, EXONS-INTRONS, CTCF, OTHER GENOMIC REGIONS (CPG ISLANDS, EXONS, LNCRNA, PSEUDOGENES, LOW COMPLEXITY REGIONS, SIMPLE REPEATS, SINES, SATELLITES
5HMC AND ALTERNATIVE SPLICING
11. FAST ANALYSIS: ANNOTATING REGIONS OF THE GENOME BASED ON DISEASE RELEVANT SNP CONTENT
12. Homology ANALYSIS:PHASTCONS, ETC


# Links:
http://homer.salk.edu/homer/ngs/annotation.html
http://homer.salk.edu/homer/ngs/quantification.html
http://homer.salk.edu/homer/ngs/peakMotifs.html
http://homer.salk.edu/homer/motif/creatingCustomMotifs.html
http://liulab.dfci.harvard.edu/MACS/00README.html

###STAR Genome Alignment

##RepeatMasker Database Creation
#Approach: download repeat masked genome fasta; download genomic repeat element annotation file. Run alignment on the repeat masked fasta, so that repeat elements are captured. 
#links: http://www.repeatmasker.org/; http://www.repeatmasker.org/genomicDatasets/RMGenomicDatasets.html


##convert .out file to .gtf file
```
write.table(tbl, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10.fa.out_2")


awk '{ print $5"\t""repbase""\t"$11"\t"$6"\t"$7"\t"$1"\t"$9 }' mm10.fa.out_2 >mm10.fa.out.gtf
```
chr = 5
source = repbase
feature = 11
str = 6
stp = 7
score =1 
attribute = 9



###Analyzing repetitive genomic sequyences: genome fasta file - does it contain all the repetitive sequences or is it altered somehow to represent these difficult to map regions as N? gtf file - need one that represents all these elements. BAM alignments - need to feed them a gtf file with complete set of annotations for best alignment




##Generate STAR Genome Indices from RepeatMasked Genome fasta and annotation file
#Can't be done. Fasta file is in a weird fucking format. Appears to be designed for primer design really..
/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --runMode genomeGenerate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --genomeFastaFiles /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/mm10.fa.align --runThreadN 25 --sjdbGTFfile /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10.fa.out.gtf --sjdbOverhang 49

#Generate STAR Indices using repeat masked gtf file from UCSC, cat with a normal gtf file --> see what happens

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --runMode genomeGenerate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --genomeFastaFiles /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Sequence/WholeGenomeFasta/genome.fa --runThreadN 25 --sjdbGTFfile /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10_UCSC_RepeatMasked_Genes.gtf --sjdbOverhang 100 --limitSjdbInsertNsj 5000000

#Alignment Run:

for f in `cat files`; do STAR --genomeDir ../STAR/ENSEMBL.homo_sapiens.release-75 \
--readFilesIn fastq/$f\_1.fastq fastq/$f\_2.fastq \
--runThreadN 12 --outFileNamePrefix aligned/$f.; done


#!/usr/bin/bash

#19_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 19_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/19_Homecage

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/19_Homecage_ACAGTG_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/19_Homecage/19_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 

#20_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/

mkdir 20_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/20_Homecage

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/20_Homecage_GCCAAT_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/20_Homecage/20_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 

#21_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 21_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/21_Homecage

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/21_Homecage_CAGATC_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/21_Homecage/21_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 

#22_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 22_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/22_Homecage

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/22_Homecage_CTTGTA_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/22_Homecage/22_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 

#23_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 23_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/23_Homecage


/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/23_Homecage_AGTCAA_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/23_Homecage/23_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts 

#24_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 24_Homecage

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/24_Homecage

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/24_Homecage_AGTTCC_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/24_Homecage/24_Homecage_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts

#10_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 10_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/10_FC-Alone

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/10_FC-Alone_AGTTCC_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/10_FC-Alone/10_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
 
#11_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 11_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/11_FC-Alone

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/11_FC-Alone_CGATGT_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/11_FC-Alone/11_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts


#12_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 12_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/12_FC-Alone

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/12_FC-Alone_TGACCA_L002_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/12_FC-Alone/12_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts

#9_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 9_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/9_FC-Alone

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/9_FC-Alone_AGTCAA_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/9_FC-Alone/9_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts
 
#8_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker

mkdir 8_FC-Alone

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/8_FC-Alone

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/8_FC-Alone_CTTGTA_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/8_FC-Alone/8_FC-Alone_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker


mkdir 1_IMMO-FC

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/1_IMMO-FC

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/1_IMMO-FC_CGATGT_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/1_IMMO-FC/1_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts


mkdir 2_IMMO-FC

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/2_IMMO-FC

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/2_IMMO-FC_TGACCA_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/2_IMMO-FC/2_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts

mkdir 3_IMMO-FC

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/3_IMMO-FC

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/3_IMMO-FC_ACAGTG_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/3_IMMO-FC/3_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts

mkdir 4_IMMO-FC

cd /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/4_IMMO-FC

/home/ssharma/bin/STAR-STAR_2.4.2a/bin/Linux_x86_64/STAR --readFilesCommand zcat --readFilesIn /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/fastq_files/4_IMMO-FC_GCCAAT_L001_R1_001.fastq.gz --outFileNamePrefix /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/4_IMMO-FC/4_IMMO-FC_ --outSAMtype BAM SortedByCoordinate --genomeDir /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Sequence/STARIndex --runThreadN 25 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --quantMode GeneCounts


##RNAseq

#CUFFDIFF 

#IMMO-FC
cuffdiff -o ./AmygHC_vs_AmygIMMO-FC_RepeatMasked_SMALL -L HC,IMMO-FC -p 25 -u /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10_UCSC_RepeatMasked.gtf /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/19_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/20_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/21_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/22_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/23_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/24_Homecage_Aligned.sortedByCoord.out.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/1_IMMO-FC_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/2_IMMO-FC_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/3_IMMO-FC_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/4_IMMO-FC_Aligned.sortedByCoord.out.bam 

$FC
cuffdiff -o ./AmygFC_vs_AmygHC_RepeatMasked_SMALL -L HC,FC -p 25 -u /home/Shared/PengLab/iGenomes/Mus_musculus/RepeatMasker/mm10/Annotation/mm10_UCSC_RepeatMasked.gtf /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/19_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/20_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/21_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/22_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/23_Homecage_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/24_Homecage_Aligned.sortedByCoord.out.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/10_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/11_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/12_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/8_FC-Alone_Aligned.sortedByCoord.out.bam,/home/ssharma/Ressler_RNASeq/data/FearConditioning_Immobilization_Extinction_Amygdala_RNASeq/STAR_Aligned_BAMs_UCSC_RepeatMasker/BAMs/9_FC-Alone_Aligned.sortedByCoord.out.bam &


#DESEQ2

se is the RangedSummarizedExperiment object containing counts and information about the samples. In this example design we control for batch and condition, which should be columns of colData(se)
'''{r}
dds <- DESeqDataSet(se, design = ~ batch + condition)
dds <- DESeq(dds)
res <- results(dds, contrast=c("condition","trt","con"))
'''

Combine counts from geneCounts function of STAR to generate count matrices:

Read the Reads/Gene table 
'''{r}
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

'''


Combine column 1, with column 2 from every dataframe, add a header with the name of the sample that column came from
'''{r}

HCvsFC_gene_counts = data.frame(FC10Alone$V2, FC11Alone$V2, FC12Alone$V2, FC8Alone$V2, FC9Alone$V2, Homecage19$V2, Homecage20$V2, Homecage21$V2, Homecage22$V2, Homecage23$V2, Homecage24$V2, row.names = FC10Alone$V1) 

library(plyr)
rename(HCvsFC_gene_counts, c("FC10Alone.V2"="FC10Alone"))

FC11Alone$V2, FC12Alone$V2, FC8Alone$V2, FC9Alone$V2, Homecage19$V2, Homecage20$V2, Homecage21$V2, Homecage22$V2, Homecage23$V2, Homecage24$V2

col.names = c("FC10Alone$V2", "FC11Alone$V2", "FC12Alone$V2", "FC8Alone$V2", "FC9Alone$V2", "Homecage19$V2", "Homecage20$V2", "Homecage21$V2", "Homecage22$V2", "Homecage23$V2", "Homecage24$V2)

'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''

'''{r}
'''
##5hmCseq


###DEseq2




###MACS Peak Calling                                                                                      
#peaksplitter to call subpeaks within complex regions

#MACS/FC1
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_FC1_5hmC_3 --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC1.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &

#MACS/FC2
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_FC2_5hmC_MACS --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC2.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &

#MACS/HC1
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_HC1_5hmC_MACS --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC1.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &

#MACS/HC2
/home/ssharma/bin/MACS-1.4.2/bin/macs14 --name fear_conditioning_HC2_5hmC_MACS --format BAM --gsize mm --call-subpeaks --bdg --treatment /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC2.bam --control /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &









###Bedtools/
##calling DhMRs common to both sequencing experiments. Save the sequence JUST where two peaks overlap (no extra peak space unique to one sample or the other). Also create a "Peakheight" file that contains the entries that are overlapped; allows downstream analysis of relative 5hmC changes
#Note: needed to remove the header line -- bedtools was unable to recognize it: Chromosome      Start   End     Height  SummitPosition

#bedtools/HC1_HC2_BTIntersect.peaks.subpeaks.bed
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC1_5hmC_MACS/fear_conditioning_HC1_5hmC_MACS_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC2_5hmC_MACS/fear_conditioning_HC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >HC1_HC2_BTIntersect.peaks.subpeaks.bed

#bedtools/HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC1_5hmC_MACS/fear_conditioning_HC1_5hmC_MACS_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_HC2_5hmC_MACS/fear_conditioning_HC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -wa -wb -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed

#bedtools/FC1_FC2_BTIntersect.peaks.subpeaks.bed
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC1_5hmC_MACS/fear_conditioning_FC1_5hmC_3_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC2_5hmC_MACS/fear_conditioning_FC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC1_FC2_BTIntersect.peaks.subpeaks.bed

#bedtools/FC1_FC2_BTIntersect.peaks.subpeaks_PeakHeight.bed
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC1_5hmC_MACS/fear_conditioning_FC1_5hmC_3_peaks.subpeaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/fear_conditioning_FC2_5hmC_MACS/fear_conditioning_FC2_5hmC_MACS_peaks.subpeaks.bed -bed -sorted -wa -wb -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC1_FC2_BTIntersect.peaks.subpeaks_PeakHeight.bed


##R Script to take the average of columns 4 and 9 from the HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed file and adds it as a new column to the HC1_HC2_BTIntersect.peaks.subpeaks.bed file (ditto for FC files). End up with a BED file containing DhMRs + the average peak height from both datasets at this locus.

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


#FC_5hmC_Inc: Peaks that appear in fear conditioning; no peak at this loci in HC

bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -v -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_5hmC_Inc.bed


#FC_5hmC_Dec: Peaks disappear in fear conditioning; peak at this locus in HC dissapears in FC

bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed  -v -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_5hmC_Dec.bed


#Remove tmp files
rm FC1_FC2_BTIntersect.peaks.subpeaks.bed FC1_FC2_BTIntersect.peaks.subpeaks_PeakHeight.bed FC_5hmC_Dec_2.bed HC1_HC2_BTIntersect.peaks.subpeaks.bed FC_5hmC_Inc_2.bed HC1_HC2_BTIntersect.peaks.subpeaks_PeakHeight.bed



#FC_5hmC_common_Inc: Peak is present in both HC and FC samples; but peak is smaller in HC than FC (cutoff = ??)

#bedtools/FC_HC_Common.bed
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -bed -sorted -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_HC_Common.bed

#bedtools/FC_HC_Common_PeakHeights.bed
bedtools intersect -a /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_peaks.bed -b /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_peaks.bed -bed -sorted -wa -wb -g /home/Shared/PengLab/iGenomes/Mus_musculus/UCSC/mm10/Annotation/Genes/ChromInfo.txt >FC_HC_Common_PeakHeights.bed

#RScript/FC>HC_diff25_Common.bed & FC<HC_diff25_Common.bed
#Cutoff = 25 (arbitrary)

FC_HC_common_peaks = read.table("FC_HC_Common.bed")
FC_HC_common_heights = read.table("FC_HC_Common_PeakHeights.bed")
FC_HC_common_peaks$FCminusHC <- FC_HC_common_heights$V4 - FC_HC_common_heights$V10
FC_HC_common_peaks$HCminusFC <- FC_HC_common_heights$V10 - FC_HC_common_heights$V4
FC_HC_Common_FCgreaterHC = subset(FC_HC_common_peaks, FCminusHC >=25)
write.table(FC_HC_Common_FCgreaterHC, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_HC_Common_FCgreaterHC.bed")
FC_HC_Common_HCgreaterFC = subset(FC_HC_common_peaks, HCminusFC >=25)
write.table(FC_HC_Common_HCgreaterFC, sep = "\t", col.names = F, row.names = F, quote = F, file = "/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_HC_Common_HCgreaterFC.bed")


#cat/FC_5hmC_Inc
cat FC_HC_Common_FCgreaterHC.bed FC_5hmC_Inc.bed > FC_5hmC_Inc_increment.bed
#cat/HC_5hmC_Inc
cat FC_HC_Common_HCgreaterFC.bed FC_5hmC_Dec.bed > FC_5hmC_Dec_increment.bed
#cat/FC_AllDynamic5hmC
cat HCvsFC_5hmC_Inc_increment.bed HCvsFC_5hmC_Dec_increment.bed > HCvsFC_AllDynamic5hmC.bed
#cat/RNAseq_wholegenes/lncRNA_array_wholegenes/all
#cat/RNAseq_wholegenes_uprgulated/lncRNA_array_wholegenes_upregulated
#cat/RNAseq_wholegenes_downregulated/lncRNA_array_wholegenes_downregulated
#cat/RNAseq/microRNA


##RNseq v TRAPseq data
#Which genes are most regulated in the consolidation window at the transcriptional and translational level. Any isoform differences that occur in the RNAseq which are captured by the TRAP seq (i.e. which genes are being dynamically spliced at the level of the nucleus and cell-wide). Establish which biochemical circuits are most active in the consolidation window by seeing which protein coding RNAs are being dynamically spliced. Co-transcriptional translation, which RNAs are being created to be poised for translation vs which ones might have reached peak expression earlier, or will reach it later. Most of the 5-hmC signal in genes is in intronic regions. Most striking annotation enrichment changes that occur in FC are at introns (high 5hmC HC introns, low 5hmC FC introns), and SINE elements. Look at splicing, and regulation of genes by mobile, repetitive elements --> how would these contribute to individual differences in cognition/how variable are SINE elements in the population. 



###Annotation analysis
#AnnotatePeaks/HCvsFC_5hmCseq_Inc
#AnnotatePeaks/HCvsFC_5hmCseq_Dec
#AnnotatePeaks/HCvsFC_5hmCseq_I


ORDER OF ANNOTATION ENRICHMENT CHANGE (HC > FC)
intron>exon>intergenic>LTR~>promoter

#cat/RNAseq/TRAP_seq

##prep format and sort the BED files
#RNAseq/wholegene
bedtools sort -chrThenSizeA -i HCvsFC_RNAseq_wholegene_ALL.bed >HCvsFC_RNAseq_wholegene_ALL_sorted.bed
#RNAseq/upregulated
#RNAseq/downregulated

#5hmC-seq/all
bedtools sort -chrThenSizeA -i HCvsFC_AllDynamic5hmC.bed >HCvsFC_AllDynamic5hmC_sorted.bed
#5hmC-seq/upregulated
bedtools sort -chrThenSizeA -i HCvsFC_5hmC_Inc_increment.bed HCvsFC_5hmC_Inc_increment_sorted.bed
#5hmC-seq/downregulated
bedtools sort -chrThenSizeA -i HCvsFC_5hmC_Dec_increment.bed >HCvsFC_5hmC_Dec_increment_sorted.bed





#bedtools/RNAseq_wholegene/5hmC_ALL
# -wb option --> write out the entirety of the 5hmC peak that overlaps with a 5hmC region; find any motifs even if they aren't precisely within body of gene
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

#bedtools/RNAseq_all/5hmCseq_genes_TSS_all

#bedtools/RNAseq_all/5hmCseq_genes_TSS_all

#bedtools/RNAseq_all/5hmCseq_genes_TSS_all

#bedtools/RNAseq_splicing_all/5hmCseq_exons_all
#bedtools/RNAseq_splicing_all/5hmCseq_introns_all
#bedtools/RNAseq_splicing_all/5hmCseq_exon-intron_all
#bedtools/intersect -v/RNAseq_wholegene_all/RNAseq_splicing (alternatively spliced genes not called for expression level changes --> what exactly is delineation between isoform expression and expression level?)
#bedtools/intersect/RNAseq_splicing_unique/TRAP_seq_splicing_unique (how do isoforms captured in TRAP_seq differ from those captured by total RNA_seq; are certain subsets of spliced pools being translated  --> what is concordance between splicing as a whole, and splice variants that end up being translated?)

#bedtools/RNAseq_promoters/5hmC_Inc
#bedtools/RNAseq_promoters/5hmC_Dec
#bedtools/RNAseq_promoters_upregulated/5hmC_Inc
#bedtools/RNAseq_promoters_downregulated/5hmC_Dec

#bedtools/CTCFbindingsites/5hmC_all
#bedtools/CTCFbindingsites/5hmC_inc
#bedtools/CTCFbindingsites/5hmC_dec
#bedtools/CTCFbindingsites/5hmC_Inc/annotation (what genes are regulated CTCF binding sites near, are any of these in the RNAseq's (lncRNA and polyA))

#bedtools/lncRNAs_all/5hmc_all
#bedtools/lncRNAs_/5hmc_all


###DEXSeq: pictures of DEU for different targets





#FindMotifs/5hmC-Seq/FC_5hmC_Inc
#FindMotifs/5hmC-Seq/FC_5hmC_Dec
#FindMotifs/5hmC-Seq/FC_5hmC_Common_Inc
#FindMotifs/5hmC-Seq/FC_5hmC_Common_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Dec

#FindMotifs/5hmC-Seq_Enhancers/5hmC_Enhancers_overlap
#FindMotifs/5hmC-Seq_CTCF/5hmC_CTCF_overlap





##CTCF binding sites with dynamic 5hmC in FC
#whole genome CTCF binding sites from Corces Lab

##Enhancers with dynamic 5hmC in FC
#FANTOM identified enhancers
#Neural specific enhancers


##Other Datasets? Hi-seq? TADs or mini-TADs, or CTFCF binding sites influential at these positions..proxy for conformational change


###Tag Directories of 5hmC BAM files to calculate enrichment of 5hmC at loci specified in BED format
makeTagDirectory <Output Dir name> [options] <alignment file1> <"2> <etc>

#makeTagDirectory/HC
makeTagDirectory HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC1.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_HC2.bam

#makeTagDirectory/FC
makeTagDirectory FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC1.bam /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_FC2.bam &

#makeTagDirectory/Input
makeTagDirectory Input_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/data/FearConditioning_5hmC_Seq/STAR_Aligned_BAMs_UCSC/BAMs/Sample_InputHC1.bam &



###Annotate motifs in the promoters, exons, and introns of the upregulated RNA-seq genes

##Background Files
#wholegene - mm10 all genes
#exons - mm10 all exons
#introns - mm10 all introns
#5hmC regions - random sequences from the genome

##FindMotifs/RNAseq/wholegene/all

#testing out different -size parameters (avg size of the DNA in the BED file)
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_2k/ -size 2000 -mis 1 -S 10 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &

findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_100k/ -size 100000 -mis 1 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &

findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_10k/ -size 10000 -mis 1 -S 10 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &

findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_expandedcoordinates.bed mm10 HCvsFC_RNAseq_wholegene_motifs_50k/ -size 50000 -mis 1 -S 10 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &

#Conclusion about  -size parameter: having a size closer to the real average length of the regions in the bedfile makes for more statistically significant results, but much longer run time. 100k size failed to finish.
#Awk script to determine average size of regions in a bed file
cat CTCFPeaksClustered.bed | awk -F'\t' 'BEGIN{SUM=0}{ SUM+=$3-$2 }END{print SUM}'
#RNAseq Introns: 15000 (avg was 8800)
#RNAseq exons: 3000 (avg 325)
#CTCFPeaksClustered = 500 (avg 362)
#Overshooting may be better than under
#HCvsFC_5hmC_Dec
#HCvsFC_5hmC_Inc
#HCvsFC_5hmC_All

#FindMotifs/RNAseq/wholegene/upreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_upregulated.bed mm10 HCvsFC_RNAseq_wholegene_upregulated_motifs/ -size 50000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &

#FindMotifs/RNAseq/wholegene/downreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_wholegene_downregulated.bed mm10 HCvsFC_RNAseq_wholegene_downregulated_motifs/ -size 50000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allgenes_background.bed -h &

##Promoter Motifs

#FindMotifs/RNAseq/promoters/all
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_promoters_ALL.bed mm10 HCvsFC_RNAseq_promoters_motifs/ -size 2000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allpromoters_-2000_background.bed -h &

#FindMotifs/RNAseq/promoters/upreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_promoter_upregulated.bed mm10 HCvsFC_RNAseq_promoter_upregulated_motifs/ -size 2000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allpromoters_-2000_background.bed -h &

#FindMotifs/RNAseq/promoters/downreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_promoters_downregulated.bed mm10 HCvsFC_RNAseq_promoter_downregulated_motifs/ -size 2000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allpromoters_-2000_background.bed -h &

##Intron motifs

#FindMotifs/RNAseq/introns/all
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_introns_ALL.bed mm10 HCvsFC_RNAseq_intron_motifs/ -size 15000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allintrons_background.bed -h &

#FindMotifs/RNAseq/introns/upreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_introns_upregulated.bed mm10 HCvsFC_RNAseq_intron_upregulated_motifs/ -size 15000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allintrons_background.bed -h &

#FindMotifs/RNAseq/introns/downreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_introns_downregulated.bed mm10 HCvsFC_RNAseq_intron_downregulated_motifs/ -size 15000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allintrons_background.bed -h &

##Exon Motifs

#FindMotifs/RNAseq/exons/all
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_exons_ALL.bed mm10 HCvsFC_RNAseq_exon_motifs/ -size 3000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allexons_background.bed -h &

#FindMotifs/RNAseq/exons/upreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_exons_upregulated.bed mm10 HCvsFC_RNAseq_exon_upregulated_motifs/ -size 3000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allexons_background.bed -h &

#FindMotifs/RNAseq/exons/downreg
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/HCvsFC_RNAseq_exons_downregulated.bed mm10 HCvsFC_RNAseq_exon_downregulated_motifs/ -size 3000 -mis 2 -S 25 -p 25 -bg /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/background_files/mm10_allexons_background.bed -h &



#5hmC Peak Motifs

#FindMotifs/5hmC_All
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HCvsFC_AllDHMR.bed mm10 HCvsFC_5hmC_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &

#FindMotifs/5hmC_Inc
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HCvsFC_5hmC_Inc_increment.bed mm10 HCvsFC_5hmC_Inc_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &

#FindMotifs/5hmC_Dec
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HCvsFC_5hmC_Dec_increment.bed mm10 HCvsFC_5hmC_Dec_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &





#FindMotifs/5hmC-Seq/FC_5hmC_Inc
#FindMotifs/5hmC-Seq/FC_5hmC_Dec
#FindMotifs/5hmC-Seq/FC_5hmC_Common_Inc
#FindMotifs/5hmC-Seq/FC_5hmC_Common_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_wholegene_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_wholegene_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_promoter_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_promoter_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_exons_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_exons_Dec

#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Inc_RNAseq_introns_Dec
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Inc
#FindMotifs/5hmC-Seq_RNAseq/5hmC_Dec_RNAseq_introns_Dec

#FindMotifs/5hmC-Seq_Enhancers/5hmC_Enhancers_overlap
#FindMotifs/5hmC-Seq_CTCF/5hmC_CTCF_overlap
#FindMotifs/CTCF Clustered Peaks
findMotifsGenome.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/CTCFPeaksClustered.bed mm10 CTCFPeaksClustered_motifs/ -size 700 -mis 2 -S 25 -p 25 -h &


###Scatterplots in Homer


#Compare significant RNAseq genomic loci with each dataset: RNAseq promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_promoters.bed mm10 -size 1000 -log -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_promoters_ComprehensiveComparison 

#RNAseq exons
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_exons.bed mm10 -size 1000 -fragLength 150 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/  >Homer_FearConditioning_5hmC_FC_RNAseq_exons_ComprehensiveComparison_2 &

#RNAseq introns
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_introns.bed mm10 -size 1000 -p 10 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_introns_ComprehensiveComparison_2 &

#CTCF binding sites
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/CTCFPeaksClustered.bed mm10 -size 1000 -fragLength 150 -annStats Homer_CTCFPeaksClustered_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_CTCFPeaksClustered_ComprehensiveComparison_2 & 

#lncRNAs
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_lncRNA.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_lncRNA_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_lncRNA_ComprehensiveComparison_2 &

#enhancers: cell based
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_primarycell_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_primarycell_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_primarycell_based_ComprehensiveComparison_2 &

#enhancers: tissue based
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_tissue_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_tissue_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_tissue_based_ComprehensiveComparison_2 &

#exon boundaries: +/- 50 bp from exons
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_exonboundaries.bed mm10 -size 1000 -fragLength 150 -annStats Homer_mm10_exonboundaries_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_mm10_exonboundaries_ComprehensiveComparison_2 &



###Finding instances of motifs 
1) Create a .motif file using seq2profile, or download the .motfi file from http://homer.salk.edu/homer/custom.motifs
2) Predict sites where the TF will bind in a set of coordinates using annotatepeaks -m -mbed 
3) Find intensity of epigenetic marks at predetermined set of motif binding sites (as from #2) using annotatepeaks -hist -d to feed in tag directories

HIF-1b	RTACGTGC
c-Myc	VVCCACGTGG
n-Myc	VRCCACGTGG
Maz     GGGGGGGG
Stat3 (control)	CTTCCGGGAA
HRE(HSF)	TTCTAGAABNTTCTA
PRDM1/Blimp1	ACTTTCACTTTC

##creating .motif files - least stringent: 2 mismatches allowed
seq2profile.pl RTACGTGC 2 HIF-1b > HIF-1b.motif
seq2profile.pl VVCCACGTGG 2 c-Myc > c-Myc.motif
seq2profile.pl VRCCACGTGG 2 n-Myc > n-Myc.motif
seq2profile.pl GGGGGGGG 2 Maz > Maz_2.motif
seq2profile.pl CTTCCGGGAA 2 Stat3 > Stat3.motif
seq2profile.pl TTCTAGAABNTTCTA 2 HRE > HRE.motif
seq2profile.pl ACTTTCACTTTC 2 PRDM1 > PRDM1_2.motif

##creating .motif files -  medium stringent: 1 mismatches allowed
seq2profile.pl ACTTTCACTTTC 1 PRDM1 > PRDM1_1.motif

##creating .motif files - most stringent: 0 mismatches allowed
seq2profile.pl ACTTTCACTTTC 0 PRDM1 > PRDM1_0.motif


###making histograms of 5-hmC near motifs
##HC_unique
#HIF-1b
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m HIF-1b.motif -mbed HIF-1b_HC_unique.bed -hist 10 > HIF_motifs_HC_unique.txt

#c-Myc
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m c-Myc.motif -mbed c-Myc_HC_unique.bed > c-Myc_motifs_HC_unique.txt &

#n-Myc
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m n-Myc.motif -mbed n-Myc_HC_unique.bed > n-Myc_motifs_HC_unique.txt &

#Maz
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m Maz.motif -mbed Maz_HC_unique.bed > Maz_motifs_HC_unique.txt &

#Stat3 (control)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -m Stat3.motif -mbed Stat3_HC_unique.bed > Stat3_motifs_HC_unique.txt &


##FC_unique
#HIF-1b
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m HIF-1b.motif -mbed HIF-1b_FC_unique.bed -hist 10 > HIF_motifs_FC_unique.txt

#c-Myc
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m c-Myc.motif -mbed c-Myc_FC_unique.bed > c-Myc_motifs_FC_unique.txt &

#n-Myc
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m n-Myc.motif -mbed n-Myc_FC_unique.bed > n-Myc_motifs_FC_unique.txt &

#Maz
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m Maz.motif -mbed Maz_FC_unique.bed > Maz_motifs_FC_unique.txt &

#Stat3
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -m Stat3.motif -mbed Stat3.motif_FC_unique.bed > Stat3_motifs_FC_unique.txt &

annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_allpromoters_-2000_background.bed mm10 -size 1000 -m Stat3.motif -mbed Stat3.motif_mm10_promoters.bed > Stat3_motifs_mm10_promoters.txt &

#HRE (control)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_allpromoters_-2000_background.bed mm10 -size 1000 -m HRE.motif -mbed HRE.motif_mm10_promoters.bed > HRE_motifs_mm10_promoters.txt &


#Maz in the RNAseq promoters
#Using new .motif file
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_promoters.bed mm10 -size 1000 -m Maz.motif -mbed Maz_RNAseq_promoters.bed > Maz_motifs_RNAseq_promoters_2.txt &

#PRDM1/RNAseq promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_promoters.bed mm10 -size 2000 -m PRDM1.motif -mbed PRDM1_RNAseq_promoters.bed > PRDM1_motifs_RNAseq_promoters.txt &

#PRDM1/upregulated RNAseq promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_promoters_upregulated.bed mm10 -size 2000 -m PRDM1.motif -mbed PRDM1_RNAseq_promoters_upreg.bed > PRDM1_motifs_RNAseq_promoters_upreg.txt &


##Concatenate all HIF-1a binding sites in both HC_unique and FC_unique peaks
cat HIF-1b_HC_unique.bed HIF-1b_FC_unique.bed > HIF_motifs_all.bed
cat c-Myc_HC_unique.bed c-Myc_FC_unique.bed > c-Myc_motifs_all.bed
cat n-Myc_HC_unique.bed n-Myc_FC_unique.bed > n-Myc_motifs_all.bed
cat Maz_HC_unique.bed Maz_FC_unique.bed > Maz_motifs_all.bed
cat Stat3_motifs_HC_unique.txt Stat3_motifs_FC_unique.txt > Stat3_motifs_all.bed


###Histograms in Homer
##5-hmC in HC vs FC centered around these motifs
#HIF1b binding sites
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/HIF_motifs_all.bed mm10 -size 1000 -m HIF-1b.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > HIF_motifs_hist_HC_FC.txt

#c-Myc
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/c-Myc_motifs_all.bed mm10 -size 1000 -m c-Myc.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > c-Myc_motifs_hist_HC_FC.txt &


#n-Myc
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/n-Myc_motifs_all.bed mm10 -size 1000 -m n-Myc.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > n-Myc_motifs_hist_HC_FC.txt &

#Maz: in all 5-hmC peaks
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/Maz_motifs_all.bed mm10 -size 1000 -m Maz.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ > Maz_motifs_hist_HC_FC.txt &

#Maz: in all RNA-seq promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/Maz_RNAseq_promoters.bed mm10 -size 10 -m Maz.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/  >Maz_motifs_RNAseq_promoters_hist_HC_FC_Input_10Window.txt &

#control 
/home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/ mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >trunc_allgenes.bed_hist_HC_FC.txt &

#Stat3
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/Stat3.motif_mm10_promoters.bed mm10 -size 1000 -m Stat3.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Stat3_motifs_hist_mm10proms.txt &

#HRE (HSF)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/HRE.motif_mm10_promoters.bed mm10 -size 1000 -m HRE.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >HRE_motifs_hist_mm10proms.txt &

#PRDM1/RNAseq promoters/upregulated ##2 mismatches
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1_RNAseq_promoters_upreg.bed mm10 -size 1000 -m /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >PRDM1_motifs_hist_mm10proms.txt &
 
 #PRDM1 0 mismatches
 
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1_RNAseq_promoters_upreg.bed mm10 -size 1000 -m /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/motif_analysis/PRDM1_0.motif -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >PRDM1_0_motifs_hist_mm10proms.txt &

annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_promoters_upregulated.bed mm10 -size 2000 -m PRDM1.motif -mbed PRDM1_RNAseq_promoters_upreg.bed > PRDM1_motifs_RNAseq_promoters_upreg.txt &

##RNA-seq
#exon-boundaries
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_RNAseq_exonboundaries.bed mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >RNAseq_exon_boundaries_hist_HC_FC.txt &

#Upregulated Promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_upregulatedgenes_promoters.bed mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >RNAseq_upregulated_promoters_hist_HC_FC_Input.txt &

#Downregulated Promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/datasets/FC_downregulatedgenes_promoters.bed mm10 -size 1000 -hist 10 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/Input_hMeDIP_BAMs_tag_directory/ >RNAseq_downregulated_promoters_hist_HC_FC_Input.txt &




##Calculating Tag Densities Across Different Experiments for Scatterplots
#Compare HC all peaks to everything else (HC, FC, HC_unique, FC_unique)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC.bed mm10 -size 1000 -fragLength 150 -annStats Homer_HC_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_HC_ComprehensiveComparison &

#Compare FC all peaks to everything else (HC, FC, HC_unique, FC_unique)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_FC_ComprehensiveComparison &

#Compare HC_unique all peaks to everything else (HC, FC, HC_unique, FC_unique)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/HC_unique.bed mm10 -size 1000 -fragLength 150 -annStats Homer_HC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_HC_unique_ComprehensiveComparison &

#Compare FC_unique all peaks to everything else (HC, FC, HC_unique, FC_unique)
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_unique.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_tag_directory /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_unique_hMeDIP_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_unique_hMeDIP_tag_directory/ >Homer_FearConditioning_5hmC_FC_unique_ComprehensiveComparison &

#Compare significant RNAseq genomic loci with each dataset: RNAseq promoters
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_promoters.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_promoters_ComprehensiveComparison_2 &

#RNAseq exons
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_exons.bed mm10 -size 1000 -fragLength 150 -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/  >Homer_FearConditioning_5hmC_FC_RNAseq_exons_ComprehensiveComparison_2 &

#RNAseq introns
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_RNAseq_introns.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_unique_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_RNAseq_introns_ComprehensiveComparison_2 &

#CTCF binding sites
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/CTCFPeaksClustered.bed mm10 -size 1000 -fragLength 150 -annStats Homer_CTCFPeaksClustered_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_CTCFPeaksClustered_ComprehensiveComparison_2 & 

#lncRNAs
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/FC_lncRNA.bed mm10 -size 1000 -fragLength 150 -annStats Homer_FC_lncRNA_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_FC_lncRNA_ComprehensiveComparison_2 &

#enhancers: cell based
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_primarycell_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_primarycell_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_primarycell_based_ComprehensiveComparison_2 &

#enhancers: tissue based
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/enhancers_tissue_based.bed mm10 -size 1000 -fragLength 150 -annStats Homer_enhancers_tissue_based_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_enhancers_tissue_based_ComprehensiveComparison_2 &

#exon boundaries: +/- 50 bp from exons
annotatePeaks.pl /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/bedtools/mm10_exonboundaries.bed mm10 -size 1000 -fragLength 150 -annStats Homer_mm10_exonboundaries_annStats -d /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/HC_hMeDIP_BAMs_tag_directory/ /home/ssharma/Ressler_RNASeq/analysis/FearConditioning_5hmCSeq_MACS/homer/Homer_Tag_Directories/FC_hMeDIP_BAMs_tag_directory/ >Homer_FearConditioning_5hmC_mm10_exonboundaries_ComprehensiveComparison_2 &






###Plink Set-based test analysis
##Homology: pull out homologous human sequences of DhMRs -> run set based test to see which regions correspond to SNP-rich human regulatory regions. Pick the overlap of the most significant SNPs and the most conserved region

##Set-based test




