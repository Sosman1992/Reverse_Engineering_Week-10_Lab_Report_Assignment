#  Reverse Engineering Ransomware 

The Weeks Lab focus was on Reverse Engineering Ransomeware with crackmes which involves running the crackmes with tools including but not limited to (strings, uftrace) at the terminal, Ghidra- a reverse engineering tool that was developed by the NSA for debugging crackmes etc. to enable in finding of essential information from the crackme basically a PASSWORD.

---
# SOLUTION TO [ransomware.zip] AND EXPLANATION OF HOW I SOLVE IT USING THE TOOLS
## STEPS
After downloading the file from the website, I extracted the downloaded zip file `ransomeware1` to my Desktop and it provided with a files namely `important.docx, important.docx.pay_up,ransomware_1,secret.txt.pay_up`. I proceeded by running the command `file ransomware_1` in my linux terminal to ascertain the type of file and from the output it revealed to me `ransomware_1` ELF 32-bit LSB shared object, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=cab8f1307a69efae4b940d0a8fef3c5c79d83da9, for GNU/Linux 3.2.0, not stripped Similarly using  command `file important.docx.pay_up` provided with an ouptput `important.docx.pay_up: data` and lastly, executing the command `file secret.txt.pay_up` provided an ouptput `secret.txt.pay_up: data`.

I then proceeded to Ghidra to see how best I can decompile and analyze the `ransomware_1` executable file since running string a on the file was not providing me the password. First and foremost after launching Ghidra, from the `Symbol Tree` pane I searched for the actual main function which is basically the entry point of the program and double click it to launch. Ghidra launched the program and provide me with C-code like program in the decompiler pane of Ghidra consisting of a main function that takes no arguments and returns a boolean value. The purpose of the program was to ask the user for an input,then compare it to two hardcoded strings, and print a message indicating whether the password was correct or not. So I assumed the password to be `"lumpy_cactus_fruit"`, I rerun the command `./ransomware_1` again on my terminal and when it executed it printed the string 'please insert the password:' and upon typing the string `lumpy_cactus_fruit` it outputs the success message `This is a simulated ransomware. I have stolen your secret recipe and encrypted it. Pay 1 zillion dollars to us and we will give you a decryption key.`, hence the password for ransomware_1. However from the output it can be seen that, the file is has been encrypted even after entering the right key has been entered, therefore a decryption program needs to needs to be written so as to avoid paying 1 zillion to be provided the key by the attacker
 
**My solution of a decryption program to obtain the is shown below:**
<pre><code>
#!/usr/bin/env python3
import string
import random

""" """

</pre></code>


**Image of the decrypted secret.txt file:**


**Screenshot of the decryption function in Ghidra updated with human-readable labels.:** 

**Explanation on how my decryption works:**

