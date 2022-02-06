---
layout: post
title: "SNP Calling"
author: "Yingwei Guo"

---

01.QC
02.REFERENCE INDEX
03.MAPPING
04.CALLING
05.FILTER
06.CONCATENATE
07.ANNOTATION 
08.PRUNE
09.PHASE
10.IMPUTATION

## 01.QC

`/stor9000/apps/users/NWSUAF/2016050001/software/fastp -i ${1}_1.fq.gz -I ${1}_2.fq.gz -o ${1}_1.clean.fq.gz -O ${1}_2.clean.fq.gz -h ${1}.html`

`-n 5 -q 15 -u 40`
去除接头序列(adapter);
当测序read中含有的N的含量超过该条read长度比例的5%时，需要去除此对paired reads；
当测序read中含有的低质量（Q ≤15）碱基数超过该条read长度比例的40% 时，需要去除此对paired reads。
[fastp](https://github.com/OpenGene/fastp)

## 02.REFERENCE INDEX

1.Generate the BWA index using BWA. This will create the “.fasta.amb”, “.fasta.ann”, “.fasta.bwt”, “.fasta.pac” and “.fasta.sa” files.
`bwa index GCF_002742125.1_Oar_rambouillet_v1.0_genomic.rename.AddY.fna`

2.Generate the FASTA file index using samtools. This will create the “.fasta.fai” file.
`samtools faidx GCF_002742125.1_Oar_rambouillet_v1.0_genomic.rename.AddY.fna`

3.Generate the sequence dictionary using Picard. This will create the “.dict” file.
`/stor9000/apps/appsoftware/BioSoftware/bin/java -jar bin/picard_2.18.17.jar CreateSequenceDictionary R=GCF_002742125.1_Oar_rambouillet_v1.0_genomic.rename.AddY.fna O=GCF_002742125.1_Oar_rambouillet_v1.0_genomic.rename.AddY.dict`

## 03.MAPPING
define variable

```
export sm=${1}
export fq1=${2}
export fq2=${3}
export ref=Sheep/reference/Oar4.0/Oar4.0_add_CPY_for_target_seq/Oar4.0_add_CPY.fa
```

 mapping
```
/Software/bwa-0.7.17/bwa mem \
-t 4 \
-R '@RG\tID:'${sm}'\tLB:'${sm}'\tPL:ILLUMINA\tSM:'${sm} \
${ref} \
${fq1} \
${fq2} \
| /Software/samtools1.9/bin/samtools view -bS -o ${sm}.bam
```
sort
```
/stor9000/apps/appsoftware/BioSoftware/bin/java \
-Xmx10g \
-Djava.io.tmpdir=files/tmp \
-Dpicard.useLegacyParser=false \
-jar bin/picard_2.18.17.jar \
SortSam \
-I ${sm}.bam \
-O ${sm}.sort.bam \
-SORT_ORDER coordinate \
-VALIDATION_STRINGENCY LENIENT
```

```
dedup 去除PCR重复
/stor9000/apps/appsoftware/BioSoftware/bin/java \
-Xmx10g \
-Djava.io.tmpdir=files/tmp \
-Dpicard.useLegacyParser=false \
-jar bin/picard_2.18.17.jar \
MarkDuplicates \
-I ${sm}.sort.bam \
-O ${sm}.dedup.bam \
-ASSUME_SORT_ORDER coordinate \
-METRICS_FILE ${sm}.dedup.txt \
-VALIDATION_STRINGENCY LENIENT
```

```
# index bam
/stor9000/apps/users/NWSUAF/2014010784/software/samtools/samtools-1.12/samtools index ${sm}.dedup.bam
# realign Indel区域的重新比对
/stor9000/apps/appsoftware/BioSoftware/bin/java \
-Xmx10g \
-Djava.io.tmpdir=files/tmp \
-jar bin/GATK3.7/GenomeAnalysisTK.jar \
-T RealignerTargetCreator \
-I ${sm}.dedup.bam \
-R ${ref} \
-o ${sm}.intervals

/stor9000/apps/appsoftware/BioSoftware/bin/java \
-Xmx10g \
-Djava.io.tmpdir=files/tmp \
-jar bin/GATK3.7/GenomeAnalysisTK.jar \
-T IndelRealigner \
-I ${sm}.dedup.bam \
-R ${ref} \
-targetIntervals ${sm}.intervals \
-o ${sm}.realign.bam

```

## 04.CALLING

```
export sm=${1}
export chr=${3}
export bam=${2}
export ref=Sheep/reference/Oar4.0/Oar4.0_add_CPY_for_target_seq/Oar4.0_add_CPY.fa
bin/gatk-4.1.2.0/gatk \
--spark-runner LOCAL \
--java-options "-Djava.io.tmpdir=files/tmp -Xmx10G" \
**HaplotypeCaller** \
-R ${ref} \
-I ${bam} \
-ERC GVCF \
-L ${chr} \
-O 01.gvcf/${sm}/${sm}.${chr}.gvcf.gz

export chr=${1}
export ref=Sheep/reference/Oar4.0/Oar4.0_add_CPY_for_target_seq/Oar4.0_add_CPY.fa

bin/gatk-4.1.2.0/gatk \
--java-options "-Djava.io.tmpdir=files/tmp -Xmx10G" \
**CombineGVCFs** \
-R ${ref} \
--variant files/snp_calling/04.calling/01.gvcf/GJ01429/GJ01429.chr${chr}.gvcf.gz \
--variant files/snp_calling/04.calling/01.gvcf/GJ01741/GJ01741.chr${chr}.gvcf.gz \
--output 02.merged_gvcf/chr${chr}.gvcf.gz

export chr=${1}
export ref=Sheep/reference/Oar4.0/Oar4.0_add_CPY_for_target_seq/Oar4.0_add_CPY.fa
bin/gatk-4.1.2.0/gatk \
--java-options "-Djava.io.tmpdir=files/tmp -Xmx10G" \
**GenotypeGVCFs** \
-R ${ref} \
--variant 02.merged_gvcf/chr${chr}.gvcf.gz \
--output 03.vcf/chr${chr}.vcf.gz

```
## 05.FILTER

```
bcftools view \
-v snps \
-e 'QD < 2.0 | QUAL < 30.0 | SOR > 3.0 | FS > 60.0 | MQ < 40.0 | MQRankSum < -12.5 | ReadPosRankSum < -8.0 ' \
-m2 -M2 \
../04.calling/03.vcf/chr${1}.vcf.gz \
-Oz \
-o chr${1}.filtered.vcf.gz


#bcftools view -i 'F_MISSING < 0.1 & MAF > 0.05' vcf -Oz -o filter.vcf

```