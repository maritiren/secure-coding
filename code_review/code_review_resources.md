# Code review resources

This note is as of this writing mostly based on PentesterLab's exercise 
"Introduction to code review". 

Content list:
* [Resources](#resources)
* [Grep for bugs (string matching)](#grep-for-bugs)
* [Follow user inputs](#follow-user-inputs)
* [Check one functionality at a time (login, password reset..)](#by-functionality)

## Resources
* Linting tools 
* [GRaudit](https://github.com/wireghoul/graudit): "graudit is a simple script 
and signature sets that allows you to find potential security flaws in source 
code using the GNU utility grep. It's comparable to other static analysis 
applications like RATS, SWAAT and flaw-finder while keeping the technical 
requirements to a minimum and being very flexible."
* [CodeQL](https://securitylab.github.com/tools/codeql): "Discover
vulnerabilities across a codebase with CodeQL, our industry-leading semantic 
code analysis engine. CodeQL lets you query code as though it were data. Write 
a query to find all variants of a vulnerability, eradicating it forever. Then 
share your query to help others do the same."
* [PentesterLab's "Introduction to code review" exercise](https://pentesterlab.com/exercises/codereview/course/#what-to-look-for).

## What to look for
This entire section is copy/pasted from 
[PentesterLab](https://pentesterlab.com/exercises/codereview/course).

When doing a review, you need to look for everything:

* Weird behavior
* Missing checks
* Complexity
* Security checks already in place
* Differences between 2 functions/methods/classes
* Comparison and conditions (if/else)
* Regular expressions/string matching
* What is missing?

You will probably end up seeing function/class/method you don't know. To solve this issue, you need to

* Google it
* Test its’ behavior

It’s going to take time (especially early on) but the more code you review, the 
easier it gets. Make sure you create a snippet with the function/class/method 
to test its' behavior. It will be convenient for your future reviews. To test 
it, you need to run it locally and try to find some edge cases that the 
developers may have missed.

Also, I want to add a comment. There are tools that can be configured to 
prevent a couple of these bullet point. E.g. linting tools. Linting tools can
prevent complexity, duplication of code and can make the code easier to read.

## Grep for bugs
To find low-hanging fruits, grepping after known vulnerable functions or other
patterns might help disclose vulnerable code. The snippet below shows how to 
grep for usage of the `system` function in PHP:
```
$ grep -R 'system\(\$_' *
```

You may use [GRaudit](https://github.com/wireghoul/graudit) to get a starting
point containing several databases with predefined strings to grep for. Each 
database is for a specific programming language. Here is an example of running
the tool:
```console
$ git clone https://github.com/wireghoul/graudit
$ graudit -l
===========================================================
                                      .___ __  __
          _________________  __ __  __| _/|__|/  |_
         / ___\_` __ \__  \ |  |  \/ __ | | \\_  __\
        / /_/  >  | \// __ \|  |  / /_/ | |  ||  |
        \___  /|__|  (____  /____/\____ | |__||__|
       /_____/            \/           \/
              grep rough audit - static analysis tool
                  v2.4 written by @Wireghoul
=================================[justanotherhacker.com]===
/path/to/graudit/signatures/actionscript.db
/path/to/graudit/signatures/android.db
/path/to/graudit/signatures/asp.db
/path/to/graudit/signatures/c.db
/path/to/graudit/signatures/cobol.db
/path/to/graudit/signatures/default.db
/path/to/graudit/signatures/dotnet.db
/path/to/graudit/signatures/exec.db
/path/to/graudit/signatures/fruit.db
/path/to/graudit/signatures/go.db
/path/to/graudit/signatures/ios.db
/path/to/graudit/signatures/java.db
/path/to/graudit/signatures/js.db
/path/to/graudit/signatures/nim.db
/path/to/graudit/signatures/perl.db
/path/to/graudit/signatures/php.db
/path/to/graudit/signatures/python.db
/path/to/graudit/signatures/ruby.db
/path/to/graudit/signatures/secrets-b64.db
/path/to/graudit/signatures/secrets.db
/path/to/graudit/signatures/spsqli.db
/path/to/graudit/signatures/sql.db
/path/to/graudit/signatures/strings.db
/path/to/graudit/signatures/xss.db
$ graudit -d /path/to/graudit/signatures/js.db .
===========================================================
                                      .___ __  __   
          _________________  __ __  __| _/|__|/  |_ 
         / ___\_` __ \__  \ |  |  \/ __ | | \\_  __\
        / /_/  >  | \// __ \|  |  / /_/ | |  ||  |  
        \___  /|__|  (____  /____/\____ | |__||__|  
       /_____/            \/           \/           
              grep rough audit - static analysis tool
                  v2.4 written by @Wireghoul
=================================[justanotherhacker.com]===
./register.php-5-
./register.php:6:  if ((isset($_POST['username']) and isset($_POST['password']) and isset($_POST['password_again']))){
./register.php:7:    if ($_POST['password'] === $_POST['password_again']) {
./register.php:8:      if (User::register($_POST['username'], $_POST['password'])){
./register.php:9:        setcookie("auth", User::createcookie($_POST['username'], $_POST['password']));
./register.php-10-        header( 'Location: /index.php' ) ;
##############################################
./classes/user.php-15-  public static function addfile($user) {
./classes/user.php:16:    $file = "files/".$user."/".basename($_FILES["file"]["name"]);
./classes/user.php-17-    if (!preg_match("/\.pdf/", $file)) {
./classes/user.php-18-      return  "Only PDF are allowed"; 
./classes/user.php:19:    } elseif (!move_uploaded_file($_FILES["file"]["tmp_name"], $file)) {
./classes/user.php-20-      return "Sorry, there was an error uploading your file.";
##############################################
./classes/user.php-38-    $sql.= "\"";
./classes/user.php:39:    $result = mysql_query($sql);
./classes/user.php-40-    if ($result) {
##############################################
./classes/user.php-53-    $sql.= "\")";
./classes/user.php:54:    $result = mysql_query($sql);
./classes/user.php-55-    if ($result) {
##############################################
./classes/user.php-71-    $sql.= "\"))";
./classes/user.php:72:    $result = mysql_query($sql);
./classes/user.php-73-    if ($result) {
```

Seems like it is not possible to insert several databases to graudit. From my
tries, it looks like it processes the latter one if each are added with the 
`-d` flag. While if you add all databases after one `-d` flag, it will parse
the databases as files to audit. 

Note to self: Could be nice with a list of insecure functions for each language 
as well.

Resources: 
* [GRaudit](https://github.com/wireghoul/graudit): "graudit is a simple script 
and signature sets that allows you to find potential security flaws in source 
code using the GNU utility grep. It's comparable to other static analysis 
applications like RATS, SWAAT and flaw-finder while keeping the technical 
requirements to a minimum and being very flexible."
* [CodeQL](https://securitylab.github.com/tools/codeql): "Discover
vulnerabilities across a codebase with CodeQL, our industry-leading semantic 
code analysis engine. CodeQL lets you query code as though it were data. Write 
a query to find all variants of a vulnerability, eradicating it forever. Then 
share your query to help others do the same."

## Follow user inputs
Find all ways to provide data for the application:
* Requests (POST, GET, PUT, etc.)
	* Headers
	* Query params
* Cookie
* Data from the database
* Data read from file or cache
* ...

## By functionality
You may check each functionality, e.g. "Password reset", "Database access", 
"Authentication". Review the code of that functionality. If you can, you should
also try to do manual tests on the application. 

