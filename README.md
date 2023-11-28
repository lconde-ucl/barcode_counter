# barcode_counter

This is a simple pipeline to generate frequency of barcodes on amplicon sequencing data. The pipeline runs:
* fastQC v0.11.8
* TrimGalore v0.6.10
* bbMerge (bbmap v39.04)
* bbDuk (bbmap v39.04)

## ARGUMENTS

You need to provide:
* An INPUTDIR folder with **fastQ files**, with the usual naming convention, e.g.:
  *  INPUTDIR/SAMPLE1_R1_001.fastq.gz
  *  INPUTDIR/SAMPLE1_R2_001.fastq.gz
  *  INPUTDIR/SAMPLE2_R1_001.fastq.gz
  *  INPUTDIR/SAMPLE2_R2_001.fastq.gz
  *  *  [...]
* A **SAMPLEFILE** with the sample names, e.g.:
  * L1
  * L2
  * [...]
* A **BARCODEFILE** file in fasta format, e.g.:
    * \> barcode1
    * agtgcgtatggatgc
    * \> barcode2
    * gtcgtgtagctagca
    * [...]
* Additionally, the pipeline needs a **barcode_length** argument specifying the length of the barcodes (in the above example, 15)
* Finally, provide an **OUTPUTDIR** name (e.g., "results"), where all the generated output will be stored.

## USAGE

Download the qsub script and prepare the data. The pipeline will submit a job array, i.e., one job per sample

qsub -t i-j barcode_counter.qsub \\\
&nbsp;&nbsp;INPUTDIR \\\
&nbsp;&nbsp;SAMPLESFILE \\\
&nbsp;&nbsp;BARCODEFILE \\\
&nbsp;&nbsp;BARCODELENGTH \\\
&nbsp;&nbsp;OUTPUTDIR

## EXAMPLE

Assuming you have 5 samples, and the length of your barcodes is 15:

qsub -t 1-5 barcode_counter.qsub \\\
&nbsp;&nbsp;fastqs \\\
&nbsp;&nbsp;samples.txt \\\
&nbsp;&nbsp;barcodes.fa \\\
&nbsp;&nbsp;15 \\\
&nbsp;&nbsp;results\\


## OUTPUT

For each sample, the pipeline will output 4 folders:
* OUTPUTDIR/SAMPLENAME/1_fastqc: fastQC results of raw data
* OUTPUTDIR/SAMPLENAME/2_trim_galore: trimmed fastq files and fastQC results of the trimmed data
* OUTPUTDIR/SAMPLENAME/3_bbmerge: R1+R2 merged fastQ
* OUTPUTDIR/SAMPLENAME/3_bbduk: a stats file with the frequency of each barcode in the sample


