# SV Genotyping in the Single Cell Pipeline
One analysis we are interested in having in the single cell pipeline is structural variant (SV) genotyping. We currently have a solution, but it doesn't match expectations on datasets with reasonably sure ground truths. Your task will be to explore other possible SV genotypers, compare them to our current solution with test datasets and improve the current solution in the single cell pipeline.

Some useful links.

1. Running Svtyper on the commandline to 

A good first step will be to run svtyper from the commandline using a docker container to get familiar with the expected inputs and outputs of an sv genotyper. 
all the data you will need is in `/juno/work/shah/abramsd/sv-genotyping-hackathon`.
Follow these steps to run svtyper:
```buildoutcfg
module load singularity

singularity run --bind /admin --bind /common --bind /juno/work  docker://docker.io/singlecellpipeline/svtyper:v0.0.1 svtyper -B SA535X4XB05649-A98261A-R06-C22.bam -i SA535X4XB05649-A98261A-R06-C22_lumpy.vcf -o genotyped.vcf
```
The genotypes for the data are in `genotyped.vcf`. 

2. Comparing the results to our expectations

In the same directory, there is also `compare.py`, run it with the outputs from the previous run and the `lumpy_evidence` file, which contains the number of reads for each variant at every cell, to compare the read evidences provided originally by lumpy and then given by svtyper. This is how to check if an sv genotyper is working.

output:
```buildoutcfg
    breakpoint_id                         cell_id  count  svtyper_evidence
0               4  SA535X4XB05649-A98261A-R06-C22      2                 1
1              69  SA535X4XB05649-A98261A-R06-C22      1                 3
2             103  SA535X4XB05649-A98261A-R06-C22      1                 0
3             114  SA535X4XB05649-A98261A-R06-C22      2                 1
4             378  SA535X4XB05649-A98261A-R06-C22     52               245
..            ...                             ...    ...               ...
173          8974  SA535X4XB05649-A98261A-R06-C22      1                38
174          8993  SA535X4XB05649-A98261A-R06-C22      6               172
175          9001  SA535X4XB05649-A98261A-R06-C22      1                36
176          9009  SA535X4XB05649-A98261A-R06-C22      1                83
177          9015  SA535X4XB05649-A98261A-R06-C22      1                65
```
As you can see, theres an incredible amount of discordance between lumpy's evidence (count) and svtyper's evidence. We need to figure out why and test alternate tools.

3. testing other tools

In particular, there are two additional tools we would like you to test.
Stix: `https://github.com/ryanlayer/stix`
Delly: `https://github.com/dellytools/delly`

run these methods on the data given and any test data they provide. Then compare the results to our expectations in a manner similar to above.


4. Working on the SV genotyping pipeline

We currently have a semi-working implementation of sv genotyping with svtyper in the single cell pipeline.

Follow the directions for setup under the header `SV_Genotyping` on this documentation page: 
`https://github.com/DouglasAbrams/single_cell_pipeline/blob/docs_merge/docs/source/index.md`. You can skip the steps regarding setting-up of reference data. 

## Additional references

Paragraph: a graph based sv genotyper.  Untested on cancer genomes. From illumina.
https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1909-7

vg toolkit: another graph based sv genotyper using a graph toolkit.
https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1941-7

SMRT-SV: a long read sv caller that also has a genotyper for short reads.
https://github.com/EichlerLab/smrtsv2


