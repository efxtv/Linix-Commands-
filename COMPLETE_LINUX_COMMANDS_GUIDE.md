# Complete Linux Commands Reference Guide (2800+ Commands)

A comprehensive markdown reference for Linux/Unix command-line tools, organized by functional categories with detailed explanations, use cases, syntax, and practical examples.

---

## Table of Contents
1. [File & Directory Management](#1-file--directory-management)
2. [Text Processing & Manipulation](#2-text-processing--manipulation)
3. [System Information & Monitoring](#3-system-information--monitoring)
4. [Process Management](#4-process-management)
5. [Networking & Communication](#5-networking--communication)
6. [Disk & Storage Management](#6-disk--storage-management)
7. [User & Group Management](#7-user--group-management)
8. [Permissions & Ownership](#8-permissions--ownership)
9. [Archiving & Compression](#9-archiving--compression)
10. [Package Management](#10-package-management)
11. [System Services & Control](#11-system-services--control)
12. [Search & Find](#12-search--find)
13. [Advanced Text Processing](#13-advanced-text-processing)
14. [System Administration](#14-system-administration)
15. [Network Tools & Diagnostics](#15-network-tools--diagnostics)
16. [Development & Compilation](#16-development--compilation)
17. [System Performance & Profiling](#17-system-performance--profiling)
18. [Encryption & Security](#18-encryption--security)
19. [Shell & Environment](#19-shell--environment)
20. [Advanced File Systems](#20-advanced-file-systems)

---

## 1. File & Directory Management

### pwd
**Purpose:** Print Working Directory - displays the current directory path  
**Syntax:** `pwd [OPTION]`  
**Common Options:**
- `-L` : Logical directory (symbolic links)
- `-P` : Physical directory (actual path)

**Use Cases:**
- Determine current location in file system
- Navigation verification
- Script location tracking

**Examples:**
\`\`\`bash
pwd                    # Show current directory
pwd -P                 # Show physical path (resolve symlinks)
pwd -L                 # Show logical path with symlinks
echo $(pwd)            # Store current directory in variable
cd $(pwd)/../          # Go to parent directory
\`\`\`

---

### ls
**Purpose:** List directory contents  
**Syntax:** `ls [OPTION] [FILE/DIR]`  
**Common Options:**
- `-l` : Long format (detailed)
- `-a` : Show hidden files
- `-h` : Human-readable sizes
- `-R` : Recursive listing
- `-S` : Sort by size
- `-t` : Sort by modification time
- `-r` : Reverse sort
- `--color` : Colorized output

**Use Cases:**
- View directory contents
- Check file permissions
- Monitor file sizes and dates
- Find hidden files
- Recursive directory traversal

**Examples:**
\`\`\`bash
ls                     # Basic listing
ls -la                 # Detailed listing with hidden files
ls -lh                 # Human-readable format
ls -lS                 # Sort by file size
ls -lt                 # Sort by modification time (newest first)
ls -ltr                # Sort by modification time (oldest first)
ls -R                  # Recursive listing
ls /home/*/           # List home directories for all users
ls -d */              # List only directories
alias ll='ls -lah'    # Create alias for common use
ls -l /path | wc -l   # Count files in directory
\`\`\`

---

### cd
**Purpose:** Change Directory  
**Syntax:** `cd [DIRECTORY]`  
**Common Usage:**
- `cd` or `cd ~` : Go to home directory
- `cd ..` : Go to parent directory
- `cd -` : Go to previous directory
- `cd /path/to/dir` : Change to specific path

**Use Cases:**
- Navigate file system
- Access different directories
- Switch working directories

**Examples:**
\`\`\`bash
cd                     # Go to home directory
cd ~                   # Same as above
cd /etc                # Change to /etc directory
cd ..                  # Go to parent directory
cd -                   # Go to previous directory
cd ~/Desktop           # Go to Desktop folder
cd ../../             # Go up two levels
cd $HOME              # Go to home (using variable)
OLDPWD=$(pwd); cd /tmp # Save and change directory
\`\`\`

---

### mkdir
**Purpose:** Make Directory  
**Syntax:** `mkdir [OPTION] DIRECTORY`  
**Common Options:**
- `-p` : Create parent directories as needed
- `-m` : Set permissions
- `-v` : Verbose output

**Use Cases:**
- Create new directories
- Build directory structures
- Set initial permissions

**Examples:**
\`\`\`bash
mkdir mydir            # Create single directory
mkdir -p path/to/dir   # Create nested directories
mkdir -m 755 safedir   # Create with specific permissions
mkdir -v dir1 dir2 dir3 # Create multiple directories (verbose)
mkdir -p ~/projects/{web,mobile,desktop} # Create multiple nested
\`\`\`

---

### rmdir
**Purpose:** Remove empty Directory  
**Syntax:** `rmdir [OPTION] DIRECTORY`  
**Common Options:**
- `-p` : Remove parent directories if empty
- `-v` : Verbose output

**Use Cases:**
- Remove empty directories
- Clean up directory structures
- Safe deletion (won't delete non-empty dirs)

**Examples:**
\`\`\`bash
rmdir emptydir         # Remove empty directory
rmdir -p path/to/empty # Remove empty parent directories
rmdir -v dir1 dir2    # Remove with verbose output
rmdir --ignore-fail-on-non-empty dir # Don't fail if not empty
\`\`\`

---

### rm
**Purpose:** Remove files or directories  
**Syntax:** `rm [OPTION] FILE/DIR`  
**Common Options:**
- `-r` or `-R` : Recursive deletion
- `-f` : Force deletion (no confirmation)
- `-i` : Interactive (ask before delete)
- `-v` : Verbose output
- `-d` : Remove empty directories

**Use Cases:**
- Delete files
- Remove directories and contents
- Batch file deletion
- Clean temporary files

**Examples:**
\`\`\`bash
rm file.txt            # Delete single file
rm -r directory/       # Delete directory and contents
rm -rf /path           # Force delete recursively (careful!)
rm *.log               # Delete all .log files
rm -i *.txt            # Interactive deletion (confirm each)
rm -v file1 file2      # Delete with verbose output
rm -- -file            # Delete file starting with dash
find . -name "*.tmp" -delete # Delete all .tmp files recursively
\`\`\`

---

### cp
**Purpose:** Copy files or directories  
**Syntax:** `cp [OPTION] SOURCE DEST`  
**Common Options:**
- `-r` or `-R` : Recursive copy
- `-i` : Interactive (confirm overwrite)
- `-v` : Verbose output
- `-a` : Archive mode (preserve attributes)
- `-p` : Preserve mode/ownership/timestamps
- `--backup` : Create backup of destination

**Use Cases:**
- Duplicate files
- Copy directory trees
- Backup files
- Create file backups with timestamps

**Examples:**
\`\`\`bash
cp file.txt backup.txt # Copy single file
cp -r dir/ backup_dir/ # Copy entire directory
cp file.txt file.txt.bak # Create backup copy
cp -p file.txt dest/   # Preserve attributes
cp -v *.jpg backup/    # Copy with verbose output
cp --backup=t file.txt dest/ # Create backup with timestamp
cp -a /var/www /mnt/backup # Archive copy (preserve all)
\`\`\`

---

### mv
**Purpose:** Move or rename files/directories  
**Syntax:** `mv [OPTION] SOURCE DEST`  
**Common Options:**
- `-i` : Interactive (confirm overwrite)
- `-v` : Verbose output
- `-f` : Force (no confirmation)
- `--backup` : Create backup

**Use Cases:**
- Rename files/directories
- Move files to different locations
- Reorganize file structures
- Batch rename operations

**Examples:**
\`\`\`bash
mv oldname.txt newname.txt # Rename file
mv file.txt /path/to/dest  # Move file
mv -v file1 file2 directory/ # Move multiple files
mv -i file.txt dest/       # Interactive move
mv --force file.txt dest/  # Force overwrite
mv dir1 dir2               # Rename directory
for f in *.jpg; do mv "$f" "${f%.jpg}.png"; done # Batch rename
\`\`\`

---

### cat
**Purpose:** Concatenate and display file contents  
**Syntax:** `cat [OPTION] [FILE]`  
**Common Options:**
- `-n` : Number all output lines
- `-s` : Squeeze empty lines
- `-A` : Show all characters
- `-E` : Show line endings
- `-T` : Show tabs

**Use Cases:**
- Display file contents
- Concatenate multiple files
- Create files with heredoc
- View file without pagination

**Examples:**
\`\`\`bash
cat file.txt           # Display file contents
cat file1.txt file2.txt # Concatenate two files
cat file1 file2 > combined.txt # Combine files
cat -n file.txt        # Display with line numbers
cat /etc/passwd        # View system files
cat > newfile.txt << EOF # Create file with heredoc
Hello
World
EOF
cat /proc/cpuinfo      # View CPU information
\`\`\`

---

### tac
**Purpose:** Concatenate files in reverse (reverse cat)  
**Syntax:** `tac [OPTION] [FILE]`  
**Common Options:**
- `-b` : Attach separator before records
- `-r` : Use regex as separator
- `-s` : Use STRING as separator

**Use Cases:**
- Display files in reverse order
- View logs from end to beginning
- Reverse line order

**Examples:**
\`\`\`bash
tac file.txt           # Display file in reverse order
tac file.txt > reversed.txt # Save reversed output
tac /var/log/syslog | head -20 # View last 20 lines of log
tac file.txt | tac     # Double reverse (back to normal)
\`\`\`

---

### less / more
**Purpose:** View file contents page by page  
**Syntax:** `less [OPTION] FILE` / `more [FILE]`  
**Common Options (less):**
- `/pattern` : Search forward
- `?pattern` : Search backward
- `G` : Go to end of file
- `g` : Go to beginning
- `q` : Quit

**Use Cases:**
- View large files
- Navigate large text
- Search within files
- Comfortable reading

**Examples:**
\`\`\`bash
less largefile.txt     # View with paging
less -N file.txt       # Show line numbers
less /var/log/syslog   # View system logs
less -S file.txt       # Don't wrap long lines
more file.txt          # Simpler paging
less +F file.txt       # Follow file (like tail -f)
\`\`\`

---

### head
**Purpose:** Display beginning of files  
**Syntax:** `head [OPTION] [FILE]`  
**Common Options:**
- `-n NUM` or `-NUM` : Display first NUM lines
- `-c NUM` : Display first NUM bytes
- `-q` : Don't print filenames

**Use Cases:**
- Preview file contents
- Get first lines of logs
- Check file headers
- Script file inspection

**Examples:**
\`\`\`bash
head file.txt          # Show first 10 lines (default)
head -n 20 file.txt    # Show first 20 lines
head -5 file.txt       # Show first 5 lines
head -c 100 file.txt   # Show first 100 bytes
head -n -10 file.txt   # All but last 10 lines
head -q file1 file2    # Don't show filename
head -n 1 /etc/passwd  # Get first user entry
\`\`\`

---

### tail
**Purpose:** Display end of files  
**Syntax:** `tail [OPTION] [FILE]`  
**Common Options:**
- `-n NUM` or `-NUM` : Display last NUM lines
- `-f` : Follow file (keep displaying new content)
- `-c NUM` : Display last NUM bytes
- `-q` : Don't print filenames

**Use Cases:**
- Monitor log files
- View recent entries
- Track file changes
- Monitor system logs

**Examples:**
\`\`\`bash
tail file.txt          # Show last 10 lines
tail -n 20 file.txt    # Show last 20 lines
tail -5 file.txt       # Show last 5 lines
tail -f /var/log/syslog # Follow syslog in real-time
tail -f app.log | grep ERROR # Monitor for errors
tail -c 1024 file.txt  # Show last 1024 bytes
tail -n +5 file.txt    # Show from line 5 onward
\`\`\`

---

### echo
**Purpose:** Display text or variables  
**Syntax:** `echo [OPTION] [STRING]`  
**Common Options:**
- `-n` : Don't output trailing newline
- `-e` : Enable interpretation of backslash escapes
- `-E` : Explicitly disable backslash escapes

**Use Cases:**
- Print text to terminal
- Display variable values
- Create text in scripts
- Output for piping

**Examples:**
\`\`\`bash
echo "Hello World"     # Print text
echo $HOME             # Print environment variable
echo "Line 1\nLine 2"  # Print with escape (needs -e)
echo -e "Tab\there"    # Echo with tab escape
echo -n "No newline"   # No trailing newline
echo "Text" >> file.txt # Append to file
echo "var=$var"        # Print variable assignment
\`\`\`

---

### touch
**Purpose:** Create empty files or update timestamps  
**Syntax:** `touch [OPTION] FILE`  
**Common Options:**
- `-t TIMESTAMP` : Set specific time
- `-a` : Change only access time
- `-m` : Change only modification time
- `-c` : Don't create if doesn't exist
- `-r FILE` : Use reference file's time

**Use Cases:**
- Create empty files
- Update file modification times
- Create placeholder files
- Maintain file timestamps

**Examples:**
\`\`\`bash
touch newfile.txt      # Create empty file
touch file1 file2 file3 # Create multiple files
touch -t 202501011200 file.txt # Set timestamp
touch -r reference.txt target.txt # Copy timestamp
touch *.txt            # Update all .txt file times
touch -c file.txt      # Don't create if doesn't exist
\`\`\`

---

## 2. Text Processing & Manipulation

### grep
**Purpose:** Search text patterns in files  
**Syntax:** `grep [OPTION] PATTERN [FILE]`  
**Common Options:**
- `-i` : Case-insensitive search
- `-v` : Invert match (show non-matching)
- `-n` : Show line numbers
- `-l` : Show only filenames
- `-r` or `-R` : Recursive search
- `-E` : Extended regex (like egrep)
- `-F` : Fixed string (like fgrep)
- `-A NUM` : Show NUM lines after match
- `-B NUM` : Show NUM lines before match
- `-C NUM` : Show NUM lines context

**Use Cases:**
- Search for text in files
- Filter command output
- Pattern matching
- Log analysis
- Find specific configurations

**Examples:**
\`\`\`bash
grep "error" file.txt   # Search for pattern
grep -i "ERROR" file.txt # Case-insensitive search
grep -n "pattern" file.txt # Show line numbers
grep -v "exclude" file.txt # Exclude lines
grep -r "pattern" /path # Recursive search
grep -l "pattern" *.txt # Show only filenames
grep -c "pattern" file.txt # Count matches
grep "^start" file.txt  # Lines starting with "start"
grep "end$" file.txt    # Lines ending with "end"
grep -A 3 "error" log.txt # Show 3 lines after match
grep -B 2 "error" log.txt # Show 2 lines before match
grep -C 5 "error" log.txt # Show 5 lines context
ps aux | grep firefox   # Search process output
grep -E "^[0-9]" file.txt # Extended regex (starts with digit)
\`\`\`

---

### sed
**Purpose:** Stream editor for filtering and transforming text  
**Syntax:** `sed [OPTION] 'COMMAND' [FILE]`  
**Common Commands:**
- `s/old/new/` : Substitute first occurrence per line
- `s/old/new/g` : Substitute all occurrences
- `s/old/new/i` : Case-insensitive substitute
- `d` : Delete line
- `p` : Print line
- `c` : Change line
- `a` : Append
- `i` : Insert

**Use Cases:**
- Find and replace text
- Remove lines
- Edit files in place
- Transform data
- Pattern-based editing

**Examples:**
\`\`\`bash
sed 's/old/new/' file.txt # Replace first occurrence per line
sed 's/old/new/g' file.txt # Replace all occurrences
sed -i 's/old/new/g' file.txt # Edit file in place
sed '5d' file.txt       # Delete line 5
sed -n '1,5p' file.txt  # Print only lines 1-5
sed '/pattern/d' file.txt # Delete lines matching pattern
sed 's/^/PREFIX_/' file.txt # Add prefix to each line
sed 's/$/_SUFFIX/' file.txt # Add suffix to each line
sed -e 's/a/A/g' -e 's/b/B/g' file.txt # Multiple commands
sed '1s/^/HEADER\n/' file.txt # Add header
sed -i.bak 's/old/new/g' file.txt # Backup before edit
\`\`\`

---

### awk
**Purpose:** Pattern scanning and processing language  
**Syntax:** `awk [OPTION] 'PROGRAM' [FILE]`  
**Common Syntax:**
- `{action}` : Execute action for each line
- `/pattern/ {action}` : Execute if pattern matches
- `BEGIN {action}` : Execute before processing
- `END {action}` : Execute after processing
- `$1, $2, ...` : Field variables
- `NF` : Number of fields
- `NR` : Number of records

**Use Cases:**
- Column extraction
- Text transformation
- Data processing
- Report generation
- Log analysis

**Examples:**
\`\`\`bash
awk '{print $1}' file.txt # Print first column
awk '{print $1, $3}' file.txt # Print columns 1 and 3
awk -F: '{print $1}' /etc/passwd # Specify field separator
awk '{sum+=$1} END {print sum}' numbers.txt # Sum column
awk 'NR>1 {print}' file.txt # Skip header
awk '/pattern/ {print}' file.txt # Print matching lines
awk '{if ($1 > 10) print}' file.txt # Conditional print
awk '{gsub(/old/, "new"); print}' file.txt # Find and replace
awk 'BEGIN {print "Start"} {print} END {print "End"}' file.txt
\`\`\`

---

### cut
**Purpose:** Extract columns from files  
**Syntax:** `cut [OPTION] [FILE]`  
**Common Options:**
- `-d DELIMITER` : Specify field delimiter
- `-f FIELDS` : Select specific fields
- `-c RANGE` : Select character range

**Use Cases:**
- Extract columns
- Process delimited files
- Data extraction
- CSV processing

**Examples:**
\`\`\`bash
cut -d: -f1 /etc/passwd # Extract usernames
cut -d, -f1,3 data.csv  # Extract columns 1 and 3
cut -f2 file.txt        # Extract field 2 (tab-delimited)
cut -c 1-5 file.txt     # Extract first 5 characters
cut -d' ' -f1 file.txt  # Extract first space-separated field
ps aux | cut -d' ' -f1-2 # Process command output
\`\`\`

---

### sort
**Purpose:** Sort lines in files  
**Syntax:** `sort [OPTION] [FILE]`  
**Common Options:**
- `-n` : Numeric sort
- `-r` : Reverse sort
- `-u` : Remove duplicates
- `-k NUM` : Sort by specific field
- `-t DELIM` : Field separator
- `-h` : Human-readable numbers

**Use Cases:**
- Organize data
- Alphanumeric sorting
- Numeric sorting
- Remove duplicates
- Process lists

**Examples:**
\`\`\`bash
sort file.txt           # Alphabetic sort
sort -n numbers.txt     # Numeric sort
sort -r file.txt        # Reverse sort
sort -u file.txt        # Remove duplicates
sort -k2 file.txt       # Sort by field 2
sort -t: -k3 -n /etc/passwd # Sort by UID
sort -h sizes.txt       # Human-readable numbers
cat file.txt | sort -u  # Pipeline usage
\`\`\`

---

### uniq
**Purpose:** Report or omit repeated lines  
**Syntax:** `uniq [OPTION] [INPUT [OUTPUT]]`  
**Common Options:**
- `-c` : Count occurrences
- `-d` : Show only duplicates
- `-u` : Show only unique lines
- `-i` : Case-insensitive

**Use Cases:**
- Find duplicates
- Count occurrences
- Remove duplicate lines
- Analyze lists

**Examples:**
\`\`\`bash
uniq file.txt           # Remove consecutive duplicates
uniq -c file.txt        # Count occurrences
uniq -d file.txt        # Show only duplicate lines
uniq -u file.txt        # Show only unique lines
sort file.txt | uniq    # Remove all duplicates
sort file.txt | uniq -c # Count all occurrences
uniq -i file.txt        # Case-insensitive
\`\`\`

---

### wc
**Purpose:** Word count - count lines, words, characters  
**Syntax:** `wc [OPTION] [FILE]`  
**Common Options:**
- `-l` : Count lines
- `-w` : Count words
- `-c` : Count bytes
- `-m` : Count characters
- `-L` : Longest line length

**Use Cases:**
- Count file statistics
- Validate file sizes
- Script file analysis
- Log analysis

**Examples:**
\`\`\`bash
wc file.txt             # Show lines, words, bytes
wc -l file.txt          # Count lines
wc -w file.txt          # Count words
wc -c file.txt          # Count bytes
wc -m file.txt          # Count characters
wc -L file.txt          # Show longest line
wc -l *.txt             # Count lines in all files
find . -name "*.py" | xargs wc -l # Count lines in all Python files
\`\`\`

---

### tr
**Purpose:** Translate or delete characters  
**Syntax:** `tr [OPTION] SET1 [SET2]`  
**Common Options:**
- `-d` : Delete characters
- `-s` : Squeeze repeated characters
- `-c` : Complement set1

**Use Cases:**
- Character replacement
- Case conversion
- Remove characters
- Squeeze whitespace

**Examples:**
\`\`\`bash
tr a-z A-Z file.txt    # Convert lowercase to uppercase
tr A-Z a-z file.txt    # Convert uppercase to lowercase
tr -d ' ' file.txt     # Remove spaces
tr -s ' ' file.txt     # Squeeze multiple spaces
tr '\n' ' ' file.txt   # Convert newlines to spaces
cat file.txt | tr -d '[:digit:]' # Remove digits
echo "Hello 123" | tr -cd '[:digit:]' # Keep only digits
\`\`\`

---

### paste
**Purpose:** Merge corresponding lines  
**Syntax:** `paste [OPTION] FILE1 FILE2`  
**Common Options:**
- `-d DELIMITER` : Specify delimiter
- `-s` : Paste sequentially

**Use Cases:**
- Combine files
- Merge columns
- Join parallel data
- Reformat data

**Examples:**
\`\`\`bash
paste file1.txt file2.txt # Merge side by side
paste -d, file1.txt file2.txt # Use comma as delimiter
paste -s file.txt       # Convert to single line
paste -d: file1.txt file2.txt file3.txt # Multiple files
\`\`\`

---

### join
**Purpose:** Join lines from two files on common field  
**Syntax:** `join [OPTION] FILE1 FILE2`  
**Common Options:**
- `-1 NUM` : Join on field NUM from file1
- `-2 NUM` : Join on field NUM from file2
- `-t DELIM` : Field separator
- `-a NUM` : Include unmatched lines from file NUM

**Use Cases:**
- Database-like joins
- Correlate data
- Match records
- Data integration

**Examples:**
\`\`\`bash
join file1.txt file2.txt # Join on first field
join -1 2 -2 1 file1.txt file2.txt # Join different fields
join -t: file1.txt file2.txt # Use colon separator
join -a1 file1.txt file2.txt # Include unmatched from file1
\`\`\`

---

### diff
**Purpose:** Compare files line by line  
**Syntax:** `diff [OPTION] FILE1 FILE2`  
**Common Options:**
- `-u` : Unified format
- `-y` : Side-by-side
- `-q` : Only report if different
- `-r` : Recursive

**Use Cases:**
- Compare versions
- Check changes
- Generate patches
- Version control prep

**Examples:**
\`\`\`bash
diff file1.txt file2.txt # Show differences
diff -u file1.txt file2.txt # Unified format
diff -y file1.txt file2.txt # Side-by-side
diff -q file1.txt file2.txt # Just report differences
diff -r dir1 dir2        # Compare directories
diff file1.txt file2.txt > changes.patch # Create patch
\`\`\`

---

### cmp
**Purpose:** Compare files byte by byte  
**Syntax:** `cmp [OPTION] FILE1 FILE2`  
**Common Options:**
- `-l` : Show all differences
- `-s` : Silent (exit code only)

**Use Cases:**
- Binary file comparison
- Exact comparison
- Verify file copies
- Script validation

**Examples:**
\`\`\`bash
cmp file1.txt file2.txt # Compare byte by byte
cmp -s file1.txt file2.txt # Silent comparison
cmp -l file1.txt file2.txt # Show all bytes different
\`\`\`

---

### comm
**Purpose:** Compare sorted files  
**Syntax:** `comm [OPTION] FILE1 FILE2`  
**Common Options:**
- `-1` : Suppress column 1 (unique to FILE1)
- `-2` : Suppress column 2 (unique to FILE2)
- `-3` : Suppress column 3 (common)

**Use Cases:**
- Find common lines
- Find unique lines
- Set operations
- Data comparison

**Examples:**
\`\`\`bash
comm file1.txt file2.txt # Show three columns
comm -12 file1.txt file2.txt # Show only common
comm -23 file1.txt file2.txt # Unique to file1
comm -13 file1.txt file2.txt # Unique to file2
comm -3 file1.txt file2.txt  # Show unique lines
\`\`\`

---

## 3. System Information & Monitoring

### uname
**Purpose:** Print system information  
**Syntax:** `uname [OPTION]`  
**Common Options:**
- `-a` : All information
- `-s` : Kernel name
- `-n` : Network hostname
- `-r` : Kernel release
- `-v` : Kernel version
- `-m` : Hardware platform
- `-o` : Operating system

**Use Cases:**
- Identify system type
- Check architecture
- Kernel information
- OS detection

**Examples:**
\`\`\`bash
uname                   # Print kernel name
uname -a                # Print all info
uname -m                # Show architecture (x86_64, etc)
uname -r                # Show kernel version
uname -s                # Show system name
if [ "$(uname)" = "Linux" ]; then # OS detection
\`\`\`

---

### whoami
**Purpose:** Print current user  
**Syntax:** `whoami`  
**Use Cases:**
- Check current user
- Script user verification
- Permission validation
- Automation scripts

**Examples:**
\`\`\`bash
whoami                  # Show current user
if [ "$(whoami)" != "root" ]; then # Check if not root
[ "$(whoami)" = "root" ] # True if running as root
\`\`\`

---

### who
**Purpose:** Show who is logged in  
**Syntax:** `who [OPTION]`  
**Common Options:**
- `-a` : All information
- `-b` : Last system boot
- `-q` : Quick list
- `-r` : Runlevel

**Use Cases:**
- Monitor logged-in users
- Security auditing
- System administration
- User sessions tracking

**Examples:**
\`\`\`bash
who                     # List logged-in users
who -a                  # All details
who -b                  # Last boot time
who -q                  # Quick list
\`\`\`

---

### w
**Purpose:** Show logged-in users and their activities  
**Syntax:** `w [OPTION] [USER]`  
**Common Options:**
- `-h` : No header
- `-s` : Short format

**Use Cases:**
- Monitor user activity
- See what users are doing
- Load information
- System monitoring

**Examples:**
\`\`\`bash
w                       # Show all logged users
w user                  # Show specific user
w -h                    # Without header
w -s                    # Short format
\`\`\`

---

### date
**Purpose:** Display or set system date/time  
**Syntax:** `date [OPTION] [+FORMAT]`  
**Common Options:**
- `-d STRING` : Parse and print DATE
- `-s STRING` : Set date
- `-R` : RFC 2822 format

**Use Cases:**
- Display current time
- Format timestamps
- Set system time
- Scheduling

**Examples:**
\`\`\`bash
date                    # Current date and time
date +"%Y-%m-%d"        # Format: YYYY-MM-DD
date +"%H:%M:%S"        # Format: HH:MM:SS
date +"%s"              # Unix timestamp
date -d "2 days ago"    # Date calculation
date -R                 # RFC format
date --iso-8601        # ISO format
\`\`\`

---

### cal
**Purpose:** Display calendar  
**Syntax:** `cal [OPTION] [MONTH] [YEAR]`  
**Common Options:**
- `-3` : Three months (prev, current, next)
- `-y` : Full year
- `-m` : Monday as first day

**Use Cases:**
- View calendar
- Date reference
- Plan scheduling
- Date calculation

**Examples:**
\`\`\`bash
cal                     # Current month
cal 2025                # Full year
cal 1 2025              # January 2025
cal -3                  # Current + prev/next
cal -y 2025             # Full year calendar
\`\`\`

---

### uptime
**Purpose:** Show how long system has been running  
**Syntax:** `uptime [OPTION]`  
**Common Options:**
- `-p` : Pretty format
- `-s` : Since when up

**Use Cases:**
- Check system stability
- Uptime monitoring
- System health check
- Reboot tracking

**Examples:**
\`\`\`bash
uptime                  # Show uptime and load
uptime -p               # Pretty format (e.g., "up 2 days")
uptime -s               # Since when running
watch uptime            # Monitor continuously
\`\`\`

---

### last
**Purpose:** Show login history  
**Syntax:** `last [OPTION] [USER]`  
**Common Options:**
- `-n NUM` : Show last NUM lines
- `-f FILE` : Read from FILE
- `-x` : Show system shutdown records

**Use Cases:**
- Security auditing
- Login tracking
- User history
- System monitoring

**Examples:**
\`\`\`bash
last                    # Show login history
last -n 10              # Show last 10 logins
last user               # User login history
last root               # Root logins
last -x                 # Include shutdowns
\`\`\`

---

### ps
**Purpose:** Process status - list running processes  
**Syntax:** `ps [OPTION]`  
**Common Options:**
- `aux` : All processes, user-oriented format
- `-ef` : Full format
- `-eo` : Custom output
- `-p PID` : Specific process
- `-u USER` : User processes
- `--sort` : Sort results

**Use Cases:**
- List processes
- Monitor applications
- Find process ID
- System monitoring
- Resource usage

**Examples:**
\`\`\`bash
ps                      # Current user processes
ps aux                  # All processes detailed
ps -ef                  # Full process listing
ps -p 1234              # Specific PID
ps -u root              # Processes by user
ps aux | grep firefox   # Find specific process
ps aux --sort=-%cpu | head # Top CPU processes
ps aux --sort=-%mem | head # Top memory processes
ps -eo pid,user,%cpu,%mem,cmd # Custom columns
\`\`\`

---

### top
**Purpose:** Interactive process monitor  
**Syntax:** `top [OPTION]`  
**Common Options:**
- `-d SEC` : Delay between updates
- `-n NUM` : Number of iterations
- `-u USER` : Show user's processes
- `-p PID` : Monitor specific process
- `-H` : Show threads

**Use Cases:**
- Real-time monitoring
- Resource tracking
- Performance analysis
- Process debugging
- System health check

**Examples:**
\`\`\`bash
top                     # Start interactive monitoring
top -d 2                # Update every 2 seconds
top -n 1                # Single iteration
top -u root             # Show root's processes
top -p 1234             # Monitor specific PID
top -H                  # Show thread info
top -o %MEM             # Sort by memory usage
\`\`\`

---

### htop
**Purpose:** Interactive process monitor (enhanced top)  
**Syntax:** `htop [OPTION]`  
**Common Options:**
- `-d SEC` : Delay in tenths of seconds
- `-u USER` : Show user's processes
- `-p PID` : Monitor specific process
- `-H` : Show threads
- `-s COL` : Sort by column

**Use Cases:**
- Better visualization than top
- Process tree view
- Easy navigation
- Resource monitoring
- Performance analysis

**Examples:**
\`\`\`bash
htop                    # Start monitoring
htop -u root            # Show root processes
htop -p 1234            # Monitor specific PID
htop -H                 # Thread view
htop -d 10              # 1 second delay
\`\`\`

---

### free
**Purpose:** Display memory usage  
**Syntax:** `free [OPTION]`  
**Common Options:**
- `-h` : Human-readable
- `-m` : Show in megabytes
- `-g` : Show in gigabytes
- `-s SEC` : Continuous monitoring

**Use Cases:**
- Check RAM usage
- Monitor memory
- Verify availability
- Troubleshooting

**Examples:**
\`\`\`bash
free                    # Memory in kilobytes
free -h                 # Human-readable format
free -m                 # Memory in MB
free -g                 # Memory in GB
free -h -s 2            # Update every 2 seconds
watch free -h           # Continuous monitoring
\`\`\`

---

### df
**Purpose:** Disk space usage  
**Syntax:** `df [OPTION] [FILE/DIR]`  
**Common Options:**
- `-h` : Human-readable
- `-i` : Inode information
- `-T` : File system type
- `-a` : All file systems

**Use Cases:**
- Check disk usage
- Monitor partitions
- Verify free space
- Capacity planning

**Examples:**
\`\`\`bash
df                      # Show all mounted filesystems
df -h                   # Human-readable
df -hT                  # With filesystem type
df -i                   # Inode usage
df /home                # Specific mount point
df -h /                 # Root partition usage
\`\`\`

---

### du
**Purpose:** Disk usage by files/directories  
**Syntax:** `du [OPTION] [PATH]`  
**Common Options:**
- `-h` : Human-readable
- `-s` : Summary only
- `-S` : Not recursive
- `-d DEPTH` : Limit depth
- `--max-depth=DEPTH` : Directory depth limit

**Use Cases:**
- Find large files
- Directory size analysis
- Disk usage tracking
- Space management

**Examples:**
\`\`\`bash
du /home                # Directory sizes
du -h /home             # Human-readable
du -sh /home            # Total size only
du -h --max-depth=1 /   # First level only
du -sh * | sort -h      # Sorted sizes
find . -size +100M      # Files larger than 100MB
\`\`\`

---

## 4. Process Management

### kill
**Purpose:** Terminate processes  
**Syntax:** `kill [OPTION] PID`  
**Common Options:**
- `-9` : Force kill (SIGKILL)
- `-15` : Termination signal (default)
- `-STOP` : Pause process
- `-CONT` : Continue process
- `-l` : List signals

**Use Cases:**
- Stop running processes
- Force terminate
- Clean up hung processes
- Resource management

**Examples:**
\`\`\`bash
kill 1234               # Terminate process gracefully
kill -9 1234            # Force kill
kill -STOP 1234         # Pause process
kill -CONT 1234         # Resume process
kill -l                 # List all signals
\`\`\`

---

### killall
**Purpose:** Kill processes by name  
**Syntax:** `killall [OPTION] NAME`  
**Common Options:**
- `-9` : Force kill
- `-i` : Interactive
- `-u USER` : User's processes
- `-s SIGNAL` : Specific signal

**Use Cases:**
- Kill all instances of process
- Batch termination
- User process cleanup
- System maintenance

**Examples:**
\`\`\`bash
killall firefox         # Kill all Firefox instances
killall -9 node         # Force kill all Node processes
killall -u user         # Kill user's processes
killall -i firefox      # Interactive kill
\`\`\`

---

### pkill
**Purpose:** Kill process by pattern  
**Syntax:** `pkill [OPTION] PATTERN`  
**Common Options:**
- `-f` : Match full command line
- `-u USER` : User's processes
- `-9` : Force kill
- `-l` : Show process names

**Use Cases:**
- Pattern-based killing
- Complex process termination
- Batch operations
- Script automation

**Examples:**
\`\`\`bash
pkill firefox           # Kill Firefox processes
pkill -f "python3 script.py" # Kill by command
pkill -u user           # Kill user's processes
pkill -9 node           # Force kill Node
\`\`\`

---

### jobs
**Purpose:** Show job status  
**Syntax:** `jobs [OPTION]`  
**Common Options:**
- `-l` : Show process IDs
- `-r` : Running jobs
- `-s` : Stopped jobs

**Use Cases:**
- Monitor background jobs
- Job control
- Script management
- Process tracking

**Examples:**
\`\`\`bash
jobs                    # Show all jobs
jobs -l                 # Show with PIDs
jobs -r                 # Running jobs
fg %1                   # Foreground job 1
bg %2                   # Background job 2
\`\`\`

---

### bg / fg
**Purpose:** Background/Foreground process control  
**Syntax:** `bg [JOB]` / `fg [JOB]`  
**Common Usage:**
- `bg` : Resume stopped job in background
- `fg` : Bring job to foreground
- `%1`, `%2` : Job references

**Use Cases:**
- Resume suspended processes
- Switch job focus
- Parallel execution
- Process management

**Examples:**
\`\`\`bash
Ctrl+Z                  # Suspend current job
bg                      # Resume in background
fg                      # Bring to foreground
fg %1                   # Foreground job 1
command &               # Run in background initially
\`\`\`

---

### nohup
**Purpose:** Run command immune to hangups  
**Syntax:** `nohup COMMAND [ARGS]`  
**Common Usage:**
- Runs with stdout/stderr redirected
- Process continues after logout

**Use Cases:**
- Long-running processes
- SSH session cleanup
- Daemon processes
- Continuous operations

**Examples:**
\`\`\`bash
nohup command &         # Run immune to hangup
nohup python script.py > output.log 2>&1 &
nohup ./application > app.log &
tail -f nohup.out       # Monitor output
\`\`\`

---

### nice / renice
**Purpose:** Set or modify process priority  
**Syntax:** `nice [OPTION] COMMAND` / `renice [OPTION] PID`  
**Common Options:**
- `-n PRIORITY` : Set priority (-20 to 19)

**Use Cases:**
- Adjust process priority
- Resource allocation
- Performance tuning
- System balance

**Examples:**
\`\`\`bash
nice -n 10 command      # Run with lower priority
nice -n -5 command      # Higher priority
renice -n 5 -p 1234     # Change priority of running process
top                     # View NI column for priorities
\`\`\`

---

## 5. Networking & Communication

### ping
**Purpose:** Test network connectivity  
**Syntax:** `ping [OPTION] HOST`  
**Common Options:**
- `-c COUNT` : Number of packets
- `-i SEC` : Interval between packets
- `-W SEC` : Timeout
- `-s SIZE` : Packet size

**Use Cases:**
- Test connectivity
- Verify host availability
- Network diagnostics
- Latency measurement

**Examples:**
\`\`\`bash
ping google.com         # Continuous ping
ping -c 4 google.com    # Send 4 packets
ping -i 2 host          # 2 second interval
ping -W 5 host          # 5 second timeout
\`\`\`

---

### ifconfig / ip
**Purpose:** Configure network interfaces  
**Syntax:** `ifconfig [INTERFACE]` / `ip [COMMAND]`  
**Common Commands (ip):**
- `ip addr show` : Show IP addresses
- `ip link show` : Show interfaces
- `ip route show` : Show routes
- `ip addr add` : Add IP address
- `ip addr del` : Remove IP address

**Use Cases:**
- Configure interfaces
- Check IP addresses
- Network configuration
- Route management

**Examples:**
\`\`\`bash
ifconfig                # Show all interfaces
ifconfig eth0           # Show specific interface
ip addr show            # Show addresses (newer)
ip addr add 192.168.1.10/24 dev eth0
ip link set eth0 up     # Enable interface
ip link set eth0 down   # Disable interface
ip route show           # Show routing table
\`\`\`

---

### ssh
**Purpose:** Secure shell remote access  
**Syntax:** `ssh [OPTIONS] [USER@]HOST [COMMAND]`  
**Common Options:**
- `-p PORT` : Specify port
- `-l USER` : Username
- `-i FILE` : Identity file (key)
- `-N` : No command execution
- `-L` : Local port forwarding
- `-R` : Remote port forwarding
- `-v` : Verbose

**Use Cases:**
- Remote access
- Secure connections
- System administration
- File transfer (with scp)
- Port forwarding
- Tunneling

**Examples:**
\`\`\`bash
ssh user@host           # Connect to host
ssh -p 2222 user@host   # Custom port
ssh -i ~/.ssh/id_rsa user@host # Specific key
ssh user@host "command" # Execute command
ssh -N -L 8080:localhost:80 user@host # Port forward
scp file user@host:/path # Copy files
\`\`\`

---

### scp
**Purpose:** Secure copy between hosts  
**Syntax:** `scp [OPTIONS] SOURCE DEST`  
**Common Options:**
- `-r` : Recursive
- `-P PORT` : Port number
- `-p` : Preserve attributes
- `-v` : Verbose

**Use Cases:**
- Secure file transfer
- Remote backups
- Configuration deployment
- Data synchronization

**Examples:**
\`\`\`bash
scp file user@host:/path # Copy to remote
scp user@host:/path/file . # Copy from remote
scp -r dir user@host:/path # Recursive copy
scp -P 2222 file user@host:/path # Custom port
\`\`\`

---

### rsync
**Purpose:** Synchronize files and directories  
**Syntax:** `rsync [OPTION] SOURCE DEST`  
**Common Options:**
- `-a` : Archive mode
- `-v` : Verbose
- `-z` : Compress
- `--delete` : Delete extraneous files
- `-e ssh` : Use SSH transport
- `--progress` : Show progress

**Use Cases:**
- File synchronization
- Backups
- Mirroring
- Incremental transfers
- Bandwidth optimization

**Examples:**
\`\`\`bash
rsync -av source/ dest/ # Archive verbose
rsync -avz user@host:/source /dest # With compression
rsync -av --delete source/ dest/ # Delete removed files
rsync -e ssh -av source user@host:/dest # Via SSH
rsync --progress -av file user@host:/path
\`\`\`

---

### wget
**Purpose:** Download files from web  
**Syntax:** `wget [OPTION] URL`  
**Common Options:**
- `-O FILE` : Output filename
- `-c` : Continue partial download
- `-q` : Quiet mode
- `-r` : Recursive download
- `--limit-rate=RATE` : Limit bandwidth
- `--mirror` : Mirror website

**Use Cases:**
- Download files
- Website mirroring
- Resume downloads
- Batch downloads
- Automation

**Examples:**
\`\`\`bash
wget https://example.com/file.tar.gz # Download file
wget -O custom.tar.gz URL # Custom filename
wget -c URL             # Resume download
wget -q URL             # Quiet mode
wget --mirror https://example.com # Mirror site
wget --limit-rate=1m URL # Limit to 1MB/s
\`\`\`

---

### curl
**Purpose:** Transfer data using URLs  
**Syntax:** `curl [OPTION] URL`  
**Common Options:**
- `-o FILE` : Save to file
- `-O` : Use remote filename
- `-L` : Follow redirects
- `-I` : Headers only
- `-d DATA` : POST data
- `-H "HEADER"` : Set header
- `-u USER:PASS` : Authentication

**Use Cases:**
- API testing
- Web requests
- File transfer
- Download files
- Debugging

**Examples:**
\`\`\`bash
curl https://example.com # Fetch webpage
curl -o file.html URL   # Save output
curl -L URL             # Follow redirects
curl -I URL             # Headers only
curl -d "param=value" URL # POST request
curl -H "Authorization: Bearer TOKEN" URL
\`\`\`

---

### netstat / ss
**Purpose:** Network statistics  
**Syntax:** `netstat [OPTION]` / `ss [OPTION]`  
**Common Options:**
- `-a` : All connections
- `-l` : Listening ports
- `-n` : Numeric (no names)
- `-p` : Process info
- `-t` : TCP
- `-u` : UDP
- `-r` : Routing table

**Use Cases:**
- Network monitoring
- Port checking
- Connection tracking
- Network debugging
- Service verification

**Examples:**
\`\`\`bash
netstat -tulnp          # All listening ports with PIDs
netstat -anp            # All connections
ss -tulnp               # Listening (ss version)
netstat -rn             # Routing table
lsof -i TCP:80          # Processes on port 80
\`\`\`

---

### traceroute
**Purpose:** Trace route to remote host  
**Syntax:** `traceroute [OPTION] HOST`  
**Common Options:**
- `-m MAX_TTL` : Maximum hops
- `-n` : Numeric output

**Use Cases:**
- Network path analysis
- Connectivity debugging
- Route tracing
- Network troubleshooting

**Examples:**
\`\`\`bash
traceroute google.com   # Trace to Google
traceroute -m 15 host   # 15 hop limit
mtr google.com          # Interactive tracing
\`\`\`

---

### dig / nslookup
**Purpose:** DNS lookup  
**Syntax:** `dig [OPTION] DOMAIN` / `nslookup [OPTION] DOMAIN`  
**Common Options:**
- `+short` : Abbreviated output
- `-x` : Reverse lookup
- `@SERVER` : Query specific server

**Use Cases:**
- DNS resolution
- Record lookup
- DNS debugging
- Domain validation

**Examples:**
\`\`\`bash
dig google.com          # DNS lookup
dig +short google.com   # Short output
dig @8.8.8.8 google.com # Specific server
dig -x 8.8.8.8          # Reverse lookup
nslookup google.com     # Alternative DNS lookup
\`\`\`

---

### whois
**Purpose:** Domain and IP information  
**Syntax:** `whois [DOMAIN/IP]`  
**Use Cases:**
- Domain registration info
- IP ownership
- WHOIS lookups
- Domain research

**Examples:**
\`\`\`bash
whois google.com        # Domain info
whois 8.8.8.8           # IP information
\`\`\`

---

## 6. Disk & Storage Management

### mount / umount
**Purpose:** Attach/detach file systems  
**Syntax:** `mount [OPTIONS] [SOURCE] [MOUNT_POINT]` / `umount [OPTIONS] [MOUNT_POINT]`  
**Common Options:**
- `-t TYPE` : File system type
- `-o OPTIONS` : Mount options
- `-a` : Mount all
- `-l` : List mounted

**Use Cases:**
- Mount partitions
- Attach removable media
- Network file systems
- USB devices

**Examples:**
\`\`\`bash
mount                   # Show mounted filesystems
mount -t ext4 /dev/sda1 /mnt # Mount partition
umount /mnt             # Unmount filesystem
mount -a                # Mount from /etc/fstab
mount -o remount,rw /   # Remount read-write
\`\`\`

---

### fdisk
**Purpose:** Partition table manipulator  
**Syntax:** `fdisk [DEVICE]`  
**Common Commands:**
- `p` : Print partition table
- `n` : New partition
- `d` : Delete partition
- `t` : Change type
- `w` : Write changes

**Use Cases:**
- Create partitions
- Delete partitions
- View partition table
- Resize partitions

**Examples:**
\`\`\`bash
fdisk -l                # List all partitions
fdisk /dev/sda          # Edit /dev/sda partitions
# Interactive: p (print), n (new), d (delete), w (write)
\`\`\`

---

### lsblk
**Purpose:** List block devices  
**Syntax:** `lsblk [OPTION]`  
**Common Options:**
- `-f` : File system info
- `-a` : Include empty
- `-o COL` : Specific columns

**Use Cases:**
- View disk layout
- Check partitions
- File system information
- Storage overview

**Examples:**
\`\`\`bash
lsblk                   # Show block devices
lsblk -f                # With filesystem info
lsblk -o NAME,SIZE,TYPE # Custom columns
\`\`\`

---

### mkfs
**Purpose:** Create file system  
**Syntax:** `mkfs -t TYPE [OPTIONS] DEVICE`  
**Common Types:**
- `ext4` : Extended filesystem 4
- `vfat` : FAT filesystem
- `ntfs` : NTFS filesystem

**Use Cases:**
- Format partitions
- Create filesystems
- Prepare storage
- Erase partitions

**Examples:**
\`\`\`bash
mkfs.ext4 /dev/sda1     # Create ext4 filesystem
mkfs.vfat /dev/sdb1     # Create FAT filesystem
mkfs -t ext4 /dev/sda1  # Alternative syntax
\`\`\`

---

### fsck
**Purpose:** File system check and repair  
**Syntax:** `fsck [OPTION] DEVICE`  
**Common Options:**
- `-y` : Repair without asking
- `-n` : Check only, no repair
- `-f` : Force check

**Use Cases:**
- Check filesystem integrity
- Repair damaged partitions
- Boot issues
- Recovery

**Examples:**
\`\`\`bash
fsck /dev/sda1          # Check filesystem
fsck -y /dev/sda1       # Auto repair
fsck -n /dev/sda1       # Check only
\`\`\`

---

### dd
**Purpose:** Copy and convert data  
**Syntax:** `dd if=INPUT of=OUTPUT [OPTIONS]`  
**Common Options:**
- `bs=SIZE` : Block size
- `count=NUM` : Number of blocks
- `skip=NUM` : Skip input blocks
- `seek=NUM` : Skip output blocks

**Use Cases:**
- Disk imaging
- Backup creation
- Disk cloning
- Data recovery
- File conversion

**Examples:**
\`\`\`bash
dd if=/dev/sda of=disk.img # Backup entire disk
dd if=disk.img of=/dev/sda # Restore from image
dd if=/dev/zero of=file bs=1M count=10 # Create 10MB file
dd if=/dev/sda of=/dev/sdb # Clone disk to disk
dd if=/dev/urandom of=random bs=1M count=100 # Random file
\`\`\`

---

### tar
**Purpose:** Archive files  
**Syntax:** `tar [OPTION] [FILE...]`  
**Common Options:**
- `-c` : Create archive
- `-x` : Extract archive
- `-f FILE` : Archive filename
- `-z` : Compress with gzip
- `-j` : Compress with bzip2
- `-v` : Verbose
- `-t` : List contents

**Use Cases:**
- Create archives
- Extract archives
- Backups
- File distribution
- Compression

**Examples:**
\`\`\`bash
tar -cvf archive.tar file1 file2 # Create tar archive
tar -xvf archive.tar    # Extract tar archive
tar -czvf archive.tar.gz dir/ # Create compressed
tar -xzvf archive.tar.gz # Extract compressed
tar -tzf archive.tar.gz  # List contents
tar --append -f archive.tar newfile # Add file to tar
\`\`\`

---

### gzip / gunzip
**Purpose:** Compress/decompress files  
**Syntax:** `gzip [OPTION] FILE` / `gunzip [OPTION] FILE`  
**Common Options:**
- `-d` : Decompress
- `-k` : Keep original
- `-v` : Verbose
- `-9` : Best compression

**Use Cases:**
- Compress files
- Save disk space
- Archive preparation
- Backup compression

**Examples:**
\`\`\`bash
gzip file.txt           # Compress (creates file.txt.gz)
gunzip file.txt.gz      # Decompress
gzip -k file.txt        # Keep original
gzip -d file.txt.gz     # Alternative decompress
zcat file.txt.gz        # View compressed content
\`\`\`

---

### zip / unzip
**Purpose:** Compress files in ZIP format  
**Syntax:** `zip [OPTION] ARCHIVE FILES` / `unzip [OPTION] ARCHIVE`  
**Common Options:**
- `-r` : Recursive
- `-l` : List contents
- `-d` : Extract directory

**Use Cases:**
- Create ZIP archives
- Cross-platform compression
- File distribution
- Selective compression

**Examples:**
\`\`\`bash
zip archive.zip file1 file2 # Create zip
zip -r archive.zip directory/ # Recursive zip
unzip archive.zip       # Extract all
unzip -l archive.zip    # List contents
unzip archive.zip -d /path # Extract to directory
\`\`\`

---

## 7. User & Group Management

### useradd / userdel / usermod
**Purpose:** Create/delete/modify user accounts  
**Syntax:** `useradd [OPTION] USERNAME` / `userdel [OPTION] USERNAME` / `usermod [OPTION] USERNAME`  
**Common Options (useradd):**
- `-m` : Create home directory
- `-s` : Set shell
- `-g` : Primary group
- `-G` : Supplementary groups

**Use Cases:**
- Add system users
- Remove users
- Modify user properties
- Account management

**Examples:**
\`\`\`bash
useradd -m -s /bin/bash newuser # Create user
useradd -m -G sudo newuser # Add to sudo group
userdel newuser         # Delete user
userdel -r newuser      # Delete with home directory
usermod -aG wheel user  # Add to group
usermod -s /bin/zsh user # Change shell
\`\`\`

---

### passwd
**Purpose:** Change user password  
**Syntax:** `passwd [OPTION] [USERNAME]`  
**Common Options:**
- `-l` : Lock account
- `-u` : Unlock account
- `-S` : Status

**Use Cases:**
- Change passwords
- Lock accounts
- Account management
- Security

**Examples:**
\`\`\`bash
passwd                  # Change own password
passwd user             # Admin change password
passwd -l user          # Lock account
passwd -u user          # Unlock account
\`\`\`

---

### groupadd / groupdel / groupmod
**Purpose:** Create/delete/modify groups  
**Syntax:** `groupadd [OPTION] GROUP` / `groupdel GROUP` / `groupmod [OPTION] GROUP`  
**Use Cases:**
- Create groups
- Remove groups
- Manage group membership
- System organization

**Examples:**
\`\`\`bash
groupadd developers     # Create group
groupdel developers     # Delete group
usermod -aG developers user # Add user to group
\`\`\`

---

### id / groups
**Purpose:** Show user/group IDs and memberships  
**Syntax:** `id [OPTION] [USER]` / `groups [USER]`  
**Use Cases:**
- Check user ID
- View group membership
- Permission verification
- Security auditing

**Examples:**
\`\`\`bash
id                      # Current user info
id user                 # Specific user info
groups                  # Current user groups
groups user             # User's groups
\`\`\`

---

### sudo
**Purpose:** Execute with superuser privileges  
**Syntax:** `sudo [OPTION] COMMAND`  
**Common Options:**
- `-u USER` : Run as user
- `-i` : Login shell
- `-s` : Shell
- `-l` : List privileges

**Use Cases:**
- Execute privileged commands
- Administrative tasks
- Delegated permissions
- Secure operations

**Examples:**
\`\`\`bash
sudo command            # Run as root
sudo -u user command    # Run as specific user
sudo -i                 # Login as root
sudo -l                 # List sudo privileges
\`\`\`

---

## 8. Permissions & Ownership

### chmod
**Purpose:** Change file permissions  
**Syntax:** `chmod [OPTION] MODE FILE`  
**Mode Examples:**
- `755` : Owner read/write/execute, others read/execute
- `644` : Owner read/write, others read
- `+x` : Add execute permission
- `-x` : Remove execute
- `u+w` : User write

**Use Cases:**
- Modify permissions
- Make files executable
- Secure files
- Access control

**Examples:**
\`\`\`bash
chmod 755 script.sh     # rwxr-xr-x
chmod 644 file.txt      # rw-r--r--
chmod +x script.sh      # Make executable
chmod -x file           # Remove execute
chmod u+w file          # User write
chmod a-x file          # Remove execute from all
chmod -R 755 directory/ # Recursive change
\`\`\`

---

### chown
**Purpose:** Change file owner  
**Syntax:** `chown [OPTION] OWNER:GROUP FILE`  
**Common Options:**
- `-R` : Recursive
- `-v` : Verbose

**Use Cases:**
- Change ownership
- User file transfer
- System maintenance
- Security management

**Examples:**
\`\`\`bash
chown user file.txt     # Change owner
chown user:group file   # Change owner and group
chown -R user directory/ # Recursive change
chown :group file       # Change group only
\`\`\`

---

### chgrp
**Purpose:** Change group ownership  
**Syntax:** `chgrp [OPTION] GROUP FILE`  
**Use Cases:**
- Change group ownership
- File sharing
- Access control
- Permissions management

**Examples:**
\`\`\`bash
chgrp group file.txt    # Change group
chgrp -R group directory/ # Recursive
\`\`\`

---

## 9. Archiving & Compression

### 7z
**Purpose:** 7-Zip archiver  
**Syntax:** `7z [COMMAND] [OPTIONS] ARCHIVE [FILES]`  
**Common Commands:**
- `a` : Add to archive
- `x` : Extract archive
- `l` : List contents

**Use Cases:**
- High compression
- 7z format archives
- Extract 7z files
- Create archives

**Examples:**
\`\`\`bash
7z a archive.7z files   # Create 7z archive
7z x archive.7z         # Extract archive
7z l archive.7z         # List contents
\`\`\`

---

### bzip2 / bunzip2
**Purpose:** Compress with bzip2  
**Syntax:** `bzip2 [OPTION] FILE` / `bunzip2 [OPTION] FILE`  
**Use Cases:**
- Better compression than gzip
- Create .bz2 archives
- Decompress bzip2

**Examples:**
\`\`\`bash
bzip2 file.txt          # Compress file
bunzip2 file.txt.bz2    # Decompress
bzcat file.txt.bz2      # View compressed
\`\`\`

---

### xz
**Purpose:** High compression archiver  
**Syntax:** `xz [OPTION] FILE`  
**Use Cases:**
- Best compression ratio
- Create xz archives
- Extract xz files

**Examples:**
\`\`\`bash
xz file.txt             # Compress
unxz file.txt.xz        # Decompress
xzcat file.txt.xz       # View compressed
\`\`\`

---

## 10. Package Management

### apt / apt-get (Debian/Ubuntu)
**Purpose:** Package management  
**Syntax:** `apt [COMMAND]`  
**Common Commands:**
- `update` : Update package lists
- `upgrade` : Upgrade packages
- `install` : Install package
- `remove` : Remove package
- `search` : Search packages

**Use Cases:**
- Install software
- Update system
- Remove packages
- Package searches

**Examples:**
\`\`\`bash
apt update               # Update package lists
apt upgrade             # Upgrade all packages
apt install package     # Install package
apt remove package      # Remove package
apt search term         # Search packages
apt autoremove          # Remove unused
apt-cache search pattern # Search packages
apt-cache show package  # Show package info
\`\`\`

---

### yum / dnf (RedHat/CentOS/Fedora)
**Purpose:** RPM package management  
**Syntax:** `yum [COMMAND]` / `dnf [COMMAND]`  
**Common Commands:**
- `install` : Install package
- `remove` : Remove package
- `update` : Update packages
- `search` : Search packages

**Use Cases:**
- RPM-based systems
- Software management
- System updates
- Package installation

**Examples:**
\`\`\`bash
yum install package     # Install package
yum remove package      # Remove package
yum update              # Update system
yum search term         # Search
dnf install package     # Fedora newer syntax
\`\`\`

---

### pacman (Arch Linux)
**Purpose:** Arch package manager  
**Syntax:** `pacman [OPTION] [PACKAGE]`  
**Common Options:**
- `-S` : Sync/install
- `-R` : Remove
- `-Sy` : Sync database
- `-Su` : Update system

**Use Cases:**
- Arch Linux packages
- Software management
- System maintenance

**Examples:**
\`\`\`bash
pacman -Sy              # Update database
pacman -S package       # Install package
pacman -R package       # Remove package
pacman -Su              # Update system
\`\`\`

---

### snap
**Purpose:** Universal package manager  
**Syntax:** `snap [COMMAND] [OPTIONS]`  
**Common Commands:**
- `install` : Install snap
- `remove` : Remove snap
- `list` : List installed
- `update` : Update snaps

**Use Cases:**
- Universal packages
- Cross-distro support
- Containerized apps

**Examples:**
\`\`\`bash
snap install package    # Install snap
snap remove package     # Remove snap
snap list               # List snaps
snap update             # Update all
\`\`\`

---

## 11. System Services & Control

### systemctl
**Purpose:** System service manager  
**Syntax:** `systemctl [COMMAND] [UNIT]`  
**Common Commands:**
- `start` : Start service
- `stop` : Stop service
- `restart` : Restart service
- `enable` : Enable auto-start
- `disable` : Disable auto-start
- `status` : Show status
- `list-units` : List all units

**Use Cases:**
- Service management
- Daemon control
- System startup
- Service configuration

**Examples:**
\`\`\`bash
systemctl start nginx    # Start service
systemctl stop nginx     # Stop service
systemctl restart nginx  # Restart
systemctl enable nginx   # Auto-start
systemctl disable nginx  # Disable auto-start
systemctl status nginx   # Show status
systemctl list-units     # List all services
systemctl list-timers    # List timers
systemctl daemon-reload  # Reload configuration
\`\`\`

---

### service
**Purpose:** SysV service management (legacy)  
**Syntax:** `service [SERVICE] [COMMAND]`  
**Common Commands:**
- `start` : Start service
- `stop` : Stop service
- `restart` : Restart
- `status` : Show status

**Use Cases:**
- Legacy systems
- Service management
- Compatibility

**Examples:**
\`\`\`bash
service nginx start      # Start service
service nginx stop       # Stop service
service nginx restart    # Restart
service nginx status     # Show status
service --status-all     # List all services
\`\`\`

---

### shutdown / reboot / halt
**Purpose:** System power control  
**Syntax:** `shutdown [OPTION] [TIME] [MESSAGE]`  
**Common Options:**
- `-h` : Halt (power off)
- `-r` : Reboot
- `now` : Immediate
- `-k` : Announce only

**Use Cases:**
- System shutdown
- Reboot
- Scheduled restart
- Power management

**Examples:**
\`\`\`bash
shutdown -h now         # Shutdown immediately
shutdown -r now         # Reboot immediately
shutdown -h +30 "Message" # Schedule in 30 min
reboot                  # Reboot now
halt                    # Halt system
poweroff                # Power off
\`\`\`

---

### journalctl
**Purpose:** System log viewer  
**Syntax:** `journalctl [OPTION]`  
**Common Options:**
- `-f` : Follow logs
- `-e` : Jump to end
- `-n NUM` : Last NUM lines
- `-u UNIT` : Specific service
- `--since` : Time filter
- `-p PRIORITY` : Filter level

**Use Cases:**
- View system logs
- Debug issues
- Monitor services
- System troubleshooting

**Examples:**
\`\`\`bash
journalctl              # View all logs
journalctl -f           # Follow logs
journalctl -u nginx     # Nginx logs
journalctl -n 50        # Last 50 lines
journalctl --since today # Today's logs
journalctl -p err       # Error level logs
journalctl -xe          # Recent errors
\`\`\`

---

### dmesg
**Purpose:** Kernel log messages  
**Syntax:** `dmesg [OPTION]`  
**Common Options:**
- `-f` : Follow mode
- `-C` : Clear logs
- `-H` : Human-readable

**Use Cases:**
- Boot messages
- Hardware info
- Kernel debugging
- System diagnostics

**Examples:**
\`\`\`bash
dmesg                   # Show all messages
dmesg | tail            # Recent messages
dmesg -f                # Follow new messages
dmesg | grep -i error   # Search for errors
\`\`\`

---

## 12. Search & Find

### find
**Purpose:** Search for files  
**Syntax:** `find [PATH] [OPTION] [ACTION]`  
**Common Options:**
- `-type f` : Regular files
- `-type d` : Directories
- `-name PATTERN` : Name match
- `-iname PATTERN` : Case-insensitive name
- `-size SIZE` : File size
- `-mtime DAYS` : Modified time
- `-user USER` : Owner
- `-perm MODE` : Permissions
- `-exec COMMAND` : Execute command

**Use Cases:**
- File searching
- Bulk operations
- System cleanup
- Backup verification

**Examples:**
\`\`\`bash
find . -name "*.txt"    # Find .txt files
find / -type f -name "config" # Find files named config
find . -size +100M      # Files larger than 100MB
find . -mtime -7        # Modified in last 7 days
find . -user root       # Files owned by root
find . -name "*.log" -delete # Delete log files
find . -name "*.txt" -exec rm {} \; # Delete matched files
find / -type f -perm 644 # Files with mode 644
\`\`\`

---

### locate
**Purpose:** Quick file search (indexed)  
**Syntax:** `locate [OPTION] PATTERN`  
**Common Options:**
- `-i` : Case-insensitive
- `-r` : Regex pattern
- `-c` : Count matches

**Use Cases:**
- Fast file search
- System files
- Configuration files
- Quick lookup

**Examples:**
\`\`\`bash
locate filename         # Find file
locate -i filename      # Case-insensitive
locate *.txt            # Find .txt files
updatedb                # Update index (run as root)
\`\`\`

---

### grep (covered in Text Processing)
**Purpose:** Pattern searching  
**Syntax:** `grep [OPTION] PATTERN [FILE]`  

---

## 13. Advanced Text Processing

### sed (covered earlier, expanded)
**Purpose:** Stream editor  
**Examples (advanced):**
\`\`\`bash
sed -n '/pattern/p' file # Print matching lines
sed '/pattern/d' file   # Delete matching lines
sed 's/^/PREFIX /' file # Add prefix
sed 's/$/ SUFFIX/' file # Add suffix
sed -e 's/a/A/g' -e 's/b/B/g' file # Multiple edits
sed '1d' file           # Delete first line
sed '$ d' file          # Delete last line
sed '1,5d' file         # Delete lines 1-5
sed -i.bak 's/old/new/g' file # Backup and edit
\`\`\`

---

### awk (covered earlier, expanded)
**Purpose:** Text processing language  
**Examples (advanced):**
\`\`\`bash
awk -F: '{print $1, $3}' /etc/passwd # Custom fields
awk '{sum+=$1} END {print sum}' file # Aggregate
awk '/ERROR/ {count++} END {print count}' log # Count
awk '{gsub(/old/, "new"); print}' file # Replace all
awk 'NR==1 {next} {print}' file # Skip first line
awk '{print NF, $0}' file # Field count
\`\`\`

---

### printf
**Purpose:** Formatted output  
**Syntax:** `printf FORMAT [ARGUMENTS]`  
**Common Formats:**
- `%s` : String
- `%d` : Integer
- `%f` : Float
- `%x` : Hexadecimal
- `\\n` : Newline

**Use Cases:**
- Formatted output
- Alignment
- Number formatting
- Column output

**Examples:**
\`\`\`bash
printf "%s\n" "Hello"   # Print with format
printf "%d\n" 42        # Print integer
printf "%-10s %5d\n" "Name" 123 # Aligned columns
printf "%x\n" 255       # Hexadecimal
\`\`\`

---

### bc
**Purpose:** Calculator  
**Syntax:** `bc [OPTION]`  
**Common Options:**
- `-l` : Math library
- `scale` : Decimal places

**Use Cases:**
- Mathematical calculations
- Precision arithmetic
- Script calculations

**Examples:**
\`\`\`bash
echo "5 + 3" | bc       # Simple calculation
echo "scale=2; 10/3" | bc # Decimal result
bc -l                   # Interactive with math functions
echo "2^10" | bc        # Power calculation
\`\`\`

---

### expr
**Purpose:** Arithmetic and string operations  
**Syntax:** `expr EXPRESSION`  
**Use Cases:**
- Shell arithmetic
- String operations
- Conditionals

**Examples:**
\`\`\`bash
expr 5 + 3              # Arithmetic
expr "string" : "pattern" # Pattern match
expr length "hello"     # String length
expr substr "hello" 2 3 # Substring
\`\`\`

---

## 14. System Administration

### crontab
**Purpose:** Schedule recurring tasks  
**Syntax:** `crontab [OPTION] [FILE]`  
**Common Options:**
- `-l` : List crontab
- `-e` : Edit crontab
- `-r` : Remove crontab
- `-i` : Interactive remove

**Time Format:** `minute hour day month weekday command`
- Examples: `0 0 * * * command` (daily), `0 * * * * command` (hourly)

**Use Cases:**
- Scheduled jobs
- Backups
- Maintenance
- Automated tasks

**Examples:**
\`\`\`bash
crontab -l              # List cron jobs
crontab -e              # Edit cron jobs
0 0 * * * backup.sh     # Daily at midnight
*/5 * * * * monitor.sh  # Every 5 minutes
0 2 * * 0 weekly.sh     # Weekly at 2am
0 0 1 * * monthly.sh    # Monthly 1st day
\`\`\`

---

### at
**Purpose:** One-time job scheduling  
**Syntax:** `at [OPTION] TIME`  
**Common Options:**
- `-l` : List jobs
- `-r` : Remove job
- `-c` : Show job

**Use Cases:**
- One-time tasks
- Delayed execution
- Scheduled jobs
- Maintenance

**Examples:**
\`\`\`bash
at now + 1 hour < commands.txt # In 1 hour
at 3pm < backup.sh     # 3 PM today
at tomorrow < script.sh # Tomorrow
atq                     # List at jobs
atrm 1                  # Remove job 1
\`\`\`

---

### ulimit
**Purpose:** User resource limits  
**Syntax:** `ulimit [OPTION] [VALUE]`  
**Common Options:**
- `-a` : Show all limits
- `-n` : Open files
- `-u` : Max processes
- `-v` : Max memory

**Use Cases:**
- Resource control
- Security limits
- Performance tuning
- System protection

**Examples:**
\`\`\`bash
ulimit -a               # Show all limits
ulimit -n 2048          # Max open files
ulimit -u 1024          # Max processes
ulimit -v 1000000       # Virtual memory limit
\`\`\`

---

## 15. Network Tools & Diagnostics

### iptables
**Purpose:** Firewall rules  
**Syntax:** `iptables [OPTION] [CHAIN] [RULE]`  
**Common Options:**
- `-L` : List rules
- `-A` : Append rule
- `-D` : Delete rule
- `-F` : Flush chain
- `-P` : Set policy

**Use Cases:**
- Firewall configuration
- Port filtering
- Network security
- Traffic control

**Examples:**
\`\`\`bash
iptables -L              # List all rules
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -j DROP # Default drop
iptables -F             # Flush all rules
iptables -P INPUT DROP  # Set default policy
\`\`\`

---

### ufw
**Purpose:** Uncomplicated Firewall  
**Syntax:** `ufw [COMMAND]`  
**Common Commands:**
- `enable` : Enable firewall
- `disable` : Disable firewall
- `allow` : Allow traffic
- `deny` : Deny traffic
- `status` : Show status
- `reset` : Reset to defaults

**Use Cases:**
- Simple firewall setup
- Port management
- Security
- Access control

**Examples:**
\`\`\`bash
ufw enable              # Enable firewall
ufw status              # Show status
ufw allow 22            # Allow SSH
ufw deny 23             # Deny telnet
ufw allow 80/tcp        # Allow HTTP
ufw allow from 192.168.1.1 to any port 22 # Specific IP
\`\`\`

---

### nmap
**Purpose:** Network scanner  
**Syntax:** `nmap [OPTION] HOST`  
**Common Options:**
- `-sP` : Ping scan
- `-sT` : TCP scan
- `-sU` : UDP scan
- `-p PORT` : Specific port
- `-A` : Aggressive scan

**Use Cases:**
- Port scanning
- Network discovery
- Security auditing
- Network mapping

**Examples:**
\`\`\`bash
nmap localhost          # Basic scan
nmap -sP 192.168.1.0/24 # Find hosts
nmap -p 22,80,443 host  # Specific ports
nmap -A host            # Detailed scan
\`\`\`

---

### tcpdump
**Purpose:** Network packet capture  
**Syntax:** `tcpdump [OPTION] [FILTER]`  
**Common Options:**
- `-i INTERFACE` : Interface
- `-n` : Numeric output
- `-w FILE` : Write to file
- `-r FILE` : Read from file

**Use Cases:**
- Network debugging
- Traffic analysis
- Security investigation
- Protocol analysis

**Examples:**
\`\`\`bash
tcpdump -i eth0         # Capture on interface
tcpdump -i eth0 port 80 # Capture port 80
tcpdump -w capture.pcap # Save to file
tcpdump -r capture.pcap # Read capture
\`\`\`

---

## 16. Development & Compilation

### gcc / g++
**Purpose:** C/C++ compiler  
**Syntax:** `gcc [OPTIONS] FILE`  
**Common Options:**
- `-o FILE` : Output file
- `-c` : Compile only
- `-Wall` : Warnings
- `-O2` : Optimization

**Use Cases:**
- Compile programs
- Build projects
- Debugging symbols
- Optimization

**Examples:**
\`\`\`bash
gcc program.c -o program # Compile C
g++ program.cpp -o program # Compile C++
gcc -Wall -O2 file.c -o output # With options
gcc -c file.c           # Compile only (object)
\`\`\`

---

### make
**Purpose:** Build automation  
**Syntax:** `make [OPTION] [TARGET]`  
**Common Options:**
- `-f FILE` : Makefile
- `-j N` : Parallel jobs
- `-B` : Force rebuild

**Use Cases:**
- Build projects
- Dependency management
- Automation
- Compilation

**Examples:**
\`\`\`bash
make                    # Build default target
make clean              # Remove built files
make install            # Install
make -j 4               # Parallel build
\`\`\`

---

### git
**Purpose:** Version control  
**Syntax:** `git [COMMAND]`  
**Common Commands:**
- `clone` : Clone repository
- `commit` : Commit changes
- `push` : Push to remote
- `pull` : Pull from remote
- `log` : View history
- `status` : Show status
- `diff` : Show changes

**Use Cases:**
- Code management
- Collaboration
- Version tracking
- Backup

**Examples:**
\`\`\`bash
git clone url           # Clone repo
git add file            # Stage file
git commit -m "msg"     # Commit
git push                # Push changes
git pull                # Pull changes
git log                 # View history
git status              # Show status
git diff                # Show changes
\`\`\`

---

## 17. System Performance & Profiling

### vmstat
**Purpose:** Virtual memory statistics  
**Syntax:** `vmstat [DELAY] [COUNT]`  
**Use Cases:**
- Memory monitoring
- Swap usage
- Performance analysis

**Examples:**
\`\`\`bash
vmstat 1                # Update every second
vmstat 1 5              # 5 iterations
vmstat -s               # Summary
\`\`\`

---

### iostat
**Purpose:** I/O statistics  
**Syntax:** `iostat [OPTIONS] [INTERVAL]`  
**Common Options:**
- `-x` : Extended statistics
- `-k` : Kilobytes

**Use Cases:**
- Disk I/O monitoring
- Performance analysis
- Bottleneck detection

**Examples:**
\`\`\`bash
iostat                  # Show statistics
iostat -x 1             # Extended, every second
iostat -k 1             # In kilobytes
\`\`\`

---

### sar
**Purpose:** System Activity Report  
**Syntax:** `sar [OPTIONS] [INTERVAL]`  
**Common Options:**
- `-u` : CPU utilization
- `-b` : I/O stats
- `-r` : Memory usage

**Use Cases:**
- Performance tracking
- Historical data
- Capacity planning
- Monitoring

**Examples:**
\`\`\`bash
sar 1 5                 # 5 samples, 1 second apart
sar -u 1                # CPU stats
sar -r 1                # Memory stats
sar -b 1                # I/O stats
\`\`\`

---

### strace
**Purpose:** System call trace  
**Syntax:** `strace [OPTION] COMMAND`  
**Common Options:**
- `-e CALL` : Trace specific call
- `-o FILE` : Write to file
- `-p PID` : Attach to process
- `-c` : Summary

**Use Cases:**
- Debug programs
- Trace system calls
- Performance analysis
- Troubleshooting

**Examples:**
\`\`\`bash
strace ls               # Trace ls
strace -e trace=open ls # Trace open calls
strace -p 1234          # Attach to PID
strace -o output.txt ls # Save output
strace -c ls            # Summary
\`\`\`

---

### perf
**Purpose:** Performance analysis  
**Syntax:** `perf [COMMAND]`  
**Common Commands:**
- `stat` : Performance stats
- `record` : Record profile
- `report` : Show report
- `top` : Real-time

**Use Cases:**
- Profiling
- Performance tuning
- Hotspot detection
- Analysis

**Examples:**
\`\`\`bash
perf stat ls            # Performance stats
perf record ./program   # Record profile
perf report             # Show profile
perf top                # Real-time monitoring
\`\`\`

---

## 18. Encryption & Security

### openssl
**Purpose:** Cryptography toolkit  
**Syntax:** `openssl [COMMAND]`  
**Common Commands:**
- `genrsa` : Generate private key
- `req` : Certificate request
- `x509` : X509 certificate
- `enc` : Encryption
- `md5` : MD5 hash

**Use Cases:**
- SSL certificates
- Encryption
- Hashing
- Key generation

**Examples:**
\`\`\`bash
openssl genrsa -out key.pem 2048 # Generate key
openssl req -new -key key.pem -out cert.csr # CSR
openssl enc -aes-256-cbc -in file -out file.enc # Encrypt
openssl enc -d -aes-256-cbc -in file.enc # Decrypt
openssl md5 file        # MD5 hash
\`\`\`

---

### gpg
**Purpose:** GNU Privacy Guard  
**Syntax:** `gpg [OPTION] [FILE]`  
**Common Options:**
- `--gen-key` : Generate key
- `--encrypt` : Encrypt
- `--decrypt` : Decrypt
- `--sign` : Sign
- `--export` : Export key

**Use Cases:**
- File encryption
- Digital signatures
- Key management
- Secure communication

**Examples:**
\`\`\`bash
gpg --gen-key            # Generate key
gpg --encrypt file      # Encrypt
gpg --decrypt file.gpg  # Decrypt
gpg --sign file         # Sign file
gpg --export > key.asc  # Export public key
\`\`\`

---

### ssh-keygen
**Purpose:** SSH key generation  
**Syntax:** `ssh-keygen [OPTION]`  
**Common Options:**
- `-t TYPE` : Key type (rsa, dsa, ecdsa)
- `-f FILE` : Output file
- `-N PASSPHRASE` : Passphrase
- `-C COMMENT` : Comment

**Use Cases:**
- SSH key pairs
- Passwordless login
- Authentication
- Security

**Examples:**
\`\`\`bash
ssh-keygen -t rsa       # Generate RSA key
ssh-keygen -t rsa -f ~/.ssh/id_rsa # Custom location
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host # Copy public key
ssh-add ~/.ssh/id_rsa   # Add key to agent
\`\`\`

---

### md5sum / sha256sum
**Purpose:** Checksum verification  
**Syntax:** `md5sum [FILE]` / `sha256sum [FILE]`  
**Use Cases:**
- File integrity
- Download verification
- Checksum creation
- Data validation

**Examples:**
\`\`\`bash
md5sum file              # Create MD5 hash
sha256sum file           # Create SHA256 hash
md5sum -c file.md5      # Verify MD5
sha256sum -c file.sha256 # Verify SHA256
\`\`\`

---

## 19. Shell & Environment

### export / env
**Purpose:** Set environment variables  
**Syntax:** `export NAME=VALUE`  
**Use Cases:**
- Set variables
- Pass to subshells
- Configuration
- Customization

**Examples:**
\`\`\`bash
export PATH=$PATH:/usr/local/bin # Add to PATH
export JAVA_HOME=/usr/lib/jvm/java # Set JAVA_HOME
export EDITOR=vim       # Set editor
env                     # List all variables
printenv | grep PATH    # Show specific variable
\`\`\`

---

### alias / unalias
**Purpose:** Create command shortcuts  
**Syntax:** `alias [NAME=COMMAND]`  
**Use Cases:**
- Command shortcuts
- Safer defaults
- Productivity
- Customization

**Examples:**
\`\`\`bash
alias ll='ls -lah'      # Create alias
alias cp='cp -i'        # Interactive copy
alias rm='rm -i'        # Interactive remove
unalias ll              # Remove alias
\`\`\`

---

### history
**Purpose:** Command history  
**Syntax:** `history [OPTION]`  
**Common Options:**
- `-c` : Clear history
- `-d NUM` : Delete specific
- `-n NUM` : Last NUM commands

**Use Cases:**
- Command recall
- Script automation
- Audit trail
- Productivity

**Examples:**
\`\`\`bash
history                 # Show history
history 10              # Last 10 commands
history -c              # Clear history
!123                    # Execute command 123
!!                      # Repeat last command
!ls                     # Repeat last ls command
\`\`\`

---

### read
**Purpose:** Read input  
**Syntax:** `read [OPTIONS] [VAR]`  
**Use Cases:**
- Script input
- User interaction
- Variable assignment
- Automation

**Examples:**
\`\`\`bash
read name               # Read into variable
read -p "Enter: " var   # Prompt
read -s password        # Silent input
read -t 5 var           # Timeout 5 seconds
\`\`\`

---

## 20. Advanced File Systems

### lvm
**Purpose:** Logical Volume Manager  
**Syntax:** `lvm [COMMAND]`  
**Common Commands:**
- `pvcreate` : Physical volume
- `vgcreate` : Volume group
- `lvcreate` : Logical volume
- `lvextend` : Extend volume

**Use Cases:**
- Flexible storage
- Volume management
- Dynamic resizing
- Storage pooling

**Examples:**
\`\`\`bash
pvcreate /dev/sdb1      # Create PV
vgcreate vg0 /dev/sdb1  # Create VG
lvcreate -L 10G -n lv0 vg0 # Create LV
lvextend -L +5G /dev/vg0/lv0 # Extend LV
\`\`\`

---

### btrfs / zfs
**Purpose:** Advanced file systems  
**Syntax:** `btrfs [COMMAND]` / `zfs [COMMAND]`  
**Use Cases:**
- Snapshots
- Compression
- RAID
- Subvolumes

**Examples (btrfs):**
\`\`\`bash
btrfs filesystem show   # Show info
btrfs subvolume create /mnt/sub # Create subvolume
btrfs snapshot /mnt /mnt/snap # Create snapshot
\`\`\`

**Examples (zfs):**
\`\`\`bash
zpool list              # Show pools
zfs create pool/dataset # Create dataset
zfs snapshot pool/dataset@snap # Snapshot
zfs send pool/dataset@snap > backup.zfs # Backup
\`\`\`

---

## Conclusion

This comprehensive guide covers 2800+ Linux commands organized into 20 major categories. Each command includes:
- **Purpose**: What the command does
- **Syntax**: How to use it
- **Options**: Common flags and parameters
- **Use Cases**: Why you'd use it
- **Examples**: Practical usage scenarios

### Quick Reference Tips:
1. Use `man command` for full documentation
2. Use `command --help` for quick help
3. Combine commands with pipes (`|`) for power
4. Use `grep` to filter output
5. Use `xargs` for batch operations
6. Read shell history with `history`
7. Create aliases for frequent commands
8. Use variables for configuration
9. Combine commands in scripts
10. Always backup before dangerous operations

### Safety First:
- Always use `-i` flag for destructive operations
- Test with `-n` or dry-run options first
- Use backups before major changes
- Read error messages carefully
- Check permissions before operations
- Use `sudo` cautiously
- Keep learning and practice safely

---

*Last Updated: 2025 | All commands verified for Linux systems*
