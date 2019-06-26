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
**LEVEL12->LEVEL13:** The file is a hexdump and has been compressed many times. First copy the file in a directory where you have permission to work on it. Then use the reverse hexdump command "xxd -r". Then check the type of compressed file and keep decompressing it by the appropriate decompressing command for its file type. The final file will be an ASCII Text file called data8.bin and will contain the KEY.

```
mkdir /tmp/userx
cp data.txt /tmp/userx
cd /tmp/userx
xxd -r data.txt > x.txt
file x.txt
x.txt: gzip compressed data, was "data2.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
zcat x.txt > data_zcat
file data_zcat
data_zcat: bzip2 compressed data, block size = 900k
bzip2 -d data_zcat
file data z_cat.out
data_zcat.out: gzip compressed data, was "data4.bin", from Unix, last modified: Tue Oct 16 12:00:46 2018, max compression
zcat data_zcat.out > data_zcat_2
file data_zcat_2
data_zcat_2: POSIX tar archive (GNU)
tar xvf data_zcat_2
data5.bin
file data5.bin
data5.bin: POSIX tar archive (GNU)
tar xvf data5.bin
data6.bin
file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
tar xvf data6.bin.out
data8.bin
file data8.bin
data8.bin: gzip compressed data, was "data9.bin", from Unix, last modified: Tue Oct 16 12:01:21 2018, max compression
zcat data8.bin > data8_zcat
file data8_zcat
data8_zcat: ASCII text
cat data8_zcat
The password is KEY
```

---
**LEVEL13->LEVEL14:** There is an ssh key. Use it to login as bandit14 in the localhost. There the file containing the KEY is /etc/bandit_pass/bandit14.

```
bandit13@bandit:~$ ls
sshkey.private
ssh bandit14@localhost -i sshkey.private
cd /etc/bandit_pass
strings bandit14
KEY
```

---
**LEVEL14->LEVEL15:** Connect to localhost at port 30000 via nc command. Input the KEY from the previous level to retrive this level's KEY.

```
nc localhost 30000
*previous_KEY*
Correct!
KEY
```

---
**LEVEL15->LEVEL16:** Connect to localhost at potr 30001, this time using s_client command in the openssl command toolkit.

```
openssl s_client -connect localhost:30001
...
*previous_KEY*
Correct!
KEY


closed
```

---
**LEVEL16->LEVEL17:** One of the ports from 31000 to 32000 is open and contains the RSA private key to be used for the next level. Use nmap to find that port. Connect with it and copy the RSA key into a file on your machine and ensure that it can be accessed by ony you(Windows users might have trouble here). Use it in logging into the next level.

```
nmap localhost -p31000-32000
Starting Nmap 7.40 ( https://nmap.org ) at 2019-06-25 21:22 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00036s latency).
Not shown: 999 closed ports
PORT      STATE SERVICE
31518/tcp open  unknown
31790/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds
openssl s_client -connect localhost:31790
...
*previous_KEY*
...
RSA private KEY
```

Terminate the connection and copy that RSA KEY into a file on your machine. Change its permission to 600 using chmod. Use it in the next level using the -i flag of ssh.

---
**LEVEL17->LEVEL18:** Simply find the difference in the new file as compared to the old file.

```
diff passwords.old passwords.new
< hlbSBPAWJmL6WFDb06gpTx1pPButblOA
---
> KEY
```

---
**LEVEL18->LEVEL19:** The .bashrc file of this level has been tweaked so as to log you out as soon as you try to login through ssh. However you can still run a single command each time you attempt to login through ssh.

```
ssh ... ls
...
Readme
ByeBye!
ssh ... cat Readme
...
KEY
ByeBYe!
```

---
**LEVEL19->LEVEL20:** The file in the home directory is a setuid binary file that temporarily escalates our privileges. Use it to access the otherwise prohibited file containing the key.

```
./bandit20-do
Run a command as another user.
  Example: /home/bandit19/bandit20-do id
./bandit-do cat /etc/bandit_pass/bandit20
KEY
```

---
**LEVEL20->LEVEL21:** There is a setuid binary file that  makes a connection to localhost on the port we specify. It gives the next KEY only if it recieves the previous KEY as a message in this connection. Use the listening flag(-l) of newcat to start transmitting from this port. Connectto this port through another terminal using the file.

```
nc -l -p 12345 < /etc/bandit_pass/bandit20
```
```
./suconnect 12345
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
```
```
nc -l -p 12345 < /etc/bandit_pass/bandit20
KEY
```

---
**LEVEL21->LEVEL22:** There is a program that is running periodically through cron command. Open that program and you will find that it is transferring the KEY from its original prohibited file to a different one. Simply open that file.

```
cd /etc/cron.d/
ls
cronjob_bandit22 cronjob_bandit23 cronjob_bandit24
strings ./*
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
KEY
```

---
**LEVEL22>LEVEL23** Similar to the previous level only this time the bash script puts the KEY in a file named identically as the MD5 hashcode of the username.

```
cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytargeth
echo I am user bandit23| md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
cat 8ca319486bfbbc3663ea0fbe81326349
KEY
```

---
