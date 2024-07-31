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

`$m = preg_replace("/\./", " x ", $m);` This takes m and replaces every instance of `.`with `x`, `a.bc` -> `axbc`
