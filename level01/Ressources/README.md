# level01

As I was bashing my head against various directories in the previous exercise I stumbled upon a promising file but was unable to use it.
Well it is time.

The `/etc/passwd` file is a plain text file with information for all user accounts. It includes a list of user accounts on the system, as well as details such as user ID, group ID, home directory, and default shell.
[See](https://phoenixnap.com/kb/etc-passwd#:~:text=The%2Fetc%2Fpasswd%20file%20is,home%20directory%2C%20and%20default%20shell.)

![image](https://github.com/user-attachments/assets/db6f332b-5bc5-47c2-94c5-f0dcea2e3d93)
![image](https://github.com/user-attachments/assets/e0e774c1-a028-45b7-9026-9af3eb66da61)

The second column represents the encrypted password stored in the `/etc/shadow` file (we of course don't have accesss to that).
And it is normally not supposed show the password or the hash like that but we are lucky today.

Let's `su flag01` and try entering `42hDRfypTqqnw` as the password.

![image](https://github.com/user-attachments/assets/56b319e9-675b-4e85-a6f1-706f57cc942b)

Okay this much was expected, now how do we crack this? I once again tried my luck with [multidecoder](https://www.cachesleuth.com/multidecoder/) but it was no use.
Then I gave [crackstation](https://crackstation.net/) ,which is a free password hash cracker, a try. But it was unable to even detect the type of hash.

![image](https://github.com/user-attachments/assets/f0afd8a2-7b4e-4322-acf5-a3c2207bb00e)

John the Ripper it is, 

`echo 42hDRfypTqqnw > flag01.txt`

`john flag01.txt`

![image](https://github.com/user-attachments/assets/aa432fbf-2bb6-453b-94ba-38c86cdf47c8)


`abcdefg`, really?

Also keep in mind that once John finds a password, it will be printed to the terminal and saved into a file called `~/.john/john.pot.`  John  will  read  this file  when  it  restarts  so  it doesn't try to crack already done passwords.

To see the cracked passwords, use `john -show passwd`

Important: do this under the same directory where the password was cracked (when using  the  cronjob,  /var/lib/john),  otherwise it won't work.

example: 

![image](https://github.com/user-attachments/assets/c1394d56-a630-484a-b486-46fd1c22ef8d)

Or,

![image](https://github.com/user-attachments/assets/fbffe65f-64ff-4b58-a5ba-2fee2bcc4f23)

Allright, let's check if our result was correct

![image](https://github.com/user-attachments/assets/64bef436-e751-4ef8-90f7-648c078cb2d4)

![image](https://github.com/user-attachments/assets/509c785d-fba5-44f7-af6d-d2854657cf46)

Great!
