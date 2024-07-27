![image](https://github.com/user-attachments/assets/87ab7b93-e29c-4c7c-b8e9-cbdd9a9ad828)

`level05@SnowCrash:~$ find / -name mail 2>/dev/null`

![image](https://github.com/user-attachments/assets/517f1790-d4ca-4573-9c6b-2cd67210827d)

![image](https://github.com/user-attachments/assets/0fc7e992-9d27-4af9-85a1-0887a472aeec)

![image](https://github.com/user-attachments/assets/b292d9a3-f7c3-466a-8444-ef9119382e91)

`*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05`

![image](https://github.com/user-attachments/assets/3fb1d7d0-947d-4781-a613-d2b851c505bf)

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
