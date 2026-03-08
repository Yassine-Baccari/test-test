# Linux Shell — Complete Course

> A structured English course based on the Linux Shell guide, with examples for each section and a final lab encompassing all concepts.

---

## Table of Contents

1. [History of Linux Shells](#1-history-of-linux-shells)
2. [Basic Shell Concepts](#2-basic-shell-concepts)
3. [Running Commands and Scripts](#3-running-commands-and-scripts)
4. [Redirections and Pipes](#4-redirections-and-pipes)
5. [Environment Variables](#5-environment-variables)
6. [Security and Best Practices](#6-security-and-best-practices)
7. [Profile Loading Mechanism](#7-profile-loading-mechanism)
8. [Final Lab — System Audit Script](#final-lab--system-audit-script)

---

## 1. History of Linux Shells

### Course

The Linux shell has its roots in Unix. Here is a brief timeline of major shells:

| Year | Shell | Creator | Key Feature |
|------|-------|---------|-------------|
| 1971 | Thompson Shell | Ken Thompson | First Unix shell, basic commands |
| 1979 | Bourne Shell (`sh`) | Stephen Bourne | Scripting, variables, control flow |
| 1989 | Bash (`bash`) | Brian Fox | GNU project, default on most Linux distros |
| 1990s | Zsh (`zsh`) | — | Advanced autocomplete, plugins |
| 2005 | Fish (`fish`) | — | User-friendly, modern UX |

Bash remains the most widely used shell on Linux today due to its compatibility and rich feature set.

### Example

Check which shell you are currently using:

```bash
echo $SHELL
# Output example: /bin/bash

# List all available shells on the system
cat /etc/shells
```

---

## 2. Basic Shell Concepts

### Course

#### Commands
A command is an instruction given to the shell. It can be a built-in function, an executable, or a script.

#### Aliases
Aliases are shortcuts for long or frequently used commands.

#### Auto-completion
Press `Tab` to auto-complete commands, file names, and paths.

#### Command History
The shell stores your command history. Use arrow keys or `Ctrl+R` to search.

#### Glob Patterns
Special characters to match multiple files:
- `*` — matches any number of characters
- `?` — matches exactly one character
- `{a,b,c}` — matches any of the listed values

#### Job Control
Manage foreground and background processes.

### Examples

```bash
# --- Commands ---
ls              # List files
ls -l           # Detailed list
ls -la          # Include hidden files

# --- Aliases ---
alias ll="ls -la"
alias cls="clear"
ll              # Now runs: ls -la

# --- History ---
history         # Show all past commands
history | tail -10   # Show last 10 commands
!42             # Re-run command number 42
# Press Ctrl+R then type to search history

# --- Glob Patterns ---
ls *.txt                        # All .txt files
ls file?.txt                    # file1.txt, file2.txt, etc.
tar -cvf archive.tar file{1,2,3}.txt  # Brace expansion

# --- Job Control ---
sleep 60 &      # Run in background
jobs            # List background jobs
fg %1           # Bring job 1 to foreground
bg %1           # Send job 1 to background
kill %1         # Terminate job 1
```

---

## 3. Running Commands and Scripts

### Course

You can run a single command, chain multiple commands, or package commands into a reusable **shell script**.

**Chaining operators:**
- `;` — run commands sequentially regardless of success/failure
- `&&` — run next command only if previous succeeded
- `||` — run next command only if previous failed

**Shell scripts** start with a **shebang** (`#!/bin/bash`) and must be made executable with `chmod +x`.

### Examples

```bash
# --- Single commands ---
ls -l
cp source.txt /tmp/destination.txt

# --- Chaining ---
echo "Start" ; ls ; echo "Done"          # Always runs all
mkdir /tmp/newdir && cd /tmp/newdir      # cd only if mkdir succeeds
cat missing_file.txt || echo "File not found"  # fallback on error

# --- Creating a script ---
cat > greet.sh << 'EOF'
#!/bin/bash
echo "Hello, $USER!"
echo "Today is: $(date)"
echo "You are in: $(pwd)"
EOF

chmod +x greet.sh
./greet.sh
```

---

## 4. Redirections and Pipes

### Course

The shell lets you control **where data flows** using redirections and pipes.

| Symbol | Meaning |
|--------|---------|
| `>` | Redirect stdout to a file (overwrite) |
| `>>` | Redirect stdout to a file (append) |
| `2>` | Redirect stderr to a file |
| `&>` | Redirect both stdout and stderr |
| `<` | Read stdin from a file |
| `\|` | Pipe stdout of one command to stdin of the next |
| `<<EOF` | Here Document — inline multi-line input |
| `/dev/null` | Discard output entirely |

### Examples

```bash
# --- Output Redirection ---
ls > filelist.txt           # Save output to file
ls >> filelist.txt          # Append to file
ls /nonexistent 2> errors.txt         # Capture errors
ls /nonexistent &> all_output.txt     # Capture everything

# --- Input Redirection ---
sort < filelist.txt         # Sort lines from file

# --- Pipes ---
ls -l | grep ".txt"         # Filter ls output
cat /etc/passwd | wc -l    # Count lines
ps aux | grep bash | grep -v grep  # Find bash processes

# --- Combined ---
ls -la 2> err.txt | grep "^d" > dirs.txt  # dirs only, errors separately

# --- Here Document ---
cat << EOF > welcome.txt
Welcome to the Linux Shell Course.
This file was created automatically.
Today: $(date)
EOF

# --- Discard output ---
apt-get update > /dev/null 2>&1    # Silent update
```

---

## 5. Environment Variables

### Course

Environment variables are **key-value pairs** that configure the shell and applications.

**Common system variables:**

| Variable | Description |
|----------|-------------|
| `PATH` | Directories searched for executable commands |
| `HOME` | Current user's home directory |
| `USER` | Current username |
| `SHELL` | Path to current shell |
| `LANG` | System language/locale |

- `export VAR="value"` — create/modify a global variable
- `unset VAR` — delete a variable
- Variables without `export` are **local** to the current shell
- Add to `~/.bashrc` to persist across sessions

### Examples

```bash
# --- View variables ---
echo $PATH
echo $HOME
echo $USER
printenv              # Show all environment variables
env | grep LANG       # Filter by keyword

# --- Create and use ---
export MY_NAME="Alice"
echo "Hello, $MY_NAME"

export PROJECT_DIR="/home/$USER/projects"
cd $PROJECT_DIR

# --- Local vs Global ---
LOCAL_VAR="only here"   # NOT exported
export GLOBAL_VAR="visible to child processes"

bash -c 'echo $GLOBAL_VAR'   # Works
bash -c 'echo $LOCAL_VAR'    # Empty

# --- Modify PATH ---
export PATH=$PATH:/opt/my-tools/bin
which my-tool         # Now findable

# --- Persist in config ---
echo 'export MY_NAME="Alice"' >> ~/.bashrc
source ~/.bashrc      # Reload config

# --- Unset ---
unset MY_NAME
echo $MY_NAME         # Empty now

# --- Use in a script ---
cat > show_env.sh << 'EOF'
#!/bin/bash
echo "User: $USER"
echo "Home: $HOME"
echo "Shell: $SHELL"
echo "Working dir: $(pwd)"
EOF
chmod +x show_env.sh
./show_env.sh
```

---

## 6. Security and Best Practices

### Course

Security in the shell comes down to three pillars:
1. **Permissions** — control who can read, write, or execute files
2. **Careful scripting** — validate input, avoid dangerous commands
3. **Script hardening** — use `set` options to catch errors early

**Permission notation:** `rwxrwxrwx` = owner | group | others

**`chmod` numeric mode:**
- `7` = rwx, `6` = rw-, `5` = r-x, `4` = r--

**Dangerous commands to handle with care:**
- `rm -rf /` — deletes everything
- `dd if=/dev/zero of=/dev/sda` — wipes a disk
- `mkfs` — formats a partition

**Harden scripts with:**
```bash
set -euo pipefail
```
- `-e` — exit on error
- `-u` — error on undefined variable
- `-o pipefail` — catch pipe failures

### Examples

```bash
# --- View permissions ---
ls -l script.sh
# Example: -rwxr-xr-x  1 alice staff  512 Jan 1 10:00 script.sh

# --- Change permissions ---
chmod +x script.sh          # Add execute for owner
chmod 755 script.sh         # rwxr-xr-x
chmod 600 secret.txt        # rw------- (owner only)

# --- Change ownership ---
chown alice script.sh
chown alice:developers script.sh

# --- Inspect a script before running it ---
cat downloaded_script.sh
less downloaded_script.sh

# --- Safe variable usage ---
USER_INPUT="/tmp/mydir"

# Unsafe:
# rm -rf $USER_INPUT   ← dangerous if variable is empty or manipulated

# Safe:
if [[ -n "$USER_INPUT" && "$USER_INPUT" != "/" ]]; then
  rm -rf "$USER_INPUT"
fi

# --- Hardened script template ---
cat > safe_script.sh << 'EOF'
#!/bin/bash
set -euo pipefail

TARGET_DIR="${1:-}"   # First argument or empty

if [[ -z "$TARGET_DIR" ]]; then
  echo "Error: No directory provided." >&2
  exit 1
fi

echo "Processing: $TARGET_DIR"
ls -la "$TARGET_DIR"
EOF
chmod +x safe_script.sh
```

---

## 7. Profile Loading Mechanism

### Course

When you open a shell session, configuration files are loaded automatically to set up your environment.

**Session types:**

| Type | Description | Example |
|------|-------------|---------|
| Login + Interactive | User authenticates | SSH login, TTY |
| Non-login + Interactive | Shell within a session | New terminal tab |
| Non-interactive | Automated execution | Running a script |

**Files loaded (Bash):**

| File | When loaded |
|------|-------------|
| `/etc/profile` | Every login session (all users) |
| `~/.bash_profile` | Login session (user-specific) |
| `~/.bashrc` | Every interactive non-login session |
| `~/.bash_logout` | On logout |

**Load order (login session):**
1. `/etc/profile`
2. `~/.bash_profile` → `~/.bash_login` → `~/.profile` (first found)

### Examples

```bash
# --- Check current session type ---
[[ -o login ]] && echo "Login shell" || echo "Non-login shell"

# --- Add alias to persist across sessions ---
echo 'alias ll="ls -la"' >> ~/.bashrc
source ~/.bashrc          # Apply immediately

# --- Add PATH entry for login sessions ---
echo 'export PATH=$PATH:/opt/my-tools/bin' >> ~/.bash_profile

# --- Reload config manually ---
source ~/.bashrc
. ~/.bashrc               # Equivalent shorthand

# --- View what's in your current config ---
cat ~/.bashrc
cat ~/.bash_profile

# --- Add a welcome message on login ---
echo 'echo "Welcome back, $USER! $(date)"' >> ~/.bash_profile
```

---

## Final Lab — System Audit Script

### Objective

Apply **all concepts** from this course in a single practical script:
- Commands and chaining
- Pipes and redirections
- Environment variables
- Glob patterns and job control
- Security best practices (hardening, permissions)
- Profile-aware configuration

### Lab Instructions

Save the script below as `audit.sh`, make it executable, and run it.

```bash
#!/bin/bash
# ============================================================
# SYSTEM AUDIT SCRIPT — Linux Shell Course Final Lab
# Covers: commands, variables, redirections, pipes, security
# ============================================================

set -euo pipefail   # Harden the script (Section 6)

# --- Environment Variables (Section 5) ---
REPORT_DIR="${HOME}/shell_audit"
REPORT_FILE="${REPORT_DIR}/report_$(date +%Y%m%d_%H%M%S).txt"
ERRORS_FILE="${REPORT_DIR}/errors.txt"
LOG_LINES=20

# --- Setup ---
mkdir -p "$REPORT_DIR"
echo "Audit started: $(date)" | tee "$REPORT_FILE"
echo "User: $USER | Shell: $SHELL | Home: $HOME" | tee -a "$REPORT_FILE"
echo "=======================================" >> "$REPORT_FILE"

# --- Section 3: Running commands and chaining ---
echo "" >> "$REPORT_FILE"
echo "[ SYSTEM INFO ]" >> "$REPORT_FILE"
uname -a >> "$REPORT_FILE" 2>> "$ERRORS_FILE"
uptime >> "$REPORT_FILE" 2>> "$ERRORS_FILE"

# --- Section 4: Pipes and Redirections ---
echo "" >> "$REPORT_FILE"
echo "[ TOP 5 MEMORY-CONSUMING PROCESSES ]" >> "$REPORT_FILE"
ps aux --sort=-%mem | head -6 | awk '{print $1, $2, $3, $4, $11}' >> "$REPORT_FILE"

echo "" >> "$REPORT_FILE"
echo "[ DISK USAGE (top 5 largest dirs in HOME) ]" >> "$REPORT_FILE"
du -sh "${HOME}"/* 2>/dev/null | sort -rh | head -5 >> "$REPORT_FILE" || echo "Could not read some dirs" >> "$ERRORS_FILE"

# --- Section 2: Glob Patterns ---
echo "" >> "$REPORT_FILE"
echo "[ SHELL CONFIG FILES FOUND ]" >> "$REPORT_FILE"
for f in ~/.bash_profile ~/.bashrc ~/.bash_logout ~/.profile ~/.zshrc ~/.config/fish/config.fish; do
  if [[ -f "$f" ]]; then
    echo "  FOUND: $f ($(wc -l < "$f") lines)" >> "$REPORT_FILE"
  fi
done

# --- Section 7: Profile / Environment ---
echo "" >> "$REPORT_FILE"
echo "[ CURRENT PATH ENTRIES ]" >> "$REPORT_FILE"
echo "$PATH" | tr ':' '\n' | nl >> "$REPORT_FILE"

echo "" >> "$REPORT_FILE"
echo "[ EXPORTED VARIABLES (sample) ]" >> "$REPORT_FILE"
env | grep -E "^(HOME|USER|SHELL|LANG|TERM|PATH)" | sort >> "$REPORT_FILE"

# --- Section 6: Security — Check sensitive file permissions ---
echo "" >> "$REPORT_FILE"
echo "[ PERMISSION CHECK ]" >> "$REPORT_FILE"
for f in ~/.bashrc ~/.bash_profile ~/.ssh/authorized_keys /etc/passwd /etc/shadow; do
  if [[ -e "$f" ]]; then
    perm=$(stat -c "%a %n" "$f" 2>/dev/null || echo "N/A")
    echo "  $perm" >> "$REPORT_FILE"
  fi
done

# --- Section 4: Here Document — Summary block ---
cat << EOF >> "$REPORT_FILE"

=======================================
AUDIT COMPLETE
Report saved to : $REPORT_FILE
Errors logged at: $ERRORS_FILE
Generated by    : $USER on $(hostname)
Date            : $(date)
=======================================
EOF

# --- Final output ---
echo ""
echo "✅ Audit complete! Report saved to:"
echo "   $REPORT_FILE"

# Show report with pipe + less (Section 4)
cat "$REPORT_FILE"
```

### Running the Lab

```bash
# Save and execute
chmod +x audit.sh    # Section 6 — permissions
./audit.sh

# View the report
cat ~/shell_audit/report_*.txt

# Check for any errors
cat ~/shell_audit/errors.txt 2>/dev/null || echo "No errors recorded"
```

### What This Lab Covers

| Concept | Where used |
|---------|-----------|
| Commands & chaining | `uname`, `uptime`, `&&`, `;` |
| Aliases & variables | `REPORT_DIR`, `REPORT_FILE`, `LOG_LINES` |
| Pipes | `ps aux \| head \| awk`, `du \| sort \| head` |
| Output redirection | `>>`, `2>>`, `tee`, `2>/dev/null` |
| Here Document | Final summary block with `<< EOF` |
| Glob patterns | `for f in ~/.bash*` |
| Environment variables | `$HOME`, `$USER`, `$SHELL`, `$PATH` |
| Security & hardening | `set -euo pipefail`, permission checks |
| Profile files | Detection of `.bashrc`, `.zshrc`, `.profile` |

---

*End of Linux Shell Course — Happy scripting!*
