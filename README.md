# Imputation Workflow

This repo contains the workflow which was developed as part of
the H3ABioNet Hackathon held in Pretoria, SA in 2016.
 - We track our open tasks using github's [issues](https://github.com/h3abionet/chipimputation/issues)
 - The 1000ft view is located on [our Trello board](https://trello.com/b/Dp08chq7/stream-d-imputation-and-phasing).

## Setup (native cluster)

#### Headnode
  - [Nextflow](https://www.nextflow.io/) (can be installed as local user)
   - NXF_HOME needs to be set, and must be in the PATH
   - Note that we've experienced problems running Nextflow when NXF_HOME is on an NFS mount.
   - The Nextflow script also needs to be invoked in a non-NFS folder
  - Java 1.8+

#### Compute nodes

- The compute nodes need access to shared storage for input, references, output
- The following commands need to be available in PATH on the compute nodes

  - `impute2` from [IMPUTE2](http://mathgen.stats.ox.ac.uk/impute/impute_v2.html)
  - `plink` from [PLINK 1.9+](https://www.cog-genomics.org/plink2)
  - `vcftools` from [VCFtools](https://vcftools.github.io/index.html)
  - `bcftools`from [bcftools](https://samtools.github.io/bcftools/bcftools.html)
  - `bgzip` from [htslib](http://www.htslib.org)
  - `eagle` from [Eagle](https://data.broadinstitute.org/alkesgroup/Eagle/)
  - `python2.7`

## Getting started

### Basic test
 1. Clone this repo
 2. Run the "tiny" dataset included
```
nextflow run imputation.nf -c nextflow.test.tiny.config
```
 3. check for results in `outfolder`
```
wc -l output/impute_results/FINAL_VCFS/*
```

### Larger dataset
 1. Download this slightly larger dataset: [small.tar.bz2](https://goo.gl/cYk51U) and extract into the `samples` folder
 2. Run this "small" dataset with
```
nextflow run imputation.nf -c nextflow.test.small.config
```
 3. check for results in `outfolder`
```
wc -l output/impute_results/FINAL_VCFS/*
```

### AWS Batch

This workflow can be run on AWS Batch, an Amazon service that streamlines the running of containerised workflows. Nextflow

1. Set up the AWS environment. The requirements are:
  - an AWS account with credits. See the [AWS grants page](https://aws.amazon.com/grants/) for grants
  - a configured Compute Environment and associated AWS Job Queue
  - a configured S3 bucket

2. Populate the `awsbatch` profile in 
```
aws.region = 'eu-west-1'
aws.client.storageEncryption = 'AES256'
process.queue = 'large'
executor.name = 'awsbatch'
executor.awscli = '/home/ec2-user/miniconda/bin/aws'
```
3. run with the `awsbatch` profile
```
nextflow run imputation.nf -c nextflow.test.small.config -profile awsbatch
```

### References

To cite this pipeline, please use:
Baichoo, S., Souilmi, Y., Panji, S., Botha, G., Meintjes, A., Hazelhurst, S., Bendou, H., Beste, E. de, et al. 2018. Developing reproducible bioinformatics analysis workflows for heterogeneous computing environments to support African genomics. BMC Bioinformatics. 19(1):457. DOI: [10.1186/s12859-018-2446-1](https://doi.org/10.1186/s12859-018-2446-1).
