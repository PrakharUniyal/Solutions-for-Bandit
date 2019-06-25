# Solutions-for-Bandit
The solutions for the wargame Bandit at [OverTheWire](overthewire.org/bandit).

**LEVEL0:** Connect with the web server with the given login credentials using ssh.
```
ssh bandit0@bandit.labs.overthewire.org -p 2220
Password: bandit0
```

*In every subsequent level, you have to use this login method to access the resources. The username for the i<sup>th</sup> level is `"bandit+str(i)"` and the password is the KEY that you will get in the (i-1)<sup>th</sup> level.*

---

**LEVEL0->LEVEL1:** Access the readme file using basic commands like ls, cat etc.

```
ls
readme
cat readme
KEY
```

---
**LEVEL1->LEVEL2:** Accessing file with the name -, as - is often used for directing the stdin stream so cat - does not work. A good solution is to use ./- as it specifies the file present in .(the current directory).

```
ls
-
cat ./-
KEY
```

---
**LEVEL2->LEVEL3:** Accessing a file that has spaces in its name. Put the backslash character or the escape charater in front of every space in the filename. Also if the filename doesn't have ' use cat 'file name'. We can also make use of *(The wild card).

```
ls
spaces in this filename
cat spaces*
KEY
```

---
**LEVEL3->LEVEL4:** Access the inhere folder using cd command. If you use ls to find the file containing the key then it wouldn't show any file as the filename is prfixed with a . which is usually used to make important system files hidden. You can see it via the command ls -a.

```
cd inhere
ls -a
. .. .hidden
cat .hidden
KEY
```

---
**LEVEL4->LEVEL5:** Access the inhere folder and try ls. There are 10 files indexed 00 to 09 and the key is in the only human readable file. Use file command to check the type of each file by './-file*'. The file with the KEY would be an ASCII text file.

```
cd inhere
ls
-file00 -file01 -file02 -file03 -file04 -file05 -file06 -file07 -file08 -file09
file ./-file*
...
./-file07: ASCII text
...
cat ./-file07
KEY
```

---
**LEVEL5->LEVEL6:** Access the inhere folder. It contains many subfolders further containing many files. The key is in a human readable, non executable file of size 1033 Bytes. find command with the flags for size(-size), readability(-readable) and executability(-executable) have to be used.

```
cd inhere
ls
...
find -size 1033c -readable ! -executable
./maybehere07/.file2
cat ./maybehere07/.file2
KEY
```

---
**LEVEL6->LEVEL7:** File can be anywhere on the server so go to the root directory first. Then use the find command with flags size, user and group as specified. A useful command over here can be 2>/dev/null more information on which can be found [here](https://askubuntu.com/questions/350208/what-does-2-dev-null-mean).

```
cd /
find -user bandit7 -group bandit6 -size 33c 2>/dev/null
.var/lib/dpkg/info/bandit.password
cat .var/lib/dpkg/info/bandit.password
KEY
```

---
**LEVEL7->LEVEL8:** Use grep to find the word "millionth" in the file data.txt

```
grep millionth data.txt
millionth KEY
```

---
**LEVEL8->LEVEL9:** The file has repeated lines out of which the only unique line is the KEY. Use sort and uniq commands to print this unique line.

```
sort data.txt | uniq -u
KEY
```

---
**LEVEL9->LEVEL10:** The file has the KEY followed by a lot of '='. Simply use the grep and strings commands.

```
strings data.txt | grep '='
...
========== KEY
...
```

---
**LEVEL10->LEVEL11:** The file is base64 encrypted. Decrypt it.

```
base64 -d data.txt
The password is KEY
```

---
**LEVEL11->LEVEL12:** The file is encypted with ROT13 encryption. As ROT13(ROT13(x))=x, use the implementation of ROT13 once on the file to get the original string and hence the key.

```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m
The password is KEY
```

---
