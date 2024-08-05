# level05

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

In our case the job is scheduled for every 2 minutes. But what is going to happen every 2 minutes? The `-c` flag (aka `--command`) specifies a command that will be invoked by the shell using its -c. In our case this command is `sh /usr/sbin/openarenaserver` which simply runs the script called `openarenaserver` located in `/usr/sbin`. It then provides the username with the `-` option. 

So when this command runs the enviroment will behave as if the flag05 user had logged in directly. (Great news for us!)

(Also I don't know why but the man page for su mentions that 
<i>When `-` is used, it must be specified as the last su option. The other forms (-l and --login) do not have this restriction.</i> For anyone who was wondering)

![image](https://github.com/user-attachments/assets/e7fa4a20-25c6-403f-a2cf-80ed8d4b9a4c)

```
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
This script one by one takes each file in the /opt/openarenaserver directory and runs them with the `bash` command. `-x` option will print out the commands as they are executed.

Then the files are removed with `rm -f`. Which would make this script somewhat dangerous if it were used on the wrong directory. 

The `ulimit` command is used to put various boundries for system resources. In this case it was used to put limit on maximum cpu time used. Which means that we can't exploit this code **too much** and execute commands that will run indefinetly.

This is not an issue, all we want is to get the flag and `ls -l` shows us that this script belongs to the flag05

![image](https://github.com/user-attachments/assets/7549b0d5-13c4-4fba-ba30-fbe2ed4e760f)

We don't have the execute right but that doesn't matter becuase it runs automatically in the background thanks to cron! Let's get to exploiting then.

I would like to start by experimenting with the `bash` command.

![image](https://github.com/user-attachments/assets/f02c1f9c-aa6f-4530-ae0b-fa9b0592fb5c)

![image](https://github.com/user-attachments/assets/b87abe0b-a821-4aeb-8825-df6c51f46fdd)

With the `-x` option

![image](https://github.com/user-attachments/assets/26ac9f42-a231-4027-b136-c2d97316d302)

Okay all we need to do is run getflag in a similar way. We will put the file in `/opt/openarenaserver/`
Let's go there first and inspect

![image](https://github.com/user-attachments/assets/3b8515a6-d804-49d6-9297-a6b91595bad1)

It is empty, which makes sense as the directory is wiped every 2 minutes.

We just put our exploit file in.

`level05@SnowCrash:/opt/openarenaserver$ echo getflag > /opt/openarenaserver/flag`

![image](https://github.com/user-attachments/assets/86d331f9-ec8a-41fa-87b3-868b45507df9)

 Now we wait...and nothing happens. The output is not printed on the terminal. I am assuming that this is some [subshell](https://www.geeksforgeeks.org/shell-scripting-subshell/) behaviour.

Subshell allow you to execute commands within a separate shell environment. They can be created using the parentheses operator or the bash command, and they provide a way to isolate the effects of certain commands or operations. Subshells can be useful for setting temporary variables, changing directories, executing commands in the background etc. without affecting the current shell environment. (You can correct me here though)

Long story short we will just include ouput redirection in our script: `$ echo "getflag > /tmp/myflag" > /opt/openarenaserver/getmyflag`

![image](https://github.com/user-attachments/assets/90012be8-594c-4e5a-a4d0-bc8a1055f68d)

All we need to do is wait for a maximum of 2 minutes as getflag is run with flag05's priviliges.

Let's check:

![image](https://github.com/user-attachments/assets/04fd3376-9878-4325-9f9e-dc1af4c432fb)

Woah, I somewhat expected this to not work not gonna lie.

![image](https://github.com/user-attachments/assets/2670d89f-3b31-4828-b43b-d2aeda2da80e)

Time for level 6!
