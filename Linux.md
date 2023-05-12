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
- `chage` : to change the password configuration as the expire date and others

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

### Linux User Management

- Each user in Linux has a unique identifier called UID (user ID).
- The root user always has a UID of 0, which gives them full administrative privileges.
- Every user is assigned a primary group (usually named after the user) and can also belong to secondary groups, such as designers or programmers.
- To create a new user and group named "ali," you can use the `useradd ali` command. This will create a private group for the user named "ali."

### File Permissions in Linux

- In Linux, file permissions determine which users can read, write, or execute a file.
- Permissions are divided into three categories: OWNER, GROUP, and OTHER.
- Each file has a 10-bit permission code, represented as `b rwx rwx rwx`.
  - The first bit represents the type of the file:
    - `d`: for directory
    - `-`: for regular files
    - `c`: for character files (data transferred char by char)
    - `b`: for block files (data transferred via blocks/chunks)
    - `l`: for link files (shortcuts)
    - `p`: for pipe files
- The next three bits represent the permissions of the OWNER.
- The following three bits represent the permissions of the GROUP.
- The final three bits represent the permissions of OTHER users.

## File Modes

- `r`:
  - For a file, this means the ability to read the contents of the file.
  - For a directory, this means the ability to list the files and directories within the directory using the `ls` command.
- `w`:
  - For a file, this means the ability to edit the contents of the file or delete it.
  - For a directory, this means the ability to add or remove files from the directory.
- `x`:
  - For a file, this means the ability to execute the file if it is an application or script.
  - For a directory, this means the ability to change to that directory using the `cd` command.

### Changing File Ownership and Group Membership

- The `chown` command is used to change the owner of a file or directory.
- Only the root user can change the ownership of files.
- To change the owner of the file `hamada` to user `ali` in the `/home/student/` directory, you can use the command `chown ali /home/student/hamada`.
- To change the group ownership of `hamada` to the `admins` group, you can use the command `chown :admins /home/student/hamada`.
- The `chgrp` command is used to change the group ownership of a file. For example, `chgrp admin hamada` changes the group ownership of `hamada` to the `admin` group.

### Changing File Permissions with chmod

- The `chmod` command is used to change the permissions of a file or directory.
- `g+w` adds write permission for the group, `o+r` adds read permission for others, and `u-w` removes write permission for the owner.
- You can change the permissions for multiple users by separating their permissions with a space, like `u+x g+x`.
- `u+rw` grants read and write permissions for the owner, while `ug-w` grants write permission for both the owner and group.
- `u=w` sets the permissions to only allow the owner to write, while `a+x` grants execute permission for all users.
- `chmod 644 hamada` means that `hamada` has permissions `wr- r-- r--`.
- `chmod =rwx hamada` changes the owner permissions to read, write, and execute.
- `chmod -R 644 hamada` changes the permissions for the directory `hamada` and all files and directories within it recursively.

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

## Process

- echo $$ : print the number of the processes of the shell (processId)
- ps : all the process that run on the shell right now
- ps aux : all the process that run on my terminal or on other terminal
  - a : for all process attached to a terminal
  - u : provides more columns
  - x : all other processes related to the systems
- ls /proc : give all the running processes each process is represents as directory named of the processId
- ps aux | grep vim : get the data of the process by name
- pidof vim : get the data of the process by name
- ps -ef : get all process include the parent process
- ps -l : the same as the above
- ps fex : get all process running in the shape of tree
- pstree : get all process running in the shape of tree
- top : get the process in a dynamically way
  - refresh every 3 seconds
  - give all information about the system
  - press 1 give you your consumption of each core you have
  - s : for change the default refresh time
  - h : for help
  - k : for kill process
  - r : for renice a process
  - M : to sort by amount of usage from memory
  - P : to sort by amount of usage from CPU utilization
  - n : change the number of processes shown
  - w : to save the current display configuration
  - q : for quit
- uptime : get the information about the consumption of the process of the cpu's

- the first process running on my system is `systemd` and have a processId of 1
- if `systemd` is killed the system shutdown
- if the `TTY = ?` is running in the background
- `STAT`
  - `S` : for sleep
  - `R` : for running
- `START` : this process started at
- `TIME` : take how much time to run
- `COMMAND` : what the in charge of this process

### Controlling Jobs

- `CTRL + c` : means kill the process
- `CTRL + z` : means suspend the current process
- fg %(processNumber) : get the suspended process into fore ground
- jobs : get all the suspended jobs you have right now
- sleep 20000& : run this commend in the back ground

### Kill process

- kill -l : list all type of signals that i have
- types of signals
  - 1 : SighUp : reread the configuration file
  - 9 : SigKill : Dirty Kill, kill without consequence
  - 15 : SIGTERM : gentle kill, kill your self please
- kill -15 9855 : kill the process 9855
- pkill vim : kill all vims that opened

### priority

- nice value represents the priority value of the process from -20 to 19
- nice sleep 200 : get nice value of 10
- nice -n 15 sleep 200 : nice value of 15
- renice -n 17 1021 : reset the nice value of the process 1021 to be 17

## Diamonds

- difference between systemd and init diamond
- systemctl : show all the diamods that running on my machine
- systemctl --type service : show the service only
- systemctl status sshd : show information abot the servie sshd
- systemctl status sshd -l : show the service with the log file
- systemctl stop sshd : stop the service sshd
- echo $? : print the status of the last commend run
- systemctl enable sshd : enable service of sshd to run on the startup of the machine
- systemctl disable sshd : disable the service from start up when the machine boot
- systemctl is-active sshd : ask if the sshd is active write now ?
- systemctl restart sshd : restart is stop the service and start it again
- systemctl reload sshd : reread the configuration file of the sshd
- systemctl list-dependencies sshd : list all the services that depends on the sshd
- systemctl list-dependencies sshd --reverse : list all the services that sshd depends on
-

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
