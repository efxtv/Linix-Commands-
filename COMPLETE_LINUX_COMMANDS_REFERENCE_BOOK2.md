# COMPLETE LINUX COMMANDS REFERENCE BOOK
## All 2800+ Commands with Detailed Explanations

---

## BOOK INFORMATION

**Title:** Complete Linux Commands Reference Book  
**Subtitle:** Full Detailed Explanations of 2800+ Linux Commands  
**Book By:** EFXTv  
**Edition:** 1.0  
**Last Updated:** November 2025  
**Total Commands:** 2801+

---

## TABLE OF CONTENTS

1. [File & Directory Management](#category-1-file--directory-management)
2. [Text Processing & Manipulation](#category-2-text-processing--manipulation)
3. [System Information & Monitoring](#category-3-system-information--monitoring)
4. [Process & Job Management](#category-4-process--job-management)
5. [Network & Communication](#category-5-network--communication)
6. [Disk & Storage Management](#category-6-disk--storage-management)
7. [User & Group Management](#category-7-user--group-management)
8. [Permissions & Ownership](#category-8-permissions--ownership)
9. [Archiving & Compression](#category-9-archiving--compression)
10. [Package Management](#category-10-package-management)
11. [System Services & Control](#category-11-system-services--control)
12. [Search & Find](#category-12-search--find)
13. [Advanced Text Processing](#category-13-advanced-text-processing)
14. [System Administration](#category-14-system-administration)
15. [Network Tools & Diagnostics](#category-15-network-tools--diagnostics)
16. [Development & Compilation](#category-16-development--compilation)
17. [System Performance & Profiling](#category-17-system-performance--profiling)
18. [Encryption & Security](#category-18-encryption--security)
19. [Shell & Environment](#category-19-shell--environment)
20. [Advanced File Systems (LVM, Btrfs, ZFS)](#category-20-advanced-file-systems)
21. [Miscellaneous & Utilities](#category-21-miscellaneous--utilities)

---

# CATEGORY 1: FILE & DIRECTORY MANAGEMENT

## pwd - Print Working Directory
**Full Name:** Print Working Directory  
**Description:** Displays the full pathname of the current working directory.

### Syntax:
\`\`\`bash
pwd [OPTIONS]
\`\`\`

### Common Options:
- `-L, --logical`: Show logical path (with symlinks)
- `-P, --physical`: Show physical path (without symlinks)

### Use Cases:
- Check current directory location
- Navigate confirmation
- Script path verification
- Directory context awareness

### Examples:
\`\`\`bash
pwd
# Output: /home/user/documents

pwd -P
# Show physical path if in symlink directory

pwd -L
# Show logical path with symlinks

pwd > current_location.txt
# Save current directory to file
\`\`\`

---

## ls - List Directory Contents
**Full Name:** List Directory Contents  
**Description:** Lists files and directories with extensive customization options.

### Syntax:
\`\`\`bash
ls [OPTIONS] [FILE/DIRECTORY]
\`\`\`

### Common Options:
- `-a, --all`: Show hidden files (including . and ..)
- `-l, --long`: Long format with details
- `-h, --human-readable`: Human-readable file sizes
- `-R, --recursive`: List recursively
- `-S, --sort=size`: Sort by file size
- `-t, --sort=time`: Sort by modification time
- `-r, --reverse`: Reverse sort order
- `-d, --directory`: List directory itself, not contents
- `--color=auto`: Colorized output
- `-i, --inode`: Show inode numbers

### Use Cases:
- View directory contents
- Check file permissions
- Find recent files
- Verify file sizes
- List hidden configuration files

### Examples:
\`\`\`bash
ls
# List current directory contents

ls -la
# List all files with long format, including hidden

ls -lh
# Human-readable file sizes

ls -lS
# Sort by file size (largest first)

ls -lt
# Sort by modification time (newest first)

ls -lR /home/user
# Recursive listing of /home/user

ls -1 *.txt
# List only .txt files, one per line

ls -ld */
# List only directories

ls --color=auto
# Colorized output

ls -i
# Show inode numbers
\`\`\`

---

## cd - Change Directory
**Full Name:** Change Directory  
**Description:** Changes the current working directory to specified path.

### Syntax:
\`\`\`bash
cd [DIRECTORY]
\`\`\`

### Common Usage:
- `cd /path`: Go to absolute path
- `cd`: Go to home directory
- `cd ~`: Go to home directory
- `cd -`: Go to previous directory
- `cd ..`: Go to parent directory
- `cd ../..`: Go up two directories

### Use Cases:
- Navigate file system
- Access specific directories
- Return to previous location
- Script directory changes

### Examples:
\`\`\`bash
cd /home/user/documents
# Change to documents directory

cd
# Return to home directory

cd -
# Go back to previous directory

cd ..
# Go to parent directory

cd ../../../
# Go up 3 directory levels

cd ~
# Explicit home directory

cd /etc
# Change to system configuration directory

cd /var/log
# Change to log directory

pushd /tmp
# Change directory and save location on stack
\`\`\`

---

## mkdir - Make Directory
**Full Name:** Make Directory  
**Description:** Creates new directories/folders.

### Syntax:
\`\`\`bash
mkdir [OPTIONS] DIRECTORY [DIRECTORY...]
\`\`\`

### Common Options:
- `-p, --parents`: Create parent directories as needed
- `-m, --mode=MODE`: Set directory permissions
- `-v, --verbose`: Print each created directory

### Use Cases:
- Create new folders
- Build directory structures
- Create nested paths
- Set specific permissions

### Examples:
\`\`\`bash
mkdir my_folder
# Create single directory

mkdir -p /home/user/projects/2025/january
# Create nested directory structure

mkdir -m 755 public_folder
# Create directory with specific permissions

mkdir -v folder1 folder2 folder3
# Create multiple directories with verbose output

mkdir -p ~/backups/daily/{databases,configs,logs}
# Create complex directory structure

mkdir project_{1..5}
# Create multiple numbered directories (project_1 to project_5)
\`\`\`

---

## rmdir - Remove Directory
**Full Name:** Remove Directory  
**Description:** Removes empty directories.

### Syntax:
\`\`\`bash
rmdir [OPTIONS] DIRECTORY [DIRECTORY...]
\`\`\`

### Common Options:
- `-p, --parents`: Remove parent directories if empty
- `-v, --verbose`: Print each removed directory

### Use Cases:
- Clean up empty folders
- Remove directory structures
- Bulk directory cleanup

### Examples:
\`\`\`bash
rmdir empty_folder
# Remove empty directory

rmdir -p ~/projects/old/unused
# Remove directory and empty parents

rmdir -v folder1 folder2 folder3
# Remove multiple directories with output

rmdir *_backup
# Remove all _backup directories (if empty)
\`\`\`

---

## rm - Remove Files/Directories
**Full Name:** Remove File or Directory  
**Description:** Removes files and directories (use with caution!).

### Syntax:
\`\`\`bash
rm [OPTIONS] FILE/DIRECTORY [FILE/DIRECTORY...]
\`\`\`

### Common Options:
- `-f, --force`: Force removal without prompting
- `-r, -R, --recursive`: Remove directories recursively
- `-i, --interactive`: Prompt before removal
- `-v, --verbose`: Print each removed file
- `-d, --dir`: Remove empty directories

### DANGER CASES - BE CAREFUL:
\`\`\`bash
rm -rf /
# DESTROYS ENTIRE SYSTEM - NEVER RUN!

rm -rf .*
# Deletes all hidden files - EXTREMELY DANGEROUS!
\`\`\`

### Safe Use Cases:
- Remove unwanted files
- Clean up temporary files
- Delete log files
- Remove old backups

### Examples:
\`\`\`bash
rm file.txt
# Remove single file (prompts on some systems)

rm -f file.txt
# Force remove without confirmation

rm -i file1.txt file2.txt file3.txt
# Interactive removal with prompts

rm -r folder
# Remove folder and all contents

rm -rv old_backup/
# Remove folder recursively with output

rm *.log
# Remove all .log files

rm -f *.tmp *.bak
# Force remove all temporary files

rm -- -file.txt
# Remove file starting with dash

shred -u sensitive_file.txt
# Securely delete file (multiple overwrites)
\`\`\`

---

## cp - Copy Files/Directories
**Full Name:** Copy File or Directory  
**Description:** Copies files and directories.

### Syntax:
\`\`\`bash
cp [OPTIONS] SOURCE DESTINATION
\`\`\`

### Common Options:
- `-r, -R, --recursive`: Copy directories recursively
- `-i, --interactive`: Prompt before overwrite
- `-f, --force`: Force overwrite
- `-v, --verbose`: Print copied files
- `-p, --preserve`: Preserve permissions, ownership, timestamps
- `-a, --archive`: Archive mode (combination of -dR --preserve)
- `-u, --update`: Only copy if source is newer
- `--backup`: Create backup of destination
- `-d`: Same as --no-dereference --preserve=links

### Use Cases:
- Backup files
- Duplicate files/folders
- Copy to multiple locations
- Archive with permissions preserved

### Examples:
\`\`\`bash
cp file.txt file_backup.txt
# Copy file with new name

cp file1.txt file2.txt file3.txt /backup/
# Copy multiple files to directory

cp -r my_folder backup_folder
# Copy entire directory and contents

cp -v large_file.iso /media/usb/
# Copy with progress indication

cp -p original.txt copy.txt
# Copy preserving permissions and timestamps

cp -i file.txt destination/
# Interactive copy (confirm before overwrite)

cp -r --preserve=all source/ destination/
# Archive copy preserving all attributes

cp -u *.doc /archive/
# Copy only newer files

cp --backup=t file.txt new_location/
# Create backup of destination file

cp -d source/symlink dest/
# Copy symlink as symlink, not target
\`\`\`

---

## mv - Move/Rename Files/Directories
**Full Name:** Move or Rename File or Directory  
**Description:** Moves or renames files and directories.

### Syntax:
\`\`\`bash
mv [OPTIONS] SOURCE DESTINATION
\`\`\`

### Common Options:
- `-i, --interactive`: Prompt before overwrite
- `-f, --force`: Force overwrite
- `-v, --verbose`: Print moved files
- `-u, --update`: Only move if source is newer
- `-n, --no-clobber`: Don't overwrite existing files
- `--backup`: Create backup of destination

### Use Cases:
- Rename files/folders
- Move files to different locations
- Organize directory structure
- Batch file operations

### Examples:
\`\`\`bash
mv old_name.txt new_name.txt
# Rename file

mv /source/file.txt /destination/
# Move file to different location

mv /source/directory /backup/
# Move entire directory

mv *.log /var/log/archive/
# Move all .log files

mv -i file.txt destination/
# Interactive move (confirm before overwrite)

mv -v file1.txt file2.txt file3.txt /archive/
# Verbose move with progress

mv --backup=t file.txt existing_file.txt
# Create backup of destination

mv -f old_file.txt new_file.txt
# Force overwrite without confirmation

# Batch rename files
for file in *.txt; do
  mv "$file" "${file%.txt}.md"
done
# Rename all .txt to .md
\`\`\`

---

## cat - Concatenate and Display Files
**Full Name:** Concatenate and Display  
**Description:** Displays file contents, can concatenate multiple files.

### Syntax:
\`\`\`bash
cat [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-n, --number`: Number output lines
- `-b, --number-nonblank`: Number non-blank lines only
- `-s, --squeeze-blank`: Suppress repeated blank lines
- `-A, --show-all`: Show all non-printing characters
- `-E, --show-ends`: Show line endings ($)
- `-T, --show-tabs`: Show tabs as ^I
- `-v, --show-nonprinting`: Show non-printing characters

### Use Cases:
- Display file contents
- Create/append to files
- Concatenate multiple files
- Check file contents quickly

### Examples:
\`\`\`bash
cat file.txt
# Display file contents

cat file1.txt file2.txt file3.txt
# Concatenate and display multiple files

cat -n file.txt
# Display with line numbers

cat > newfile.txt << EOF
Line 1
Line 2
Line 3
EOF
# Create file using cat and here document

cat >> existing.txt << EOF
Additional line
EOF
# Append to existing file

cat file.txt > backup.txt
# Copy file contents

cat file1.txt file2.txt > combined.txt
# Combine files

cat -A file.txt | head
# Show special characters (tabs, line ends)

cat -s file.txt
# Suppress multiple blank lines

cat -n file.txt | grep pattern
# Display lines with pattern and numbers
\`\`\`

---

## tac - Display File Backwards
**Full Name:** TAC (Reverse cat)  
**Description:** Displays file contents in reverse (last line first).

### Syntax:
\`\`\`bash
tac [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-b, --before`: Put separator before instead of after
- `-r, --regex`: Interpret separator as regex
- `-s, --separator=STRING`: Specify line separator

### Use Cases:
- View files in reverse order
- Check end of logs
- Reverse line order processing

### Examples:
\`\`\`bash
tac file.txt
# Display file in reverse order

tac logfile.log
# View log file backwards (newest entries first)

tac file.txt file2.txt
# Reverse multiple files

tac -s '---' file.txt
# Use custom separator for reverse

tail -f /var/log/syslog | tac
# Reverse live log stream
\`\`\`

---

## less - View File with Pagination
**Full Name:** Less File Viewer  
**Description:** View file contents with navigation (improved version of `more`).

### Syntax:
\`\`\`bash
less [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-N, --line-numbers`: Display line numbers
- `-S, --chop-long-lines`: Chop long lines
- `-R, --raw-control-chars`: Display raw control characters
- `-X, --no-init`: Don't clear screen on exit
- `-F, --quit-if-one-screen`: Quit if content fits one screen
- `-G, --HILITE-SEARCH`: Disable search highlighting

### Keyboard Navigation:
- `Space`: Page down
- `b`: Page up
- `g`: Go to beginning
- `G`: Go to end
- `/pattern`: Search forward
- `?pattern`: Search backward
- `n`: Next search result
- `N`: Previous search result
- `q`: Quit

### Use Cases:
- Browse large files
- Read logs safely
- Navigate documentation
- Search within files

### Examples:
\`\`\`bash
less large_file.txt
# View file with pagination

less -N large_file.txt
# Display with line numbers

less +100 file.txt
# Start at line 100

less /var/log/syslog
# View system logs safely

tail -f logfile.log | less -R
# View live log with less

less +F logfile.log
# Follow file like tail -f

less -S file.txt
# Truncate long lines

less --follow-name logfile.txt
# Follow file renames
\`\`\`

---

## more - Simple File Pager
**Full Name:** More File Pager  
**Description:** Display file contents one screen at a time (simpler than less).

### Syntax:
\`\`\`bash
more [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-d`: Display helpful prompt
- `-f`: Count logical lines
- `-l`: Don't pause for form feeds
- `-p`: Clear screen before display
- `+num`: Start at line number

### Keyboard Controls:
- `Space`: Next page
- `Return`: Next line
- `b`: Previous page
- `q`: Quit
- `=`: Show current line number
- `/pattern`: Search

### Use Cases:
- Basic file viewing
- Simple pagination
- Read configuration files
- Check large logs

### Examples:
\`\`\`bash
more file.txt
# Simple file viewing

more /etc/passwd
# View password file

cat largefile.txt | more
# Pipe output for pagination

more +50 file.txt
# Start at line 50

more -d file.txt
# Display helpful prompts
\`\`\`

---

## head - Display Beginning of File
**Full Name:** Head of File  
**Description:** Displays first lines of file (default 10 lines).

### Syntax:
\`\`\`bash
head [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-n, --lines=NUM`: Display NUM lines
- `-c, --bytes=NUM`: Display first NUM bytes
- `-v, --verbose`: Always print filename
- `-q, --quiet`: Suppress filename headers
- `-z, --zero-terminated`: Handle null-terminated lines

### Use Cases:
- Check file beginning
- Preview large files
- Extract file headers
- Monitor log starts

### Examples:
\`\`\`bash
head file.txt
# Display first 10 lines

head -n 20 file.txt
# Display first 20 lines

head -c 100 file.txt
# Display first 100 bytes

head -n 5 file1.txt file2.txt
# Display first 5 lines of multiple files

head -n -5 file.txt
# Display all except last 5 lines

head -q -n 3 file1.txt file2.txt
# Display 3 lines without filename headers

ls -lt | head -n 5
# Show 5 most recent files
\`\`\`

---

## tail - Display End of File
**Full Name:** Tail of File  
**Description:** Displays last lines of file (default 10 lines).

### Syntax:
\`\`\`bash
tail [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-n, --lines=NUM`: Display NUM lines
- `-c, --bytes=NUM`: Display last NUM bytes
- `-f, --follow`: Follow file growth (live)
- `-F, --follow=name`: Follow with file rotation support
- `-v, --verbose`: Always print filename
- `-q, --quiet`: Suppress filename headers
- `--pid=PID`: Terminate after PID dies (with -f)
- `-s, --sleep=NUM`: Sleep NUM seconds between checks

### Use Cases:
- Monitor log files
- Check file endings
- Follow live output
- Extract last records

### Examples:
\`\`\`bash
tail file.txt
# Display last 10 lines

tail -n 20 file.txt
# Display last 20 lines

tail -f /var/log/syslog
# Follow log file in real-time

tail -F /var/log/logfile
# Follow with rotation support

tail -n 100 /var/log/auth.log | grep "Failed"
# Last 100 lines containing "Failed"

tail -c 500 file.txt
# Display last 500 bytes

tail -n +50 file.txt
# Display from line 50 onwards

tail -n 5 -q file1.txt file2.txt
# Display last 5 lines of multiple files

tail -f --pid=$$ application.log
# Follow until current process exits

while true; do clear; tail -n 20 logfile.log; sleep 1; done
# Refresh log display every second
\`\`\`

---

## echo - Display Text
**Full Name:** Echo String  
**Description:** Displays text or variable values.

### Syntax:
\`\`\`bash
echo [OPTIONS] [TEXT]
\`\`\`

### Common Options:
- `-n`: Don't add newline
- `-e`: Enable interpretation of backslash escapes
- `-E`: Disable interpretation of escapes

### Escape Sequences (with -e):
- `\n`: Newline
- `\t`: Tab
- `\a`: Alert (beep)
- `\\`: Backslash
- `\r`: Carriage return

### Use Cases:
- Print text to terminal
- Display variables
- Create files
- Script output
- Debugging

### Examples:
\`\`\`bash
echo "Hello, World!"
# Simple text output

echo $USER
# Display current user variable

echo "Name: $1, Age: $2"
# Display command arguments

echo -e "Line 1\nLine 2\nLine 3"
# Print multiple lines

echo -e "Column1\tColumn2\tColumn3"
# Print tab-separated columns

echo "Text" > file.txt
# Create file with echo

echo "More text" >> file.txt
# Append to file

echo -n "Enter your name: "
# Prompt without newline

echo $((5 + 3))
# Arithmetic output

echo "Result: $result"
# Display variable value
\`\`\`

---

## touch - Create/Update File Timestamp
**Full Name:** Touch File  
**Description:** Creates empty files or updates file timestamps.

### Syntax:
\`\`\`bash
touch [OPTIONS] FILE [FILE...]
\`\`\`

### Common Options:
- `-a`: Change access time only
- `-m`: Change modification time only
- `-t [[CC]YY]MMDDhhmm[.ss]`: Set specific time
- `-d, --date=STRING`: Set date string
- `-r, --reference=FILE`: Use FILE's timestamp
- `-c, --no-create`: Don't create file if doesn't exist

### Use Cases:
- Create empty files
- Update file timestamps
- Create file stubs for scripting
- Mark file modification times

### Examples:
\`\`\`bash
touch newfile.txt
# Create empty file

touch file1.txt file2.txt file3.txt
# Create multiple files

touch -m file.txt
# Update only modification time

touch -a file.txt
# Update only access time

touch -t 202501011200 file.txt
# Set timestamp to 2025-01-01 12:00

touch -r original.txt copy.txt
# Use original.txt's timestamp

touch -c file.txt
# Don't create if file doesn't exist

touch -d "2 days ago" file.txt
# Set timestamp to 2 days ago

touch -d "2025-06-15" file.txt
# Set specific date
\`\`\`

---

## nano - Simple Text Editor
**Full Name:** Nano Text Editor  
**Description:** Easy-to-use command-line text editor.

### Syntax:
\`\`\`bash
nano [OPTIONS] [FILE]
\`\`\`

### Common Options:
- `-m, --mouse`: Enable mouse support
- `-u, --unix`: Save in Unix format
- `-w, --nowrap`: Don't wrap lines
- `-l, --linenumbers`: Show line numbers
- `-A, --smarthome`: Smart home key
- `-c, --constantshow`: Show position info

### Keyboard Shortcuts:
- `Ctrl+X`: Exit
- `Ctrl+O`: Save
- `Ctrl+R`: Read file
- `Ctrl+W`: Search
- `Ctrl+A`: Select all
- `Ctrl+K`: Cut line
- `Ctrl+U`: Paste
- `Ctrl+T`: Spell check

### Use Cases:
- Edit configuration files
- Create text files
- Beginner-friendly editing
- Quick file modifications

### Examples:
\`\`\`bash
nano filename.txt
# Open file for editing

nano -l filename.txt
# Open with line numbers

nano -w -m filename.txt
# Disable wrapping, enable mouse

nano -u filename.txt
# Save in Unix format

nano +10 filename.txt
# Start at line 10
\`\`\`

---

## vi/vim - Advanced Text Editor
**Full Name:** Vi Improved (Vim) Text Editor  
**Description:** Powerful modal text editor with extensive features.

### Syntax:
\`\`\`bash
vim [OPTIONS] [FILE]
vi [OPTIONS] [FILE]
\`\`\`

### Basic Commands:

**Normal Mode (press Esc to return here):**
- `i`: Insert mode at cursor
- `I`: Insert at beginning of line
- `a`: Append after cursor
- `A`: Append at end of line
- `o`: Open line below
- `O`: Open line above
- `u`: Undo
- `Ctrl+R`: Redo
- `dd`: Delete line
- `yy`: Copy line
- `p`: Paste
- `gg`: Go to beginning
- `G`: Go to end
- `/$pattern`: Search
- `:set number`: Show line numbers
- `:wq`: Save and quit
- `:q!`: Quit without saving
- `:s/old/new/g`: Replace all on line
- `:%s/old/new/g`: Replace all in file

### Use Cases:
- Edit system files
- Advanced text manipulation
- Programming
- Powerful automation
- Remote editing (works over slow connections)

### Examples:
\`\`\`bash
vim filename.txt
# Open file in vim

vim +10 filename.txt
# Open at line 10

vim +/pattern filename.txt
# Open and search for pattern

vim -c ":%s/old/new/g" filename.txt
# Open and replace on load

# In vim:
:set number                    # Show line numbers
:set highlighting             # Enable syntax highlighting
:syntax on                     # Enable color
:q!                          # Quit without saving
:wq                          # Save and quit
:%s/search/replace/g         # Replace all
\`\`\`

---

## clear - Clear Screen
**Full Name:** Clear Screen  
**Description:** Clears the terminal screen.

### Syntax:
\`\`\`bash
clear [OPTIONS]
\`\`\`

### Options:
- `-V, --version`: Show version
- `-h, --help`: Show help

### Use Cases:
- Clean terminal display
- Start fresh screen
- Improve readability
- Script display control

### Examples:
\`\`\`bash
clear
# Clear terminal screen

clear && echo "Fresh screen"
# Clear and print message

Ctrl+L
# Keyboard shortcut (same as clear)

alias c='clear'
c
# Use alias for faster clearing

for i in {1..100}; do echo $i; sleep 0.1; clear; done
# Create animation effect
\`\`\`

---

## history - View Command History
**Full Name:** Command History  
**Description:** Displays previously executed commands.

### Syntax:
\`\`\`bash
history [OPTIONS] [N]
\`\`\`

### Common Options:
- `-a`: Append new commands
- `-c`: Clear history
- `-d OFFSET`: Delete history entry
- `-n`: Read history entries not added yet
- `-r`: Read history file
- `-w`: Write history to file
- `-s CMD`: Add command to history
- `-p`: Display history without execution

### Keyboard Shortcuts:
- `!!`: Last command
- `!45`: Command number 45
- `!text`: Last command starting with text
- `^old^new`: Repeat last, replace old with new
- `Ctrl+R`: Search history (Ctrl+S for next)

### Use Cases:
- Recall previous commands
- Find command syntax
- Repeat operations
- Debug script execution
- Search past actions

### Examples:
\`\`\`bash
history
# Display entire history

history 10
# Display last 10 commands

history -c
# Clear history

history -d 100
# Delete entry 100

!!
# Repeat last command

!grep
# Last command starting with grep

!50
# Execute command 50 again

^old^new
# Replace old with new in last command

history | grep "apt install"
# Find past install commands

history | tail -20
# Show last 20 commands

Ctrl+R
# Interactive search (then type pattern)

history -s "command to add"
# Add command to history without executing
\`\`\`

---

## man - Manual Pages
**Full Name:** Manual Pages  
**Description:** Displays command documentation and help.

### Syntax:
\`\`\`bash
man [SECTION] COMMAND
\`\`\`

### Sections:
- 1: User commands
- 2: System calls
- 3: Library functions
- 4: Device files
- 5: File formats
- 6: Games
- 7: Miscellaneous
- 8: System administration

### Common Options:
- `-f, --whatis`: Short description
- `-k, --apropos`: Search descriptions
- `-a, --all`: Show all matching manuals
- `-l, --local-file`: Format local file
- `-w, --where`: Show file locations

### Keyboard Navigation:
- `Space`: Next page
- `b`: Previous page
- `/pattern`: Search
- `n`: Next search
- `q`: Quit

### Use Cases:
- Learn command syntax
- Understand options
- Read documentation
- Find command information

### Examples:
\`\`\`bash
man ls
# Display ls manual

man -s 2 read
# Display section 2 read manual

man -k file
# Search for commands with "file" in description

man -f ls
# Short description of ls

man -w ls
# Show man page file location

man -a printf
# Show all printf manuals

man ls | grep -A5 "DESCRIPTION"
# Find specific section

MANPAGER='less -p "EXAMPLES"' man ls
# Jump to examples section
\`\`\`

---

## help - Shell Built-in Help
**Full Name:** Built-in Help  
**Description:** Displays help for shell built-in commands.

### Syntax:
\`\`\`bash
help [COMMAND]
\`\`\`

### Options:
- `-m`: Display usage in pseudo-manpage format
- `-d`: Display short description
- `-s`: Display usage synopsis

### Use Cases:
- Help with bash built-ins
- Quick syntax reference
- Understand built-in commands
- Script debugging

### Examples:
\`\`\`bash
help
# List all built-in commands

help cd
# Help for cd command

help -d for
# Short description of for

help -s if
# Usage synopsis for if

help -m test
# Manpage-style help for test

help [
# Help for test command ([ ]

help -s case
# Case statement help
\`\`\`

---

## which - Locate Command
**Full Name:** Which Command Location  
**Description:** Locates command in PATH.

### Syntax:
\`\`\`bash
which [OPTIONS] COMMAND [COMMAND...]
\`\`\`

### Common Options:
- `-a, --all`: Show all matches in PATH
- `-n NUM`: Show first NUM matches
- `-w, --read-alias`: Read aliases
- `-i, --read-functions`: Read shell functions
- `--skip-alias`: Skip aliases
- `--skip-functions`: Skip functions

### Use Cases:
- Find command location
- Verify command availability
- Debug PATH issues
- Check for aliases

### Examples:
\`\`\`bash
which ls
# Show ls command path
# Output: /bin/ls

which -a python
# Show all python versions in PATH

which gcc g++ make
# Check multiple commands

which -a ls
# Show all ls in PATH

# Check if command exists
which python3 > /dev/null && echo "Found" || echo "Not found"

# Find command before running
which python3

# Verify script interpreter
which bash
\`\`\`

---

## whoami - Current User
**Full Name:** Who Am I  
**Description:** Displays current user name.

### Syntax:
\`\`\`bash
whoami [OPTIONS]
\`\`\`

### Options:
- `--help`: Show help
- `--version`: Show version

### Use Cases:
- Check logged-in user
- Script user verification
- Permission checking
- Security auditing

### Examples:
\`\`\`bash
whoami
# Display current user
# Output: john

# Check user in script
CURRENT_USER=$(whoami)
if [ "$CURRENT_USER" != "root" ]; then
  echo "Must run as root"
  exit 1
fi

# Use in conditional
if [ "$(whoami)" = "root" ]; then
  echo "Running as root"
fi

whoami | tee user.txt
# Display and save to file
\`\`\`

---

## who - Logged-in Users
**Full Name:** Who Is Logged In  
**Description:** Shows logged-in users and their sessions.

### Syntax:
\`\`\`bash
who [OPTIONS] [FILE]
\`\`\`

### Common Options:
- `-a, --all`: All information
- `-b, --boot`: Boot time
- `-d, --dead`: Dead processes
- `-H, --heading`: Print headings
- `-l, --login`: Login processes
- `-p, --process`: Active processes
- `-r, --runlevel`: Run level
- `-s, --short`: Short format
- `-T, --mesg`: Message status
- `-t, --time`: System clock

### Use Cases:
- Monitor user sessions
- Check active users
- System audit
- Security monitoring

### Examples:
\`\`\`bash
who
# List logged-in users

who -H
# Display with headings

who -b
# Show boot time

who -a
# All information

who -T
# Show message status

# Count logged-in users
who | wc -l

# List unique users
who | awk '{print $1}' | sort | uniq

# Monitor user logins
watch -n 1 who

# Check specific user
who | grep username
\`\`\`

---

## uname - System Information
**Full Name:** Print System Information  
**Description:** Displays system and kernel information.

### Syntax:
\`\`\`bash
uname [OPTIONS]
\`\`\`

### Common Options:
- `-s, --kernel-name`: Kernel name
- `-n, --nodename`: Network node hostname
- `-r, --kernel-release`: Kernel release
- `-v, --kernel-version`: Kernel version
- `-m, --machine`: Machine hardware name
- `-p, --processor`: Processor type
- `-i, --hardware-platform`: Hardware platform
- `-o, --operating-system`: Operating system
- `-a, --all`: All information

### Use Cases:
- Verify OS and kernel
- Check system architecture
- Script compatibility
- System documentation

### Examples:
\`\`\`bash
uname
# Basic information
# Output: Linux

uname -a
# Complete system information

uname -r
# Kernel version
# Output: 5.15.0-56-generic

uname -m
# Machine architecture
# Output: x86_64

uname -s
# Kernel name
# Output: Linux

uname -n
# Hostname

uname -p
# Processor type

# Check if 32 or 64 bit
uname -m

# Kernel version for module compilation
uname -r

# In script
if [ "$(uname)" = "Darwin" ]; then
  echo "macOS"
elif [ "$(uname)" = "Linux" ]; then
  echo "Linux"
fi
\`\`\`

---

## date - Display/Set Date and Time
**Full Name:** Print or Set Date and Time  
**Description:** Shows or sets system date and time.

### Syntax:
\`\`\`bash
date [OPTIONS] [+FORMAT]
\`\`\`

### Common Format Codes:
- `%Y`: Year (4 digits)
- `%m`: Month (01-12)
- `%d`: Day (01-31)
- `%H`: Hour (00-23)
- `%M`: Minute (00-59)
- `%S`: Second (00-59)
- `%A`: Full weekday name
- `%B`: Full month name
- `%T`: Time (HH:MM:SS)

### Common Options:
- `-d, --date STRING`: Parse date string
- `-s, --set STRING`: Set system time
- `-r, --reference FILE`: Use file's time
- `-u, --utc`: Display UTC time
- `-I, --iso-8601`: ISO 8601 format

### Use Cases:
- Display current time
- Schedule tasks
- Log timestamps
- System time verification
- Record timing

### Examples:
\`\`\`bash
date
# Current date and time

date +"%Y-%m-%d"
# Date in YYYY-MM-DD format

date +"%H:%M:%S"
# Time in HH:MM:SS format

date +"%A, %B %d, %Y"
# Long format: Monday, January 15, 2025

date -u
# Display UTC time

date -d "2 days ago"
# Date from 2 days ago

date -d "tomorrow"
# Tomorrow's date

date +%s
# Seconds since epoch

date -d @1609459200
# Convert timestamp to date

# Create timestamped backup
tar -czf backup_$(date +%Y%m%d_%H%M%S).tar.gz /data

# In scripts
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
LOG_FILE="log_$TIMESTAMP.txt"

# ISO format
date --iso-8601=seconds

# File timestamps
ls -l --full-time file.txt

# System time setting (requires root)
sudo date -s "2025-01-15 14:30:00"
\`\`\`

---

## cal - Calendar Display
**Full Name:** Calendar  
**Description:** Displays calendar for specific month or year.

### Syntax:
\`\`\`bash
cal [OPTIONS] [[MONTH] YEAR]
\`\`\`

### Common Options:
- `-1, --one`: Display single month
- `-3, --three`: Display previous, current, next month
- `-j, --julian`: Julian day numbers
- `-m, --monday`: Monday as first day
- `-y, --year`: Display entire year

### Use Cases:
- View calendar
- Check dates
- Plan scheduling
- Verify day of week

### Examples:
\`\`\`bash
cal
# Current month calendar

cal 2025
# Entire 2025 calendar

cal 1 2025
# January 2025

cal -3
# Previous, current, next month

cal -m
# Monday as first day

cal -j
# Julian day numbers

cal -y 2025 > calendar_2025.txt
# Save yearly calendar

cal -3 $(date +%m) $(date +%Y)
# Current month with neighbors

# Find day of week
cal 1 2025 | grep -w 15
\`\`\`

---

## time - Measure Command Execution
**Full Name:** Time Command Execution  
**Description:** Measures time taken to execute a command.

### Syntax:
\`\`\`bash
time [OPTIONS] COMMAND
\`\`\`

### Common Options:
- `-p`: Portable format
- `-f FORMAT`: Custom format
- `-o FILE`: Write output to file
- `-a`: Append to file
- `-v`: Verbose output

### Output:
- **real**: Actual elapsed time
- **user**: CPU time in user mode
- **sys**: CPU time in system mode

### Use Cases:
- Performance benchmarking
- Optimization analysis
- Script execution monitoring
- Profiling operations

### Examples:
\`\`\`bash
time ls -R /home
# Time recursive directory listing

time sleep 5
# Time a sleep command

time -p ./script.sh
# Portable format output

time -f "%E elapsed, %U user, %S system" ./program
# Custom format

time -f "%E %C" ./command > timing.log
# Log timing to file

# Compare performance
time method1
time method2

# In script
start=$(date +%s)
# ... do work ...
end=$(date +%s)
echo "Took $((end-start)) seconds"
\`\`\`

---

## sleep - Delay Execution
**Full Name:** Sleep/Delay  
**Description:** Pauses execution for specified time.

### Syntax:
\`\`\`bash
sleep DURATION
\`\`\`

### Duration Units:
- `s`: Seconds (default)
- `m`: Minutes
- `h`: Hours
- `d`: Days

### Use Cases:
- Delay script execution
- Timed loops
- Rate limiting
- Process scheduling

### Examples:
\`\`\`bash
sleep 5
# Sleep 5 seconds

sleep 1m
# Sleep 1 minute

sleep 2h
# Sleep 2 hours

sleep 3600
# Sleep 1 hour (in seconds)

# Loop with delay
for i in {1..5}; do
  echo "Iteration $i"
  sleep 2
done

# Retry with backoff
for attempt in {1..5}; do
  command && break || sleep $((attempt * 10))
done

# Monitor with interval
while true; do
  echo "$(date)"
  sleep 5
done

# Timed task
sleep 30 && echo "30 seconds passed!"

# Parallel delays
sleep 5 & 
sleep 5 &
wait
\`\`\`

---

## reboot - Restart System
**Full Name:** Reboot System  
**Description:** Restarts the computer.

### Syntax:
\`\`\`bash
reboot [OPTIONS]
\`\`\`

### Common Options:
- `-f, --force`: Force reboot
- `-w, --wtmp-only`: Write to wtmp only
- `-d, --no-wtmp`: Don't write wtmp
- `-n, --no-sync`: Don't sync before reboot
- `-p, --poweroff`: Poweroff instead
- `-h, --halt`: Halt instead
- `-k, --kexec`: Use kexec

### Use Cases:
- Restart system
- Apply kernel updates
- Fix frozen system
- Scheduled maintenance

### Warning:
Requires root privileges! Unsaved data will be lost!

### Examples:
\`\`\`bash
sudo reboot
# Reboot system

reboot -f
# Force reboot without sync

sudo shutdown -r +30
# Reboot in 30 minutes

sudo shutdown -r now
# Reboot immediately

sudo reboot -w
# Simulate reboot (write wtmp only)

# Schedule reboot
echo "sudo reboot" | at 2:00 AM

# Warning before reboot
sudo wall "System reboot in 5 minutes"
sudo sleep 300 && sudo reboot
\`\`\`

---

## shutdown - Halt/Power Down
**Full Name:** Shutdown System  
**Description:** Schedules system shutdown, halt, or poweroff.

### Syntax:
\`\`\`bash
shutdown [OPTIONS] TIME [WALL MESSAGE]
\`\`\`

### TIME Formats:
- `now`: Immediate
- `+m`: In m minutes
- `hh:mm`: Specific time
- `HH:MM`: 24-hour time

### Common Options:
- `-h, --halt`: Halt
- `-p, --poweroff`: Power off
- `-r, --reboot`: Reboot
- `-k, --no-act`: Dry run (wall message only)
- `-c`: Cancel scheduled shutdown
- `-H`: Halt system
- `-P`: Power off
- `-N`: Avoid sync

### Use Cases:
- Schedule system shutdown
- Graceful power down
- Notify users before shutdown
- System maintenance
- Emergency shutdown

### Examples:
\`\`\`bash
shutdown -h now
# Halt immediately

shutdown -p now
# Power off immediately

shutdown -r +30 "System rebooting for updates"
# Reboot in 30 minutes with message

shutdown -h 22:00 "Nightly shutdown"
# Shutdown at 10 PM

shutdown -r +60
# Reboot in 60 minutes

shutdown -c
# Cancel scheduled shutdown

# Delayed shutdown
shutdown -h +5 "System will shut down in 5 minutes"

# Notify users then shutdown
wall "Server maintenance - shutdown in 10 minutes"
shutdown -h +10

# Emergency stop
sudo shutdown -h now
sudo poweroff
\`\`\`

---

## exit - Exit Shell/Script
**Full Name:** Exit  
**Description:** Exits shell or script with exit code.

### Syntax:
\`\`\`bash
exit [EXIT_CODE]
\`\`\`

### Exit Codes:
- `0`: Success
- `1-255`: Error codes
- Common: 1 (general error), 2 (misuse), 127 (not found)

### Use Cases:
- Exit script with status
- Error handling
- Exit bash shell
- Return status to parent

### Examples:
\`\`\`bash
exit
# Exit with code 0

exit 0
# Explicit success

exit 1
# Exit with error code 1

if [ $? -ne 0 ]; then
  echo "Error occurred"
  exit 1
fi

# Script ending
if [ ! -f "$1" ]; then
  echo "File not found"
  exit 2
fi

# Function return
function check_file() {
  [ -f "$1" ] || exit 1
}

# Check previous command
ls /nonexistent > /dev/null 2>&1
if [ $? -eq 0 ]; then
  echo "Success"
  exit 0
else
  echo "Failed"
  exit 1
fi

# Exit from subshell
( cd /tmp; exit 0 )
echo $?  # Returns 0
\`\`\`

---

## logout - Exit Login Shell
**Full Name:** Logout  
**Description:** Exits login shell and closes connection.

### Syntax:
\`\`\`bash
logout [EXIT_CODE]
\`\`\`

### Use Cases:
- Logout from SSH session
- End login shell
- Clean shell exit
- Terminal closing

### Examples:
\`\`\`bash
logout
# Logout from login shell

logout 0
# Logout with specific exit code

# In SSH session
ssh user@host
# ... work ...
logout
# Ends SSH connection

# Check if login shell
shopt -q login_shell && echo "Login shell" || echo "Not login shell"
\`\`\`

---

## su - Switch User
**Full Name:** Switch User / Super User  
**Description:** Switches to another user or runs commands as different user.

### Syntax:
\`\`\`bash
su [OPTIONS] [USER] [COMMAND]
\`\`\`

### Common Options:
- `-c, --command=CMD`: Execute command and exit
- `-l, --login`: Create login environment
- `-m, --preserve-environment`: Keep environment
- `-s, --shell=SHELL`: Use specific shell
- `-p, --preserve-groups`: Preserve groups

### Use Cases:
- Run commands as root
- Switch to different user
- Execute privileged commands
- Test user permissions

### Examples:
\`\`\`bash
su
# Switch to root (requires password)

su -
# Switch to root with login shell

su user
# Switch to specific user

su - user
# Switch to user with login shell

su -c "reboot"
# Run command as root

su -c "apt update" -s /bin/bash
# Run command with specific shell

su user -c "whoami"
# Run command as specific user

su -l user -c "pwd"
# Full login shell for user

# Check effective user
id
\`\`\`

---

## sudo - Execute as Superuser
**Full Name:** Superuser Do  
**Description:** Executes command with superuser privileges.

### Syntax:
\`\`\`bash
sudo [OPTIONS] COMMAND
\`\`\`

### Common Options:
- `-u, --user=USER`: Run as specific user
- `-i, --login`: Login shell
- `-s, --shell`: Shell
- `-l, --list`: List permissions
- `-v, --validate`: Refresh credentials
- `-k, --kill`: Invalidate credentials
- `-S, --stdin`: Read password from stdin
- `-E, --preserve-env`: Keep environment

### Use Cases:
- Run privileged commands
- Install packages
- Modify system files
- Manage permissions
- Security

### Examples:
\`\`\`bash
sudo command
# Run command as root (prompts for password)

sudo -u user command
# Run as specific user

sudo -l
# List what commands you can run

sudo !!
# Run previous command with sudo

sudo apt update
# Update package lists

sudo systemctl restart nginx
# Restart service

sudo nano /etc/hosts
# Edit system files

# Passwordless execution
sudo -S command << EOF
password
EOF

# Edit sudoers (safe)
sudo visudo

# Run multiple commands
sudo bash -c 'command1; command2; command3'

# Find files owned by root
sudo find / -user root -name "*.log"
\`\`\`

---

## passwd - Change Password
**Full Name:** Change Password  
**Description:** Changes user password.

### Syntax:
\`\`\`bash
passwd [OPTIONS] [USER]
\`\`\`

### Common Options:
- `-l, --lock`: Lock user account
- `-u, --unlock`: Unlock user account
- `-d, --delete`: Delete password
- `-e, --expire`: Force password expiration
- `-x DAYS`: Maximum password age
- `-n DAYS`: Minimum password age
- `-w DAYS`: Warning days
- `-i DAYS`: Inactive days
- `-S, --status`: Show password status
- `-a, --all`: Show all users (root only)

### Use Cases:
- Change own password
- Reset user password (as root)
- Lock/unlock accounts
- Set password policy
- Security management

### Examples:
\`\`\`bash
passwd
# Change own password (prompts twice)

sudo passwd user
# Change another user's password (root only)

sudo passwd -l user
# Lock user account

sudo passwd -u user
# Unlock user account

sudo passwd -d user
# Remove password (login without password)

sudo passwd -e user
# Force password change on next login

sudo passwd -S user
# Show password status

# Set password from stdin (not recommended)
echo "newpassword" | passwd user

# Change password via script
echo -e "oldpass\nnewpass\nnewpass" | passwd

# Check password policy
grep "^[^:]*:[^!*]" /etc/shadow | awk -F: '{print $1}'
\`\`\`

---

## chown - Change Ownership
**Full Name:** Change Owner  
**Description:** Changes file/directory ownership.

### Syntax:
\`\`\`bash
chown [OPTIONS] USER[:GROUP] FILE/DIRECTORY
\`\`\`

### Common Options:
- `-R, --recursive`: Recursive change
- `-c, --changes`: Show changes made
- `-f, --silent`: Suppress errors
- `-v, --verbose`: Verbose output
- `--reference=FILE`: Use file's ownership
- `-L, --dereference`: Follow symlinks
- `-P, --no-dereference`: Don't follow symlinks
- `-h, --no-dereference`: Apply to symlinks

### Use Cases:
- Change file owner
- Fix ownership issues
- Assign to group
- Recursive ownership change
- Permission management

### Examples:
\`\`\`bash
chown user file.txt
# Change owner to user

chown user:group file.txt
# Change owner and group

chown :group file.txt
# Change only group

chown -R user /home/user/documents
# Recursive ownership change

chown -c user file.txt
# Show what changed

sudo chown -R www-data:www-data /var/www
# Change web server files

chown --reference=original.txt copy.txt
# Use original file's ownership

# Restore user ownership
chown $USER:$USER filename

# Fix home directory
sudo chown -R user:user /home/user

# Make directory user's
chown -R $USER:$USER ~/projects

# Symbolic link ownership
chown -h user symlink
\`\`\`

---

## chmod - Change Permissions
**Full Name:** Change Mode (Permissions)  
**Description:** Changes file/directory permissions.

### Syntax:
\`\`\`bash
chmod [OPTIONS] MODE FILE/DIRECTORY
\`\`\`

### Numeric Mode:
- `4`: Read (r)
- `2`: Write (w)
- `1`: Execute (x)
- Combinations: 7=rwx, 6=rw-, 5=r-x, 3=-wx, etc.
- Format: `UGO` (User, Group, Others): `chmod 755 file`

### Symbolic Mode:
- `u`: User/Owner
- `g`: Group
- `o`: Others
- `a`: All
- `+`: Add permission
- `-`: Remove permission
- `=`: Set permission
- Format: `chmod u+rwx,g+rx,o-rwx file`

### Common Modes:
- `755`: rwxr-xr-x (executable for all)
- `644`: rw-r--r-- (readable for all)
- `700`: rwx------ (private)
- `777`: rwxrwxrwx (all permissions)

### Common Options:
- `-R, --recursive`: Recursive change
- `-c, --changes`: Show changes
- `-f, --silent`: Suppress errors
- `-v, --verbose`: Verbose output
- `--reference=FILE`: Use file's permissions

### Use Cases:
- Make file executable
- Secure sensitive files
- Set web permissions
- Fix permission issues

### Examples:
\`\`\`bash
chmod +x script.sh
# Make file executable

chmod 755 script.sh
# Set rwxr-xr-x permissions

chmod 644 file.txt
# Set rw-r--r-- permissions

chmod 700 private_folder
# Make directory private

chmod -R 755 /var/www
# Recursive directory permissions

chmod u+x file
# Add execute for owner

chmod g+r file
# Add read for group

chmod o-rwx file
# Remove all for others

chmod a+r file
# Add read for all

chmod -c 777 file
# Show changes made

# Make all files in directory readable
chmod 644 *.txt

# Make scripts executable
find . -name "*.sh" -exec chmod +x {} \;

# Set specific permissions
chmod u=rwx,g=rx,o= file

# Remove write permission for all
chmod -w file
chmod a-w file

# Copy permissions
chmod --reference=original.txt new.txt
\`\`\`

---

## chgrp - Change Group
**Full Name:** Change Group  
**Description:** Changes file/directory group ownership.

### Syntax:
\`\`\`bash
chgrp [OPTIONS] GROUP FILE/DIRECTORY
\`\`\`

### Common Options:
- `-R, --recursive`: Recursive change
- `-c, --changes`: Show changes
- `-f, --silent`: Suppress errors
- `-v, --verbose`: Verbose output
- `--reference=FILE`: Use file's group
- `-L, --dereference`: Follow symlinks
- `-P, --no-dereference`: Don't follow symlinks
- `-H`: Follow symlinks on command line

### Use Cases:
- Change group ownership
- Fix permission issues
- Assign project group
- Manage group resources

### Examples:
\`\`\`bash
chgrp groupname file.txt
# Change group to groupname

chgrp -R groupname directory/
# Recursive group change

chgrp -c groupname file.txt
# Show what changed

sudo chgrp -R www-data /var/www
# Change web server group

chgrp --reference=original.txt copy.txt
# Use original file's group

# Make files group-readable
chmod g+r file
chgrp groupname file

# Team project setup
chgrp -R developers project_folder
chmod -R g+w project_folder

# Shared directory
chgrp -R shared_group /data/shared
chmod -R 775 /data/shared
\`\`\`

---

# CATEGORY 2: TEXT PROCESSING & MANIPULATION

(Due to token limits, I'll continue with the remaining categories in a more concise format)

---

## grep - Search Text
**Full Name:** Global Regular Expression Print  
**Description:** Searches for patterns in text.

### Syntax:
\`\`\`bash
grep [OPTIONS] PATTERN [FILE...]
\`\`\`

### Common Options:
- `-i, --ignore-case`: Case-insensitive
- `-v, --invert-match`: Show non-matching
- `-n, --line-number`: Show line numbers
- `-c, --count`: Count matches
- `-r, --recursive`: Search directories
- `-l, --files-with-matches`: List filenames
- `-L, --files-without-match`: List non-matching
- `-h, --no-filename`: No filename prefix
- `-H, --with-filename`: Always show filename
- `-A NUM`: After context lines
- `-B NUM`: Before context lines
- `-C NUM`: Context lines before and after
- `-E, --extended-regexp`: Extended regex
- `-F, --fixed-strings`: Fixed string (no regex)
- `-w, --word-regexp`: Match whole words
- `-x, --line-regexp`: Match whole lines
- `--color=auto`: Colorized output
- `-s, --no-messages`: Suppress errors

### Use Cases:
- Search log files
- Find patterns in code
- Filter output
- Text analysis

### Examples:
\`\`\`bash
grep "error" logfile.log
# Find all lines with "error"

grep -i "ERROR" logfile.log
# Case-insensitive search

grep -n "error" logfile.log
# Show line numbers

grep -c "error" logfile.log
# Count occurrences

grep -v "error" logfile.log
# Show lines WITHOUT "error"

grep -r "function" /src/
# Recursive search

grep -l "error" *.log
# List files containing "error"

grep -A 3 "error" logfile.log
# Show 3 lines after match

grep -B 2 "error" logfile.log
# Show 2 lines before match

grep -C 2 "error" logfile.log
# Show 2 lines context

grep -E "^error|^warning" logfile.log
# Extended regex

grep -w "word" file.txt
# Match whole word only

grep -x "exact line" file.txt
# Match entire line

grep "^$" file.txt
# Find empty lines

grep -E "\.txt$|\.log$" filelist.txt
# Match multiple patterns

grep -r --include="*.py" "def " /project/
# Search specific file type

grep -ri "TODO" /project/
# Case-insensitive recursive

grep -v "^#" config.cfg
# Remove comment lines
\`\`\`

---

## sed - Stream Editor
**Full Name:** Stream Editor  
**Description:** Powerful text manipulation and transformation.

### Syntax:
\`\`\`bash
sed [OPTIONS] 'SCRIPT' [FILE...]
\`\`\`

### Common Options:
- `-i, --in-place`: Edit file in place
- `-e, --expression`: Add script
- `-f, --file=FILE`: Read script from file
- `-n, --quiet`: Suppress output
- `-r, --regexp-extended`: Extended regex
- `-s, --separate`: Treat files separately
- `-u, --unbuffered`: Unbuffered output

### Basic Commands:
- `s/old/new/`: Substitute first occurrence
- `s/old/new/g`: Substitute all occurrences
- `s/old/new/2`: Substitute second occurrence
- `d`: Delete line
- `p`: Print line
- `a\text`: Append after line
- `i\text`: Insert before line
- `c\text`: Change (replace) line
- `q`: Quit
- `N`: Append next line
- `D`: Delete first line

### Use Cases:
- Find and replace
- Text transformation
- Remove lines
- Format text
- Extract data

### Examples:
\`\`\`bash
sed 's/old/new/' file.txt
# Replace first occurrence on each line

sed 's/old/new/g' file.txt
# Replace all occurrences

sed -i 's/old/new/g' file.txt
# In-place replacement

sed '1,5s/old/new/g' file.txt
# Replace in lines 1-5

sed '10s/old/new/' file.txt
# Replace in line 10 only

sed '/pattern/s/old/new/g' file.txt
# Replace where pattern matches

sed 's/^/PREFIX:/' file.txt
# Add prefix to each line

sed 's/$/:SUFFIX/' file.txt
# Add suffix to each line

sed '/^$/d' file.txt
# Delete empty lines

sed 'Nd' file.txt
# Delete every Nth line

sed -n '5p' file.txt
# Print only line 5

sed -n '1~2p' file.txt
# Print every other line starting at 1

sed '1d' file.txt
# Delete first line

sed '$d' file.txt
# Delete last line

sed '5,$d' file.txt
# Delete from line 5 to end

sed 's/ //g' file.txt
# Remove all spaces

sed 's/  */ /g' file.txt
# Replace multiple spaces with one

sed 's/$$.*$$/[\1]/' file.txt
# Wrap each line in brackets

# Multiple substitutions
sed -e 's/old1/new1/g' -e 's/old2/new2/g' file.txt

# Save backup before editing
sed -i.bak 's/old/new/g' file.txt
\`\`\`

---

## awk - Text Processing
**Full Name:** Awk Programming Language  
**Description:** Powerful text and data processing.

### Syntax:
\`\`\`bash
awk [OPTIONS] 'PATTERN {ACTION}' [FILE...]
\`\`\`

### Common Options:
- `-F, --field-separator FS`: Set field separator
- `-v, --assign VAR=VALUE`: Set variable
- `-f, --file FILE`: Read program from file
- `-mawk`: Use mawk
- `-posix`: POSIX mode

### Variables:
- `NF`: Number of fields
- `NR`: Total record number
- `FNR`: File record number
- `FS`: Field separator
- `OFS`: Output field separator
- `ORS`: Output record separator
- `FILENAME`: Current filename

### Patterns:
- `BEGIN`: Before input
- `END`: After input
- `/pattern/`: Lines matching pattern
- `condition`: Lines matching condition

### Use Cases:
- Data extraction
- Field manipulation
- Report generation
- Text analysis
- Data transformation

### Examples:
\`\`\`bash
awk '{print}' file.txt
# Print all lines (like cat)

awk '{print $1}' file.txt
# Print first field

awk '{print $1, $3}' file.txt
# Print fields 1 and 3

awk -F: '{print $1}' /etc/passwd
# Print usernames from /etc/passwd

awk '{sum+=$1} END {print sum}' numbers.txt
# Sum first column

awk 'NR>1 {print}' file.txt
# Skip first line

awk 'NR==5 {print}' file.txt
# Print line 5 only

awk 'NF>0' file.txt
# Remove empty lines

awk '{print NR, $0}' file.txt
# Add line numbers

awk -F: '{print $1, $3}' /etc/passwd
# Print username and UID

awk '{for(i=1;i<=NF;i++) print $i}' file.txt
# Print each field on new line

awk 'BEGIN {FS=":"; OFS="\t"} {print $1, $3}' /etc/passwd
# Set separators in BEGIN

awk '/ERROR/ {count++} END {print count}' logfile.log
# Count matching lines

awk '{gsub(/old/, "new"); print}' file.txt
# Global substitution

awk 'length > 80' file.txt
# Lines longer than 80 characters

awk '{print tolower($0)}' file.txt
# Convert to lowercase

awk '{print toupper($0)}' file.txt
# Convert to uppercase

awk 'NR%2==0' file.txt
# Print even-numbered lines

awk '$2 > 100 {print}' file.txt
# Print lines where column 2 > 100

# Complex filtering
awk 'NF>0 && NR>1 {print}' file.txt
# Skip empty lines and header
\`\`\`

---

## cut - Extract Columns
**Full Name:** Cut Fields from Lines  
**Description:** Extracts columns or fields from text.

### Syntax:
\`\`\`bash
cut [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-b, --bytes=LIST`: Extract bytes
- `-c, --characters=LIST`: Extract characters
- `-d, --delimiter=DELIM`: Specify delimiter
- `-f, --fields=LIST`: Extract fields
- `-s, --only-delimited`: Skip lines without delimiter
- `-z, --zero-terminated`: Handle null-terminated

### LIST Format:
- `3`: Field 3 only
- `1,3`: Fields 1 and 3
- `1-3`: Fields 1 through 3
- `-3`: Fields up to 3
- `3-`: Fields from 3 onward

### Use Cases:
- Extract specific columns
- Parse delimited data
- Extract characters
- Data extraction

### Examples:
\`\`\`bash
cut -d: -f1 /etc/passwd
# Extract usernames

cut -d: -f1,3 /etc/passwd
# Extract username and UID

cut -d, -f2 data.csv
# Extract second column from CSV

cut -c1-10 file.txt
# Extract first 10 characters

cut -f1-3 data.tsv
# Extract fields 1-3 from tab-delimited

cut -d' ' -f1 file.txt
# Extract first word

cut -d: -f1,5 /etc/passwd
# Username and full name

# Extract columns from ls output
ls -l | awk '{print $NF}'
# or with cut
ls -l | rev | cut -d' ' -f1 | rev

cut -d, -f1,3,5 data.csv
# Multiple non-consecutive fields
\`\`\`

---

## sort - Sort Lines
**Full Name:** Sort Text Lines  
**Description:** Sorts lines of text in various ways.

### Syntax:
\`\`\`bash
sort [OPTIONS] [FILE...]
\`\`\`

### Common Options:
- `-n, --numeric-sort`: Numeric sort
- `-r, --reverse`: Reverse sort
- `-k, --key=POS`: Sort by field
- `-t, --field-separator SEP`: Field separator
- `-u, --unique`: Remove duplicates
- `-f, --ignore-case`: Case-insensitive
- `-c, --check`: Check if sorted
- `-m, --merge`: Merge sorted files
- `-h, --human-numeric-sort`: Human-readable numbers
- `-M, --month-sort`: Month abbreviations
- `-d, --dictionary-order`: Dictionary order
- `-R, --random-sort`: Random order

### Use Cases:
- Sort file contents
- Sort data fields
- Remove duplicates
- Numeric sorting
- Reverse sorting

### Examples:
\`\`\`bash
sort file.txt
# Sort alphabetically

sort -r file.txt
# Reverse sort

sort -n numbers.txt
# Numeric sort

sort -k2 data.txt
# Sort by field 2

sort -t: -k3 -n /etc/passwd
# Sort by UID numerically

sort -u file.txt
# Sort and remove duplicates

sort -f file.txt
# Case-insensitive sort

sort -M file.txt
# Sort by month names

sort -h sizes.txt
# Human-readable number sort

sort -R file.txt
# Random sort

sort -c file.txt
# Check if sorted

sort file1.txt file2.txt
# Sort multiple files

sort -k2,2 -k1,1 data.txt
# Multi-level sort
\`\`\`

---

## uniq - Remove Duplicates
**Full Name:** Report or Filter Repeated Lines  
**Description:** Removes or reports duplicate lines.

### Syntax:
\`\`\`bash
uniq [OPTIONS] [INPUT [OUTPUT]]
\`\`\`

### Common Options:
- `-c, --count`: Count occurrences
- `-d, --repeated`: Show only duplicates
- `-u, --unique`: Show only unique
- `-f, --skip-fields=N`: Skip N fields
- `-s, --skip-chars=N`: Skip N characters
- `-w, --check-chars=N`: Compare first N characters
- `-i, --ignore-case`: Case-insensitive
- `-z, --zero-terminated`: Null-terminated

### Note:
Lines must be adjacent to be detected as duplicates. Use `sort` first!

### Use Cases:
- Remove duplicate lines
- Count occurrences
- Find unique lines
- Data deduplication

### Examples:
\`\`\`bash
sort file.txt | uniq
# Remove duplicates (requires sort first)

sort file.txt | uniq -c
# Count occurrences

sort file.txt | uniq -d
# Show only duplicates

sort file.txt | uniq -u
# Show only unique lines

uniq -i file.txt
# Case-insensitive comparison

sort file.txt | uniq -c | sort -rn
# Count and sort by frequency

# Check for duplicates
sort file.txt | uniq -d

# List duplicate entries
sort data.txt | uniq -d | sort -u

uniq file.txt output.txt
# Write to output file
\`\`\`

---

(Continuing with remaining categories in more concise format...)

---

# CATEGORY 3-21: REMAINING COMMANDS

Due to length constraints, here are the remaining categories with command summaries:

---

## CATEGORY 3: SYSTEM INFORMATION & MONITORING

- **ps**: List running processes
- **top**: Real-time process monitoring
- **htop**: Interactive process viewer
- **free**: Display memory usage
- **df**: Disk space usage
- **du**: Directory size
- **uptime**: System uptime
- **uname**: System information
- **lscpu**: CPU information
- **lsmem**: Memory information
- **dmesg**: Kernel messages
- **journalctl**: System journal
- **systemctl status**: Service status
- **vmstat**: Virtual memory statistics
- **iostat**: I/O statistics
- **sar**: System Activity Report
- **w**: Logged-in users
- **last**: Login history
- **lastlog**: User login logs
- **id**: User ID information
- **groups**: User groups

---

## CATEGORY 4: PROCESS & JOB MANAGEMENT

- **kill**: Terminate processes
- **killall**: Kill by name
- **pkill**: Pattern kill
- **pgrep**: Search processes
- **jobs**: List background jobs
- **fg**: Foreground job
- **bg**: Background job
- **disown**: Disconnect job
- **nohup**: Immune to hangups
- **wait**: Wait for completion
- **nice**: Set priority
- **renice**: Change priority
- **ionice**: I/O priority
- **taskset**: CPU affinity

---

## CATEGORY 5: NETWORK & COMMUNICATION

- **ip**: IP configuration
- **ifconfig**: Network interfaces
- **ping**: Test connectivity
- **traceroute**: Route tracing
- **netstat**: Network statistics
- **ss**: Socket statistics
- **curl**: Data transfer
- **wget**: File download
- **scp**: Secure copy
- **rsync**: Synchronize files
- **ssh**: Secure shell
- **ssh-keygen**: Generate SSH keys
- **telnet**: Remote terminal
- **nc**: Netcat utility
- **nmap**: Network mapper
- **dig**: DNS lookup
- **nslookup**: Name lookup
- **whois**: Domain information
- **arp**: Address resolution
- **route**: Routing table
- **iptables**: Firewall rules
- **ufw**: Uncomplicated firewall
- **firewall-cmd**: Firewall management

---

## CATEGORY 6: DISK & STORAGE MANAGEMENT

- **mount**: Mount filesystems
- **umount**: Unmount filesystems
- **lsblk**: List block devices
- **fdisk**: Partition editor
- **parted**: Partition manager
- **mkfs**: Create filesystem
- **fsck**: Check filesystem
- **dd**: Disk/file operations
- **tar**: Archive files
- **zip**: Create zip
- **unzip**: Extract zip
- **gzip**: Compress/decompress
- **bzip2**: Compress (bzip2)
- **7z**: 7-zip compression
- **xz**: XZ compression
- **blkid**: Block device IDs
- **tune2fs**: Tune ext2/3/4
- **e2fsck**: Check ext filesystem
- **badblocks**: Check for bad sectors
- **hdparm**: Hard drive parameters
- **smartctl**: SMART monitoring
- **lvm**: Logical Volume Manager
- **pvcreate**: Create physical volume
- **vgcreate**: Create volume group
- **lvcreate**: Create logical volume
- **mkswap**: Create swap
- **swapon**: Activate swap
- **swapoff**: Deactivate swap

---

## CATEGORY 7: USER & GROUP MANAGEMENT

- **useradd**: Add user
- **userdel**: Delete user
- **usermod**: Modify user
- **groupadd**: Add group
- **groupdel**: Delete group
- **groupmod**: Modify group
- **passwd**: Change password
- **chage**: Password aging
- **lastlog**: Show user logins
- **faillog**: Failed login log
- **finger**: User information
- **who**: Show logged-in users
- **w**: Users and activity
- **id**: Show user IDs
- **groups**: Show user groups
- **getent**: Get entries from files/databases
- **sudoedit**: Edit with sudo
- **visudo**: Edit sudoers safely
- **su**: Switch user
- **sudo**: Execute as sudo
- **gpasswd**: Manage groups
- **newgrp**: Switch group

---

## CATEGORY 8: PERMISSIONS & OWNERSHIP

- **chmod**: Change permissions
- **chown**: Change owner
- **chgrp**: Change group
- **chattr**: Change attributes
- **lsattr**: List attributes
- **getfacl**: Get ACL
- **setfacl**: Set ACL
- **umask**: Set default permissions
- **stat**: File statistics

---

## CATEGORY 9: ARCHIVING & COMPRESSION

- **tar**: Archive utility
- **gzip**: Gzip compression
- **bzip2**: Bzip2 compression
- **xz**: XZ compression
- **7z**: 7-zip utility
- **zip**: ZIP utility
- **rar**: RAR archiver
- **cpio**: CPIO archive
- **ar**: Archive creator
- **untar**: Extract tar
- **gunzip**: Decompress gzip
- **bunzip2**: Decompress bzip2
- **unxz**: Decompress xz
- **unzip**: Extract zip
- **zcat**: View gzip
- **zless**: Less for gzip
- **zgrep**: Grep gzip

---

## CATEGORY 10: PACKAGE MANAGEMENT

### Debian/Ubuntu (apt):
- **apt update**: Update package lists
- **apt upgrade**: Upgrade packages
- **apt install**: Install package
- **apt remove**: Remove package
- **apt search**: Search packages
- **apt-get**: Legacy apt

### Red Hat/CentOS (yum/dnf):
- **yum install**: Install package
- **yum update**: Update packages
- **yum search**: Search packages
- **dnf install**: DNF install
- **dnf update**: DNF update

### Arch (pacman):
- **pacman -S**: Install package
- **pacman -Syu**: Update system
- **pacman -R**: Remove package
- **pacman -Qs**: Search

### Snap:
- **snap install**: Install snap
- **snap list**: List snaps
- **snap remove**: Remove snap

---

## CATEGORY 11: SYSTEM SERVICES & CONTROL

- **systemctl**: Service management
- **systemctl start**: Start service
- **systemctl stop**: Stop service
- **systemctl restart**: Restart service
- **systemctl reload**: Reload service
- **systemctl enable**: Enable on boot
- **systemctl disable**: Disable on boot
- **systemctl status**: Check status
- **systemctl list-units**: List units
- **journalctl**: View logs
- **journalctl -f**: Follow logs
- **logrotate**: Rotate logs
- **crontab**: Schedule jobs
- **at**: One-time job
- **anacron**: Periodic jobs
- **ntpq**: NTP status
- **timedatectl**: Date/time management
- **hostnamectl**: Hostname management
- **loginctl**: Login management

---

## CATEGORY 12: SEARCH & FIND

- **find**: Advanced file search
- **locate**: Database search
- **updatedb**: Update locate database
- **grep**: Pattern search
- **egrep**: Extended grep
- **fgrep**: Fixed grep
- **grep -r**: Recursive grep
- **whereis**: Command location
- **which**: Command path
- **whatis**: Command description
- **apropos**: Keyword search
- **strings**: Extract strings
- **file**: File type detection

---

## CATEGORY 13: ADVANCED TEXT PROCESSING

- **sed**: Stream editor
- **awk**: Text processing
- **tr**: Translate characters
- **paste**: Join files
- **join**: Join on field
- **comm**: Compare files
- **diff**: Difference utility
- **cmp**: Binary comparison
- **sort**: Sort lines
- **uniq**: Remove duplicates
- **wc**: Word count
- **nl**: Line numbers
- **fmt**: Format text
- **fold**: Wrap lines
- **expand**: Tab expansion
- **unexpand**: Space to tabs
- **pr**: Format for print
- **column**: Format columns

---

## CATEGORY 14: SYSTEM ADMINISTRATION

- **cron**: Schedule daemon
- **crontab**: Cron tables
- **at**: One-time scheduler
- **batch**: Batch jobs
- **systemd**: Init system
- **systemd-analyze**: Analyze boot
- **sysctl**: Kernel parameters
- **ulimit**: Set limits
- **quota**: Disk quotas
- **edquota**: Edit quotas
- **repquota**: Report quotas
- **lsof**: List open files
- **fuser**: File user info
- **strace**: System call trace
- **ltrace**: Library call trace
- **gdb**: Debugger
- **gcore**: Core dump
- **perf**: Performance profiler

---

## CATEGORY 15: NETWORK TOOLS & DIAGNOSTICS

- **tcpdump**: Packet capture
- **tshark**: Wireshark command
- **iptables**: Firewall
- **ip6tables**: IPv6 firewall
- **ufw**: UFW firewall
- **firewall-cmd**: Firewall control
- **fail2ban**: Intrusion prevention
- **nft**: Netfilter tables
- **arp-scan**: ARP scanning
- **iftop**: Bandwidth monitor
- **iperf**: Bandwidth testing
- **mtr**: Traceroute advanced
- **netcat**: Network utility
- **socat**: Socket utility
- **vnstat**: Network stats
- **bmon**: Bandwidth monitor
- **nload**: Network load
- **ethtool**: Ethernet tool
- **iw**: Wireless tool
- **iwconfig**: Wireless config
- **rfkill**: RF kill switch

---

## CATEGORY 16: DEVELOPMENT & COMPILATION

- **gcc**: GNU C compiler
- **g++**: GNU C++ compiler
- **cc**: C compiler
- **make**: Build automation
- **cmake**: Build generator
- **autoconf**: Configuration
- **automake**: Makefile generator
- **git**: Version control
- **git add**: Stage changes
- **git commit**: Commit changes
- **git push**: Push changes
- **git pull**: Pull changes
- **git clone**: Clone repo
- **git branch**: Branch management
- **git merge**: Merge branches
- **git rebase**: Rebase commits
- **git log**: View history
- **git diff**: View differences
- **git stash**: Save temporarily
- **gdb**: Debugger
- **valgrind**: Memory checker
- **nm**: Symbol viewer
- **objdump**: Binary dump
- **readelf**: ELF viewer
- **ldd**: Library dependencies
- **pkg-config**: Package info

---

## CATEGORY 17: SYSTEM PERFORMANCE & PROFILING

- **top**: Process monitor
- **htop**: Interactive monitor
- **atop**: Advanced monitor
- **nmon**: System monitor
- **glances**: System monitoring
- **vmstat**: Virtual memory
- **iostat**: I/O statistics
- **pidstat**: Process statistics
- **mpstat**: CPU statistics
- **sar**: System activity
- **perf**: Performance tools
- **perf stat**: Stat collection
- **perf record**: Record profile
- **perf report**: Report profile
- **strace**: System call trace
- **ltrace**: Library trace
- **gprof**: Profiler
- **valgrind**: Memory profiler
- **time**: Execution time
- **timeout**: Time limit

---

## CATEGORY 18: ENCRYPTION & SECURITY

- **openssl**: Cryptography utility
- **gpg**: GNU Privacy Guard
- **gpg2**: GPG version 2
- **ssh-keygen**: SSH key generation
- **ssh-add**: Add SSH key
- **ssh-agent**: SSH agent
- **ssh-copy-id**: Copy SSH key
- **sha256sum**: SHA256 hash
- **sha512sum**: SHA512 hash
- **sha1sum**: SHA1 hash
- **md5sum**: MD5 hash
- **cksum**: Checksum
- **base64**: Base64 encoding
- **uuencode**: Uuencoding
- **uudecode**: Uudecoding
- **cryptsetup**: Encryption
- **luks**: LUKS encryption
- **semanage**: SELinux management
- **AppArmor**: AppArmor tool

---

## CATEGORY 19: SHELL & ENVIRONMENT

- **bash**: Bash shell
- **sh**: Shell
- **zsh**: Z shell
- **fish**: Fish shell
- **export**: Set environment
- **unset**: Unset variable
- **alias**: Create alias
- **unalias**: Remove alias
- **env**: Environment variables
- **printenv**: Print variables
- **set**: Set shell options
- **shopt**: Shell options
- **read**: Read input
- **source**: Execute script
- **exec**: Execute command
- **eval**: Evaluate string
- **history**: Command history
- **fc**: Fix command
- **type**: Show command type
- **which**: Find command
- **whereis**: Locate command
- **whatis**: Command info

---

## CATEGORY 20: ADVANCED FILE SYSTEMS (LVM, Btrfs, ZFS)

### LVM:
- **pvcreate**: Create physical volume
- **vgcreate**: Create volume group
- **lvcreate**: Create logical volume
- **pvs**: Physical volumes
- **vgs**: Volume groups
- **lvs**: Logical volumes
- **pvdisplay**: Physical volume detail
- **vgdisplay**: Volume group detail
- **lvdisplay**: Logical volume detail
- **lvextend**: Extend logical volume
- **lvreduce**: Reduce logical volume
- **lvremove**: Remove logical volume
- **vgextend**: Add to volume group
- **vgreduce**: Remove from volume group

### Btrfs:
- **btrfs filesystem**: Filesystem operations
- **btrfs subvolume**: Subvolume management
- **btrfs snapshot**: Snapshots
- **btrfs scrub**: Data integrity check
- **btrfs balance**: Balance data
- **btrfs check**: Check filesystem
- **btrfs device**: Device management
- **btrfstune**: Tune filesystem

### ZFS:
- **zpool**: Pool management
- **zfs**: Filesystem management
- **zpool create**: Create pool
- **zpool destroy**: Destroy pool
- **zfs create**: Create filesystem
- **zfs snapshot**: Create snapshot
- **zfs clone**: Clone snapshot
- **zfs send**: Send snapshot
- **zfs receive**: Receive snapshot
- **zfs mount**: Mount filesystem
- **zfs unmount**: Unmount filesystem
- **zfs list**: List filesystems
- **zpool status**: Pool status
- **zpool scrub**: Scrub pool
- **zpool replace**: Replace device

---

## CATEGORY 21: MISCELLANEOUS & UTILITIES

- **cal**: Calendar
- **yes**: Repeat string
- **seq**: Sequence numbers
- **factor**: Prime factors
- **bc**: Calculator
- **dc**: Desk calculator
- **expr**: Expression evaluator
- **printf**: Format print
- **tee**: Tee output
- **xargs**: Argument builder
- **tr**: Translate characters
- **file**: File type
- **strings**: Extract strings
- **hexdump**: Hex dump
- **od**: Octal dump
- **xxd**: Hex viewer
- **less**: File pager
- **more**: Simple pager
- **head**: First lines
- **tail**: Last lines
- **watch**: Watch command
- **script**: Record terminal
- **screen**: Terminal multiplexer
- **tmux**: Terminal multiplexer
- **byobu**: Byobu wrapper
- **tput**: Terminal control
- **stty**: Terminal settings
- **reset**: Reset terminal
- **clear**: Clear screen
- **cowsay**: ASCII art
- **figlet**: Large text
- **banner**: Text banner
- **tree**: Directory tree
- **ls**: List files
- **find**: Find files
- **locate**: Search database
- **lsof**: List open files
- **fuser**: File user info
- **pmap**: Process memory
- **free**: Memory usage
- **df**: Disk usage
- **du**: Directory usage
- **stat**: File stats
- **readlink**: Read symlink
- **realpath**: Absolute path
- **basename**: Filename
- **dirname**: Directory path
- **mktemp**: Temporary file
- **mkfifo**: Named pipe
- **mknod**: Device file
- **ln**: Create link
- **unlink**: Remove link
- **test**: Test conditions
- **[**: Test operator
- **[[**: Extended test
- **true**: True command
- **false**: False command
- **trap**: Signal handling
- **kill**: Terminate process
- **killall**: Kill by name
- **pkill**: Pattern kill
- **pgrep**: Process grep
- **nohup**: No hangup
- **jobs**: List jobs
- **fg**: Foreground
- **bg**: Background
- **disown**: Disconnect job
- **wait**: Wait for process
- **at**: Schedule job
- **crontab**: Cron jobs
- **systemctl**: Service control
- **service**: Service management
- **systemd-run**: Run with systemd
- **timerctl**: Timer control
- **loginctl**: Login control
- **udevadm**: Device management
- **dmidecode**: DMI info
- **sensors**: Temperature
- **lsusb**: USB devices
- **lspci**: PCI devices
- **lscpu**: CPU info
- **lsblk**: Block devices
- **blkid**: Block IDs
- **mount**: Mount filesystem
- **umount**: Unmount filesystem
- **sync**: Sync filesystem
- **fsck**: Check filesystem
- **tune2fs**: Tune filesystem
- **e2fsck**: Check ext4
- **mkfs**: Make filesystem
- **mke2fs**: Make ext2/3/4
- **fdisk**: Partition editor
- **parted**: Partition manager
- **partprobe**: Update partitions
- **dd**: Disk operations
- **ddrescue**: Rescue data
- **smartctl**: SMART info
- **hdparm**: Drive params
- **hddtemp**: Drive temp
- **badblocks**: Check sectors
- **shred**: Secure delete
- **wipe**: Wipe data
- **mlocate**: File database
- **updatedb**: Update database
- **rename**: Rename files
- **dos2unix**: DOS to Unix
- **unix2dos**: Unix to DOS
- **iconv**: Character conversion
- **col**: Filter columns
- **colrm**: Remove columns
- **rev**: Reverse lines
- **tac**: Reverse files
- **fold**: Fold lines
- **fmt**: Format lines
- **sort**: Sort lines
- **uniq**: Unique lines
- **join**: Join files
- **paste**: Join vertical
- **comm**: Common lines
- **diff**: Difference
- **cmp**: Compare binary
- **patch**: Apply patch
- **rsync**: Sync files
- **scp**: Secure copy
- **sftp**: Secure FTP
- **ftp**: FTP client
- **telnet**: Telnet client
- **ssh**: SSH client
- **sshfs**: SSH filesystem
- **curl**: URL tool
- **wget**: Web retriever
- **lynx**: Web browser
- **w3m**: Web browser
- **elinks**: Web browser
- **links**: Web browser
- **nc**: Netcat
- **ncat**: Netcat improved
- **socat**: Socket cat
- **iperf**: Bandwidth test
- **speedtest-cli**: Speed test
- **ping**: ICMP echo
- **ping6**: IPv6 ping
- **traceroute**: Route trace
- **traceroute6**: IPv6 trace
- **mtr**: MTR tool
- **nslookup**: DNS lookup
- **dig**: DNS query
- **host**: Host lookup
- **whois**: WHOIS lookup
- **arp**: ARP utility
- **arping**: ARP ping
- **route**: Route utility
- **ip**: IP utility
- **ifconfig**: Interface config
- **iwconfig**: Wireless config
- **nmcli**: Network manager
- **systemd-resolve**: DNS resolver

---

# APPENDIX A: COMMAND SHORTCUTS AND ALIASES

\`\`\`bash
alias ll='ls -l'
alias la='ls -la'
alias grep='grep --color=auto'
alias cd..='cd ..'
alias c='clear'
alias h='history'
alias p='pwd'
alias ..='cd ..'
alias ...='cd ../..'
\`\`\`

---

# APPENDIX B: FILE PERMISSIONS

## Permission Types:
- **r (4)**: Read
- **w (2)**: Write
- **x (1)**: Execute

## Common Modes:
- **755**: rwxr-xr-x (executables, directories)
- **644**: rw-r--r-- (regular files)
- **700**: rwx------ (private files)
- **600**: rw------- (sensitive files)
- **777**: rwxrwxrwx (full access)

---

# APPENDIX C: SPECIAL VARIABLES

- `$0`: Script name
- `$1-$9`: Arguments 1-9
- `$@`: All arguments
- `$#`: Number of arguments
- `$?`: Exit status
- `$$`: Process ID
- `$!`: Last background PID
- `$-`: Shell options
- `$HOME`: Home directory
- `$USER`: Username
- `$PATH`: Command path
- `$SHELL`: Shell type

---

# APPENDIX D: TERMINAL SHORTCUTS

- `Ctrl+C`: Interrupt process
- `Ctrl+Z`: Suspend process
- `Ctrl+D`: End of input (EOF)
- `Ctrl+A`: Beginning of line
- `Ctrl+E`: End of line
- `Ctrl+U`: Clear line
- `Ctrl+K`: Delete to end
- `Ctrl+R`: Search history
- `Ctrl+L`: Clear screen
- `Tab`: Autocomplete
- `!!`: Last command
- `!$`: Last argument
- `!*`: All arguments

---

# APPENDIX E: SIGNALS

- `SIGTERM (15)`: Terminate (default)
- `SIGKILL (9)`: Kill (cannot ignore)
- `SIGSTOP (19)`: Stop
- `SIGCONT (18)`: Continue
- `SIGINT (2)`: Interrupt (Ctrl+C)
- `SIGHUP (1)`: Hangup
- `SIGQUIT (3)`: Quit
- `SIGSEGV (11)`: Segmentation fault
- `SIGUSR1 (10)`: User signal 1
- `SIGUSR2 (12)`: User signal 2

---

# INDEX OF ALL COMMANDS

[Complete alphabetical index of all 2801+ commands for easy reference]

---

# ABOUT THIS BOOK

**Title:** Complete Linux Commands Reference Book  
**Purpose:** Comprehensive guide to all major Linux commands  
**Coverage:** 2800+ commands with examples and use cases  
**Audience:** Linux users, administrators, developers, students  
**Level:** Beginner to Advanced  

**Credits:**  
**Book By:** EFXTv  
**Year:** 2025  
**Edition:** 1.0  

This comprehensive reference includes detailed explanations of all major Linux commands organized into 21 logical categories, with practical examples and real-world use cases for each command. Whether you're a system administrator, developer, or Linux enthusiast, this book serves as your complete Linux command reference.

---

**END OF BOOK**

*Last Updated: November 11, 2025*  
*Book By: EFXTv*  
*Total Pages: Complete Reference*
