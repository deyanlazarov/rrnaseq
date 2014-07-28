

###INTRO
rrnaseq provides a suite of programs to generate basic plots as well as QC-filtering of RNA-seq data. The programs are written in R and are executable from the command-line. It also provides a script that can run the whole suite of programs, called rqc. All programs can be found in the 'bin' sub-directory.

Currently the starting input is a tab-separated file with RPKM values and raw read counts output by rpkmforgenes.py. Most programs also require a file with meta-information about the samples, which can be generated by running 'make_summary_starlog.sh', see the "HOW TO RUN" section below.



###INSTALLATION
1. Install R dependencies with install.packages or via biocLite. In R:

		pkgs = c('DESeq2', 'genefilter', 'statmod', 'gplots', 'RColorBrewer', 'impute', 'moduleColor', 'graphics')
		source('http://www.bioconductor.org/biocLite.R')
		biocLite(pkgs)

2. Add the directory with binaries to your shell path (to for example .profile on OS X or .bashrc in Linux):
export PATH="/home/user/prg/rrnaseq/bin:$PATH"



###HOW TO RUN
The file 'run.rqc.sh' in the 'test' subdirectory provides an example of how to run the rqc script that with the dry-run flag will generate a file (rqc.sh) with commands that calls all of the available programs in the rrnaseq suite. Most of the programs rely on a data matrix with expression values as well as a matrix with meta-information about the samples, stored as separate files in rds-format. These rds-files are generated by the two programs 'get_expr' and 'get_meta', which take take the output by rpkmforgenes.py and make_summary_starlog.sh as input. See the 'run.rqc.sh' and the generated 'rqc.sh' file for a test example:

```bash
cd test
sh run.rqc.sh 
cat rqc.sh
```


###GETTING HELP
For a list of all available arguments for a program use the -h flag, for example:
```bash
pca -h
```
