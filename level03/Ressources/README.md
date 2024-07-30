# Level03
Once again we are given a mysterious file:

![image](https://github.com/user-attachments/assets/21f69c42-1945-4a6b-b2ca-ebd4b3210196)

Let's learn more about it:

![image](https://github.com/user-attachments/assets/18448ea6-339b-4ca5-ae23-835f47bde88d)

We see that it has the SUID bit set. A file with [SUID](https://www.redhat.com/sysadmin/suid-sgid-sticky-bit) always executes as the user who owns the file, regardless of the user passing the command. In this case the owner of the file is flag03.

As the user level03 I can borrow the priviliges of flag03 when executing this file. This can be potentially exploited so we'll keep it in mind.

However when we run it nothing too exciting happens

![image](https://github.com/user-attachments/assets/d950f2fa-c121-4211-a6e5-61d50bf0e5dc)

I tried giving it different input values but the output was always the same. Unfortunately we aren't provided with the source code. So some [reverse engineering](https://www.codementor.io/@packt/reverse-engineering-a-linux-executable-hello-world-rjceryk5d) will be necessary.

As an example of good practice, the process of reversing a program first needs to start with proper identification. Let's start with `level03@SnowCrash:~$ file level030
`
![image](https://github.com/user-attachments/assets/60243797-a254-4474-901a-3fabebef1c88)

It is a 32-bit ELF file-type. ELF files are native executables on Linux platforms. When we try to use `cat` on binary files we get gibberish at best and a blue-screen at worst. But these files might still contain valuable human readable text. So how do we see them?

Using the `strings` command:

![image](https://github.com/user-attachments/assets/ce0aeba4-7c6b-4bdb-b642-2dd44d1d81d2)

The strings are listed in order from the start of the file. And we see that this program simply runs `echo Exploit me` and echo is located in `/usr/bin/env`, how do we exploit this?

Well we can write a script called echo and make it run that perhaps?

`level03@SnowCrash:~$ vim /tmp/echo`

```
#!/bin/sh
/bin/sh
```
Also don't forget to give it the permissions: `level03@SnowCrash:~$ chmod 777 /tmp/echo` (execute permission would be enough by itsef too) 

![image](https://github.com/user-attachments/assets/14369589-b885-4a9c-b9c6-76e0c96f08ea)

This script will simply invoke a new shell. We'll soon see how that becomes useful. 
Now we will change the enviroment variables to trick the program into running this before the default echo command.

![image](https://github.com/user-attachments/assets/a935bf82-f56c-44c0-bd70-0aa192e02823)

The [PATH](https://www.digitalocean.com/community/tutorials/how-to-view-and-update-the-linux-path-environment-variable) variable contains a list of directories the system checks before running a command. Updating the PATH variable will enable you to run any executables found in the directories mentioned in PATH from any directory without typing the absolute file path.

We will add the /tmp directory at the beginning of our PATH so that it will be checked first. `level03@SnowCrash:~$ export PATH=/tmp:$PATH`

![image](https://github.com/user-attachments/assets/dfd2000e-0bfa-40b1-8762-f4e24a6e11d1)

Let's run level03 and see what happens.

![image](https://github.com/user-attachments/assets/2d868c60-db28-4640-93b1-353f72b26876)

Okay, what happened?

![image](https://github.com/user-attachments/assets/3f3436bd-2cb9-46d8-b4d1-b275eb9f864e)

We got reverse shell 🎉 level03 runs with the priviliges of user flag03, so we ended up starting a new shell as flag03.
We can simply run `getflag` from here.

![image](https://github.com/user-attachments/assets/f11312ea-28ea-4183-b56f-171b1bb2de57)

![image](https://github.com/user-attachments/assets/767fd644-da97-4bef-bb30-f5169509080f)
