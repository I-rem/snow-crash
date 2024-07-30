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
After a crash course in perl [subroutines](https://www.tutorialspoint.com/perl/perl_subroutines.htm) and the [CGI](https://www.perl.com/article/perl-and-cgi/) module we see that we are provided with a neatly vulnerable code.

First let's discuss some syntax. The [qw](https://www.geeksforgeeks.org/perl-qw-operator/)(quote-word) operator in Perl is used to extract each element of the given string as it is in an array of elements in single-quote. For example: qw(Hello world) becomes ('Hello', 'world')

In our case it is used to import a list of functions from the CGI module. This list consists of only one element though...Anyway's the function we imorted is `param` and we will come back to it later

In Perl we can pass various arguments to a subroutine like you do in any other programming language and they can be acessed inside the function using the special array @_. Thus the first argument to the function is in $_[0], the second is in $_[1], and so on.

In our script we have a subroutine called `x`. Within x a variable called `y` is defined. y takes the value of the first argument passed to x, as indicated by `$y = $_[0]` . Then it prints the value of y with echo.

Rest of the stuff requires us to understand the CGI module. 

CGI stands for [Common Gateway Interface](https://datatracker.ietf.org/doc/html/rfc3875), it’s a protocol for executing scripts via web requests, and in the late 1990’s was the main way to write dynamic programs for the Web. It’s also the name of the Perl module that is now deprecated.

The first way to pass data is with the query string, (the portion of a URI beginning with ?), which you see in URLs like `https://example.com/?foo=bar`. This uses the “GET” request method, and becomes available to the program as $ENV->{QUERY_STRING}, which in this case is foo=bar (CGI programs receive their arguments as environment variables). But CGI provides the `param` method which parses the query string into key value pairs, so you can work with them like a hash.

Essentialy we have a script that is able to handle [HTTP requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). Subroutine x is called with the argument `param("x")`, here the `param()` function retrieves the value of parameter x from the HTTP request. And then this value is printed.

Easy enough right? We can start interacting with the script in a meaningful way now. Let's give the server some input to work with.

`level04@SnowCrash:~$ curl http://localhost:4747?x=hello`

![image](https://github.com/user-attachments/assets/58986977-11e9-4d9c-b7c5-f2a7a0597748)

If proper input sanitization is not made this script could be vulnerable to command injections. Since it runs with the priviliges of `flag04` the consequences could be severe. Let's exploit!

Our goal is to run `getflag` as `flag04` somehow. A naive first attempt would be to simply try `level04@SnowCrash:~$ curl http://localhost:4747?x=getflag`

![image](https://github.com/user-attachments/assets/1cb59aef-72ea-4ac6-ad08-f7d02426af4f)

This is the same as running `echo getflag`, it doesn't run `getflag` but simply prints it out as any other string. How do we subsitute commands in shell-scripting? By using **$()** or **backticks**!

![image](https://github.com/user-attachments/assets/ae0eac71-be2a-4d01-b18e-500eac4988ce)

This didn't exactly go as intended because `getflag` was executed as level04 before curl and the resulting string was put as parameters in the URL.

However if I use single quotes everything inside will be taken as literally and no command substitution will happen (at least as far as curl is conerned): `curl 'http://localhost:4747?x=$(getflag)'`

![image](https://github.com/user-attachments/assets/f314e30b-c37a-4d4c-af35-940a683f8e3e)

Yay! But how did this work if single quotes interpert the strings literally? Well that's the vulnerable script's fault. 

```
print `echo $y 2>&1`;
````

becomes,

```
print `echo $(getflag) 2>&1` ;
```

And now getflag will run with the privileges of the user under which the Perl CGI script is executed. Then echo will print out the result.

**Alternatively** we can make use of backticks too:

```
level04@SnowCrash:~$ curl 'http://localhost:4747/level04.pl?x=`getflag`'
```

![image](https://github.com/user-attachments/assets/390db6a7-889b-4a9b-99f1-7a325288c15b)

![image](https://github.com/user-attachments/assets/f252564f-cfb3-4278-976d-06e83465f6b9)

_Fin_
