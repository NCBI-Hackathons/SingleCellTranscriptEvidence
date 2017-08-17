# SCANT

## Introduction
The analysis of single cell RNA-Seq data involves the use of a disparate set of software packages to identify the genes that are expressed within the sample. Although sophisticated software packages are now just a click away from being employed for these analyses, they expect the user to string the outputs from one step of the analyses to the next piece of software. We address this challenge by creating a pipeline that brings together a set of well-tested tools and combines them into a single script that is simple and easy-to-use. The pipeline described here allows users to analyze data from the Sequence Read Archive (SRA) by providing just the BioProject accession or specific run identifiers in combination with HISAT2 index files for the genome of interest. Upon execution, this pipeline identifies sequencing reads that correspond to novel genes that are yet to be annotated.

![Input/Output](https://github.com/NCBI-Hackathons/SingleCellTranscriptEvidence/blob/master/input.output.png)

## Workflow
![Workflow](https://github.com/NCBI-Hackathons/SingleCellTranscriptEvidence/blob/master/workflow.jpg)

## Setup
### System
Depending on the amount of the data being analyzed, the pipeline can require a significant amount of computational resources. We recommend using a computer that has at least 8 processor cores, 16 Gb of RAM and approximately 1.5 Gb hard drive space for each analyzed run. Additional disk space is needed for one or more HISAT2 index files.
### Software Packages
The pipeline expects the following packages are installed and available in the PATH:
* [Python](https://www.python.org/downloads/release/python-350/) (version 3.5 or above)
* [HISAT2](https://ccb.jhu.edu/software/hisat2/index.shtml) (version 2.1.0)
* [StringTie](https://ccb.jhu.edu/software/stringtie/) (version 1.3.3b)
* [Samtools](http://www.htslib.org/) (version 1.3.1)
* [gffcompare](https://github.com/gpertea/gffcompare)
* R (version 3.3.2 or above)
* [bedtools](http://bedtools.readthedocs.io/)


### Data files
HISAT2 index files are required for the genome of interest to execute the script. For a selected list of organisms, pre-computed index files can be downloaded from (ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/data/). If using `-r` option with the pre-computed HISAT2 indexes, it is recommended to download the GTF files for the appropriate organism from Ensembl to prevent any errors caused by the potential mismatch between the chromosome names used in the index files and the GTF files.

Alternately, HISAT2 index files can be generated by the user by using the `hisat2-build` tool that is packaged with the HISAT2 software. For instructions on how to build indexes, please refer to the [HISAT2 manual](https://ccb.jhu.edu/software/hisat2/manual.shtml).


## Usage
### Examples
#### One SRR accession
` scant.py --sra_acc SRR123546 genome_tran `
#### One BioProject accession
` scant.py --sra_acc PRJNA12345 genome_tran `

The script will download a list of all SRR accessions within the BioProject and run the pipeline on each of those accessions
#### A list of SRR accessions
` scant.py --file <SRRs.list> genome_tran `

The pipeline expects a list of SRR accessions, one per each line, in the following format:
```
SRR123546
SRR234567
...
```

### Help
Executing the `scant.py` script without any parameters will display a usage message as shown below:
```
usage: scant.py [-h] [--sra_acc SRA_ACC] [--file FILE] [-r REFERENCE]
               [-p PROCESSES] [-o OUTDIR] [-b BAM]
               [-nso NOVEL_SPLICESITE_OUTFILE] [-sf STRINGTIE_FILE]
               [-a ABUNDANCE] [-m MULTI_MAP_FRAC]
               genome

positional arguments:
  genome                path of genome file

optional arguments:
  -h, --help            show this help message and exit
  --sra_acc SRA_ACC     SRR or PRJNA number (default: )
  --file FILE           path to a file with newline separated SRR numbers
                        (default: )
  -r REFERENCE, --reference REFERENCE
                        path to reference .gtf file (default: )
  -p PROCESSES, --processes PROCESSES
                        number of cores to use in run (default: 4)
  -o OUTDIR, --outdir OUTDIR
                        name of directory to save everything to (default: )
  -b BAM, --bam BAM     name of hisat2 output bam file (default:
                        hisat.sorted.bam)
  -nso NOVEL_SPLICESITE_OUTFILE, --novel_splicesite_outfile NOVEL_SPLICESITE_OUTFILE
                        Set stringtie novel_splicesite_outfile parameter
                        (default: splicesite.tab)
  -sf STRINGTIE_FILE, --stringtie_file STRINGTIE_FILE
                        name of stringtie output gtf file (default:
                        stringtie_file.gtf)
  -a ABUNDANCE, --abundance ABUNDANCE
                        Set stringtie -A parameter (default: abundance.tab)
  -m MULTI_MAP_FRAC, --multi_map_frac MULTI_MAP_FRAC
                        Set stringtie -M parameter (default: .95)
```

## Output explanation

## Limitations/Caveats/To-do
* When using the `--sra_acc` option with a BioProject identifier, users should ensure that all of the runs within the BioProject are of same kind. In a situation where the BioProject includes different kind of runs, users should use the `--file` option and provide a list of only the SRR accessions of interest.


## People
* Michael Chambers <greenkidneybean@gmail.com>
* Wes Crouse <wcrouse@email.unc.edu>
* Allissa Dillman <allissa.dillman@gmail.com>
* Jessime Kirk <jessime@email.unc.edu>
* Vamsi Kodali <vkkodali@gmail.com>
* Ashis Saha <ashis@jhu.edu>
* Kimiko Suzuki <sksuzuki@ad.unc.edu>
