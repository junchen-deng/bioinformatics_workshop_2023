# Workshop 7 (8th March 2024). recap what we learned in Linux!

### Workshop 1. Introduction to Linux
* What is Linux/Unix?
* Basic commands: **pwd**, **ls**, **cd**, **history**, **mkdir**, **mv**, **cp**, **rm**, **man**, **--help**

### Workshop 2. Working with text files
* Folder and file permissions: **chmod**
* Displaying file contents: **less**, **nano**
* Redirecting output: **>**, **>>**
* Joining and Displaying files: **cat**, **head**, **tail**, **wc**
* Searching text files using **grep**
* "pipe" the output of one command as the input to another: **|**

### Workshop 3. Advanced tools
* Search and replace: **sed**
* Editing tables: **cut**, **sort**, **uniq**, **diff**
* Loops: **for [variable] in [list of variables]; do task1; task2...; done**
* The location of programs: **whereis**, **whichis**, **$PATH**, **.bashrc**
* Shell scripts: **#! /bin/bash**

### Workshop 4-5. Regular Expressions
* **REGEX** -- A language for search and replace
* Regular expression within grep and sed: **egrep**, **sed -r**

### Workshop 6. Useful UNIX/REGEX tools for biological research
* Change the names of multiple files: **rename**
* **BLAST** -- **makeblastdb**, **blastn**, **blastx**, **blastp**....
* Running jobs virtually: **screen**
* Creating an independent environment: **conda**

## Let's use all the tools to solve some real-life problems!
We received some data from amplicon sequencing recently. The files are stored in **/mnt/qnap/users/symbio/workshops/workshop_20240308**
* What kind of files are they?
* Please copy all files to your home directory. It's better to have a backup instead of working on the original raw data.
* How many files are in this folder? How many reads are in each file?
* The file name is too messy. We only need the key information, which is the sample ID (e.g. IPA0201) and the pair ID (R1 or R2). Let's change the name for all files!
'''
Before: IPA0261_S445_L001_R1_001.fastq
After: IPA0261_R1.fastq
'''
