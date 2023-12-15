# Workshop 6 (15th December 2023). Unix & REGEX in the real world - some useful tools :)

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



### 6.3. screen
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


### 6.4. conda
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

  
### 6.5. BLAST
BLAST - that is an acronym for **Basic Local Alignment Search Tool** - a sequence comparison software developed by NCBI:[https://blast.ncbi.nlm.nih.gov/Blast.cgi](https://blast.ncbi.nlm.nih.gov/Blast.cgi)
BLAST finds regions of similarity between biological sequences. The program compares nucleotide or protein sequences to sequence databases and calculates the statistical significance.
  
