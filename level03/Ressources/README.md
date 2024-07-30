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

![image](https://github.com/user-attachments/assets/679226be-e1f0-4bce-9667-7f06131efe3d)

![image](https://github.com/user-attachments/assets/3f3436bd-2cb9-46d8-b4d1-b275eb9f864e)


![image](https://github.com/user-attachments/assets/f11312ea-28ea-4183-b56f-171b1bb2de57)

![image](https://github.com/user-attachments/assets/767fd644-da97-4bef-bb30-f5169509080f)
