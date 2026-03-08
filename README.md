# 🐧 Linux Shell — Complete Course

> A structured English course covering the fundamentals of the Linux shell, with examples for every section and a hands-on final lab.

---

## 📚 Table of Contents

1. [History of Linux Shells](#1-history-of-linux-shells)
2. [Basic Shell Concepts](#2-basic-shell-concepts)
3. [Running Commands and Scripts](#3-running-commands-and-scripts)
4. [Redirections and Pipes](#4-redirections-and-pipes)
5. [Environment Variables](#5-environment-variables)
6. [Security and Best Practices](#6-security-and-best-practices)
7. [Profile Loading Mechanism](#7-profile-loading-mechanism)
8. [Final Lab — My First System Report Script](#8-final-lab--my-first-system-report-script)

---

## 1. History of Linux Shells

### Course

The Linux shell has its roots in Unix. Understanding this history helps explain why so many different shells exist today and why certain conventions are shared across all of them.

| Year | Shell | Creator | Key Feature |
|------|-------|---------|-------------|
| 1971 | Thompson Shell | Ken Thompson | First Unix shell, basic commands and redirection |
| 1979 | Bourne Shell (`sh`) | Stephen Bourne | Scripting, variables, control flow |
| 1989 | Bash (`bash`) | Brian Fox | GNU project, default on most Linux distros |
| 1990s | Zsh (`zsh`) | — | Advanced autocomplete, plugins, customization |
| 2005 | Fish (`fish`) | — | User-friendly, modern UX, auto-suggestions |

Bash remains the most widely used shell on Linux today. This course focuses on concepts that are common to all shells, giving you a solid foundation regardless of which shell you end up using.

### Examples

```bash
# Check which shell you are currently using
echo $SHELL
# Output example: /bin/bash

# List all shells available on your system
cat /etc/shells

# Check the version of Bash
bash --version
```

---

## 2. Basic Shell Concepts

### Course

Before writing scripts or running complex commands, you need to understand the core building blocks of any shell session.

#### Commands
A command is an instruction you give to the shell. It can be a built-in function, an external executable, or a script.

#### Aliases
Aliases are shortcuts for long or frequently used commands. They save time and reduce typing errors.

#### Auto-completion
Press `Tab` to auto-complete commands, file names, paths, and even command arguments. This is one of the most useful features for daily work.

#### Command History
The shell stores every command you run. You can navigate history with the arrow keys, or search through it interactively.

| Key | Action |
|-----|--------|
| `↑` / `↓` | Browse previous/next commands |
| `Ctrl+R` | Search history interactively |
| `history` | Display full history list |
| `!42` | Re-run command number 42 |

#### Glob Patterns
Special characters that match multiple files at once:

| Pattern | Matches |
|---------|---------|
| `*` | Any number of characters |
| `?` | Exactly one character |
| `{a,b,c}` | Any of the listed values |
| `[abc]` | Any single character from the set |

#### Job Control
Manage processes running in the foreground or background.

| Command | Action |
|---------|--------|
| `command &` | Run in background |
| `jobs` | List background jobs |
| `fg %1` | Bring job 1 to foreground |
| `bg %1` | Send job 1 to background |
| `kill %1` | Terminate job 1 |

### Examples

```bash
# --- Commands ---
ls              # List files in current directory
ls -l           # Detailed list with permissions and sizes
ls -la          # Include hidden files (starting with .)

# --- Aliases ---
alias ll="ls -la"          # Create a shortcut
alias gs="git status"      # Another example
ll                          # Now runs: ls -la

# To remove an alias:
unalias ll

# --- History ---
history                    # Show all past commands
history | tail -10         # Show only the last 10
!42                        # Re-run command number 42
# Press Ctrl+R then type to search interactively

# --- Glob Patterns ---
ls *.txt                              # All .txt files
ls file?.txt                          # file1.txt, file2.txt, etc.
ls report_202[34].txt                 # report_2023.txt or report_2024.txt
tar -cvf archive.tar file{1,2,3}.txt  # Brace expansion

# --- Job Control ---
sleep 60 &        # Start a long command in background
jobs              # See all background jobs
fg %1             # Bring job 1 back to foreground
bg %1             # Push it back to background
kill %1           # Kill job 1
```

---

## 3. Running Commands and Scripts

### Course

Everything in the shell starts with running a command. You can run a single command, chain several together, or package a series of commands into a reusable script file.

#### Chaining Operators

| Operator | Behavior |
|----------|----------|
| `;` | Run commands one after another, regardless of success or failure |
| `&&` | Run next command only if the previous one succeeded |
| `\|\|` | Run next command only if the previous one failed |

#### Shell Scripts
A shell script is a plain text file containing a series of commands. Key rules:

- The first line must be a **shebang**: `#!/bin/bash`
- The file must be made executable with `chmod +x`
- Run it with `./script.sh`

### Examples

```bash
# --- Single Commands ---
ls -l
cp source.txt /tmp/destination.txt
mkdir /tmp/testdir

# --- Chaining with ; ---
echo "Start" ; ls ; echo "Done"
# Runs all three regardless of outcome

# --- Chaining with && ---
mkdir /tmp/newdir && cd /tmp/newdir
# cd only runs if mkdir succeeded

# --- Chaining with || ---
cat missing_file.txt || echo "File not found, continuing..."
# The echo runs only if cat fails

# --- Creating a Script ---
cat > greet.sh << 'EOF'
#!/bin/bash
echo "Hello, $USER!"
echo "Today is: $(date)"
echo "You are in: $(pwd)"
EOF

chmod +x greet.sh
./greet.sh

# --- Script with arguments ---
cat > say_hello.sh << 'EOF'
#!/bin/bash
NAME="${1:-stranger}"     # Use first argument, or "stranger" if none given
echo "Hello, $NAME!"
EOF

chmod +x say_hello.sh
./say_hello.sh Alice
./say_hello.sh            # Uses default "stranger"
```

---

## 4. Redirections and Pipes

### Course

One of the most powerful features of the shell is the ability to control where data flows — into a command, out of a command, or from one command directly into another.

#### Redirection Symbols

| Symbol | Meaning |
|--------|---------|
| `>` | Redirect stdout to a file (overwrite) |
| `>>` | Redirect stdout to a file (append) |
| `2>` | Redirect stderr to a file |
| `2>&1` | Merge stderr into stdout |
| `&>` | Redirect both stdout and stderr to a file |
| `<` | Read stdin from a file |
| `\|` | Pipe: send stdout of one command to stdin of the next |
| `<< EOF` | Here Document: provide multi-line input inline |

#### /dev/null
A special file that silently discards anything written to it. Useful when you want to suppress output entirely.

### Examples

```bash
# --- Output Redirection ---
ls > filelist.txt               # Save output (overwrite)
ls >> filelist.txt              # Append to existing file
ls /nonexistent 2> errors.txt   # Capture error messages only
ls /nonexistent &> all.txt      # Capture everything (stdout + stderr)

# --- Input Redirection ---
sort < filelist.txt             # Sort lines from a file
wc -l < filelist.txt            # Count lines from a file

# --- Pipes ---
ls -l | grep ".txt"                    # Filter ls output
cat /etc/passwd | wc -l               # Count users
ps aux | grep bash | grep -v grep     # Find bash processes
du -sh ~/* | sort -rh | head -5       # Top 5 largest items in home

# --- Combining Redirections and Pipes ---
ls -la 2> err.txt | grep "^d" > dirs.txt
# Errors go to err.txt, only directories go to dirs.txt

# --- Here Document ---
cat << EOF > welcome.txt
Welcome to the Linux Shell Course.
This file was generated automatically.
Date: $(date)
User: $USER
EOF

cat welcome.txt

# --- /dev/null ---
apt-get update > /dev/null 2>&1    # Run silently, discard all output
some_command > /dev/null           # Discard stdout only
```

---

## 5. Environment Variables

### Course

Environment variables are key-value pairs stored in the shell session. They configure how the system and applications behave, and they are a primary way to pass information between commands and scripts.

#### Common System Variables

| Variable | Description |
|----------|-------------|
| `PATH` | Directories the shell searches for executables |
| `HOME` | Current user's home directory |
| `USER` | Current username |
| `SHELL` | Path to the current shell |
| `LANG` | System language and locale |

#### Key Rules

- Use `export VAR="value"` to create or modify a variable
- Use `$VAR` or `${VAR}` to read its value
- Variables without `export` are **local** (not passed to child processes)
- Use `unset VAR` to delete a variable
- Add to `~/.bashrc` to make a variable available in every new session

### Examples

```bash
# --- View variables ---
echo $PATH
echo $HOME
echo $USER
printenv                    # List all environment variables
env | grep LANG             # Filter by keyword

# --- Create and use a variable ---
export MY_NAME="Alice"
echo "Hello, $MY_NAME"
# Output: Hello, Alice

# --- Use in a path ---
export PROJECT_DIR="$HOME/projects"
mkdir -p $PROJECT_DIR
cd $PROJECT_DIR

# --- Local vs exported ---
LOCAL_VAR="only here"          # NOT exported
export GLOBAL_VAR="shared"     # Exported

bash -c 'echo $GLOBAL_VAR'     # Works: "shared"
bash -c 'echo $LOCAL_VAR'      # Empty: not available

# --- Modify PATH to add a custom tool directory ---
export PATH=$PATH:/opt/my-tools/bin

# --- Persist a variable across sessions ---
echo 'export MY_NAME="Alice"' >> ~/.bashrc
source ~/.bashrc               # Reload without restarting terminal

# --- Unset a variable ---
unset MY_NAME
echo $MY_NAME                  # Empty

# --- Use variables in a script ---
cat > show_env.sh << 'EOF'
#!/bin/bash
echo "User    : $USER"
echo "Home    : $HOME"
echo "Shell   : $SHELL"
echo "Working : $(pwd)"
EOF
chmod +x show_env.sh
./show_env.sh
```

---

## 6. Security and Best Practices

### Course

The shell is a powerful tool, and with power comes responsibility. Poor habits can lead to data loss, security vulnerabilities, or broken systems. This section covers the most important security practices.

#### File Permissions

Every file in Linux has three permission categories: **owner**, **group**, and **others**. Each category has three permission bits:

| Symbol | Meaning |
|--------|---------|
| `r` | Read |
| `w` | Write |
| `x` | Execute |

Numeric permission values:

| Number | Permission |
|--------|-----------|
| `7` | rwx (read + write + execute) |
| `6` | rw- (read + write) |
| `5` | r-x (read + execute) |
| `4` | r-- (read only) |

#### Dangerous Commands to Handle with Care

| Command | Risk |
|---------|------|
| `rm -rf /` | Deletes the entire filesystem |
| `dd if=/dev/zero of=/dev/sda` | Wipes a disk completely |
| `mkfs /dev/sda` | Formats a partition, erasing all data |
| `chmod 777 /` | Removes all access restrictions on root |

#### Script Hardening with `set`

Adding `set -euo pipefail` at the top of every script is a best practice:

| Option | Effect |
|--------|--------|
| `-e` | Exit immediately if any command fails |
| `-u` | Treat unset variables as errors |
| `-o pipefail` | Catch failures inside pipes |

### Examples

```bash
# --- View permissions ---
ls -l script.sh
# -rwxr-xr-x  1 alice staff  512 Jan 1 script.sh

# --- Change permissions ---
chmod +x script.sh          # Add execute for owner
chmod 755 script.sh         # rwxr-xr-x (owner all, others read+execute)
chmod 600 secret.txt        # rw------- (owner only, no one else)
chmod 644 readme.txt        # rw-r--r-- (owner writes, others read)

# --- Change ownership ---
chown alice script.sh
chown alice:developers script.sh   # owner and group at once

# --- Inspect a downloaded script before running it ---
cat downloaded.sh
less downloaded.sh          # Read it safely before executing

# --- Safe variable usage ---
TARGET="${1:-}"

# Unsafe — if TARGET is empty, this deletes root's content:
# rm -rf $TARGET

# Safe — check first:
if [[ -n "$TARGET" && "$TARGET" != "/" ]]; then
  rm -rf "$TARGET"
else
  echo "Error: invalid or empty target." >&2
  exit 1
fi

# --- Hardened script template ---
cat > safe_script.sh << 'EOF'
#!/bin/bash
set -euo pipefail

INPUT="${1:-}"

if [[ -z "$INPUT" ]]; then
  echo "Usage: $0 <argument>" >&2
  exit 1
fi

echo "Processing: $INPUT"
EOF
chmod +x safe_script.sh
```

---

## 7. Profile Loading Mechanism

### Course

When you open a terminal, the shell automatically reads and executes one or more configuration files to set up your environment. Understanding which file is loaded when helps you put your customizations in the right place.

#### Session Types

| Type | Description | Example |
|------|-------------|---------|
| **Login + Interactive** | User authenticates to start the session | SSH login, TTY terminal |
| **Non-login + Interactive** | Shell opened inside an existing session | New terminal tab |
| **Non-interactive** | Shell runs commands without user input | Running a script |

#### Configuration Files (Bash)

| File | Loaded When |
|------|-------------|
| `/etc/profile` | Every login session, for all users |
| `~/.bash_profile` | Login session, current user (takes priority) |
| `~/.bash_login` | Login session, if `~/.bash_profile` does not exist |
| `~/.profile` | Login session, if neither of the above exist |
| `~/.bashrc` | Every interactive non-login session |
| `~/.bash_logout` | When a login session ends |

#### Load Order Summary

```
Login session:
  /etc/profile
  └─► ~/.bash_profile  (or ~/.bash_login, or ~/.profile)

Non-login interactive session:
  ~/.bashrc

Script (non-interactive):
  Nothing loaded automatically
```

> 💡 Best practice: In `~/.bash_profile`, call `~/.bashrc` explicitly so your aliases and functions work in both login and non-login sessions:
> ```bash
> [[ -f ~/.bashrc ]] && source ~/.bashrc
> ```

### Examples

```bash
# --- Check if you are in a login shell ---
[[ -o login ]] && echo "Login shell" || echo "Non-login shell"

# --- Add a persistent alias (non-login sessions) ---
echo 'alias ll="ls -la"' >> ~/.bashrc
source ~/.bashrc            # Apply immediately without restarting

# --- Add a PATH entry for login sessions ---
echo 'export PATH=$PATH:/opt/my-tools/bin' >> ~/.bash_profile

# --- Reload config after editing ---
source ~/.bashrc
. ~/.bashrc                 # Equivalent shorthand

# --- Add a welcome message on login ---
echo 'echo "Welcome back, $USER! $(date)"' >> ~/.bash_profile

# --- View your current config files ---
cat ~/.bashrc
cat ~/.bash_profile

# --- Check which files exist ---
for f in ~/.bashrc ~/.bash_profile ~/.bash_login ~/.profile; do
  [[ -f "$f" ]] && echo "EXISTS: $f" || echo "missing: $f"
done
```

---

## 8. Final Lab — My First System Report Script

> **Goal:** Write a shell script step by step that generates a simple report about your Linux system.
> You will practice every concept from the course in one real exercise.

---

### What You Will Build

By the end of this lab you will have a script called `my_report.sh` that:
- Displays basic system information
- Lists the biggest files in your home directory
- Saves everything into a report file
- Shows a friendly summary at the end

---

### Before You Start

Open a terminal and create a working folder:

```bash
mkdir ~/lab
cd ~/lab
```

---

### Step 1 — Create the Script File

```bash
touch my_report.sh
chmod +x my_report.sh
nano my_report.sh
```

Add these two lines at the very top:

```bash
#!/bin/bash
set -euo pipefail
```

> 💡 Shebang + script hardening — Section 3 & 6

---

### Step 2 — Define Your Variables

Add these lines after `set -euo pipefail`:

```bash
# --- Configuration ---
REPORT_FILE=~/lab/report.txt
STUDENT_NAME="your_name_here"    # ← Change this to your name
```

Add a first echo to verify it works:

```bash
echo "Hello $STUDENT_NAME, starting the report..."
```

Run and check:

```bash
./my_report.sh
# Expected: Hello Alice, starting the report...
```

> 💡 Variables — Section 5

---

### Step 3 — Write the Report Header

Replace the echo line with:

```bash
echo "==============================" > $REPORT_FILE
echo " SYSTEM REPORT"               >> $REPORT_FILE
echo " Student : $STUDENT_NAME"     >> $REPORT_FILE
echo " Date    : $(date)"           >> $REPORT_FILE
echo "==============================">> $REPORT_FILE
```

Run and check:

```bash
./my_report.sh
cat ~/lab/report.txt
```

> 💡 Output redirection `>` and `>>` — Section 4

---

### Step 4 — Add System Information

```bash
echo ""                  >> $REPORT_FILE
echo "--- WHO AM I ---"  >> $REPORT_FILE
echo "User  : $USER"     >> $REPORT_FILE
echo "Home  : $HOME"     >> $REPORT_FILE
echo "Shell : $SHELL"    >> $REPORT_FILE

echo ""                  >> $REPORT_FILE
echo "--- SYSTEM ---"    >> $REPORT_FILE
uname -a                 >> $REPORT_FILE
uptime                   >> $REPORT_FILE
```

> 💡 Environment variables + running commands — Sections 3 & 5

---

### Step 5 — Use a Pipe to Filter Processes

```bash
echo ""                                    >> $REPORT_FILE
echo "--- TOP 5 PROCESSES (by memory) ---" >> $REPORT_FILE
ps aux --sort=-%mem | head -6              >> $REPORT_FILE
```

> 💡 Pipes `|` — Section 4

---

### Step 6 — List Files With a Glob Pattern

Create a few test files first:

```bash
touch ~/lab/notes.txt ~/lab/todo.txt
```

Then add to your script:

```bash
echo ""                            >> $REPORT_FILE
echo "--- TXT FILES IN ~/lab ---"  >> $REPORT_FILE

for FILE in ~/lab/*.txt; do
  echo "  Found: $FILE"            >> $REPORT_FILE
done
```

> 💡 Glob patterns `*.txt` — Section 2

---

### Step 7 — Check a Permission

```bash
echo ""                          >> $REPORT_FILE
echo "--- PERMISSION CHECK ---"  >> $REPORT_FILE

if [[ -x ~/lab/my_report.sh ]]; then
  echo "  my_report.sh is executable ✓" >> $REPORT_FILE
else
  echo "  WARNING: my_report.sh is NOT executable" >> $REPORT_FILE
fi
```

> 💡 Permissions and safe checks — Section 6

---

### Step 8 — Add a Final Summary With a Here Document

```bash
cat << EOF >> $REPORT_FILE

==============================
 REPORT COMPLETE
 Generated by : $STUDENT_NAME
 Saved to     : $REPORT_FILE
==============================
EOF

echo ""
echo "✅ Done! Your report is saved at: $REPORT_FILE"
```

> 💡 Here Document `<< EOF` — Section 4

---

### Your Complete Script

```bash
#!/bin/bash
set -euo pipefail

# --- Configuration ---
REPORT_FILE=~/lab/report.txt
STUDENT_NAME="your_name_here"

# Step 3 — Header
echo "==============================" > $REPORT_FILE
echo " SYSTEM REPORT"               >> $REPORT_FILE
echo " Student : $STUDENT_NAME"     >> $REPORT_FILE
echo " Date    : $(date)"           >> $REPORT_FILE
echo "==============================">> $REPORT_FILE

# Step 4 — System info
echo ""                  >> $REPORT_FILE
echo "--- WHO AM I ---"  >> $REPORT_FILE
echo "User  : $USER"     >> $REPORT_FILE
echo "Home  : $HOME"     >> $REPORT_FILE
echo "Shell : $SHELL"    >> $REPORT_FILE

echo ""               >> $REPORT_FILE
echo "--- SYSTEM ---"  >> $REPORT_FILE
uname -a              >> $REPORT_FILE
uptime                >> $REPORT_FILE

# Step 5 — Processes
echo ""                                    >> $REPORT_FILE
echo "--- TOP 5 PROCESSES (by memory) ---" >> $REPORT_FILE
ps aux --sort=-%mem | head -6              >> $REPORT_FILE

# Step 6 — Glob pattern
echo ""                            >> $REPORT_FILE
echo "--- TXT FILES IN ~/lab ---"  >> $REPORT_FILE
for FILE in ~/lab/*.txt; do
  echo "  Found: $FILE"            >> $REPORT_FILE
done

# Step 7 — Permission check
echo ""                          >> $REPORT_FILE
echo "--- PERMISSION CHECK ---"  >> $REPORT_FILE
if [[ -x ~/lab/my_report.sh ]]; then
  echo "  my_report.sh is executable ✓" >> $REPORT_FILE
else
  echo "  WARNING: my_report.sh is NOT executable" >> $REPORT_FILE
fi

# Step 8 — Summary
cat << EOF >> $REPORT_FILE

==============================
 REPORT COMPLETE
 Generated by : $STUDENT_NAME
 Saved to     : $REPORT_FILE
==============================
EOF

echo ""
echo "✅ Done! Your report is saved at: $REPORT_FILE"
```

---

### Run the Final Script

```bash
cd ~/lab
./my_report.sh
cat report.txt
```

---

### Checklist — What Did You Practice?

- [ ] Created a script with a shebang and `set -euo pipefail`
- [ ] Defined and used custom variables (`REPORT_FILE`, `STUDENT_NAME`)
- [ ] Used system environment variables (`$USER`, `$HOME`, `$SHELL`)
- [ ] Redirected output to a file with `>` and `>>`
- [ ] Used a pipe `|` to filter command output
- [ ] Used a glob pattern `*.txt` to loop over files
- [ ] Added a permission check with `if`
- [ ] Used a Here Document `<< EOF`

---

### Bonus Challenges

**🟡 Easy** — Add a line that counts how many `.txt` files were found and writes the count to the report.

**🟠 Medium** — Add a section listing the 5 largest files in `$HOME` using `du` and `sort`.

**🔴 Hard** — Write a second script `cleanup.sh` that deletes `report.txt` and all `.txt` files in `~/lab`, but only after asking the user for confirmation using `read -p`.

---

*End of Linux Shell Course — Happy scripting!* 🐧
