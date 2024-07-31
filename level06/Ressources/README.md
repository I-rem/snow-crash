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
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```
https://regex101.com/
