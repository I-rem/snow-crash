#level05

When we login to level05 we are greeted with this mysterious message:

![image](https://github.com/user-attachments/assets/87ab7b93-e29c-4c7c-b8e9-cbdd9a9ad828)

If you don't get the message above try reseting your ssh connection and logging in to level05 directly. 

Now let's see where this 'mail' is: `level05@SnowCrash:~$ find / -name mail 2>/dev/null`

![image](https://github.com/user-attachments/assets/517f1790-d4ca-4573-9c6b-2cd67210827d)

We need to investigate further: `level05@SnowCrash:~$ find / -name mail 2>/dev/null | xargs file`

![image](https://github.com/user-attachments/assets/8fa938cc-70db-464a-9e2e-30cc4949b8de)

After examining each result we find a script called flag5 in the `/var/mail` directory

![image](https://github.com/user-attachments/assets/0fc7e992-9d27-4af9-85a1-0887a472aeec)

Owner of the both the script and the directory is root but they belong to the `mail` group

![image](https://github.com/user-attachments/assets/36f3ed98-232d-4689-a3f1-b5a0ce26c994)

After all this information gathering let's finally read the file and see what's up.

![image](https://github.com/user-attachments/assets/b292d9a3-f7c3-466a-8444-ef9119382e91)

`*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05`

You might have realized that this is a [cron job](https://www.freecodecamp.org/news/cron-jobs-in-linux/) and might be getting flashbacks from born2broot at this point.

As a little reminder, **cron** is a job scheduling utility present in Unix like systems. The **crond** daemon enables cron functionality and runs in background. The cron reads the **crontab** (cron tables) for running predefined scripts. For more information see the manual pages of [crontab(5)](https://man7.org/linux/man-pages/man5/crontab.5.html) and [cron(8)](https://www.man7.org/linux/man-pages/man8/cron.8.html).

Okay, back to the file at hand: `*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05`

The crontab format is as follows: MIN HOUR DOM(Day of month) MON DOW(Day of week) CMD(command)

In our case the job is scheduled for every 2 minutes. But is going to happen every 2 minutes? The `-c` flag (aka `--command`) specifies a command that will be invoked by the shell using its -c. In our case this command is `sh /usr/sbin/openarenaserver` which simply runs the script called `openarenaserver` located in `/usr/sbin`. It then provides the username with the - option. 

So when this command runs the enviroment will behave as if the flag05 user had logged in directly. (Great news for us!)

(Also I don't know why but the man page for su mentions that 
<i>When `-` is used, it must be specified as the last su option. The other forms (-l and --login) do not have this restriction.</i> for anyone who was wondering)

![image](https://github.com/user-attachments/assets/e7fa4a20-25c6-403f-a2cf-80ed8d4b9a4c)

```
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
![image](https://github.com/user-attachments/assets/b87abe0b-a821-4aeb-8825-df6c51f46fdd)

![image](https://github.com/user-attachments/assets/3b8515a6-d804-49d6-9297-a6b91595bad1)

`level05@SnowCrash:/opt/openarenaserver$ echo "/bin/getflag > /tmp/test" > /opt/openarenaserver/test`

![image](https://github.com/user-attachments/assets/86d331f9-ec8a-41fa-87b3-868b45507df9)

![image](https://github.com/user-attachments/assets/bf7f7f7f-833a-4800-a8cd-6254228e1175)
