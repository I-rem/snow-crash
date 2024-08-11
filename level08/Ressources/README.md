# level08
We have not one but two files this time.

![image](https://github.com/user-attachments/assets/0f51ad1d-7c4b-4a1d-8210-33f0ade20606)

When we execute level07 it asks for a file to read. But when we provide it a file ,token in this case,it tells us that we don't have access. How rude!

If I don't have access then let's see who does,

![image](https://github.com/user-attachments/assets/4fecdfd8-e6f4-4ceb-9f4e-5c60d7066221)

That's a bummer I am not flag08. And you bet I tried to change the file permissions(that was of course a futile effort). We don't even have the read permissions for token so the contents of this file is a mystery to us.

We can just give level07 a different file then,

![image](https://github.com/user-attachments/assets/896750d1-dd2c-4dd6-8b61-e04e0dd44a54)

Okay it just displays the file contents for us. We definetly need it to accept token then.

Running `$ strings level08` gives us more to work with

![image](https://github.com/user-attachments/assets/de05e8b0-b144-4f5b-99fa-09e5ab385ae4)

Wee see that it uses printf, read, open and strstr. The first 3 makes sense but why does it need strstr? We can also see various error messages and even a string called token.

![image](https://github.com/user-attachments/assets/3a6df06c-642d-416d-b47a-08644da20203)

I am curious about a couple of things now, I will make it output all of these error messages to find answers.

![image](https://github.com/user-attachments/assets/ff73443b-d383-4857-97b4-b7bbcfe00a54)

![image](https://github.com/user-attachments/assets/0807852f-55dc-404f-9b06-1c57e62cb0c0)

For a nonexistent file it gives the "unable to open" message. And when the file exists but doesn't have read rights it still gives the same message. It also never gave us the "access denied" message we got at the beginning. So it must have a condition that is not related to file permissions.

Time for some [dynamic analysis](https://www.codementor.io/@packt/reverse-engineering-a-linux-executable-hello-world-rjceryk5d): `level08@SnowCrash:~$ ltrace ./level08 token`

The output of ltrace shows a readable code of what the program did. It logs the library functions that the program calls and recieves.

![image](https://github.com/user-attachments/assets/8902f3e0-f30a-4cf1-b676-3c9e57314cec)

It calls strstr to see if the filename contains the string "token"

We can verify this behaviour:

![image](https://github.com/user-attachments/assets/17e90802-1582-425c-ae53-cc5a11e34731)

It might seem a little bit silly but we can bypass this check with a symbolic link. Back to the Picine: `$ ln -s /home/user/level08/token /tmp/t0ken` 

![image](https://github.com/user-attachments/assets/91c0c5ea-1025-41b6-87bd-fe7525675e60)

Fun.

![image](https://github.com/user-attachments/assets/2e302dcc-1018-47dc-afcf-16f65edc6fdb)
