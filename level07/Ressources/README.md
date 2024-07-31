# level07
We are given a file called level07 to work with

![image](https://github.com/user-attachments/assets/62846dbd-88db-4111-a206-e910c3e3e917)

I tried to cat this file and crashed my computer. Don't do that. It is a binary afterall.

![image](https://github.com/user-attachments/assets/a6eaaed6-d583-4e70-bc4f-232e6808b74f)

We take a look at the permissions and see the potential for exploit.

![image](https://github.com/user-attachments/assets/29a3c37a-fa3e-4b7a-acca-3b0bcfcb157e)

Let's try running it:

![image](https://github.com/user-attachments/assets/24c2a827-052b-493a-8c8a-721207468a03)

Weird, does it do nothing? We can inspect further with some of the commands we have used in the previous exercises.

`level07@SnowCrash:~$ strings level07`

![image](https://github.com/user-attachments/assets/e030042e-87bb-4134-bc9b-0dd83f91f16c)

Does it simply echo the LOGNAME's value?

![image](https://github.com/user-attachments/assets/1f43e154-4402-4ef1-ae0d-1fa41a848726)

![image](https://github.com/user-attachments/assets/a31fc635-9451-4d0d-90fb-4edea50881a8)

```
level07@SnowCrash:~$ export LOGNAME=\`getflag\`
```

![image](https://github.com/user-attachments/assets/0acdda98-73eb-4cc5-a61a-53099d7b2378)


