# Workshop 4 (1st December 2023). Introduction to Regular Expressions

### Getting the right text editor
* Mac: TextWrangler / BBedit, ...
* Windows: Notepad++, .....
* Linux: Text Editor

### What is a text file?
* Wikipedia: "a kind of computer file that is structured as a sequence of lines of electronic text"
* File containing text organized in a human-readable way
* All lines carry information
* Compare TXT vs DOCX vs GZ!


### The organization of data within a text file
* In most cases, data is organized as character-delimited text.
    - Lines typically correspond to distinct observations/measurements
    - Within each line, sections correspond to different types of information
    - Delimiter: comma, space, tab...
    - Blocks of text are enclosed within quotation marks
    - Header
* Special formats: FASTA, FASTQ... suggestions of others?

### Terms and challenges
* Character types: word, numeric, whitespace, special...
* **End-of-line (EOL)** --- there is a character/characters for that! Represented by different character combinations on different platforms!
    - Linux: LF (line feed) "\n"  
    - some older Macs: CR (carriage return) "\r"  
    - Windows: LF followed by CR "\n\r"  
    - Be aware of that!  
  
### REGEX - Regular Expressions: a widespread language for search and replace
REGEX permits complex text manipulations across a wide range of environments using one skill set  
The easiest way is to get started in your REGEX-compatible text editor :)  

You can do basic search-replace in almost any text-editing software:
  * Find: A     ---> Replace: B  
  * Find: Word1 ---> Replace: Word2  
    
&nbsp;  
  
**But you can go so much further once you learn how to use wildcards and special characters in REGEX!**  
  * **\w** --- any letter (A-z) or digit (0-9) and the underscore character (_).
      * **\W** --- any character that is **not** a letter, digit, or underscore
  * **\d** --- any digit (0-9).
      * **\D** --- any character that is **not** a digit
  * **\t** --- tab characters
  * **\r**, **\n** --- End-of-line characters
  * **\s** --- any whitespace character (space, tab, end-of-line)   
      * **\S** --- any character that is **not** a whitespace character
&nbsp;  

Some more building blocks for REGEX: 
  * **.** --- Any letter, digit, or symbol, **except** end-of-line characters
  * **[]** --- making your own wildcards; matching **one** character inside the bracket
      * **[CGTAcgta]**, **[A-Z]**, **[A-z]**  --- **one** of the specified characters or characters in the range "-"  
      * **[^CGTAcgta]**, **[^A-Z]** --- anything **OTHER than** the defined characters 
  * **\\** ---  used to **escape** special (punctuation) characters
      * **\\.** --- an actual escaped dot, rather than any character!   
      * **\\\\** --- escaped "backslash" character :)  
  
&nbsp;  

We often want to search for multiple characters of the same type, like, multiple "A" characters!  
  * **A** --- searching for one A :)
  * **\d+** --- searching for **one or more** digits  
  * **\w*** --- searching for **zero or more** word characters  
  * **[AT]{10,20}** --- searching for a series of A **or** T characters of the length between 10 and 20  
  * **[A-Z]{3,}** --- searching for a series of at least three capital letters  

&nbsp;  

REGEX is greedy --- by default, the match is as long as possible. You can change it ---
  * **?** --- modifies greediness so that the match is as short as possible  

&nbsp;  

It can be helpful to nest your search term to the beginning or end of line:  
  * **^** --- beginning of line
  * **$** --- end of line

&nbsp;  

Capturing the search term and using it in replacements!
  * **()** --- captures the search term, making it available for use in replacements!
  * **\1**, **\2**...  --- when replacing, insert the first, second, (...) captured term
  
&nbsp;  

### REGEX - let's apply it to some real-life situations!
We will start during the class, and try to get as far as we can. Try to resolve the remaining examples as **homework**!


**Task 1. In a list of Dictyopharidae planthopper species names**,
```
Dictyophara europaea Linnaeus, 1767
Nersia florida Fennah, 1944
Ranissus edirneus Dlabola, 1957
Scolops abnormis Ball, 1902
Dictyophara nakanonis Matsumura, 1910
```
   - let's remove authority and year of description  
   - let's truncate generic names  
   - let's undo previous changes, and instead, put the authority and the year of description in bracket  

  
**Task 2. In the *Amplicon_library_list.txt***, 
```
(base) piotr.lukasik@azor:/mnt/qnap/users/symbio/raw_data/illumina/amplicon_sequencing/20231016_NextSeq_amplicon_workshop/GDF$ ls * | head
GDF0101_S377_R1_001.fastq.gz
GDF0101_S377_R2_001.fastq.gz
GDF0102_S378_R1_001.fastq.gz
GDF0102_S378_R2_001.fastq.gz
GDF0103_S379_R1_001.fastq.gz
GDF0103_S379_R2_001.fastq.gz
GDF0104_S380_R1_001.fastq.gz
GDF0104_S380_R2_001.fastq.gz
GDF0105_S381_R1_001.fastq.gz
GDF0105_S381_R2_001.fastq.gz
```
   - let's clean the names to keep only the sample names (GDF0101) and read number (R1). The output should change from, for example, "GDF0101_S377_R1_001.fastq.gz" to "GDF0101_R1.fastq.gz"
   - let's prepare a set of Linux commands for simplifying the file names, for example, "mv GDF0101_S377_R1_001.fastq.gz GDF0101_R1.fastq.gz".
   - let's construct the list of samples containing sample name as well as R1 and R2 read files. Let's reformat them to a tab-delimited list that R1 and R2 files corresponding to the same sample are in the same line, such as "GDF0101   GDF0101_R1.fastq.gz    GDF0101_R1.fastq.gz"


**Task 3. In the *Army_ant_COI_seqs.fasta***,
```
>KX983244.1 Eciton burchellii isolate 6 cytochrome oxidase subunit I (COI) gene, partial cds; mitochondrial
ATACTATACTTTATTTTTTCATTCTGAGCAGGAATATTAGGATCCTCAATAAGTATAATTATTCGCTTAG
AACTAGGAACATGTGGGTCCCTTCTTAATAACGACGCTGTATTCAACTCATTGATCACTAGACACGCATT
TGTAATAATTTTTTTTACAGTTATGCCTTTTATAATTGGGGGATTTGGAAATTTCTTAGTCCCATTAATA
CGGGGCTATTACTATACTTTTAACAGACCGCAATTTAAAT
>KX983246.1 Neivamyrmex opacithorax isolate CS197 cytochrome oxidase subunit I (COI) gene, partial cds; mitochondrial
ATACTATATTTTATCTTTTCATTTTGGGCCGGAATACTAGGATCATCTATAAGAATAATCATTCGATTAG
AACTAGGAACCTGTGGTTCTATAATTAATAACGAACAAGTATTCAACTCTATAATTACAAGACACGCATT
```
   - let's change the sequence organization so that the nucleotide sequences spread across multiple lines are always in one;
   - let's edit sequence names so that Accession_no, Genus, Species, and Isolate are separated by underscores, everything else removed


**Task 4. In the *Phorids_fastq_100_reads.fastq***,
```
@M01530:53:000000000-J8H3B:1:1101:16182:1770 1:N:0:508
AGTGTCAGCCGCCGCGGTAATACGGAGAGGGCTAGCGTTATTCGGAATTATTGGGCGTAAAGGGCGCGTAGGCTGGTTAATAAGTTAAAAGTGAAATCCCGAGGCTTAACCTTGGAATTGCTTTTAAGACTATTAATCTAGAGATTGAAAGAGGATAGAGGAATTCCTGATGTAGAGGTAAAATTCGTAAATATTAGGAGGAACACCAGTGGCGAAGGCGTCTATCTGGTTCAAATCTGACGCTGAAGCGCGAAGGCGTGGGGAGCAAACAGGATTAGAAACCCGCGTAGTCCAAGATCG
+
-6A@CFFFGGGGGEEEGGDGFFGGE>@FCFGGGGGGCGEEGGGG<:CFGFGG9F8EFGC:@E<C:=FGGDGGFGGCFGGGGGFFGGFGFGGGFGGGGGGGGGEGGGGGGGGGGFGDGGGFGGGGGGGDGGGGGGGGGGGGFGGFGGGFCCE,DCFFF7DC,F@FGGGGGFGGG;FAF@FGFFAFGGGGFGGGBFGGGFGFGCGGGGGGGGFGGEDGGGC8EGGGDGGCCCFGGCCEGGGFGDGGGGGGGGGCGGDGGGGDGGEG5BGGGGGG@@FFFFF6:FGGGGDFFFFFFFFF?FF<
@M01530:53:000000000-J8H3B:1:1101:20015:2113 1:N:0:508
ACCTGATATGGCCTTTCCTCGAATAAATAATGTAAGTTTCTGAATATTACCCCCATCTATCACTTTACTACTTTTAAGAAGAATAATAGAAAATGGAGCTGGAACAGGATGAACTGTTTACCCTCCTTTATCCGCCAATATTGCCCATAGAGGAACTTCTATTGATTCAGCTATTTTTTCTTTACATTTAGCTGGAATTTCCTCAATTCTAGGAGCTATCAATTTTATTTCAACAATTATTAATATACGCTCTTTAAAAATAACACTTGACCGAATACCTTTACTTGTATGATCAATTAG
+
CCCCCGGGGGFGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFGGGCCFGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFFGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGDFGGCGGGGGFGGGGGGGGGGGGDGGGGGGGGGCFGGGGGGGGGGGGGGGGGGGGFGGGGGGGGGGGGGGGFGGGGGGFGFFGDGGGGGGGGGGDGGGGGE5DFGGGFGGGGGFFFGGF??>B>?B?@FFGGFEGGCF8BAAFFFEFFF)
```
   - let's change the format from fastq to fasta
   - how many reads contain the forward primer sequence GTGYCAGCMGCCGCGGTAA? Note ambiguous positions... check [IUPAC codes](https://www.bioinformatics.org/sms/iupac.html) for the explanation
   - let's remove all the reads that don't have the primer!
   - let's trim from the reads the primer sequences (which can be preceded by up to three other nucleotides)


**Task 5. In the *Updated_cicada_collection_data.txt***,
```
Sample_no	Coll_date	Coll_location	Sex	Preservation	Genus	Tentative_morph_ID	Gene_Capture_ID																	
Claudio	30-Nov-14	Las Condes	F	ethanol	Tettigades	chilensis	chilensis																	
JM002	04-Jan-16	L1 Trapatrapa (Rio Queuco)	F	70% ethanol	Tettigades	ulnaria (23Mar16)	ulnaria																	
JM004	04-Jan-16	L1 Trapatrapa (Rio Queuco)	F	70% ethanol	Tettigades	undata (28June16)	undata
```
   - let's change the format of the collection date. It is currently "30-Dec-15" or similar; we want it to go like "2015-Dec-30".


**Task 6. Using *zOTU_table.txt*** as input,
```
TU_ID	OTU_assignment	Taxonomy	Sequence	Total	SEG1_AUC0407	SEG1_AUC0408	SEG1_AUC0409	SEG1_AUC0410	SEG1_AUC0449
Zotu1	otu1	Eukaryota(1.00),Diplocolenus_abdominalis(1.00)	ATTAAATAATATAAGATTTTGATTACTAC	154619	0	0	0	0	0
Zotu6	otu1	Eukaryota(1.00),Diplocolenus_abdominalis(1.00)	ATTAAATAATATAAGATTTCGATTACTAC	8734	0	0	0	0	0
Zotu17	otu1	Eukaryota(1.00),Diplocolenus_abdominalis(0.65)	ATTGAATAATATTAGATTTTGATTACTAC	1163	0	0	0	0	0
Zotu2	otu2	Eukaryota(1.00),Verdanus(0.93),Verdanus_evansi(0.93)	ATTGAATAATATAAGATTTTGATTACTACCGCCATCACT	14014	0	0	3846	0	0
```
   - export sequences in the fasta format, with headings containing information about zOTU, OTU, and total read number:
```
>Zotu1_otu1_reads-154619
ATTAAATAATATAAGATTTTGATTACTAC
```

