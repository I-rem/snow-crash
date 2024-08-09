# level09

Once again we have an executable and a token file.

![image](https://github.com/user-attachments/assets/879fb8f7-e0f5-4466-b522-1cbc7e9745a6)

This time we have read rights for the token file now so everything might be easier now. Let's see

![image](https://github.com/user-attachments/assets/11d57705-8294-4c45-9ac9-8619f68a680b)

We have a bunch of nonprintable characters. This token does not work as the password for any user. Let's see if level09 can help us out.

![image](https://github.com/user-attachments/assets/5a94a49d-eba7-49fb-bb63-e027c51a7d14)

Weird? This time it does not take a file as input but accepts any string and prints out nonsense. We need to test further.

![image](https://github.com/user-attachments/assets/c6f74b8c-26b2-4cf5-9eb9-155297c2d7fc)

Okay this makes more sense. The program takes a string and encrypts it by adding each character its own index. 

`abc` becomes `(a + 0)(b + 1)(c + 2)` = `ace`

So I first assumed that we need to put the contents of the token through this program `level09@SnowCrash:~$ cat token | xargs ./level09`

![image](https://github.com/user-attachments/assets/a0db69fd-3790-4ddc-bcba-b87ffc274458)

This wasn't helpful at all. I suppose the contents of the token were already encrypted. What we need to do now is to reverse the operation.
Here is a simple python script:

```
import sys

if (len(sys.argv) == 2):
        hash = sys.argv[1]
        decrypted_hash = ""
        for i in range(0, len(hash)):
                decrypted_hash = decrypted_hash + chr(ord(hash[i]) - i)
        print(decrypted_hash)
else:
        print("I need one argument")
```
Let's run it `level09@SnowCrash:~$ cat token | xargs python /tmp/decrypt.py`

![image](https://github.com/user-attachments/assets/e2f51256-4580-4957-b18a-2b48989f1412)

![image](https://github.com/user-attachments/assets/70470394-a4ca-4926-b0bc-08cf4b87b057)

🥳

Some C if you prefer that:

```
#include <stdio.h>

int main(int argc, char **argv)
{
        if (argc == 2)
        {
                int index = 0;

                while (*argv[1])
                {
                        printf("%c", *argv[1] - index);
                        ++argv[1];
                        ++index;
                }
                printf("\n");
        }
        else
                printf("Give me one(1) argument pls\n");
}
```
