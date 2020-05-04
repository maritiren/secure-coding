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

