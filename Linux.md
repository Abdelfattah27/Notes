# Lec Two

## Users:

    - Root: administrator: super user <> have all permissions
      - Have user Id of ZERO (0)
    - Normal Users <> any user : have no permission except the rules the root have assigned to him
      - UserID >> 500
    - Service Users <>  "have permission about the server"
      - UserID : > 0 && < 500 (1-499)
      - Not have permission to login on the machine

## Modes

To switch modes n vmWare Use ALT + CTRL + F1
but at first edit the hot key to by alt, ctrl, shift from preferences from edit

    - Comment Line Mode : just commend line : TTY - teletype terminal
      - this is the default mode
      - for TTY2 : ctrl+alt+f2
      - for TTY3 : ctrl+alt+f3
    - CLI with GUI
      - Not the default
      - to switch to gui : ctrl+alt+f1

## linux file system

- / : root file system
  - boot : all boot files including the kernel
  - usr : user shared files like font files
  - mnt : to make mount
  - media : also to make mount
  - srv : just optional folder u can use
  - opt : optional package
  - lib : files that permit to call them selves while running
  - etc : configuration files
  - home : store the home directories for each user
  - dev : file represent the device just like (disk, and flash memories and any other files)
  - proc : Processes
  - root : home directory fot the home user
  - bin : binary files that all users can access
  - sbin : Super user bin <> super user binary (executable files)
  - var : variable files : files that changed continuously like (mails, log files)
  - tmp : temporary files

## Commends

- hostname : print the name of machine
- df -h :print all the partitions that i have in a human readable form
- which passwd : give the location of the commend "passwd" on the machine
- pwd : print working directory
- cd : change directory
  - ./Desktop ~~ /home/abdelfattah/Desktop
  - "cd -" change to previous working directory
  - "cd | cd ~" go to your home directory
- su "username": switch user : to switch to a new user
  - "su -" : switch to root user
- useradd "username"
- passwd : to change the password of the user
- "w" : get all signed in users on the machine
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
  -

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

## Permissions

### User Id

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

### change permissions

**_chmod_**

- chmod g+w hamada : add write permission for the group
- chmod o+r hamada : add read permission for the others
- chmod u-w hamada : delete write permission for the owner
- chmod u+x hamada g+x : change mode for more than one user
- chmod u+rw hamada: read write for owner
- chmod ug-w hamada: write for owner and group
- chmod u+w hamada , u-rx **====** chmod u=w
- chmod a+x hamada **====** chmod ugo+x **====** chmod +x
- chmod 644 hamada : mae hamada **wr- r-- r--**
- chmod -R 644 hamada : change mode for the directory hamada and the files which it contains

### modes

- r :
  - for file mean read the content of the file
  - for folder list the files of the dir using ls
- w :
  - for file means edit the content of the file or delete it
  - for folder add files to folder or remove it
- x :
  - for file means execute the file if it was app or script
  - for directory means you can make **cd** to the directory
    **_chown_** : to change thw owner
- the root must who change the owner of the files
- ls /home/ : to list all the users
- chown ali /home/student/hamada :change the owner of hamada to ali
- chown :admins /home/student/hamada : change the group
