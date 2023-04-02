#  Reverse Engineering Ransomware 

The Weeks Lab focus was on Reverse Engineering Ransomeware with crackmes which involves running the crackmes with tools including but not limited to (strings, uftrace) at the terminal, Ghidra- a reverse engineering tool that was developed by the NSA for debugging crackmes etc. to enable in finding of essential information from the crackme basically a KEY and also using the ransomware program which contains enough information that will help in reverse engineering on how to decrypt the secret file.

---
# SOLUTION TO [ransomware.zip] AND EXPLANATION OF HOW I SOLVE IT USING THE TOOLS
## STEPS
After downloading the file from the website, I extracted the downloaded zip file `ransomeware1` to my Desktop and it provided with a files namely `important.docx, important.docx.pay_up,ransomware_1,secret.txt.pay_up`. I proceeded by running the command `file ransomware_1` in my linux terminal to ascertain the type of file and from the output it revealed to me `ransomware_1` is an ELF 32-bit LSB shared object with a BuildID[sha1]=cab8f1307a69efae4b940d0a8fef3c5c79d83da9, for GNU/Linux 3.2.0. Similarly using  command `file important.docx.pay_up` provided with an ouptput `important.docx.pay_up: data` and lastly, executing the command `file secret.txt.pay_up` provided an ouptput `secret.txt.pay_up: data`.

I then proceeded to Ghidra to see how best I can decompile and analyze the `ransomware_1` executable file since running string a on the file was not providing me the password. First and foremost after launching Ghidra, from the `Symbol Tree` pane I searched for the actual main function which is basically the entry point of the program and double click it to launch. Ghidra launched the program and provide me with C-code like program in the decompiler pane of Ghidra consisting of a main function that takes no arguments and returns a boolean value. The purpose of the program was to ask the user for an input,then compare it to two hardcoded strings, and print a message indicating whether the password was correct or not. So I assumed the password to be `"lumpy_cactus_fruit"`, I rerun the command `./ransomware_1` again on my terminal and when it executed it printed the string 'please insert the password:' and upon typing the string `lumpy_cactus_fruit` it outputs the success message `This is a simulated ransomware. I have stolen your secret recipe and encrypted it. Pay 1 zillion dollars to us and we will give you a decryption key.`, hence the password for ransomware_1. However from the output it can be seen that, the file is has been encrypted even after entering the right key has been entered, therefore a decryption program needs to needs to be written so as to avoid paying 1 zillion to be provided the key by the attacker
 
**My solution of a decryption program to obtain the is shown below:**
<pre><code>
path = '/home/sosman/Desktop/secret.txt.pay_up'
with open(path, 'rb') as f:
    decrypted_data = b''
    byte = f.read(1)
    while byte != b'':
        decrypted_data += bytes([byte[0] ^ 0x34])
        byte = f.read(1)

with open('decrypted_data.txt', 'wb') as f:
    f.write(decrypted_data)
</pre></code>


**Image of the decrypted secret.txt file:**
![ransomware1_decrypt](https://user-images.githubusercontent.com/66968869/229383065-b31b473d-f763-49a1-b4bf-c9479c4b7b02.png)


**Screenshot of the decryption function in Ghidra updated with human-readable labels.:** 
![Screenshot from 2023-03-31 05-04-58](https://user-images.githubusercontent.com/66968869/229383156-59e673cf-7b2b-4ed2-a318-cd870179b9fa.png)

**Explanation on how my decryption works:**
The decrytion read each byte of the file located at path, decrypt it using the XOR operation with 0x34, and store the decrypted data in the decrypted_data variable.

Then, it will open a new file named decrypted_data.txt in binary mode with the mode 'wb', and write the decrypted contents of the input file to this file using the write() method.

