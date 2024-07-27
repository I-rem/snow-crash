![image](https://github.com/user-attachments/assets/5a276f3c-077c-4f13-b572-df191e644603)

![image](https://github.com/user-attachments/assets/afec7e89-9de1-4216-9479-25f7c7e55e12)

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

![image](https://github.com/user-attachments/assets/085d513d-7dc6-4b2c-89ef-8574d0d3f4c1)

