# level00 
I was completely lost when I first started this level. I kept running around various directories unable to find a clue. (I found stuff that could be useful for the upcoming levels though)
So I went back to intra.42.fr to take a look at the resources given to us to see if I had missed anything. 

And voila~
![image](https://github.com/user-attachments/assets/8152c79d-f72f-4776-b6f5-ae5d77eb0d67)

The introduction video shows a README file that says `FIND this first file who can run only as flag00`

Okay I will just search for the file named flag00 recursively from the root

`find / -type f -name "flag00"`

![image](https://github.com/user-attachments/assets/044aaaef-ce03-49e4-b331-09a0e827c0c9)

The output is crowded with error messages, let's redirect them for readability's sake

`find / -type f -name "flag00" 2>/dev/null`

![image](https://github.com/user-attachments/assets/09ee68cc-294f-4861-910f-9ed13fc2662f)

Oh, nothing. Well it says "file who **runs** as flag00". I suppose we need a file that can be run by the **user** flag00?

find command has a neat option we can use here

![image](https://github.com/user-attachments/assets/4f444f11-e435-4728-991e-5fc817fc128d)

`find / -type f -user flag00 2>/dev/null`

![image](https://github.com/user-attachments/assets/2fadc692-45a5-4b0c-a267-d071bbfe003a)

Awesome, let's investigate `/usr/sbin/john` and `/rofs/usr/sbin/john`

I wanted to first confirm that these files indeed belong to flag00

![image](https://github.com/user-attachments/assets/0517cb34-33b5-4e36-8861-ff464142bb89)
![image](https://github.com/user-attachments/assets/4fb903c2-f6b0-4ca1-b80b-1b5d1fe50564)

So we have read-only permission for the flag00 group as well as other users.(interestingly none for the flag00 user?) Whatever I will just read the content then:

![image](https://github.com/user-attachments/assets/8d16bd44-2572-4dc5-972e-60e90a684c61)

Eazy, now all we need to do is call `su flag00` and enter `cdiiddwpgswtgt` when prompted for password.

![image](https://github.com/user-attachments/assets/677ac3dd-4a80-40d1-b8f6-6cc19c5392cb)

Or maybe not ]: (

Going back to the introduction video now. We are told that "John" is a must have utility for this project and that "John" is a software that allows you to crack passwords. Our file in this exercise was called "john", **gasp** is this a clue?!

I tried running John the Ripper password cracker but that didn't work and it would be overkill for this exercise anyway.

![image](https://github.com/user-attachments/assets/d535b814-ef6c-467c-8a44-caf1e7f8cd88)

We need to decode the password somehow and we were told that we may need to use [dcode.fr](https://www.dcode.fr/) for 1 or 2 levels. 

I didn't want to manually check different ciphers so I just used [multidecoder](https://www.cachesleuth.com/multidecoder/)

![image](https://github.com/user-attachments/assets/162c68b7-2357-4a46-89d9-64037c502bb1)

Finally! the password is `nottoohardhere`

![image](https://github.com/user-attachments/assets/bcd772cf-2df4-4d7e-985e-667e4fa79de0)

Now that I am the user flag00, running `getflag` will give me the token for the next level.

![image](https://github.com/user-attachments/assets/a66a44ce-3da3-48f6-a883-241956235701)

Hurray! Only 9 more levels to go...
