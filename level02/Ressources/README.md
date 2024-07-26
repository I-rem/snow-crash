# Level02
Surprise! Our home directory is not empty this time.

![image](https://github.com/user-attachments/assets/e90f08cc-e8c6-43d2-a211-426c455d7998)

So, what is a pcap file and how do we make sense of it? PCAP files are a common format for storing packet captures. A PCAP file includes an exact copy of every byte of every packet as seen on the network. [See](https://www.endace.com/learn/what-is-a-pcap-file)
There are various open-source tools we can use to read and write PCAP files including tcpdump, libPCAP, WinPCAP, NPCAP, Zeek, Snort, Suricata, Wireshark. Wireshark is among the recommended tools for this project and it is preinstalled on my machine so we'll go with that.

I said my machine though, not the host machine. 

![image](https://github.com/user-attachments/assets/e46661ec-337c-4ad2-bfd4-95d0e2d0b172)

When using tools like john I was easily able to copy-paste text between machines but I need to actually transfer a file now. Did you know that there is a whole course for that on [HTB Academy](https://academy.hackthebox.com/course/preview/file-transfers)?
We probably won't need to go thorough all that though. We just need to learn to use the SCP command as suggested by the project's subject.pdf.

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
