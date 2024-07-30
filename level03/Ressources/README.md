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

![image](https://github.com/user-attachments/assets/679226be-e1f0-4bce-9667-7f06131efe3d)

![image](https://github.com/user-attachments/assets/3f3436bd-2cb9-46d8-b4d1-b275eb9f864e)


![image](https://github.com/user-attachments/assets/f11312ea-28ea-4183-b56f-171b1bb2de57)

![image](https://github.com/user-attachments/assets/767fd644-da97-4bef-bb30-f5169509080f)
