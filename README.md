# Linux Shell Course

**Course:** Introduction to Linux Shells  
**Level:** Beginner / Intermediate  
**Audience:** Computer Science / DevOps / System Administration Students  

## 1. Introduction

The Linux shell is a program that allows you to interact with the operating system via a Command Line Interface (CLI). It acts as an intermediary between the user and the Linux kernel, translating your commands into system actions. With the shell, you can execute commands, manage files and directories, monitor and control processes, automate repetitive tasks, and write scripts for advanced operations. Unlike a graphical user interface (GUI), the shell provides fast, precise, and automatable control over the system. It is an essential tool for system administrators, DevOps engineers, developers, and cybersecurity engineers.

## 2. History of Shells

Thompson Shell (1971) – Developed by Ken Thompson for Unix. Basic commands and simple redirections. Limited scripting capabilities. Bourne Shell (sh, 1979) – Developed by Stephen Bourne. Introduced scripting, variables, loops, conditional statements. Became the standard for Unix scripts. Bash (1989) – Developed by Brian Fox for the GNU project. Compatible with Bourne Shell, richer in features, became the default shell on most Linux distributions. Zsh (1990s) – Advanced shell with smart autocompletion, automatic corrections, plugin support, and high customization. Fish (2005) – Friendly Interactive Shell. Modern interface, intelligent suggestions, simple configuration. Understanding the history helps you choose the right shell for your needs and understand the evolution of features over time.

## 3. Basic Linux Commands

pwd – Show current directory  
ls – List files and directories  
ls -l – List with details (permissions, owner, size)  
mkdir project – Create a directory named project  
touch file.txt – Create an empty file  
rm file.txt – Remove a file  
cp file.txt backup.txt – Copy a file  
mv file.txt folder/ – Move a file  
cat file.txt – Display file content  
less file.txt – View file content page by page  
head -n 10 file.txt – Display first 10 lines  
tail -n 10 file.txt – Display last 10 lines  
grep "error" file.log – Search for "error" in a file  
grep -i "error" file.log – Case-insensitive search  

These commands form the foundation of shell usage. Learning them allows you to manage files and explore directories efficiently.

## 4. Aliases

Aliases are shortcuts for commands, making frequently used commands quicker to type. For example, alias ll="ls -la" allows you to type ll instead of ls -la. alias rm="rm -i" ensures the shell asks for confirmation before deleting files. Using aliases can prevent mistakes and save time.

## 5. Command History

Linux keeps a history of commands you have executed. This allows you to reuse or search for previous commands. Use history to see all past commands, !45 to execute command number 45 from history, or Ctrl + R for an interactive search. Command history is essential for productivity and error recovery.

## 6. Auto-Completion

Pressing Tab can autocomplete file names, directories, or commands. For example, typing cd /etc/ap and pressing Tab might autocomplete to cd /etc/apache2/. Auto-completion saves typing time and reduces errors.

## 7. Command Chaining

You can combine commands to execute them in different ways: command1 ; command2 executes sequentially regardless of success, command1 && command2 executes the second only if the first succeeds, and command1 || command2 executes the second only if the first fails. Examples: date ; pwd ; ls, mkdir project && cd project, cd test || echo "Folder does not exist". Chaining is useful for automation and conditional operations.

## 8. Redirections

Redirections control input and output streams. ls > files.txt saves output to a file (overwrite), ls >> files.txt appends output, command 2> errors.txt redirects errors, command &> all.txt redirects both stdout and stderr, sort < file.txt uses a file as input. Here Document allows writing multiple lines to a file directly: cat <<EOL > newfile.txt line1 line2 line3 EOL. Redirections are key for logging, error handling, and automation.

## 9. Pipes

Pipes allow you to connect commands so the output of one becomes the input of another. For example: ls -l | grep log, grep "error" file.log | wc -l, or command1 2> errors.txt | command2 > output.txt. Pipes enable powerful data processing directly from the shell.

## 10. Shell Scripts

A shell script is a file containing multiple commands executed sequentially. Example: #!/bin/bash echo "Hello World" date. Make it executable: chmod +x script.sh and run: ./script.sh. Example loop in script: #!/bin/bash for i in {1..5}; do echo "Line $i"; done. Scripts automate repetitive tasks and improve efficiency.

## 11. Variables

Variables store data that can be reused. Example: NAME="Yassine"; echo $NAME; DIR="/tmp/test"; mkdir $DIR; cd $DIR. Variables are essential for dynamic scripts and automation.

## 12. Environment Variables

Environment variables affect system and shell behavior. Example: echo $PATH; export MESSAGE="Hello"; echo $MESSAGE; unset MESSAGE; export PATH=$PATH:/usr/local/bin. Use environment variables to configure paths, messages, and system behavior.

## 13. Linux Permissions

Linux uses permissions to control access. Example: ls -l shows permissions, chmod 755 script.sh sets permissions, chown user file.txt changes owner, chgrp group file.txt changes group. Permissions ensure security and proper access control.

## 14. Job Control

Control background and foreground tasks. Example: command & runs in background, jobs lists jobs, fg %1 brings job to foreground, bg %1 resumes in background, kill %1 terminates a job. Example: sleep 60 & jobs fg %1. Job control is critical for long-running tasks and multitasking.

## 15. Security Best Practices

Always check scripts before execution: cat script.sh. Be cautious with dangerous commands: rm -rf /, dd if=/dev/zero of=/dev/sda. Make scripts safe: set -euo pipefail. Validate variables: if [[ -n "$DIR" ]]; then rm -rf "$DIR"; fi. Good practices prevent accidental data loss and system damage.

## 16. Labs & Exercises

Lab 1: Files and Directories – mkdir Lab1; cd Lab1; touch file1.txt file2.txt file3.txt; ls -l; echo "Hello" > file1.txt; cat file1.txt; cp file1.txt backup_file1.txt; mv file2.txt test_folder/.  

Lab 2: Variables and Scripts – #!/bin/bash; export NAME="LabUser"; echo "Welcome $NAME"; mkdir /tmp/LabFolder.  

Lab 3: Redirections and Pipes – echo -e "line1\nline2\nline3" > file.txt; cat file.txt | grep line2 > result.txt.  

Lab 4: Jobs – sleep 30 &; jobs; fg %1.  

Lab 5: Loops and Conditions – #!/bin/bash; for i in {1..5}; do if (( i % 2 == 0 )); then echo "$i is even"; else echo "$i is odd"; fi; done.

## Conclusion

The Linux shell is a powerful tool for system administration, automation, server management, and scripting. Mastering the shell is essential for any system administrator or DevOps engineer. To improve: learn Bash scripting, explore Zsh, create automation scripts, and practice Linux commands regularly.
