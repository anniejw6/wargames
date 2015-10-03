# 7-8
The password for the next level is stored in the file data.txt next to the word millionth

```
grep millionth data.txt 
```

# 8-9

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

```
sort data.txt | uniq -u  
```

sort lines, print only unique line

# 9-10

The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several ‘=’ characters.

```
grep '==' data.txt -a
```

-a means process binary file as text

# 10-11
The password for the next level is stored in the file data.txt, which contains base64 encoded data
```
cat data.txt | base64 --decode
```

# 11-12
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```


# 12-13
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

```
mkdir /tmp/ajw
cp data.txt /tmp/ajw
cd /tmp/ajw
mv data.txt tmp.txt
xxd -r tmp.txt tmp.bin
file tmp.bin
zcat tmp.bin | bzcat | zcat | tar xO | tar xO | bzcat | tar xO | zcat 
# 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

# 13-14
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on
```
scp bandit13@bandit.labs.overthewire.org:sshkey.private .
chmod 400 sshkey.private
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org
cat /etc/bandit_pass/bandit14
# 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

# 14-15
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

```
telnet localhost 30000
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
# BfMYroe26WYalil77FoDi9qh59eK5xNr
```

# 15-16
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -quiet and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…
```
openssl s_client -connect localhost:30001 -quiet
# cluFn7wTiGryunymYOu4RcffSxQluehd
```

# 16-17
The password for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
```
nmap -sT localhost -p31000-32000
echo cluFn7wTiGryunymYOu4RcffSxQluehd | openssl s_client -connect localhost:31790 -quiet
# Okay, it's an RSA Key
mkdir /tmp/aw2
touch /tmp/aw2/key
vim /tmp/aw2/key 
exit
scp bandit16@bandit.labs.overthewire.org:/tmp/aw2/key .
chmod 600 key
ssh -i key bandit17@bandit.labs.overthewire.org
```

# 17-18
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
```
diff -u passwords.old passwords.new 
# kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

# 18-19
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.


```
ssh -t bandit18@bandit.labs.overthewire.org /bin/sh
cat readme
# IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

# 19-20
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used to setuid binary.
```
~/bandit20-do 
~/bandit20-do cat /etc/bandit_pass/bandit20
# GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

# 20-21
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: To beat this level, you need to login twice: once to run the setuid command, and once to start a network daemon to which the setuid will connect.

NOTE 2: Try connecting to your own network daemon to see if it works as you think
```
./suconnect 3222
# in separate window
nc -l 30000
# gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
```

# 21-22
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

```
cd /etc/cron.d/
cat cronjob_bandit22 
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
# Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```

# 22-23
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

```
cd /etc/cron.d/
cat cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
# jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

# 23-24
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…
```
cd /etc/cron.d/
cat cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh
vim /var/spool/bandit24/test24.sh
```
create shell script:
```
#!/bin/bash

cat /etc/bandit_pass/bandit24 >> /tmp/aw24/pass.txt
touch /tmp/aw24/ok
```
create file
```
chmod 777 /var/spool/bandit24/test24.sh
mkdir -p /tmp/aw24/
touch /tmp/aw24/pass.txt
ls /var/spool/bandit24
ls /tmp/aw24/
cat /tmp/aw24/pass.txt
# UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

# 24-25
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

```
mkdir /tmp/aw25/
vim /tmp/aw25/test25.sh
```

```
#!/bin/bash

for i in {0..9};
do
    echo $i
    for j in {0..9};
    do
        for k in {0..9};
        do
            for l in {0..9};
            do
                pass=$i$j$k$l
                echo UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $pass | nc localhost 30002
            done
        done
    done
done
```

```
chmod 777 /tmp/aw25/test25.sh
bash /tmp/aw25/test25.sh >> /tmp/aw25/test.txt
sort /tmp/aw25/test.txt | uniq -u 
# uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
```
