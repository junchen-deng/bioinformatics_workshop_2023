# Workshop P2 (20th May 2021). The first Python scripts.
  
### P2.1. Recap of the key concepts
Last week, we have learnt the key building blocks of a Python script:  
* **Variables:**  
  * Integer  
  * Float  
  * String   
  * Boolean  
* **Arrays:**  
  * List  
  * Dictionary  
* **Flow control tools:**
  * *for* loop  
  * *while* loop  
  * *if* statement  
  
&nbsp;  
These few building blocks can be combined into seriously sophisticated workflows!  
Let's see how that works!
&nbsp;  
  
### P2.2. Key components of a script
A Python script needs some essential elements:  
&nbsp;  
1) A way of informing the operating system that the Python3 should be used to interpret the script  
  * We can run the script by specifying `python set_of_instructions.txt` ...   
  * But probably a more universal way is making sure that the script starts with a line informing the operating system that the remainder of the file contents should be directed to and interpreted by Python:  
  `#! /usr/bin/env python`, and making the script executable: `chmod u+x script.py`.  
&nbsp;  
2) A way of accepting input:  
  * Asking the user for input data, for example: `name = input("Enter your name: ")`.  
  * Opening files indicated within the script, or provided as arguments at the time the script is run.
&nbsp;  
3) A way of providing output:  
  * Printing information to standard output (screen): `print("Hello " + name + "!")`.  
  * Creating output files.  

By combining these elements together, we can write our first script --- `Hello.py`
```
#! /usr/bin/env python3
name = input("Enter your name: ")
print("Hello " + name + "!")
```  
&nbsp;  
  
### P2.3. How to structure your workflow? Script 1. Reverse-complementing a user-provided nucleotide sequence!
We are going to work on the first practical case:  
**A script that will print a reverse-complement of a user-provided sequence.**  
Considarations?  
> What are the necessary steps - how to break up the job into manageable tasks?
> 
> 1. Request the sequence from the user, save it as a String variable
>  
> 2. Provide the dictionary of complementary bases, such as
> `Complements = {'G': 'C', 'C': 'G', 'A': 'T', 'T': 'A', 'N': 'N'}`
>  
> 3. While reading the input sequence backwards, compute the complement, and save it! This step could be broken up into smaller, perhaps more manageable steps! There are many ways of doing this, but perhaps the easier would be 
>  
> 4. Provide the final sequence to the user.
>  
> 5. Once you verify that it all works, foolproof your script... consider converting the input sequence into upper case (`.upper()` method), and verifying whether subsequent bases are in the 'ACGTN' set. That 
  
### P2.4. HOMEWORK! Script 2. Compute basic statistics for a user-provided sequence!

The second script will compute basic statistics for a user-provided sequence: length, count of Ns, and GC%.  
  
