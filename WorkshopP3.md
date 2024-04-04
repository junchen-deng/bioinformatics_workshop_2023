# Workshop P3 (27th May 2021). Working with files!
  
### P3.1. Homework --- how did you do?
"write a script that would compute basic statistics for a user-provided sequence: length, count of Ns, and GC%."
* How did you organize it?  
  
The priority is that the script works! Does it? Great job :)
But once it does, we should look for ways of simplifying it and preventing the computer from doing more work than necessary. What are the possible points for trimming it down, in?  
&nbsp;  
* Perhaps you are repeatedly doing the exact same job, for example counting Cs in the same sequence? If so, it would be preferable to count Cs once, assign the result to a variable, and refer to that variable in subsequent steps.
```
count_C = InputSeq.count("C")   #defining the variable
print("C count is ", count_C)   #using the variable in some context
GC% = (count_C + count_G) / ... #using the same variable in another context
```  
&nbsp;   
* If you are using the result of a certain computation just once, perhaps using the output immediately, without saving it as a variable, is a good idea?
```
print(InputSeq.count("C"))   # immediately printing the result of counting Cs in a sequence
```
&nbsp;  
* Rather than searching the string for one type of character, then for another, then for yet another, etc., consider scrolling through the sequence just once, classifying each character.
```
Seq_dict = {'C': 0, 'G': 0, 'A': 0, 'T': 0, 'N': 0}
for base in InputSeq:
    if base in 'ACGTN':
        Seq_dict[base] += 1
```
&nbsp;  
* Consider safeguarding against user error. User might provide the sequence using lowercase characters - converting it to uppercase might help! Or if the provided string contains illegal characters such as "qwerty123" you may want to refuse the computation!
```
InputSeq = input("Input sequence here: ").upper()

for base in InputSeq:
    if not base in 'ACGTN':
        print('illegal characters exist in the input sequence! Please check!')
        break
```
**Bonus**: An advanced way to raise the custom error messages with **try - except - else**
```
try:    # literally means try this block of scripts
    if [checkpoint for potential errors]:
        raise Exception
except Exception:    # if errors are raised, then...
    [error message]
else:    # if no errors are raised, then...
    [the rest of the scripts]
```
Example from the homework:
```
try:
    InputSeq = input("Input sequence here: ").upper()
    for base in InputSeq:
        if base not in 'ACGTN':
            raise Exception
except Exception: 
    print('[Error] Illegal characters detected in the input sequence')   
else: 
    print('Length: ', len(InputSeq))
    print('Count of Ns: ', InputSeq.count('N'))
    print('GC%: ', (InputSeq.count('C') + InputSeq.count('G'))/len(InputSeq))
```


&nbsp;  
&nbsp;  
### P3.2. Built-in functions and methods on objects
We have learnt how to convert string to upper case - `MYSTRING = MyString.upper()` - but this is only one of many methods available on our objects. You can display the list of object properties and methods using the `dir()` function:
```
>>> MyString = "Hello world!"
>>> dir(MyString)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```  
&nbsp;  
Those without double underscores are generally used as methods, and are preceded by a dot immediately after a variable name. You can apply multiple methods at the same time. To learn about specific methods, use your Google strategically :)
```
>>> MyString = "Hello world!"
>>> MyString.upper().                    # .upper() changes string to UPPER CASE
'HELLO WORLD!'
>>> MyString.upper().startswith("HELL")  # .startswith() tests whether the string starts with a certain char or string!
True
>>> MyString.upper().strip("!").split()
    # .strip() removes specified elements (whitespace by default) from both sides of the string. 
    # .split() breaks up string into elements of a table, using whitespace as a default separator.
['HELLO', 'WORLD']
```
&nbsp;  
Many of those with double underscores are functions, such as `len()` that computes the length of the string:
```
>>> len(MyString)
12
```  
  
  &nbsp;  
### P3.3. Working with files
Capturing user-provided input is not usually very convenient in the context of bioinformatics! Much of the time, we want to work with information saved in files.  
  
To open a file that contains input data, we want to run the `open()` function with the `"r"` parameter - opening the file in the read-only mode, rather than `"w"` - writeable, or `"rw"` - both.
```
InputFile = open("InputFile.txt", "r")
```  

Now, we can loop through lines. You can loop only once - so make sure to capture the desired information!  
Method `.readline()` captures a single line. Or you can loop through lines using the `for` loop.
```
for line in InputFile:
    LINE = line.strip().split()
    TABLE.append(LINE)
```  
  
Once you are done with the file, before you move on, make sure to close it:
```
InputFile.close()
``` 
&nbsp;  
How to write to files? First, you want to open them in the writeable mode. Then, specify the output of the `print()` function as the file. Do not forget to close the file once you are done!
```
OutputFile = open("OutputFile.txt", "w")  
for Object in ListOfObjects:   
    print(Object, file = OutputFile)  
OutputFile.close()  
```  

Don't close your file properly can cause trouble. However, it's easy to forget to close the files, and an extra line at the end looks ugly.  
Luckily, there is a better way to read a file using **with** -- a context manager helps to automatically close the file!

&nbsp;   
#### P3.3.1. Practical examples:
Now, how to open a fasta file where each sequence is in a single line, such as this:
```
>Seq1
ACTGTTAGT
>Seq2
ATGTGTACTCATA
```
... and save the contents to a dictionary? There are multiple ways - think about the organization of your data and be creative! This is one example:
```
SeqDict = {}

for line in InputFasta:
    if line.startswith(">"):
        SeqHeading = line.strip()
    else:
        NuclSequence = line.strip()
        ### If a line is not a heading, then the heading must have already been provided!
        SeqDict[SeqHeading] = NuclSequence
        ### ... and so I can now specify SeqHeading as key & NuclSequence as value
```  
   
&nbsp;     
How to type the contents of the above SeqDict to a file in the custom `SeqHeading: NuclSequence` format?
```
OutputFile = open("OutputFile.txt", "w")

for SeqName in SeqDict:
    print(SeqName, ": ", SeqDict[SeqName], sep = "", end = "\n", file = OutputFile)
    ### Note that above, I requested that elements printed are separated by nothing: `sep = ""` (default: space),
    ### ... and that each line ends with END-OF-LINE: `end = "\n"` --- which is the default anyway.

OutputFile.close()   # Don't forget about this!
```  
  
&nbsp;  
### P3.4. Homework!
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
Good luck!!! :)
