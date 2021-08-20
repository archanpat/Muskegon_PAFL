#### Grouping overlapping reads into clumps and removing duplicates using clumpify.sh.

`/stor/work/Ochman/archan/bin/bbmap/clumpify.sh in1=91614_ATTCAGAA-TATAGCCT_S1_L003_R1_001.fastq.gz in2=91614_ATTCAGAA-TATAGCCT_S1_L003_R2_001.fastq.gz out1=clumped_91614_R1.fq.gz out2=clumped_91614_R2.fq.gz reorder dedupe`

`/stor/work/Ochman/archan/bin/bbmap/clumpify.sh in1=91615_ATTCAGAA-TATAGCCT_S1_L003_R1_001.fastq.gz in2=91615_ATTCAGAA-TATAGCCT_S1_L003_R2_001.fastq.gz out1=clumped_91615_R1.fq.gz out2=clumped_91615_R2.fq.gz reorder dedupe`

#### Discovering Adapter Sequences using bbmerge.sh

`/stor/work/Ochman/archan/bin/bbmap/bbmerge.sh in1=clumped_91614_R1.fq.gz in2=clumped_91614_R2.fq.gz outa=adapters_91614.fasta`

`/stor/work/Ochman/archan/bin/bbmap/bbmerge.sh in1=clumped_91615_R1.fq.gz in2=clumped_91615_R2.fq.gz outa=adapters_91615.fasta`

#### Removing Adapter Content

`/stor/work/Ochman/archan/bin/bbmap/bbduk.sh -Xmx1g in1=clumped_91614_R1.fq.gz in2=clumped_91614_R2.fq.gz out1=bbduk_91614_R1.fastq out2=bbduk_91614_R2.fastq ref=adapters_91614.fasta ktrim=r k=23 mink=11 hdist=1 tpe tbo`

`/stor/work/Ochman/archan/bin/bbmap/bbduk.sh -Xmx1g in1=clumped_91615_R1.fq.gz in2=clumped_91615_R2.fq.gz out1=bbduk_91615_R1.fastq out2=bbduk_91615_R2.fastq ref=adapters_91615.fasta ktrim=r k=23 mink=11 hdist=1 tpe tbo`

#### Trimming Using Trimmomatic. Sliding Window Trimming Approach. Dropping the read if it is a length below 40. Cutting bases off at the start and end of the read if below a threshold quality of 20.

`trimmomatic PE -threads 12 trimmed_91614_R1.fastq trimmed_91614_R2.fastq trimmomatic_91614_R1_trimmed.fastq trimmomatic_91614_R1un.trimmed.fastq trimmomatic_91614_R2_trimmed.fastq trimmomatic_91614_R2un.trimmed.fastq SLIDINGWINDOW:4:20 MINLEN:40 LEADING:20 TRAILING:20`

`trimmomatic PE -threads 12 trimmed_91615_R1.fastq trimmed_91615_R2.fastq trimmomatic_91615_R1_trimmed.fastq trimmomatic_91615_R1un.trimmed.fastq trimmomatic_91615_R2_trimmed.fastq trimmomatic_91615_R2un.trimmed.fastq SLIDINGWINDOW:4:20 MINLEN:40 LEADING:20 TRAILING:20`

#### Assembling using MEGAHIT. Used default settings, which MEGAHIT states to be tuned for metagenomic assembly.

`megahit -1 trimmomatic_91614_R1_trimmed.fastq -2 trimmomatic_91614_R2_trimmed.fastq -o megahit-assembly -t 18`

`megahit -1 trimmomatic_91615_R1_trimmed.fastq -2 trimmomatic_91615_R2_trimmed.fastq -o megahit-assembly -t 18`


