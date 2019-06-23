# Solutions-for-Bandit
The solutions for the wargame Bandit at [OverTheWire](overthewire.org/bandit).

0. LEVEL 0: Connect with the web server with the given login credentials using ssh.

```
ssh bandit0@bandit.labs.overthewire.org -p 2220
Password: bandit0
```
**In every subsequent level, you have to use this login method to access the resources. The username for the i<sup>th</sup> level is `"bandit+str(i)"` and the password is the KEY that you will get in the (i-1)<sup>th</sup> level.**


**LEVEL0->LEVEL1:** Access the readme file using basic commands like ls, cat etc.

```
ls
readme
cat readme
KEY
```


**LEVEL1->LEVEL2:** Accessing file with the name -, as - is often used for directing the stdin stream so cat - does not work. A good solution is to use ./- as it specifies the file present in .(the current directory).

```
ls
-
cat ./-
KEY
```


**LEVEL2->LEVEL3:** Accessing a file that has spaces in its name. Put the backslash character or the escape charater in front of every space in the filename. Also if the filename doesn't have ' use cat 'file name'. We can also make use of *(The wild card).

```
ls
spaces in this filename
cat spaces*
KEY
```


**LEVEL3->LEVEL4:** Access the inhere folder using cd command. If you use ls to find the file containing the key then it wouldn't show any file as the filename is prfixed with a . which is usually used to make important system files hidden. You can see it via the command ls -a.

```
cd inhere
ls -a
. .. .hidden
cat .hidden
KEY
```

