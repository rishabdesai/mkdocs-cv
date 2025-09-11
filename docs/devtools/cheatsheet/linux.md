# Linux Commands

Basic Linux commands.

## About UNIX and Linux

UNIX : Unix is a family of Operating Systems (OS). It is made up of three parts; kernel, shell and programs.

There are different versions of UNIX. Most popular flavor is LINUX. As Linux is is Unix-like system, we can use Unix commands to control it.

Linux is designed by Linus Torvalds in 1990’s and dedicated to open source community. It is also referred as GNU/Linux as it is based on a modified version of GNU (GNU = recursive acronym “GNU is not Unix”)

```sh
ls          #list the files/directories
ls -a       #list the files/directories including hidden files.
ls -l       #list the files/directories in long format.
ls -lh      #h = human-readable format of number
ls –l /     #list the files/directories in long format of root user.
```
```sh
pwd         #present working directory
whoami      #user name
mkdir       #to make new directory.
mkdir -p    #create directory & sub-dir at same time (ex. mkdir -p dir1/dir2)
```

```sh
cd change directory
cd ..       # change directory to parent (note: space between cd and .. )
cd –        #change directory to previous directory
cd ~        #change directory to home-directory
cd ../..    #go to parent’s parent directory (two-level up)
clear       #clear the screen or using the keyboard ( ctrl+ l )
```
```sh
cp          #copy
mv          #move or rename
rm          #remove (empty file)
rm -rf      #remove file (non-empty)
rm -Ri      #i= interactive
rmdir       #remove directory (empty)
cat         #concatenation – to display the content of the file on screen
grep        #it searches files for specified words or patterns.
```

```sh
!! run      #the previous command Ex. Sudo !!
uname -s    #gives kernel name
uname -o    #gives Operating System name
uname -a    #gives all of the System information

dmesg | less        #to show Linux boot messages
cat/proc/cpuinfo    #to show processor info, model name and cup architecture info.
cat /proc/meminfo   #info. about memory installed

cat > textfile1     #send screen input to file name textfile1
```

```sh
ctrl + U = clear content of the command line only
ctrl + A = Bring cursor to start of command line
ctrl + E = Bring cursor to end of command line
ctrl+R = search for previous commands in command prompt.
```

```sh
history     #to see command history
!<history command serial number> = execute particular command (ex. !102 )
```

```sh
groups  #shows list of groups for current user
chmod   #Change Mode (two ways – number mode and letter mode)
chmod 777 t1.txt
chmod ugo=rwx t1.txt OR chmod a=rwx t1.txt
chmod ugo-wrx t1.txt OR chmod a-wrx t1.txt
chmod ugo+rx t1.txt OR chmod a+rx t1.txt
chmod -w t1.txt (remove write for all)
chown root t1.txt (change owner of t1.txt file to root)
chgrp root t1.txt (change group of t1.txt file to root)
```


```sh
find
find . -name t1.txt #(find a file named t1.txt at current directory)
find . iname t1.txt #(i = ignore case)
find . -iname T* -exec ls -l {} \; #(exec = execute the command next to it based on input before it)
find . -iname T* -exec rm -rf {} \; #(remove all files starting with T or t)
alian #(to create another name for a commnad)
ex.
alias c=’clear’
```


```sh
sudo
sudo su         #(change user to root)
sudo su –       #(change user to root and import all env veriable)
sudo su – test      #(change user to test)

```

```sh
Cron jobs
crontab -l          #(list all cron jobs)
crontab -l -u root  #(list of cron jobs for user root)
for ref : (https://crontab.guru/)
```


```sh
Add new user

Command : useradd

step 1) create user with new home directory
useradd -m -u 111 -c "this is comment" -s /bin/bash tom
useradd -m -s /bin/bash jerry
useradd -d /home/user1 -m user1 -s /bin/bash

useradd -m -s /bin/bash user1
useradd -m -s /bin/bash -G sudo user1


-m option ensures that a new home directory will be created for user tom
-c to enter comment
-u to enter unique ID (UID)
-s to set default shell as bash

step 2) set password to newly created user
passwd tom

step 3) switch to newly created user
su - tom

step 4) exit
exit
```

```sh
delete user

userdel tom
userdel -r tom (To remove the user and their data in one shot)
userdel rm -r /home/tom (to remove /home directory for the user after the account was already removed
delete user and take backup
sudo deluser –remove-all-files –backup –backup-to /user-backups/ tom
```

```sh
Modifying user attributes

command is usermod
Add a user to sudo group

sudo usermod -aG sudo tom
-a = add
-G = Secondary Group
```

```sh
/etc/passwd file

cat /etc/passwd

Entries are in below format:

chgrp hello /home/folder1 (change group of folder1 to hello)

username : x (pw is encrypted): UID : GID : User info : home dir of user (/home/tom): users shell (/bin/bash)

On a Linux system, user accounts and groups are actually referenced by their IDs. Each time we reference user tom, we are actually just referencing UID 1001.
When a user is created, the system by default automatically assigns the next available UID
UID do not synchronize between installations.
When we create a user, the users primary group is the same as their username.
```

```sh
/etc/shadow file

sudo cat /etc/shadow

username : password in hash : no in days since the password is changes from Unix epoch)
```

```sh
/etc/skel file

Files contented in /etc/skel are copied into the home directory for all new users when we create them

ls -la /etyc/skel
```

```sh
Switch users

su = to switch from one user to another. If you enter su with no option, it will switch to root and ask for root password.

sudo su - = to swithc from user account that has sudo access ( to return to previously logged-in accout, simply type exit )

su - <username1> = To switch to an account other than root. (enter username1’s password)

sudo su – <username1> = to switch to an account other than root ( enter your username password)
```

```sh
Groups

With groups , you can more efficiently control a users access to resources on your server.
check user group

In Linux, just one user and just one group assigned to each file or directory. It is just one-to-one ownership.
While each file or directory can only have one group assignment, any user account can be a member of any number of groups.
groups = To check which groups that user is a member of.

When we create a user account, your also creating a group with same name as the user.
/etc/group = contains information regarding the groups that have been created on your system.

name of group : x : GID : comma-separated list of each user that is a member of each of the groups

sudo groupadd admins = create new group

sudo groupdel admins = delate a group

associate users with groups

sudo usermod -aG <groupname> <username>
-a = append
-G = meaning we like to modify secondary group membership

change primary group of a user

sudo usermod -g <groupname> <username>d

remove a user from a group

sudo gpasswd -d <username> <grouptoremove>


```


```sh
Lock users account

sudo passwd -l <username>

unlock users account

sudo passws -u <username>
```

```sh
viewing permissions

ls -l
-l = to show files as long listing

On each line, there are seven fields of information.

Object tyle | USER | GROUP | WORLD
```


```sh
Linux – Networking commands

hostname    #display hostname
hotname -i  #display IP address
ping        #to check connection with server.
host:       #displays IP address of given domain name & vice versa.
ifconfig    #to see network setting and IP address

ip a       #get IP address

Firewall:
sudo ufw enable         #to enable firewall
sudo ufw status         #to check status of firewall
sudo ufw allow http     #to allow http traffic
sudo ufw deny http      #to deny http traffic
sudo ufw delete http    #to delte the rule
sudo ufw reset          #to set firewall to its default state

free        #RAM memory status
vmstat      #view memory stats

PS          #processes [ ps -e, ps -f ]

Kill a process:
first find process id and then kill it

ps -e | grep <process-name>
kill <PID>

start a server

sudo systemctl start apache2
sudo systemctl restart apache2
sudo systemctl stop apache2

Creating a script
1) create scirpt starting with #!/bin/sh and save with extension .sh
2) Make script executable with chmod +x test.sh
3) To execute script ./test.sh
```


```sh
Redirection
Redirection can be done for input and output
 
<       # input redirection
> or >> # output redirection
2>      # error redirection


```

```sh
Archive

tar = tape archieve

tar -cvf  #c = create v = verbose, f = specify the archieve
tar -xvf #x = extract
tar -tvf #t = view/Display/table of contents

Archive and compress

tar -cvzf #archiving and compress (z option compresses and archives together)
tar -xvzf #unarchive and compress
tar -tvzf # display list

gzip = to compress a file

gzip -k file1.tar #compress
gzip -d file1.tar.gz #uncompress

```