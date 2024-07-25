# snow-crash
This project is an introduction to computer security. Snow Crash will make you discover security in various sub-domains, with a developer-oriented approach. You will become familiar with several languages (ASM/perl/php…), develop a certain logic to understand unknown programs, and become aware of problems linked to simple programming errors

# General Instructions
- To make this project, you will have to use a VM(64 bits). Once you have started
your machine with the ISO provided with this subject, if your configuration is right,
you will get a simple prompt with an IP:

![image](https://github.com/user-attachments/assets/a3ecb991-3888-4394-8774-3eb05dac7028)

  💡 If the IP address is not visible, you can get it with the comand ifconfig once you're connected

- Then you will be able to register using the following login:password:level100:level100
  Use the SSH connection available on port 4242: `ssh level100@192.168.16.128 -p 4242`

- Once registered, you’re gonna have to find the password that will log you in with
the "flagXX" account(XX = current level number).

💡 Once logged to the "flagXX" account, launch the `getflag` command.
  It will give you the password to connect to the next level (You may
  not be able to connect to a "flagXX" account - in this case, you will
  have to find an alternative method, like a command injection on the
  program depending on its rights, for instance!).

- Here is a session example:
  
![image](https://github.com/user-attachments/assets/328f9b80-8747-4e0d-b3ef-1fbe713a2e0b)

- To help you with some levels, you’re gonna have to use external softwares. You
should learn to use the SCP command.

 💡 /tmp/ and /var/tmp/ folders have limited rights and will be reset
from time to time. You should not work directly on the machine.

- Nothing is left to chance. If there is a problem, start wondering if your code is not
the cause.

# Mandatory Part
