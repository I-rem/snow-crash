# Level02
Surprise! Our home directory is not empty this time.

![image](https://github.com/user-attachments/assets/e90f08cc-e8c6-43d2-a211-426c455d7998)

So, what is a **pcap** file and how do we make sense of it? 

PCAP files are a common format for storing packet captures. A PCAP file includes an exact copy of every byte of every packet as seen on the network. [See](https://www.endace.com/learn/what-is-a-pcap-file)
There are various open-source tools we can use to read and write PCAP files including tcpdump, libPCAP, WinPCAP, NPCAP, Zeek, Snort, Suricata, Wireshark. **Wireshark** is among the recommended tools for this project and it is preinstalled on my machine so we'll go with that.

I said my machine though, not the host machine. 

![image](https://github.com/user-attachments/assets/e46661ec-337c-4ad2-bfd4-95d0e2d0b172)

When using tools like john I was easily able to copy-paste text between machines but I need to actually transfer a file now. Did you know that there is a whole course for that on [HTB Academy](https://academy.hackthebox.com/course/preview/file-transfers)?
We probably won't need to go thorough all that though. We just need to learn to use the **SCP** command as suggested by the project's subject.pdf.

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

Now that I have the necessary file in my current directory I can run

`$ sudo wireshark level02.pcap`:

![image](https://github.com/user-attachments/assets/ba0cc406-d451-48ee-950a-3514e07a8308)

Going one by one I see the word Password on line 42(hehe) but am unable to decipher much else. Let's go to Analyze → [Follow](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowStreamSection.html) → TCP Stream window.

This will display stream content in the same sequence as it appeared on the network. Traffic from the client to the server will be colored red, while traffic from the server to the client will be colored blue.

![image](https://github.com/user-attachments/assets/ba82eb31-4bb9-4ad1-bd51-b5ddd80abd1c)

Alright, this looks promising `Password: ft_wandr...NDRel.L0L` 
