# CensusTMT2MSstatsTMT
Converter from Census TMT output file to the input of MSstatsTMT. 
  
The input file is the PSM-level census output file with TMT intensities information.  
This tool will read the input file and will generate a peptide-level text file that can be used with [MSstatsTMT](http://msstats.org/msstatstmt/). 
In [here](https://raw.githubusercontent.com/proteomicsyates/CensusTMT2MSstatsTMT/master/about%20MSstatsTMT/Install%20required%20packages_MSstatsTMT.R), we have included a R script to install the required libraries to use with MSstatsTMT. Also [here](https://raw.githubusercontent.com/proteomicsyates/CensusTMT2MSstatsTMT/master/about%20MSstatsTMT/MSstatsTMT_analysis_example.R), you will find an example of the R commands you will need to execute to perform the analysis with [MSstatsTMT](http://msstats.org/msstatstmt/) with the output generated by this converter. 
  
This tool can be executed in command line, or with a graphical interface, when using parameter *-gui* (included in batch files *START_win.bat* and *START_mac_linux.sh*).  
The GUI version is built automatically from the command line version to give graphical support to the options in the command line (implementing class [*CommandLineProgramGuiEnclosable*](https://github.com/proteomicsyates/utilities/blob/master/src/main/java/edu/scripps/yates/utilities/swing/CommandLineProgramGuiEnclosable.java)).

Both versions are available to download at: http://sealion.scripps.edu/CensusTMT2MSstatsTMT/  
  
Command line options:  
```
gui version usage: java -jar CensusTMT2MSstatsTMT -gui  
  
command line usage: java -jar CensusTMT2MSstatsTMT -i [input file] -an [annotation file]

 -an,--annotation <arg>      Path to the experimental design file.
 -d,--decoy <arg>            [OPTIONAL] Remove decoy hits. Decoys hits
                             will have this prefix in their accession
                             number. If not provided, no decoy filtering
                             will be used.
 -i,--input <arg>            Path to the input file.
 -m,--minPeptides <arg>      [OPTIONAL] Minimum number of peptides+charge
                             per protein. If not provided, even proteins
                             with 1 peptide will be quantified
 -ps,--psm_selection <arg>   [OPTIONAL] What to do with multiple PSMs of
                             the same peptide (SUM, AVERAGE or HIGHEST).
                             If not provided, HIGHEST will be choosen.
 -r,--raw                    [OPTIONAL] Use of raw intensity. If not
                             provided, normalized intensity will be used.
 -u,--unique                 [OPTIONAL] Use only unique peptides. If not
                             provided, all peptides will be used.
Contact Salvador Martinez-Bartolome at salvador at scripps.edu for more help
```
To know more about the annotation file, go to http://msstats.org/msstatstmt/  

The annotation file is a COMMA-SEPARATED file (CSV) containing the information about the experimental design.  
The file should have the following columns:

| Column | Explanation | 
| ------ | :---------- |
| Run | MS run ID. It should correspond to the column *Filename* in census out file.|
| Channel | Labeling information (126, … 131). It should only numbers and be defined in a way that being sorted correspond to either TMT-6plex, TMT-10plex or TMT-11plex in the  census file **(*)** .|
| Condition | Condition (ex. Healthy, Cancer, Time0). If the channel doesn’t have sample, please add Empty under Condition.|
| Mixture | Mixture of samples labeled with different TMT reagents, which can be analyzed in a single mass spectrometry experiment.|
| TechRepMixture | Technical replicate of one mixture. One mixture may have multiple technical replicates. For example, if TechRepMixture = 1, 2 are the two technical replicates of one mixture, then they should match with same Mixture value.|
| Fraction | Fraction ID. One technical replicate of one mixture may be fractionated into multiple fractions to increase the analytical depth. Then one technical replicate of one mixture should correspond to multuple fractions. For example, if Fraction = 1, 2, 3 are three fractions of the first technical replicate of one TMT mixture of biological subjects, then they should have same TechRepMixture and Mixture value.|
| BioReplicate | Unique ID for biological subject. If the channel doesn’t have sample, please add Empty under BioReplicate.|

**(*)** It doesn't matter which numbers you state as Channel. The only requirement is to be as many different numbers as the TMT-plex you used in Census. The map between this column and the channels in the input file will be done by sorting the values on the channel column and mapping them to the sorted channels in the census file. For example: 
  
| Channel in annotation file | TMT channel in census.out |
| ------- | ------------------------- |
| 126 | 126.127726 |
| 127.12 | 127.124761 |
| 127.13 | 127.131081 |
| 128.12 | 128.128116 |
| 128.13 | 128.134436 |
| 129.131 | 129.131471 |
| 129.137 | 129.13779 |
| 130.13 | 130.134825 |
| 130.14 | 130.141145 |
| 131 | 131.13818 |

Here you have a couple of examples of annotation files:  
[annotation file example 1](https://raw.githubusercontent.com/proteomicsyates/CensusTMT2MSstatsTMT/master/about%20MSstatsTMT/Annotation_valid_example1.csv)  
[annotation file example 2](https://raw.githubusercontent.com/proteomicsyates/CensusTMT2MSstatsTMT/master/about%20MSstatsTMT/Annotation_valid_example2.csv)
