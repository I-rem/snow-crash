# level00 
I was completely lost when I first started this level. I kept running around various directories unable to find a clue.
So I went back to intra to take a look at the resources given to us to see if I had missed anything. 

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

find command as a neat option we can use here

![image](https://github.com/user-attachments/assets/4f444f11-e435-4728-991e-5fc817fc128d)

`find / -type f -user flag00 2>/dev/null`

![image](https://github.com/user-attachments/assets/2fadc692-45a5-4b0c-a267-d071bbfe003a)

Awesome, let's investigate `/usr/sbin/john` and `/rofs/usr/sbin/john`
