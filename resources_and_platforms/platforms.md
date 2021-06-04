# Platforms for practical exercises

In this file platforms for secure coding practice are listed. Here is a table
with the contents of this file:

* [PortSwigger Web Security Academy](#portswigger-web-security-academy)
* [Secure Code Warrior](#secure-code-warrior)
* [OWASP Secure Coding Dojo](#owasp-secure-coding-dojo)
* [OWASP Juice Shop](#owasp-juice-shop)
* [PentesterLab](#pentesterlab)
* [Kubernetes Goat](#kubernetes-goat)
* [CTF labs](#ctf-labs)

---

## Portswigger Web Security Academy
[Web Security Academy](https://portswigger.net/web-security) is my personal
favorite out of all these. It is from the attacker perspective, not coding 
perspective. However, the resources are well written and there are hints if 
you get stuck on a lab.


## Secure Code Warrior
[Secure Code Warrior](https://securecodewarrior.com/) is a platform that has 
several modes such as `training`, `tournament` and `assessment`.

The platform is free for 21 days. It seems to be made for enterprise, but they 
might appear at university events as well, or may do some deals with some unis
as far as I understand.

### Prerequisites:
None :O

---


## OWASP Secure Coding Dojo
OWASP plays an important role for secure coding. OWASP is short for the Open 
Web Application Security project. If you haven't heard about it, you should 
head over to the [resources page](../resources.md) and read the OWASP section.
OWASP also provides us with several platforms for practicing secure coding. One 
of them is [OWASP Secure Coding Dojo](https://owasp.org/www-project-secure-coding-dojo/). You should read the GitHub repository [wiki page](https://github.com/trendmicro/SecureCodingDojo/wiki).

The authors describe the platform as follows:
```
In other training sites or CTFs there is a puzzle aspect to the challenges 
which is great for pen-tester audiences but can make some developers lose 
interest. In the Secure Coding Dojo the focus is on demonstrating the 
vulnerability.
```

The platform consists of three training applications (this information is 
copied from the [Home page](https://github.com/trendmicro/SecureCodingDojo/wiki)
of the wiki. 
* "Insecure.Inc" is a Java site that demonstrates simple exploits based on SANS Top 25/OWASP Top 10
* "Hacker's Den" is a Serverless application for more advanced users based on OWASP Top 10
* "Security Code Review 101" is a static web site that runs directly from the Dojo Github

### Prerequisites:
* [Docker](https://docs.docker.com/get-docker/)
* [Docker Compose](https://docs.docker.com/compose/install/)
* Or, someone hosted it for you in which you only need to be connected to a 
network and go to their website. 


### Setup
Follow the guide in the first section "Basic Setup Instruction" of 
[Deploying with Docker](https://github.com/trendmicro/SecureCodingDojo/wiki/Deploying-with-Docker)
for a local setup.

To set up this platform, you must add an environment variable called `DATA_DIR`.
If you run on Linux, want to run the platform without adding the environment 
variable to the profile file of your favorite shell, and have to run sudo in 
order du run Docker Compose, then you can add the env variable like this:
```
sudo env DATA_DIR="/path/to/your/data_dir/" docker-compose up
```

### Getting started
1. Go to website, e.g. http://localhost:8081.
2. Click the `Login` button in the top right corner. Now click the `Register`
button.
3. Log in with the registered username and password. 
4. Press the yellow bar to make a team.
5. ... and you can start with the module of your liking!

I would advice you to google and do research around each challenge for broader
understanding of each topic. Instead of guessing at challenges. Also, I started
with the Code Review 101 module and found that a good start of the Secure Coding
Dojo.


---


## OWASP Juice Shop
There is some information about OWASP and some nice resources you should check
out above. [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) is a
vulnerable web application that you run locally on your own machine.

The platform is nice for testing your skills, but migth be a little difficult
if you have no previous experience or knowledge. However, the platform has an
[official companion guide](https://pwning.owasp-juice.shop/) which will help 
you through the platform. You may also find step-by-step tutorials at [the
OWASP web page](https://owasp.org/www-project-juice-shop/#div-tutorials). As 
mentioned in the same page, you can also try to use the 
[Tutorial Mode](https://pwning.owasp-juice.shop/part1/challenges.html#tutorial-mode).

To run Juice shop follow the [instructions](https://pwning.owasp-juice.shop/part1/running.html) 
at the official companion guide. The same information may be found in the 
README of the [GitHub page](https://github.com/bkimminich/juice-shop).


## PentesterLab
[PentesterLab](https://pentesterlab.com/) has a set of both free and paid 
exercises to 

Excercises that seem relevant for developers (from free verison):
* [Introduction to code review](https://pentesterlab.com/exercises/codereview/course)
* [Play Session Injection](https://pentesterlab.com/exercises/play_session_injection/course)
* [Play XML Entities](https://pentesterlab.com/exercises/play_xxe/course)
* [XSS and MySQL FILE](https://pentesterlab.com/exercises/xss_and_mysql_file/course)

All free excersises may be found [here](https://pentesterlab.com/exercises?dir=desc&only=free&sort=published_at).
**Work in progress - I haven't actually tested these yet**


## Kubernetes Goat
[Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat) can both be
setup locally and run through [Katacoda](https://katacoda.com/madhuakula/scenarios/kubernetes-goat).


## CTF labs
Here is a list of CTF labs:

* [PicoCTF](https://play.picoctf.org/)
* [CTFLearn](https://ctflearn.com/)
* [Cyber Talents](https://cybertalents.com/)

