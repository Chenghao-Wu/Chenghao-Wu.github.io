---
title: "Linux Tutorial"
excerpt: ""
collection: blogs
---

# Unix Tutorial
## Introduction
* UNIX is an operating system which was first developed in the 1960s, and has been under constant development ever since. By operating system, we mean that the suite of programs which make the computer work. It is a stable, multi-user, multi-tasking system for servers, desktops and laptops.

### The UNIX operating system 
* It is made up of three parts: the kernel, the shell and the programs.

#### The Kernel:

The kernel of UNIX is the hub of the operating system: it allocates time and memory to programs and handles the filestore and communications in response to system calls. 

As an illustration of the way that the shell and the kernel work together, suppose a user types rm myfile (which has the effect of removing the file myfile). The shell searches the filestore for the file containing the program rm, and then requests the kernel, through system calls, to execute the program rm on myfile. When the process rm myfile has finished running, the shell then returns the UNIX prompt % to the user, indicating that it is waiting for further commands.

#### The Shell

The shell acts as an interface between the user and the kernel. When a user logs in, the login program checks the username and password, and then starts another program called the shell. The shell is a command line interpreter (CLI). It interprets the commands the user types in and arranges for them to be carried out. The commands are themselves programs: when they terminate, the shell gives the user another prompt (% on our systems).

Filename Completion - By typing part of the name of a command, filename or directory and pressing the [Tab] key, the bash shell will complete the rest of the name automatically. If the shell finds more than one name beginning with those letters you have typed, it will beep, prompting you to type a few more letters before pressing the tab key again.

History - The shell keeps a list of the commands you have typed in. If you need to repeat a command, use the cursor keys to scroll up and down the list or type history for a list of previous commands.

#### Programs/Commands

Built-in/ User defined actions

### Files and Processes

Everything in UNIX is either a file or a process.

A process is an executing program identified by a unique PID (process identifier).

A file is a collection of data. They are created by users using text editors, running compilers etc.

Examples of files:

* a document (report, essay etc.)
* the text of a program written in some high-level programming language
* instructions comprehensible directly to the machine and incomprehensible to a casual user, for example, a collection of binary digits (an executable or binary file);
* a directory, containing information about its contents, which may be a mixture of other directories (subdirectories) and ordinary files.

### The Directory Structure

All the files are grouped together in the directory structure. The file-system is arranged in a hierarchical structure, like an inverted tree. The top of the hierarchy is traditionally called *root* (written as a slash / )

![Directory Structure](/Users/bruce/Dropbox/research/TU-Darmstadt/teaching/Start_Simulation/image/Directory Structure.png)

In the diagram above, we see that the home directory of Mine "zwu" contains several sub-directories (Desktop and Documents ... ).

The full path to the folder Desktop(zwu) is "/home/zwu/Desktop"

### Operations in Unix

#### Listing files and directories
##### ls(list)
When you first login, your current working directory is your home directory. Your home directory has the same name as your user-name, for example, ee91ab, and it is where your personal files and subdirectories are saved.

To find out what is in your home directory, type

```bash
% ls
```

The ls command ( lowercase L and lowercase S ) lists the contents of your current working directory.
![](/Users/bruce/Dropbox/research/TU-Darmstadt/teaching/Start_Simulation/image/Unix Shell.png)
There may be no files visible in your home directory, in which case, the UNIX prompt will be returned. Alternatively, there may already be some files inserted by the System Administrator when your account was created.

ls does not, in fact, cause all the files in your home directory to be listed, but only those ones whose name does not begin with a dot (.) Files beginning with a dot (.) are known as hidden files and usually contain important program configuration information. They are hidden because you should not change them unless you are very familiar with UNIX!!!

To list all files in your home directory including those whose names begin with a dot, type

```bash
% ls -a
```
![](/Users/bruce/Dropbox/research/TU-Darmstadt/teaching/Start_Simulation/image/ls -a.png)

As you can see, ```ls -a``` lists files that are normally hidden.

```ls``` is an example of a command which can take options: ```-a``` is an example of an option. The options change the behaviour of the command. There are [online manual pages](https://files.fosswire.com/2007/08/fwunixref.pdf
) that tell you which options a particular command can take, and how each option modifies the behaviour of the command.

#### Making Directories
##### mkdir (make directory)

We will now make a subdirectory in your home directory to hold the files you will be creating and using in the course of this tutorial. To make a subdirectory called unixstuff in your current working directory type

```bash
% mkdir myfolder
```
To see the directory you have just created, type

```bash
% ls
```

#### Changing to a different directory 
##### cd (change directory)
The command cd directory means change the current working directory to 'directory'. The current working directory may be thought of as the directory you are in, i.e. your current position in the file-system tree.

To change to the directory you have just made, type

```bash
% cd myfolder
```

#### The directories . and ..
##### The current directory (.)
In UNIX, (.) means the current directory, so typing

```bash
% cd .
```

##### The parent directory (..)
(..) means the parent of the current directory, so typing

```bash
% cd ..
```

#### Pathnames
##### pwd (print working directory)
Pathnames enable you to work out where you are in relation to the whole file-system. For example, to find out the absolute pathname of your home-directory, type cd to get back to your home-directory and then type

```bash
% pwd
```
The full pathname will look something like this

```bash
% /home/zwu/Research/
```
which means that ```Research``` (your home directory) is in the sub-directory ```zwu``` (the group directory),which in turn is located in the its sub-directory, which is in the home sub-directory, which is in the top-level root directory called " / " .

#### ~ (your home directory)

Home directories can also be referred to by the tilde ```~``` character. It can be used to specify paths starting at your home directory. So typing

```bash
% ls ~/Research
```
It is the same as typing

```bash
% ls /home/zwu/Research
```

#### Copying Files
##### cp(copy)

```cp file1 file2``` is the command which makes a copy of file1 in the current working directory and calls it file2

#### Moving files
##### mv (move)

```mv file1 file2``` moves (or renames) file1 to file2

To move a file from one place to another, use the mv command. This has the effect of moving rather than copying the file, so you end up with only one file rather than two.

It can also be used to rename a file, by moving the file to the same directory, but giving it a different name.

#### Removing files and directories
##### rm (remove), rmdir (remove directory)

To delete (remove) a file, use the ```rm``` command. Or ```rmdir``` command to remove a directory (make sure it is empty first).

#### Displaying the contents of a file on the screen
##### clear (clear screen)
Before you start the next section, you may like to clear the terminal window of the previous commands so the output of the following commands can be clearly understood.

At the prompt, type

```bash
% clear
```

This will clear all text and leave you with the % prompt at the top of the window.

##### cat (concatenate)
The command cat can be used to display the contents of a file on the screen. Type:

```bash
% cat research.txt
```

##### less
The command less writes the contents of a file onto the screen a page at a time. Type

```bash
% less research.txt
```
Press the [space-bar] if you want to see another page, and type [q] if you want to quit reading. As you can see, less is used in preference to cat for long files.

##### head
The head command writes the first ten lines of a file to the screen.

Clear the screen then type
```bash
% head research.txt
```

##### tail
The tail command writes the last ten lines of a file to the screen.

Clear the screen and type
```bash
% tail research.txt
```

### Cheat Sheet for Linux
![](/Users/bruce/Dropbox/research/TU-Darmstadt/teaching/Start_Simulation/image/linuxcheatsheet.pdf)

### Other References
* http://www.ee.surrey.ac.uk/Teaching/Unix/index.html
