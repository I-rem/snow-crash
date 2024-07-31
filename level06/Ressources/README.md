# level06
We are greeted with 2 files

![image](https://github.com/user-attachments/assets/e705909d-7955-4dda-82f1-556b08d28e73)

Classic, we will try to use the file permissions to our advantage. Let's test level06 and see what happens.

![image](https://github.com/user-attachments/assets/6cce5522-b0d1-45d8-8b54-dfed3e628d6b)

It expects a file as input.

![image](https://github.com/user-attachments/assets/fcf9a06a-e847-479a-a539-a96af71835f9)

And seemingly it just displays the contents of the file? We can learn more from the source code `$ cat level06.php`:

```
#!/usr/bin/php
<?php
function y($m) {
  $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m;
}
function x($y, $z) {
  $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = 
preg_replace("/\]/", ")", $a); return $a;
}
$r = x($argv[1], $argv[2]); print $r;
?>
```
Behold: [W3School](https://www.w3schools.com/php/php_syntax.asp). 

The **preg_replace()** function returns a string or array of strings where all matches of a pattern or list of patterns found in the input are replaced with substrings. `preg_replace(patterns, replacements, input, limit, count)`

**file_get_contents** — Reads entire file into a string

So this script has 2 functions. The first function `y` takes an argument `m`, makes some changes on m and then returns it. The second function `x` takes two arguments `y`and `z`. y is a file whose content is read and assigned to the variable `a`. It then calls pre_replace on a and once again returns the modified string. What happened to `z` you might ask. The answer is I don't know. 

x is called with the first and second parameters provided to the script as its arguments. Then the result is printed out. I tried to run the script with different values to see if I could manipulate the output without taking a closer look at the regex but failed. I guess we actually have to understand what all these patterns are for.

[regex101](https://regex101.com/) is an amazing all-in-one tool for anything related to regex. It even has a handy substitution function which is perfect for the task at hand.

- `$m = preg_replace("/\./", " x ", $m);` This takes the string m and replaces every instance of `.`with ` x `, `a.bc` -> `a x bc`
- `$m = preg_replace("/@/", " y", $m);` This takes the string m and replaces every instance of `@` with ` y`, `a@bc` -> `a ybc`  
- `$a = preg_replace("/\[/", "(", $a);` This takes the string a and replaces every instance of `[` with ` (`, `a[bc` -> `a(bc`
- `$a = preg_replace("/\]/", ")", $a);` This takes the string a and replaces every instance of `]` with `)`, `a]bc` -> `a)bc`
-  `$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);`: This uses the now deprecated `e` modifier. [See](https://www.php.net/manual/en/reference.pcre.pattern.modifiers.php).

What does it do?

_If this modifier is set, preg_replace() does normal substitution of backreferences in the replacement string, **evaluates it as PHP code**, and uses the result for replacing the search string._ [See](https://wiki.php.net/rfc/remove_preg_replace_eval_modifier)

So anything formatted like [x*] will be replaced. And the replacement part will call the function y with everything in between x and ]. The replacement string weill be executed as a PHP code.

This is a huge vulnerability!

Only the square brackets should be replaced with an input like '[test@gmail.com]' as it will not trigger the function y. Let's test it out:

`level06@SnowCrash:~$ echo "[test@gmail.com]" > /tmp/test`
`level06@SnowCrash:~$ ./level06 /tmp/test`

![image](https://github.com/user-attachments/assets/7db9f00c-7dfe-4682-965e-f26a5acda24e)

But if we find the right format we can observe different behaviour

![image](https://github.com/user-attachments/assets/0dcb8215-bc40-4bcb-9a54-5575b24f5e56)

That's fun and all but what we really want to do is run some code.

The part of the code that will be executed is the part between x and ]. 
So perhaps something like `[x getflag]` would work?

![image](https://github.com/user-attachments/assets/2e1ddd1b-6cf3-42a8-9427-e52e05d7527f)

Okay this is better than nothing. So since it will be interpreted as php code we should work on our syntax.

