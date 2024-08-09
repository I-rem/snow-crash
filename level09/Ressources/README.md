# level09

Once again we have an executable and a token file.

![image](https://github.com/user-attachments/assets/879fb8f7-e0f5-4466-b522-1cbc7e9745a6)

This time we have read rights for the token file now so everything might be easier now. Let's see

![image](https://github.com/user-attachments/assets/11d57705-8294-4c45-9ac9-8619f68a680b)

We have a bunch of nonprintable characters. This token does not work as the password for any user. Let's see if level09 can help us out.

![image](https://github.com/user-attachments/assets/5a94a49d-eba7-49fb-bb63-e027c51a7d14)

Weird? This time it does not take a file as input but accepts any string and prints out nonsense. We need to test further.

![image](https://github.com/user-attachments/assets/c6f74b8c-26b2-4cf5-9eb9-155297c2d7fc)

Okay this makes more sense. The program takes a string and encrypts it by adding each character its index. 

`abc` becomes `(a + 0)(b + 1)(c + 2)` = `ace`

![image](https://github.com/user-attachments/assets/465226ff-b3c4-40a4-904a-d1d8e823726c)
