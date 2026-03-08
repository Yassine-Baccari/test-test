# Linux Shell Course

**Course:** Introduction to Linux Shells  
**Level:** Beginner / Intermediate  
**Audience:** Computer Science / DevOps / System Administration Students  

Le Linux shell est un programme qui permet d’interagir avec le système d’exploitation via une interface en ligne de commande (CLI). Il agit comme un intermédiaire entre l’utilisateur et le noyau Linux. Avec le shell, vous pouvez exécuter des commandes, manipuler des fichiers, gérer des processus, automatiser des tâches, et écrire des scripts. Contrairement aux interfaces graphiques (GUI), le shell permet une gestion rapide, précise et automatisable du système. Il est essentiel pour les administrateurs système, ingénieurs DevOps, développeurs et ingénieurs cybersécurité.

## History of Shells

**Thompson Shell (1971):** Développé par Ken Thompson pour Unix. Exécution de commandes et redirections simples. Limité et pas de scripting avancé.  

**Bourne Shell (1979):** Développé par Stephen Bourne (sh). Introduit les scripts, variables, structures de contrôle, boucles, conditions. Devient le standard pour les scripts Unix.  

**Bash (1989):** Développé par Brian Fox (Bourne Again Shell) dans le projet GNU. Compatible Bourne Shell, fonctionnalités étendues, shell par défaut sur Linux.  

**Zsh:** Apparait dans les années 1990. Autocomplétion avancée, correction automatique, plugins, personnalisation.  

**Fish:** Apparait en 2005. Interface moderne, suggestions automatiques, configuration simple.

## Basic Commands

ls               # list files  
ls -l            # detailed list  
pwd              # current directory  
mkdir folder     # create folder  
touch file.txt   # create file  
rm file.txt      # remove file  
cp file.txt backup.txt # copy file  
mv file.txt folder/    # move file  
cat file.txt           # show file content  

## Alias

alias ll="ls -la"  
ll  # runs ls -la  

## Command History

history       # show history  
Ctrl + R      # search history  
!45           # run command 45  

## Auto-Completion

Press Tab to autocomplete commands, files, directories. Example:  
cd /etc/ap + TAB → cd /etc/apache2/  

## Command Chaining

Sequential:  
command1 ; command2  
Example: date ; pwd ; ls  

If success:  
command1 && command2  
Example: mkdir project && cd project  

If fail:  
command1 || command2  
Example: cd test || echo "Folder does not exist"  

## Redirections

Output:  
ls > files.txt   # overwrite  
ls >> files.txt  # append  

Error:  
command 2> errors.txt  

## Pipes

ls -l | grep log  
grep "error" file.log | wc -l  

## Shell Scripts

#!/bin/bash  
echo "Hello World"  
chmod +x script.sh  
./script.sh  

## Variables

NAME="Yassine"  
echo $NAME  

## Environment Variables

echo $PATH  
export MESSAGE="Hello"  
echo $MESSAGE  
unset MESSAGE  

## Linux Permissions

r – read, w – write, x – execute  

ls -l  
chmod 755 script.sh  
chown user file.txt  

## Job Control

command &    # background  
jobs         # list jobs  
fg %1        # foreground  
kill %1      # terminate  

## Security Best Practices

Always check scripts:  
cat script.sh  

Dangerous commands:  
rm -rf /  
dd if=/dev/zero of=/dev/sda  

## Profiles & Configuration Files

/etc/profile – system login shell  
~/.bash_profile – user login shell  
~/.bashrc – user interactive shell  
~/.bash_logout – logout  

Reload:  
source ~/.bashrc  

## Conclusion

The Linux shell is a powerful tool for system administration, automation, server management, and scripting. Mastering the shell is essential for system administrators and DevOps engineers. Next steps: Learn Bash scripting, explore Zsh, create automation scripts, practice commands regularly.
