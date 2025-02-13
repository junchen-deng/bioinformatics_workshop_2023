# Workshop P4 (12th April 2024). Modules and output formatting.
  
### P4.1. Homework --- how did you do?

1. The example discussed above works when each sequence is provided as a single line. But in a large proportion of fasta files, the sequence is broken up across multiple lines:
```
>RealWorldSequence1
ACTGTCATGTCATGTTGTCATGTCATGAT
TCGATAATGACATGTCATGATTGTCAT
```  
But we can deal with that! Write a script that converts fasta files with sequences broken up across multiple lines into files where each sequence is in a single line.
  
&nbsp;  
2. Last week, we wrote a script that computes basic statistics (length, count of Ns, GC%) for a single user-provided nucleotide sequence. Rewrite the script so that it summarizes all sequences in a multifasta file.  
  
&nbsp;  
3. Write a script that converts sequences in the `fastq` format to `fasta` format.  
  
&nbsp; 
  
### P4.2. Modules

Python3 in its basic mode provides lots of tools and options... But sometimes you'll want to use a bit more!
You can substantially increase the functionality by importing `modules` - additional sets of tools.  
&nbsp;
Some of them are provided essentially by default with python3. Three such modules I use often:  
* [**sys** - System-specific parameters and functions](https://docs.python.org/3/library/sys.html)   
* [**os** - Miscellaneous operating system interfaces](https://docs.python.org/3/library/os.html)  
* [**re** - Regular expression operations](https://docs.python.org/3/library/re.html)  

&nbsp; 
Others need to be installed. Some of them can be easily installed with the command **pip install [package name]**, while others can be tricky as they may require additional permissions, dependencies, etc.   

&nbsp; 
##### Example: sys - working with file names provided as arguments 
```
import sys     # imports module - the tools within sys become available!

   ### This statement allows you to use arguments provided at the time the program is run
   ### as variables within your script! In this example, there are two of them.
if len(sys.argv) != 3:
    sys.exit('\nThis script imports an input_file and exports an output_file. Please provide file names.\n'
	           'Usage: ./script.py <input_file> <output_file>\n')   
Script, path_to_input_file, path_to_output_file = sys.argv

INPUT = open(path_to_input_file, "r")
OUTPUT = open(path_to_output_file, "w")
  
```  
&nbsp;  
Let's now edit our Homework scripts following the examples above so that they work with input files provided by the user!    
&nbsp;  
##### Example: os - working with folders and files, like what you do in the shell
```
>>> import os
>>> os.system("ls -l")
total 140
-rw-rw-rw- 1 piotr.lukasik users 62826 Jun 10 05:34 Army_ant_COI_seqs.fasta
drwxr-xr-x 2 piotr.lukasik users  4096 Jun 10 05:53 test_dir
0
>>> os.system("mv test_dir test_dir2")
0
>>> os.path.exists("test_dir")
False
>>> os.path.exists("test_dir2")
True
>>> if not os.path.exists("test_dir"):
...     os.system("mkdir test_dir")
... 
0
>>> os.system("ls -l")
total 144
-rw-rw-rw- 1 piotr.lukasik users 62826 Jun 10 05:34 Army_ant_COI_seqs.fasta
drwxr-xr-x 2 piotr.lukasik users  4096 Jun 10 05:57 test_dir
drwxr-xr-x 2 piotr.lukasik users  4096 Jun 10 05:53 test_dir2
```
A more powerful improved module that replaces **os** is [**subprocess**](https://docs.python.org/3/library/subprocess.html#module-subprocess), which can **manage the output** from the shell script within Python. If you have needs, take a look at it! Sometimes, executing shell scripts in Python makes it faster and easier to achieve your goal!  

&nbsp; 
### P4.3 Functions
A function is a block of code which only runs when it is called. Functions help to streamline your code when you need to repeatedly execute the same block of code.  
In Python, a function is defined using the **def** keyword:
```
def mystats(seq):
    print('Length: ', len(seq))
    print('Count of Ns: ', seq.count('N'))
    print('GC%: ', (seq.count('C') + seq.count('G'))/len(seq))

mystats('AGTGCGGGTATATCGTTGGGCTATCGATCAGTCAGTACGT')
```
&nbsp; 
You can also return the value by using the **return()** statement: 
```
def ImportFasta(fasta_file):    # a function imports and saves a fasta file as a list of list 
    with open(fasta_file, 'r') as FASTA:
        Seq_list = []
        Seq = ''
        for line in FASTA: 
            if line.startswith('>'): 
                if Seq != '':    # save the existing header and sequence to a list before overwriting
                    Seq_list.append([Seq_heading, Seq])
                    Seq = ''
                Seq_heading = line.strip('\n')
            else:
                Seq += line.strip('\n').upper()  

        Seq_list.append([Seq_heading, Seq]) # save the last sequence
    return(Seq_list)

Sequence_list = ImportFasta(input_file)
```
&nbsp; 
### P4.4. Formatting output

How to make your text pretty --- or just organized in the way it needs to be? Let's look at alternative ways:)

```
>>> FIRST_NAME = "John"
>>> AGE = 25
>>> 
>>> 
>>> print("Your name is", FIRST_NAME, ", you are", AGE, "years old.")
Your name is John , you are 25 years old.
>>>
>>> print("Your name is ", FIRST_NAME, ", you are ", AGE, " years old.", sep = "", end = "\n") # changing the separator from the default
Your name is John, you are 25 years old.
>>>
>>> print("Your name is %s, you are %s years old" % (FIRST_NAME, AGE))
Your name is John, you are 25 years old
>>>
>>> print("Your name is {}, you are {} years old".format(FIRST_NAME, AGE))
Your name is John, you are 25 years old
>>>
>>> print(f"Your name is {FIRST_NAME}, you are {AGE} years old.")    #only available for Python v3.6 or more
Your name is John, you are 25 years old.
```
You can find more info about these methods online, for example at [https://realpython.com/python-string-formatting/#4-template-strings-standard-library](https://realpython.com/python-string-formatting/#4-template-strings-standard-library).  
Some methods also allow you to format values, which can be a handy way of making your output more pretty :)  
```
>>> My_value = 0.12345
>>> 
>>> print("%f" % My_value)
0.123450
>>> print("%.2f" % My_value)   # Two digits after the dot!
0.12
>>> print("%8.2f" % My_value)   # Two digits after the dot, the number should occupy eight spaces in total!
    0.12
```

Text formatting can be very helpful when managing file/sample names for many different purposes. For example, when using the `os.system()` function to feed information directly to the shell!
