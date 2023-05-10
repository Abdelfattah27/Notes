# Linux

## linux file system

- `/`: Root file system
  - `/boot`: Contains boot loader files, kernel images, and other files required for booting the system.
  - `/usr`: Contains user-shared files like system-wide binaries, libraries, documentation, and configuration files.
  - `/mnt`: Used as a mount point for mounting external file systems.
  - `/media`: Used as a mount point for mounting removable media such as CDs, DVDs, and USB drives.
  - `/srv`: Used to store data for services provided by the system.
  - `/opt`: Used to install optional or third-party software.
  - `/lib`: Contains libraries that are required by the system and user applications.
  - `/etc`: Contains configuration files for the system and applications.
    - `/etc/passwd`: Contains information about the users of the system.
    - `/etc/shadow`: Contains password information for the users of the system.
    - `/etc/group`: Contains information about the groups in the system.
    - `/etc/sudoers`: Contains the permissions for users to use the `sudo` command.
  - `/home`: Contains a subdirectory for each user on the system.
  - `/dev`: Contains files that represent devices on the system.
    - `/dev/null`: A special file that discards all data written to it.
    - `/dev/pts/0`: A file that represents the screen for terminal emulators.
  - `/proc`: Contains information about running processes and system configuration.
  - `/root`: Home directory for the root user.
  - `/bin`: Contains binary files that all users can access.
  - `/sbin`: Contains system binaries that are used for system administration tasks. These binaries are typically only executable by the root user.
  - `/var`: Contains variable files that change continuously like mails and log files.
    - `/var/log`: Contains system log files.
  - `/tmp`: Used to store temporary files that are created by the system and user applications. These files are typically deleted when the system is rebooted.

## Modes

To switch modes in VMware, use `ALT + CTRL + F1`. However, before doing so, you need to edit the hotkey preferences from the Edit menu to use `ALT`, `CTRL`, and `SHIFT`.

- Comment Line Mode: This is the default mode which uses the TTY (teletype terminal).
  - To switch to TTY2: `CTRL + ALT + F2`
  - To switch to TTY3: `CTRL + ALT + F3`
- CLI with GUI:
  - This mode is not the default.
  - To switch to GUI: `CTRL + ALT + F1`

## Users

- Root: The administrator, also known as the super user, who has all permissions.
  - Has a user ID of ZERO (0).
- Normal Users: Any user who has no permission except those assigned by the root.
  - UserID is greater than or equal to 500.
- Service Users: These are users who have permission related to the server.
  - UserID is greater than 0 and less than 500 (1-499).
  - Do not have permission to log in to the machine.
- All users are stored in `/etc/passwd`.
  - Each row is represented by:
    - UserName:Password(x):userId:GroupId:GECOS(any arbitrary value):homeDir:Bash(sh, zsh, ksh, csh, python)
- User password is stored in `/etc/shadow`.
  - `!!` means there is no password for this user.
- The shell for the service user is `/usr/sbin/nologin`.
- To make a user have no permission to log in to your system, set their shell to `/usr/sbin/nologin`.
  - `usermod -s /usr/sbin/nologin`
- To add a file that will appear for each new user, add the file to `/etc/skel/test_file`.

### commends related with users

- `su "username"`: switch user - to switch to a new user
  - `su -`: switch to root user
- `useradd "username"`
  - `useradd -p password -u userId -g GroupId -c GECOS -s shell`
- `passwd`: to change the password of the user
- `w`: get all signed-in users on the machine
- `id`: information about the user that registered now such as uid, gid, and others
  - `id "userName"`
  - `id -u "userName"`: to get the id of the user
- `usermod`: to change the user info such as uid, GECOS, gid, and others
  - `usermod -G`: to set the secondary Group
  - `usermod -aG`: to append a secondary group
- `groupmod`: to change the group info
  - `groupmod -g GID -n newName admin`
- `userdel hamada`
  - `userdel -r`: delete user with the home directory
- _chage_ : to change the password configuration as the expire date and others

### Permissions

#### User Id

- for every user has a id which is an identifier to him
- for root user had s UID = 0
- each user will have a
  - primary group : user private group
  - secondary group : groups like designers and programmers
- **useradd ali** : this will create user and group named ali

  - this group is user private group

- Others : users who isn't the Owners or the memper of the related file groups
- **Permissions** will be represents for each file as follow
  - you can make **READ , WRITE , EXECUTE**
  - we can determine which can do what by partition the people into three categories **OWNER, GROUP, OTHER**
- **b rwx rwx rwx** : this is how we can represent the permissions files on 10 bits
  - first bit represent the type of the file
    - **d** : for directory
    - **-** : for regular files
    - **c** : for char files (chars for keyboard "don't forget **every thing is file** ") represent data transferred char by char
    - **b** : for block file (transfer data via blocks -chunk- not chars )
    - **l** : link file (short cut)
    - **p** : pipe file
  - first 3 bits will be permissions of the OWNER
  - second 3 bits will be permissions of the Group
  - third 3 bits will be permissions for Others

#### modes

- r :
  - for file mean read the content of the file
  - for folder list the files of the dir using ls
- w :
  - for file means edit the content of the file or delete it
  - for folder add files to folder or remove it
- x :
  - for file means execute the file if it was app or script
  - for directory means you can make **cd** to the directory
    **_chown_** : to change the owner
- the root must who change the owner of the files
- chown ali /home/student/hamada :change the owner of hamada to ali
- chown :admins /home/student/hamada : change the group
- chgrp admin hamada : change the group of the file hamada to admin

#### change permissions

**_chmod_**

- chmod g+w hamada : add write permission for the group
- chmod o+r hamada : add read permission for the others
- chmod u-w hamada : delete write permission for the owner
- chmod u+x hamada g+x : change mode for more than one user
- chmod u+rw hamada: read write for owner
- chmod ug-w hamada: write for owner and group
- chmod u+w hamada , u-rx **====** chmod u=w
- chmod a+x hamada **====** chmod ugo+x **====** chmod +x
- chmod 644 hamada : mean hamada **wr- r-- r--**
- chmod =rwx hamada : change the owner permissions to be read, write and execute
- chmod -R 644 hamada : change mode for the directory hamada and the files which it contains

#### special permissions

## Commends

- hostname : print the name of machine
- df -h :print all the partitions that i have in a human readable form
- which passwd : give the location of the commend "passwd" on the machine
- pwd : print working directory
- cd : change directory

  - ./Desktop ~~ /home/abdelfattah/Desktop
  - "cd -" change to previous working directory
  - "cd | cd ~" go to your home directory

- "cal" get the calender
  - cal 11 2011
- "date" get the date of today
- "dir | dir --color" list all directories and files
- "ls" like dir
  - ls -l : long list
  - ls -a : show all file include hidden one
  - ls -lh :long list with human readable size
  - ls -lt : list based on access time
  - ls -lr : reverse the order
  - ls -lR : recursive > like tree
  - ls -t : order by the access time
- ctrl + s : lock the screen on the tty cli mode
- ctrl + q : unlock to the screen on the cli mode
- "touch hamada" : create new file named hamada
- "mkdir hamada" : create new directory
  - mkdir -p hamada/ahmed/mohamed : create hamada , ahmed , mohamed
- cp file1 file5 : copy file1 to file5 > if file5 doesn't exist it will create it
  - cp -r hello world : copy directory hello to directory world
  - cp -r hello/\* world : copy content of directory hello to directory world
  - cp file1 file2 file3 hamada : copy list of files into directory (hamada must be directory)
- mv
  - mv file1 new_file1 : rename file1 to new_file1
  - mv file1 /root/Desktop : move file into directory
  - you can move multiple files into directory
  - you can move multiple directories under a directory
- rmdir : delete empty directory
- rm
  - rm : for root user must be interactive (ask you if you sure to remove it)
  - rm -r : remove directory can be not empty ()
  - rm -rf : remove all without asking me
- nautilus Desktop : open desktop in the gui
- echo
  - echo "hello world" : print to the console the "hello world"
  - echo ~abdelfattah : print the home directory for user abdelfattah
  - echo ~/abdelfattah : print "/root/abdelfattah" : regardless if there's no dir in this place just print it
  - z=1 : define a variable named z and have value of 1
  - echo $z : print the value of the variable z (variable substitution)
  - echo $(date) : print the value of the commend date (commend substitution)
  - echo $\[z+1\] : print the result of the mathematic operation on the summing of variable z and 1
  - echo $((z+1)) : print the result of the mathematic operation on the summing of variable z and 1
- less : open the content of the file using a file viewer
  - view a data page by page starts with the first line into the last line and after viewing press q to get out from mode
- more : open the content of the file using a file viewer
  - view a data page by page starts with the first line into the last line and after viewing it get out automatically
- wc hamada.txt : get the number of lines followed by the number of words followed by the number of chars
  - wc -l : get the count of lines
  - wc -w : get the count of words
  - wc -c : get the count of chars
- head -n 10 = head -10 : get the first 10 lines from the file
- tail -n 10 = tail -10 : get the last 10 lines from the file
- tee /dev/hamada : used in pipelining for redirect the input to specific place

## Wildcards

- \* means any chars
  - a\* : any string begin with a
  - \*a : any string ends with a
  - \*a\* : any string have a in the middle or begin or end
  - ? this for single char
  - ??f : have any two chars at begin followed by f
  - \[abf\]\* : all files that starts with a or b or f then followed by any char
  - \[^abf\]\* : all files that not start with a or b or f then followed by any char
  - \[a-l\]\* : all files that starts with any char for a to l then followed by any number chars
  - ls \[\!a\]\* : all files that not start with a
  - ls \[\!a\] : all files that not start with a and have length of one char

## Asking for Help

- date --help : this is the simple asking foe help
- info passwd
- pinfo passwd
- man date : get the manuel page fot the commend and it partitions to multiple sections
  - we have 9 chapters each chapter concerned with something
    - 1 for info
    - 5 for configurations
  - ti search for specific functionality you can use "man -k "print file"" or "apropos "print file" "
  - "backspace" to search on man page just press backspace and it well permit you to write and then write what you want to search for
  - "n" : prev match : to move between the matches
  - "N" : next match : to move between the matches
  - "g" : to get to the first page
  - "G" : last page of the man page

## techniques

- \> : redirect the output to a file
- \>\> : append to file
- 2\> : redirect the stander error to file
- &\> : redirect the error and the data into file
- 2\>&1 : append the error and data into file
  - ls ahmed.txt hamada.txt >> mahmoud.txt 2>&1
- | : pipe, get the output of the commend as input of other commend to do some filters
  - ls -lR / | less
  - ls | wc -l
  - ls -t | tail -5 > out.txt

## vim editor

have three modes

1. insert mode : identified by --insert--
2. commend mode : identified by "file_name"
   1. yy : to copy the line
   2. dd : cut the line
   3. p : to paste the copied
3. exec mode : identified by :
   1. q : for quit
   2. w : for write (save)
   3. wq : fro write thin quit
   4. q! : quit without saving
   5. set number : to number the lines

### commends

- i : to go from commend mode to insert mode
- : : to go from commend mode to exec mode
- esc : to go to commend mode from insert or exec
-
