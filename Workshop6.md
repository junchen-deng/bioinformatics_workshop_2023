# Workshop 6 (15th December 2023). Biological research using Unix and REGEX - some useful tools :)

### 6.1. Last week's REGEX exercises: how did you do?
&nbsp;  



### 6.2. rename
We normally change file or directory names using the command **mv**.
But what if you want to change the names of many files at once? There is a power tool for that!
Most modern Linux distributions ship with **rename** command by default - or you can install it on your system.
&nbsp;   
The command **rename** uses a syntax similar to **sed** to change file names:
```
rename 's/OLD/NEW/' *txt
```
&nbsp;   
For example, 
```
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ ls *9080*
BEE9080_R1.fastq  BEE9080_R2.fastq
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ rename 's/BEE/ANT/' *9080*
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ ls *9080*
ANT9080_R1.fastq  ANT9080_R2.fastq
```
&nbsp;   
Remember that once you change names, you cannot undo the changes easily! Then, before you run the command on many different files, you should run it with argument **-n**, which displays old and new names without actually changing them. When changing, consider using the **-v** argument, which informs you how the names were changed!
```
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ ls file*
file1.txt  file2.txt
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ rename -n 's/.txt/.pdf/' file*
rename(file1.txt, file1.pdf)
rename(file2.txt, file2.pdf)
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ rename -v 's/.txt/.pdf/' file*
file1.txt renamed as file1.pdf
file2.txt renamed as file2.pdf
(base) piotr.lukasik@azor:~/workshops/workshop_20231215/reads$ ls file*
file1.pdf  file2.pdf
```
&nbsp;   

Files that we receive from the sequencing facility at Malopolska Center of Biotechnology have names such as "BEE9081_S1081_R1_001.fastq" - 
where the first portion is the sample name (useful!), second - number on the list sent to the sequencing facility (not useful!), third - read number (useful!), and fourth - the same number for all samples (not useful!). 
How would you change the names of twenty files (at /mnt/qnap/users/symbio/workshops/workshop_20231215/reads/, or in files on Teams) to eliminate the second and fourth portion of the name - to obtain "BEE9081_R1.fastq"?  
&nbsp;  

### 6.3. BLAST
BLAST - that is an acronym for **Basic Local Alignment Search Tool** - a sequence comparison software developed by NCBI:[https://blast.ncbi.nlm.nih.gov/Blast.cgi](https://blast.ncbi.nlm.nih.gov/Blast.cgi)
BLAST finds regions of similarity between biological sequences. The program compares nucleotide or protein sequences to sequence databases and calculates the statistical significance.

There are several different tools within BLAST - **blastn**, **blastp**, **blastx**, **tblastn**...
And different search algorithms are available - **blastn**, **megablast**, ...

Most of us have probably used the web portal. However, blast can be an extremely useful command-line tool, installed on all our clusters.

Before running command-line blast, you need your "query" fasta file, and your "database" fasta file. You can use an existing database - like our COI sequence database at **/mnt/qnap/users/symbio/software/databases/MIDORI_with_tax_spikeins_endo_RDP.fasta**

Or, you can make your custom "database" into a BLAST-recognizable database using **makeblastdb**

```
makeblastdb -in EUROBEE_db.fasta -dbtype nucl
```

Now, you are ready to do your searches! Basic syntax ---
```
blastn -query one_BEE_barcode.fasta -db /mnt/qnap/users/symbio/software/databases/MIDORI_with_tax_spikeins_endo_RDP.fasta > one_BEE_barcode.blastn
```

But the results are not easy to process... thankfully, there are many parameters for you to play with!

```
(base) piotr.lukasik@azor:~/workshops/workshop_20231215$ blastn --help
USAGE
  blastn [-h] [-help] [-import_search_strategy filename]
    [-export_search_strategy filename] [-task task_name] [-db database_name]
    [-dbsize num_letters] [-gilist filename] [-seqidlist filename]
    [-negative_gilist filename] [-negative_seqidlist filename]
    [-taxids taxids] [-negative_taxids taxids] [-taxidlist filename]
    [-negative_taxidlist filename] [-entrez_query entrez_query]
    [-db_soft_mask filtering_algorithm] [-db_hard_mask filtering_algorithm]
    [-subject subject_input_file] [-subject_loc range] [-query input_file]
    [-out output_file] [-evalue evalue] [-word_size int_value]
    [-gapopen open_penalty] [-gapextend extend_penalty]
    [-perc_identity float_value] [-qcov_hsp_perc float_value]
    [-max_hsps int_value] [-xdrop_ungap float_value] [-xdrop_gap float_value]
    [-xdrop_gap_final float_value] [-searchsp int_value] [-penalty penalty]
    [-reward reward] [-no_greedy] [-min_raw_gapped_score int_value]
    [-template_type type] [-template_length int_value] [-dust DUST_options]
    [-filtering_db filtering_database]
    [-window_masker_taxid window_masker_taxid]
    [-window_masker_db window_masker_db] [-soft_masking soft_masking]
    [-ungapped] [-culling_limit int_value] [-best_hit_overhang float_value]
    [-best_hit_score_edge float_value] [-subject_besthit]
    [-window_size int_value] [-off_diagonal_range int_value]
    [-use_index boolean] [-index_name string] [-lcase_masking]
    [-query_loc range] [-strand strand] [-parse_deflines] [-outfmt format]
    [-show_gis] [-num_descriptions int_value] [-num_alignments int_value]
    [-line_length line_length] [-html] [-sorthits sort_hits]
    [-sorthsps sort_hsps] [-max_target_seqs num_sequences]
    [-num_threads int_value] [-mt_mode int_value] [-remote] [-version]

DESCRIPTION
   Nucleotide-Nucleotide BLAST 2.14.1+
```

One of the most useful is **-outfmt** --- and I virtually always run my searches with `-outfmt 6` argument. Let's run it, display the top lines ...
```
(base) piotr.lukasik@azor:~/workshops/workshop_20231215$ blastn -query one_BEE_barcode.fasta -db /mnt/qnap/users/symbio/software/databases/MIDORI_with_tax_spikeins_endo_RDP.fasta -outfmt 6 | head
BEE4001	KT074052.1.<1.>688;tax=d:Eukaryota,p:Arthropoda,c:Insecta,o:Hymenoptera,f:Halictidae,g:Lasioglossum,s:Lasioglossum_calceatum	100.000	418	0	0	1	418	255	672	0.0	773
BEE4001	KJ839535.1.<1.>658;tax=d:Eukaryota,p:Arthropoda,c:Insecta,o:Hymenoptera,f:Halictidae,g:Lasioglossum,s:Lasioglossum_calceatum	100.000	418	0	0	1	418	241	658	0.0	773
BEE4001	JQ909766.1.<1.>654;tax=d:Eukaryota,p:Arthropoda,c:Insecta,o:Hymenoptera,f:Halictidae,g:Lasioglossum,s:Lasioglossum_albipes	100.000	418	0	0	1	418	231	648	0.0	773
BEE4001	JQ909752.1.<1.>654;tax=d:Eukaryota,p:Arthropoda,c:Insecta,o:Hymenoptera,f:Halictidae,g:Lasioglossum,s:Lasioglossum_albipes	100.000	418	0	0	1	418	231	648	0.0	773
BEE4001	JQ909765.1.<1.>654;tax=d:Eukaryota,p:Arthropoda,c:Insecta,o:Hymenoptera,f:Halictidae,g:Lasioglossum,s:Lasioglossum_albipes	99.761	418	1	0	1	418	231	648	0.0	767
```
...and see how to interpret the results, and customize the output! [https://www.metagenomics.wiki/tools/blast/blastn-output-format-6](https://www.metagenomics.wiki/tools/blast/blastn-output-format-6)
&nbsp;   

Other useful arguments:
* task
* evalue
* max_target_seqs
* max_hsps
* perc_identity
* num_threads
&nbsp;   

Let's play with them!

Nowadays, with huge amounts of sequencing data and big databases, one of the problem with BLAST is its speed. BLAST can be super slow when searching for many sequences in a huge database (using more cores doesn't seem to make it faster). One alternative for BLAST is **DIAMOND** (https://github.com/bbuchfink/diamond), which is a re-write of BLAST targeting protein and translated DNA searches. In other words, DIAMOND can replace blastx and blastp with a 100-fold increase in running speed.    

### 6.4. screen
Typically, a Unix session lasts for as long as you have an active connection to the cluster. If your job takes 24h, and you want to go home and bring your laptop with you at some point, you have a problem...   
**screen** software allows you to create and manage virtual Unix sessions. Check out the tutorial at [https://linuxize.com/post/how-to-use-linux-screen/](https://linuxize.com/post/how-to-use-linux-screen/)
&nbsp;   

```
screen --version       # check if the software works!
screen -S MySession    # starts a new virtual session
Ctrl + a,  ?           # lists your various options
Ctrl + a d             # leaves the session
screen -ls             # lists active sessions
screen -r MySession    # rejoins an existing session
screen -rd MySession   # rejoins an existing session, forcing the software to 
exit                   # kills the virtual session
Ctrl + a, :quit        # kills the virtual session
```  
&nbsp;  


### 6.5. conda
Conda is a package manager and environment manager that you use within command line environment. 
Check [https://conda.io/projects/conda/en/latest/user-guide/getting-started.html](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html) for a tutorial!
It is installed on our computing clusters.
  
Follow the instructions in the command prompt on how to install!

```
conda info --envs         # lists available environments
conda activate bio        # starts bio environment
conda deactivate          # deactivates environments
```
&nbsp;

  
