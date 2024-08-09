# snow-crash
This project is an introduction to computer security. Snow Crash will make you discover security in various sub-domains, with a developer-oriented approach. You will become familiar with several languages (ASM/perl/php…), develop a certain logic to understand unknown programs, and become aware of problems linked to simple programming errors

**Subject Pdf:** https://cdn.intra.42.fr/pdf/pdf/67418/en.subject.pdf

**SnowCrash.iso:** https://cdn.intra.42.fr/isos/SnowCrash.iso

# General Instructions
- To make this project, you will have to use a VM(64 bits). Once you have started
your machine with the ISO provided with this subject, if your configuration is right,
you will get a simple prompt with an IP:

![image](https://github.com/user-attachments/assets/a3ecb991-3888-4394-8774-3eb05dac7028)

  💡 If the IP address is not visible, you can get it with the comamnd `ifconfig` once you're connected

- Then you will be able to register using the following login:password:level100:level100
  Use the SSH connection available on port 4242: `$>ssh level100@192.168.16.128 -p 4242`

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
- Your repo must include anything that helped you solve each validated test
- Your repository will look like this:

  ![image](https://github.com/user-attachments/assets/af5a1c13-99f6-4a80-bb55-67a2355a8fe8)
  
- You will keep everything you need to prove your results during the evaluation in
the resource folder. The flag file may be empty, but you may have to explain why

⚠️ You must be able to clearly and precisely explain anything
that is included in the folder. The folder mustn’t include ANY
binary.

- If you need to use a specific file that’s included on the project’s ISO, you must download it during the evaluation. You must put it in your repo under no circumstances.
- If you plan to use a specific external software, you must set up a specific environment (VM, docker, Vagrant).
- You’re invited to create scripts that will make you stall, but you will have to explain
them during the evaluation.
- For the mandatory part, you must complete the following list of levels:
  - [level00](level00/Ressources/README.md)
  - [level01](level01/Ressources/README.md)
  - [level02](level02/Ressources/README.md)
  - [level03](level03/Ressources/README.md)
  - [level04](level04/Ressources/README.md)
  - [level05](level05/Ressources/README.md)
  - [level06](level06/Ressources/README.md)
  - [level07](level07/Ressources/README.md)
  - [level08](level08/Ressources/README.md)
  - [level09](level09/Ressources/README.md)
  
⚠️ You cannot bruteforce the ssh flags.

# Bonus Part
For the bonus part, you can complete the following list of levels:
- level10
- level11
- level12
- level13
- level14

# Resources Suggested by Intra Notions
![image](https://github.com/user-attachments/assets/ea8bc07b-e776-4b1c-8e0a-ae2cbf55e600)

# VM Setup
![image](https://github.com/user-attachments/assets/6e326bd5-f78c-4838-901b-28dac8dd4ece)

![image](https://github.com/user-attachments/assets/104fe8d2-b2a8-479b-af3b-05c679702913)

![image](https://github.com/user-attachments/assets/a2acb07e-812d-48b4-af7e-6014d9669f5f)

![image](https://github.com/user-attachments/assets/9aa57af8-f7ec-40ae-a66e-b62c3ec1fd39)

![image](https://github.com/user-attachments/assets/eb9d9398-41da-4875-8364-f2c60e0f01f3)

![image](https://github.com/user-attachments/assets/4ba8c891-f3d4-4b02-aac0-87c7b5890d18)

