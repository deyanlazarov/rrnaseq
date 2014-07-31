

###INTRODUCTION
rrnaseq provides a suite of programs to generate basic plots as well as QC-filtering of RNA-seq data. The programs are written in R and are executable from the command-line. It also provides a script that can run the whole suite of programs, called rqc. All programs can be found in the 'bin' sub-directory.

Currently the starting input is a tab-separated file with RPKM values and raw read counts output by rpkmforgenes.py. Most programs also require a file with meta-information about the samples, which can be generated by running 'make_summary_starlog.sh', see the "HOW TO RUN" section below.



###INSTALLATION
1. Install R dependencies with install.packages or via biocLite. In R:

		pkgs = c('DESeq2', 'genefilter', 'statmod', 'gplots',
		'RColorBrewer', 'impute', 'moduleColor', 'graphics', 'getopt')
		source('http://www.bioconductor.org/biocLite.R')
		biocLite(pkgs)

2. Add the directory with binaries to your shell path (to for example .profile on OS X or .bashrc in Linux):
```Shell
export PATH="/home/user/prg/rrnaseq/bin:$PATH"
```



###HOW TO RUN
Below you find an example of how to run the three first criticial steps
(make_summary_starlog.pl, get_meta, get_expr). 'get_expr' and 'get_meta', take the output from the programs 'rpkmforgenes.py' and 'make_summary_starlog.sh' as input and generates data matrices with expression values and a matrix with meta-information about the samples, respectively, and store these as separate files in rds-format. All other programs rely on
the output from get_meta and get_expr and are straightforward to
use, see for example the 'mapstats' call below.

```Shell
#Define input and output dirs and files
#IN
projectdir='/path/to/your/PROJECT'
stardir=${projectdir}'/star_hg19'
rpkmforgenes_file=${projectdir}/rpkmforgenes_star_hg19/refseq_rpkms.txt

#OUT
datadir=${projectdir}/'rqc/refseq/data'
pdfdir=${projectdir}/'rqc/refseq/pdf'
brenneckedir=${projectdir}'/rqc/diffexp/brennecke'

#Create and change dir
mkdir -p $datadir
cd $datadir

#Get mapping statistics from STAR logs
make_summary_starlog.pl ${stardir} >mapstats..tab

#Get sample meta-information
get_meta -i mapstats.tab -o meta.rds

#Get expression values
get_expr -i $rpkmforgenes_file -o $datadir

#Plot mapping stats
mapstats -m meta.rds -c counts.rds -o ${pdfdir}/'seq_mapping' -q qc.rds

#Dry-run the program 'rqc' to generate a shell script with possible commands to execute
rqc -m mapstats.tab -e $rpkmforgenes_file -d $datadir -p $pdfdir -b $brenneckedir -y
cat rqc.sh
```

#####Further examples
Above, the program 'rqc' was dry-run to generate a shell script (rqc.sh) with possible commands to execute. Look in rqc.sh and change or add input arguments as you wish.

You can also see test/rqc.sh for a complete list of available programs and
example program calls, but there the directories are set according to the test directory.



###TEST
The file 'run.rqc.sh' in the 'test' subdirectory provides an example of how to run the script 'rqc' that with the dry-run flag will generate a file (rqc.sh) with commands that calls all of the available programs in the rrnaseq suite.. See the 'run.rqc.sh' and the generated 'rqc.sh' file for a test example:

```Shell
cd test
sh run.rqc.sh 
cat rqc.sh
```



###QC-filter
To filter genes use the program 'gene_filter'. To filter samples use the program 'sample_filter'. This program relies on an input file (default: qc.rds), which contains a data matrix with all samples as rows and different qc-metrics as columns. Elements in this qc-matrix is set to 1 if a sample failed QC for a particular QC-metric. The QC-metric columns of the qc-matrix is added when running the corresponding program, for example, if you want to add a QC-column relating to the number of expressed genes per sample, run the program 'sample2ngenes_expr'. To then apply the filter run 'sample_filter' with the column-name of that QC-metric as an argument. See test/rqc.sh for an example.



###GETTING HELP
Each program have several input arguments that should be considered. For a list of all available arguments for a program use the -h flag, for example:
```Shell
pca -h
```
