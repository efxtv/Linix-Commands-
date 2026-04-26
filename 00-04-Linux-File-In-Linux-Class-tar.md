# Complete Tar Command Guide

## Table of Contents

1. Basic Tar Operations
2. Creating Archives
3. Extracting Archives
4. Listing Archives
5. Compression Options
6. File Operations
7. Wildcards and Patterns
8. Permissions and Ownership
9. Incremental Backups
10. Exclude and Include
11. Remote Operations
12. Verification
13. Split and Combine
14. Tools and Utilities
15. Troubleshooting

---

## 1. Basic Tar Operations

Create a new tar archive.
```
tar -cvf archive.tar file1.txt file2.txt
```

Create an archive of a directory.
```
tar -cvf backup.tar /path/to/directory
```

Create archive with all files in current directory.
```
tar -cvf archive.tar *
```

Create archive showing progress.
```
tar -cvf archive.tar files/ -v
```

Create archive without compression.
```
tar -cvf backup.tar.gz files/ -G
```

Verbose mode showing file names.
```
tar -tvf archive.tar
```

Extract from standard input.
```
tar -xf - < archive.tar
```

Create to standard output.
```
tar -cf - files/ > archive.tar
```

Show version information.
```
tar --version
```

Show help.
```
tar --help
```

---

## 2. Creating Archives

Create archive named backup.tar.
```
tar -cf backup.tar documents/
```

Create archive in current directory.
```
tar -cf ./backup.tar ./
```

Create archive with absolute paths.
```
tar -cvf backup.tar -C /home username/
```

Create archive preserving paths.
```
tar -cvf backup.tar /home/user/documents
```

Create archive from multiple directories.
```
tar -cvf backup.tar dir1/ dir2/ dir3/
```

Create archive from specific files by list.
```
tar -cvf backup.tar -T filelist.txt
```

Create empty archive.
```
tar -cf empty.tar -T /dev/null
```

Create archive without parent directory.
```
tar -cvf backup.tar -C /home/user .
```

Create archive with numbered backup.
```
tar -cvf backup_$(date +%Y%m%d).tar files/
```

Create archive excluding parent folder.
```
cd /source/dir && tar -cvf ../backup.tar .
```

Create archive with hidden files.
```
tar -cvf backup.tar .[!.]* *
```

Create archive from home directory.
```
tar -cvf backup.tar ~/
```

Create archive excluding cache.
```
tar --exclude='*.cache' -cvf backup.tar files/
```

Create archive excluding git folder.
```
tar --exclude='.git' -cvf backup.tar project/
```

Create archive exclude multiple patterns.
```
tar --exclude='*.log' --exclude='*.tmp' -cvf backup.tar files/
```

Create archive of logs directory.
```
tar -cvf logs_backup.tar /var/log/
```

Create archive of config files.
```
tar -cvf config.tar /etc/*.conf
```

Create archive from symbolic links.
```
tar -cvfh backup.tar links/
```

Create archive preserving permissions.
```
tar -cvpMf backup.tar -C / source/
```

Create archive from network share.
```
tar -cvf backup.tar //network/share/folder
```

Create archive in tmp directory.
```
tar -cvf /tmp/backup.tar files/
```

Create archive on external drive.
```
tar -cvf /media/usb/backup.tar /home/user/docs
```

Create archive with timestamp in name.
```
tar -cvf backup_$(date +%Y-%m-%d_%H-%M-%S).tar files/
```

Create archive with full path.
```
tar -cvf /backup/$(hostname)_backup.tar /home/user
```

Create archive in chunks.
```
tar -cvL 100M -f backup.tar.partaa files/
```

Create archive with progress bar.
```
tar -czvf backup.tar.gz files/ --checkpoint=100
```

---

## 3. Extracting Archives

Extract all files from archive.
```
tar -xf archive.tar
```

Extract to specific directory.
```
tar -xf archive.tar -C /destination
```

Extract to current directory.
```
tar -xf archive.tar .
```

Extract with verbose output.
```
tar -xvf archive.tar
```

Extract preserving permissions.
```
tar -xpf archive.tar
```

Extract preserving timestamps.
```
tar -xpf archive.tar -m
```

Extract specific file.
```
tar -xf archive.tar filename.txt
```

Extract specific directory.
```
tar -xf archive.tar foldername/
```

Extract multiple files by pattern.
```
tar -xf archive.tar "*.txt"
```

Extract to different name.
```
tar -xf archive.tar -s '/original/newname/'
```

Extract only newer files.
```
tar -xf archive.tar -k
```

Extract without overwriting.
```
tar -xf archive.tar -K
```

Extract only if newer.
```
tar -xf archive.tar --overwrite-dir
```

Extract with strip components.
```
tar -xf archive.tar --strip-components=1
```

Extract from gzipped archive.
```
tar -xzf archive.tar.gz
```

Extract from bzip2 archive.
```
tar -xjf archive.tar.bz2
```

Extract from xz archive.
```
tar -xJf archive.tar.xz
```

Extract from stdin.
```
cat archive.tar.gz | tar -xzf -
```

Extract all archives in directory.
```
for f in *.tar.gz; do tar -xzf "$f"; done
```

Extract ignoring errors.
```
tar -xf archive.tar --ignore-unknown
```

Extract preserving ownership.
```
tar -xpf archive.tar --owner=0 --group=0
```

Extract without creating parent dirs.
```
tar -xpf archive.tar --no-recursion
```

Extract skip pre-extraction check.
```
tar -xf archive.tar --no-check-device
```

Extract and rename using script.
```
tar -xf archive.tar --transform='s/^old/new/'
```

Extract overwrite existing files.
```
tar -xf archive.tar -o
```

Extract only first archive in file.
```
tar -xf multi.tar
```

Extract symbolic links as files.
```
tar -xhf archive.tar
```

Extract with numeric owner.
```
tar -xpf archive.tar -numeric-owner
```

---

## 4. Listing Archives

List all files in archive.
```
tar -tf archive.tar
```

List with details.
```
tar -tvf archive.tar
```

List format with sizes.
```
tar -tvf archive.tar | awk '{print $3, $6}'
```

List only specific directory.
```
tar -tf archive.tar "folder/*"
```

List with numbered lines.
```
tar -tf archive.tar | nl
```

List count of files.
```
tar -tf archive.tar | wc -l
```

List matching pattern.
```
tar -tf archive.tar | grep ".txt"
```

List without directories.
```
tar -tf archive.tar | grep -v '/$'
```

List show file sizes.
```
tar -tvf archive.tar | awk '{print $5}' | numfmt --to=iec
```

List show modification time.
```
tar -tvf archive.tar | awk '{print $6, $7, $8}'
```

List format like ls -l.
```
tar -tvf archive.tar
```

List compressed archive.
```
tar -tzf archive.tar.gz
```

List sort by size.
```
tar -tvf archive.tar | sort -k5 -n
```

List sort by name.
```
tar -tf archive.tar | sort
```

List only directories.
```
tar -tf archive.tar | grep '/$'
```

List only files.
```
tar -tf archive.tar | grep -v '/$'
```

List unique file types.
```
tar -tf archive.tar | sed 's/.*\.//' | sort -u
```

List files sorted.
```
tar -tf archive.tar | sort | uniq
```

List in reverse order.
```
tar -tf archive.tar | tac
```

List with paths quoted.
```
tar -tf archive.tar -Q
```

List archive index.
```
tar -tf archive.tar --index-file=index.txt
```

List specific file info.
```
tar -tvf archive.tar filename
```

List show compression type.
```
file backup.tar.gz
```

---

## 5. Compression Options

Create gzip compressed archive.
```
tar -czvf backup.tar.gz files/
```

Create bzip2 compressed archive.
```
tar -cjvf backup.tar.bz2 files/
```

Create xz compressed archive.
```
tar -cJvf backup.tar.xz files/
```

Create lzip compressed archive.
```
tar -cvf backup.tar.lzip --lzip files/
```

Create lz4 compressed archive.
```
tar -cvf backup.tar.lz4 --lz4 files/
```

Create lzma compressed archive.
```
tar -cvf backup.tar.lzma --lzma files/
```

Create with compression level 9.
```
tar -czvf -9 backup.tar.gz files/
```

Create with compression level 1.
```
tar -czvf -1 backup.tar.gz files/
```

Create using gzip directly.
```
tar -cvf - files/ | gzip > backup.tar.gz
```

Create using pbzip2.
```
tar -cvf - files/ | pbzip2 > backup.tar.bz2
```

Create using pigz.
```
tar -cvf - files/ | pigz > backup.tar.gz
```

Create using pxz.
```
tar -cvf - files/ | pxz > backup.tar.xz
```

Auto-detect compression.
```
tar -cvf archive.tar.gz files/
```

Compression speed test gzip.
```
time tar -cvf - files/ | gzip > backup.tar.gz
```

Compression speed test bzip2.
```
time tar -cvf - files/ | bzip2 > backup.tar.bz2
```

Compression compare sizes.
```
tar -czvf backup.tar.gz files/ && ls -lh backup.tar.gz
```

Decompress with gunzip.
```
gunzip -c backup.tar.gz | tar -cvf -
```

Decompress with bunzip2.
```
bunzip2 -c backup.tar.bz2 | tar -cvf -
```

Decompress with xz.
```
xz -d -c backup.tar.xz | tar -cvf -
```

Extract gzipped archive.
```
tar -ixvf archive.tar.gz
```

Force compression format.
```
tar -a -cvf backup.tar.gz files/
```

Don't auto-detect compression.
```
tar -no-auto-compress -cvf backup.tar files/
```

Show compression info.
```
tar -tvzf backup.tar.gz
```

Use compress program.
```
gzip -d < backup.tar.gz | tar -tvf -
```

Extract compressed archive.
```
tar -I lbzip2 -xvf backup.tar.bz2
```

Use compress exclude.
```
tar -cvf - files/ | compress -c > backup.tar.Z
```

Use lzop compression.
```
tar -cvf - files/ | lzop > backup.tar.lzo
```

Test archive compression.
```
tar -czf test.tar.gz files/ && gzip -t test.tar.gz
```

---

## 6. File Operations

Add file to archive.
```
tar -rvf archive.tar newfile.txt
```

Add multiple files.
```
tar -rvf archive.tar file1.txt file2.txt
```

Add directory to archive.
```
tar -rvf archive.tar newdir/
```

Add files matching pattern.
```
tar -rvf archive.tar "*.log"
```

Update existing files only.
```
tar -uvf archive.tar files/
```

Delete file from archive.
```
tar --delete -f archive.tar filename.txt
```

Delete files by pattern.
```
tar --delete -f archive.tar "*.tmp"
```

Remove after extraction.
```
tar -xvf archive.tar --remove-files
```

Move within archive.
```
tar --delete -f archive.tar old.txt | tar -cvf archive.tar -T -
```

Rename in archive.
```
tar --rename -vvf archive.tar oldname newname
```

Rename multiple files.
```
tar --rename -vf archive.tar 's/^/prefix_/'
```

Extract and remove archive.
```
tar -xvf archive.tar --remove-files
```

Synchronize archive.
```
tar -cvf archive.tar --files-list=<(find . -type f)
```

List new files to add.
```
find . -newer backup.tar -print | tar -cvf backup.tar -T -
```

Deduplicate files.
```
tar --delete -f archive.tar $(tar -tf archive.tar | sort | uniq -d)
```

Change file permissions.
```
tar -xvf archive.tar --to-command='chmod 644 "$TAR_FILENAME"'
```

List new or changed files.
```
tardiff -backup.tar newer_file
```

Create archive with hardlinks preserved.
```
tar -cvfh archive.tar directory/
```

Extract and execute.
```
tar -xvf archive.tar -C / --to-command='bash'
```

Convert absolute to relative.
```
tar -xvf archive.tar --strip-components=2
```

Apply patch from archive.
```
tar -xf patch.tar '<patch>' | patch -p1
```

Verify file in archive.
```
tar -tf archive.tar filename && echo "Found"
```

Check file integrity.
```
tar -tf archive.tar > /dev/null && echo "OK"
```

Extract to stdout.
```
tar -Of archive.tar filename > output.txt
```

Compare archive contents.
```
tar -dvf archive.tar
```

List differences.
```
tar -Wvf archive.tar
```

Update archive.
```
tar -uvf archive.tar files/
```

Append archives.
```
tar -Af archive1.tar archive2.tar
```

Concatenate archives.
```
tar -cf combined.tar -M file1.tar -M file2.tar
```

---

## 7. Wildcards and Patterns

Extract all .txt files.
```
tar -xf archive.tar "*.txt"
```

Extract all .log files.
```
tar -xf archive.tar --wildcards "*.log"
```

Extract php files.
```
tar -xf archive.tar "*.php"
```

Extract all .jpg images.
```
tar -xf archive.tar "*.jpg"
```

Extract all .pdf documents.
```
tar -xf archive.tar "*.pdf"
```

Extract .sh scripts.
```
tar -xf archive.tar "*.sh"
```

Extract .conf files.
```
tar -xf archive.tar "*.conf"
```

Extract .json files.
```
tar -xf archive.tar "*.json"
```

Extract .xml files.
```
tar -xf archive.tar "*.xml"
```

Extract .html files.
```
tar -xf archive.tar "*.html"
```

Extract .css files.
```
tar -xf archive.tar "*.css"
```

Extract .js files.
```
tar -xf archive.tar "*.js"
```

Extract files in docs folder.
```
tar -xf archive.tar "docs/*"
```

Extract files in subfolder.
```
tar -xf archive.tar "*/data.txt"
```

Extract in specific path.
```
tar -xf archive.tar "subdir/*.txt"
```

Extract using glob pattern.
```
tar -xf archive.tar --glob '*.{txt,log}'
```

Extract multiple patterns.
```
tar -xf archive.tar -T filelist.txt
```

List matching pattern.
```
tar -tf archive.tar --wildcards "*.txt"
```

List all text files.
```
tar -tf archive.tar --wildcards "*.{txt,doc,pdf}"
```

Find in archive.
```
tar -tf archive.tar | grep -i 'error'
```

Add matching files.
```
tar -cvf archive.tar --wildcards '*.txt'
```

Exclude pattern.
```
tar -cvf archive.tar --exclude='*.log' files/
```

Exclude directory.
```
tar -cvf archive.tar --exclude='.git' project/
```

Exclude multiple patterns.
```
tar -cvf backup.tar --exclude='{.tmp,.cache}' files/
```

Exclude by size.
```
tar -cvf archive.tar --exclude='*.{o,so,class}'
```

Use regex exclude.
```
tar -cvf backup.tar --exclude-regex='^.*\.log$' files/
```

List using grep.
```
tar -xf archive.tar | grep 'pattern'
```

Find new files.
```
find . -name "*.new" | tar -cvf backup.tar -T -
```

Archive only changed.
```
tar -cvf backup.tar -N backup.tar files/
```

Archive newer than date.
```
tar -cvf backup.tar -N "2024-01-01" files/
```

Archive after date.
```
tar -cvf backup.tar --after-date="2024-01-01" files/
```

List modified after date.
```
tar -tf archive.tar --newer="2024-01-01"
```

Extract from archive newer.
```
tar -xf archive.tar --newer-mtime="2024-01-01"
```

Extract modified after.
```
tar -xf archive.tar --newer-than=file.txt
```

---

## 8. Permissions and Ownership

Extract preserving permissions.
```
tar -xpf archive.tar
```

Extract as root with owner.
```
tar -xpf archive.tar --same-owner
```

Preserve permissions.
```
tar -cvf archive.tar -p files/
```

Use numeric owner.
```
tar -xpf archive.tar -numeric-owner
```

Ignore permission errors.
```
tar -xpf archive.tar --unlink
```

Set default permissions.
```
umask 077 && tar -cvf archive.tar files/
```

Show permission errors.
```
tar -xvf archive.tar --posix 2>&1
```

Check permissions.
```
tar -tvf archive.tar | head -5
```

Preserve access times.
```
tar -cvf archive.tar -p --atime-preserve files/
```

Preserve device files.
```
tar -cvf archive.tar --preserve -H files/
```

Preserve all attributes.
```
tar -cvf archive.tar -p -a files/
```

Extract read-only as root.
```
tar -xpf archive.tar --mode=o=r
```

Preserve SELinux.
```
tar -cvf archive.tar --selinux files/
```

Preserve ACLs.
```
tar -cvf archive.tar --acls files/
```

Preserve xattrs.
```
tar -cvf archive.tar --xattrs files/
```

Preserve all.
```
tar -cvf archive.tar -p -a --numeric-owner files/
```

Ignore owner mismatch.
```
tar -xf archive.tar -o
```

Save owner info.
```
tar -cvf archive.tar --numeric-owner files/
```

Restore owner.
```
tar -xpf archive.tar -o --no-same-owner
```

Set owner.
```
tar -xf archive.tar --owner=username
```

Set group.
```
tar -xf archive.tar --group=groupname
```

Chmod extracted files.
```
tar -xf archive.tar --to-command='chmod 644 $TAR_FILE'
```

Chown extracted.
```
tar -xf archive.tar --to-command='chown user:group $TAR_FILE'
```

Set timestamp.
```
tar -xf archive.tar --touch
```

Ignore time.
```
tar -xf archive.tar -m
```

Preserve modification time.
```
tar -xvf archive.tar --preserve-modification-time
```

---

## 9. Incremental Backups

Create full backup.
```
tar -cvf full_backup.tar /home/user
```

Create incremental backup.
```
tar -cvf inc_backup.tar -N full_backup.tar /home/user
```

Backup changed files.
```
tar -cvf new_backup.tar -N last_backup.tar files/
```

Backup newer than file.
```
tar -cvf backup.tar -N marker_file files/
```

Backup after date.
```
tar -cvf backup.tar --after-date="2024-01-01" files/
```

Backup newer modified.
```
tar -cvf backup.tar --newer-mtime file.txt files/
```

List newer files.
```
tar -tf backup.tar --newer-than=file.txt
```

Restore incremental.
```
tar --incremental -xvf backup.tar
```

Create snapshot.
```
tar -cvf snapshot.snap .
```

Backup with snapshot.
```
tar -cvf backup.tar -g snapshot.snap files/
```

Diff backup.
```
tar -dvf backup1.tar backup2.tar
```

Merge archives.
```
tar -cvf merged.tar -M backup1.tar -M backup2.tar
```

Generate file list.
```
find . -type f -newer backup.tar > filelist.txt
```

Create from filelist.
```
tar -cvf backup.tar -T filelist.txt
```

Backup with tarfile module.
```
python3 -m tarfile -c backup.tar files/
```

Extract tarfile.
```
python3 -m tarfile -e backup.tar destination/
```

List tarfile.
```
python3 -m tarfile -l backup.tar
```

Check tarfile.
```
python3 -m tarfile -c backup.tar --check
```

Create diff backup.
```
tar -cvf old.tar && touch marker && tar -cvf new.tar --compare files/
```

Script backup.
```
#!/bin/bash
BACKUP="backup_$(date +%F).tar"
tar -cvf $BACKUP files/ && ls -lh $BACKUP
```

Backup rotation script.
```
#!/bin/bash
for i in 4 3 2 1; do [ -f "day$i.tar" ] && mv "day$i.tar" "day$((i+1)).tar"; done
tar -cvf day1.tar files/
```

Auto backup.
```
0 2 * * * tar -cvf /backup/$(date +\%Y\%m\%d).tar /home/user
```

Incremental restore.
```
for f in $(ls -1 day*.tar); do tar -xvf "$f"; done
```

---

## 10. Exclude and Include

Exclude all .log files.
```
tar -cvf backup.tar --exclude='*.log' files/
```

Exclude .tmp files.
```
tar -cvf backup.tar --exclude='*.tmp' files/
```

Exclude .cache directory.
```
tar -cvf backup.tar --exclude='.cache' files/
```

Exclude .git folder.
```
tar -cvf backup.tar --exclude='.git' project/
```

Exclude node_modules.
```
tar -cvf backup.tar --exclude='node_modules' project/
```

Exclude __pycache__.
```
tar -cvf backup.tar --exclude='__pycache__' files/
```

Exclude hidden files.
```
tar -cvf backup.tar --exclude='.*' files/
```

Exclude system files.
```
tar -cvf backup.tar --exclude='/proc' --exclude='/sys' --exclude='/dev' /
```

Only include .txt files.
```
tar -cvf backup.tar --include='*.txt' --include='*.md' files/
```

Only include source files.
```
tar -cvf backup.tar --include='*.c' --include='*.h' src/
```

Include specific directory.
```
tar -cvf backup.tar --include='logs/*' app/
```

Exclude then include.
```
tar -cvf backup.tar --exclude='*.tmp' --include='important.tmp' files/
```

Exclude multiple types.
```
tar -cvf backup.tar --exclude={*.log,*.tmp,*.cache} files/
```

Exclude pattern list.
```
tar -cvf backup.tar -X exclude.txt files/
```

Create exclude file.
```
echo "*.log" > exclude.txt
echo "*.tmp" >> exclude.txt
tar -cvf backup.tar -X exclude.txt files/
```

Exclude from file.
```
tar -cvf backup.tar --exclude-from=exclude.txt files/
```

Include only directories.
```
tar -cvf backup.tar --include='*/' --include='*.c' --exclude='*' files/
```

Include some files.
```
tar -cvf backup.tar -T include.txt files/
```

Filter archive.
```
tar -cvf backup.tar --posix --exclude='*.o' files/
```

No exclusion.
```
tar -cvf backup.tar --no-auto-exclude files/
```

Force exclude.
```
tar -cvf backup.tar --ignore-failed-read files/
```

Skip denied files.
```
tar -cvf backup.tar --no-ignore-recursive files/
```

Ignore archive errors.
```
tar -xvf archive.tar --ignore-zeros
```

Skip filesystem errors.
```
tar -xf archive.tar --ignore-failed-read
```

Skip permission errors.
```
tar -xf archive.tar --skip-old-files
```

Don't restore ownership.
```
tar -xf archive.tar --no-same-owner --no-same-permissions
```

Skip based on size.
```
tar -xf archive.tar --preserve -s /dev/null
```

---

## 11. Remote Operations

Create archive on remote server.
```
ssh user@server "tar -cf - files/" > backup.tar
```

Extract archive on remote.
```
ssh user@server "tar -xf - -C /destination" < backup.tar
```

Copy via tar ssh.
```
tar -cvf - files/ | ssh user@server "tar -xvf - -C /destination"
```

Compress send to remote.
```
tar -czvf - files/ | ssh user@server "cat > backup.tar.gz"
```

Receive decompress from remote.
```
ssh user@server "tar -czvf - files/" | tar -xzvf -
```

Sync to remote.
```
tar -cvf - files/ | ssh user@server "tar -xvf - -C /destination"
```

Remote backup.
```
ssh user@backup-server "tar -czvf - /home" > backup.tar.gz
```

Remote restore.
```
ssh user@backup-server "tar -czf - /home" | tar -xzvf -
```

Copy with progress.
```
tar -cvf - files/ | pv | ssh user@server "tar -xf - -C /dest"
```

Backup to remote.
```
tar -cvf - files/ | gzip | ssh user@server "gunzip > backup.tar.gz"
```

Migrate system.
```
tar -cvf - --one-file-system / | ssh user@newserver "tar -xvf - -C /"
```

Pipe through ssh.
```
tar -cf - files/ | ssh -C user@server "cat > backup.tar"
```

Netcat backup.
```
tar -cvf - files/ | nc -q0 server 9000
```

Receive tar backup.
```
nc -l -p 9000 | tar -xvf -
```

SSH tar pipe.
```
tar -cvf - files/ | ssh -C user@host "tar -xvf - -C /dest"
```

Rync with tar.
```
tar -cvf - files/ | rsync --progress -avz user@server:/dest/-
```

Remote sync.
```
rsync -avz --progress directory/ user@server:/backup/
```

SFTP upload.
```
sftp user@server
sftp> put backup.tar /backup/
sftp> bye
```

Scp upload.
```
scp backup.tar user@server:/backup/
```

Rsync incremental.
```
rsync -avz --link-dest=prev_backup backup/ user@server:/backup/
```

---

## 12. Verification

Verify archive integrity.
```
tar -tf archive.tar > /dev/null 2>&1 && echo "OK"
```

Test archive.
```
tar -tzf archive.tar.gz
```

Check compressed archive.
```
gzip -t archive.tar.gz
```

Check all files.
```
tar -tf archive.tar | while read f; do [ -e "$f" ] || echo "Missing: $f"; done
```

Compare archive contents.
```
tar -dvf archive1.tar archive2.tar
```

Test extraction without.
```
tar -tf archive.tar
```

Verify extraction.
```
tar -xvf archive.tar -C /tmp --dry-run
```

Check signatures.
```
gpg --verify backup.tar.gz.sig backup.tar.gz
```

Calculate checksum.
```
md5sum backup.tar.gz
```

Verify checksum.
```
md5sum -c checksums.md5
```

Generate sha256.
```
sha256sum backup.tar.gz > checksums.sha256
```

Verify sha256.
```
sha256sum -c checksums.sha256
```

List differences.
```
tar -Wvf archive.tar
```

Check file by file.
```
tar -tf archive.tar | wc -l
```

Verify tar file.
```
tar -tvf archive.tar | head -10
```

Test with verbose.
```
tar -tvfz archive.tar.gz
```

Verify bzip2 integrity.
```
bzip2 -t archive.tar.bz2
```

Verify xz integrity.
```
xz -t archive.tar.xz
```

Test lzip.
```
lzip -t archive.tar.lz
```

Test all archives.
```
for f in *.tar*; do tar -tf "$f" > /dev/null && echo "$f OK"; done
```

Check corrupted archive.
```
tar -xf archive.tar -O > /dev/null 2>&1
```

Extract test.
```
tar -xf archive.tar -C /tmp/test --dry-run
```

Check format.
```
file archive.tar
```

Show tar type.
```
tar -tvf archive.tar | head -1
```

Verify in-place.
```
tar -cMf archive.tar files/ 2>&1
```

---

## 13. Split and Combine

Split archive into parts.
```
split -b 100M backup.tar.gz backup_part_
```

Combine parts.
```
cat backup_part_* > backup.tar.gz
```

Split using dd.
```
dd if=backup.tar.gz bs=1M count=100 part
```

Split with size limit.
```
tar -cvL 100M -f backup.partaa files/
```

Show split files.
```
ls -lh backup_part_*
```

Recombine all parts.
```
cat backup_part_a* > original.tar.gz
```

Split with suffix.
```
split -d -a 2 -b 100M backup.tar.gz backup.
```

Numbered parts.
```
split -d -b 100M backup.tar.gz backup. --additional-suffix=.tar.gz
```

Multi-volume extract.
```
tar -M -f vol1.tar -f vol2.tar -f vol3.tar
```

Create multi-volume.
```
tar -cvM -f 1.tar -f 2.tar -f 3.tar files/
```

Continue multi-volume.
```
tar -Mv -f 1.tar -f 2.tar
```

Volume size.
```
tar -cvM -L 100m -f backup.tar files/
```

Auto volume.
```
tar -cvM -f backup.tar files/
```

Extract volume.
```
tar -xf vol1.tar -M
```

Split archive.
```
tar -czvf - files/ | split -b 100M - backup.tar.gz
```

Join split files.
```
cat backup.tar.gz* | tar -xzvf -
```

Volume info.
```
tar -tvf vol1.tar | tail -5
```

List all volumes.
```
ls -1 vol*.tar
```

Combine many archives.
```
tar -cvf combined.tar file1.tar file2.tar file3.tar
```

Merge archives.
```
tar -Af combined.tar part1.tar part2.tar
```

Concatenate archives.
```
tar -cvf new.tar -M backup1.tar -M backup2.tar
```

Extract large archive.
```
tar -xvf large.tar --tape-length=1024
```

Continue extraction.
```
tar -xvf large.tar -M
```

---

## 14. Tools and Utilities

View using vim.
```
vim backup.tar
```

Edit using vim.
```
vim backup.tar
```

List with less.
```
tar -tf archive.tar | less
```

Search with grep.
```
tar -tf archive.tar | grep 'pattern'
```

Count files.
```
tar -tf archive.tar | wc -l
```

Sort files.
```
tar -tf archive.tar | sort
```

Unique files.
```
tar -tf archive.tar | sort -u
```

List with tree.
```
tar -tf archive.tar | tree -L 2
```

View with find.
```
tar -tf archive.tar | find . -name '*pattern*'
```

Use awk.
```
tar -tvf archive.tar | awk '{print $6}'
```

Convert to csv.
```
tar -tvf archive.tar | awk '{print $1,$6}' OFS=',' > files.csv
```

Generate manifest.
```
tar -tf archive.tar | sort > manifest.txt
```

Check manifest.
```
diff <(tar -tf new.tar | sort) manifest.txt
```

Extract file list.
```
tar -tf archive.tar > filelist.txt
```

Use cut.
```
tar -tvf archive.tar | cut -d' ' -f6
```

Sed replace.
```
tar -tf archive.tar | sed 's/old/new/'
```

Use xargs.
```
tar -tf archive.tar | xargs -I {} echo {}
```

Archive from find.
```
find . -type f -name "*.txt" -print0 | tar -cvf archive.tar --null -T -
```

Archive modified.
```
find . -newer backup.tar -print0 | tar -cvf backup.tar --null -T -
```

Backup with rsync.
```
rsync -av --delete backup/ /destination/
```

Diff backup.
```
diff -rq backup1 backup2
```

Compare archives.
```
tar -dvf archive1.tar archive2.tar
```

Sync directories.
```
cd dir1 && tar -cf - . | (cd ../dir2 && tar -xvf -)
```

Copy directory.
```
tar -cf - dir/ | tar -xf - -C /destination
```

Mirror directory.
```
tar -cvf - . | (cd /dest && tar -xvf -)
```

Backup with timestamp.
```
tar -cvzf "/backup/$(date +\%Y\%m\%d_\%H\%M\%S).tar.gz" .
```

Cron backup.
```
0 3 * * * tar -cvzf /backups/backup.tar.gz /home/user
```

Auto delete old.
```
find /backup -mtime +30 -delete
```

Rotate backups.
```
for f in $(seq 7 -1 1); do mv day_$f.tar day_$((f+1)).tar; done
mv today.tar day_1.tar
```

Cleanup archive.
```
tar -xf archive.tar --delete "*.tmp"
```

Deduplicate.
```
tar -xf archive.tar -s '/old/new/'
```

Filter archive.
```
tar -cf - files/ | grep -v '\.log$' | tar -cf new.tar -
```

Transform paths.
```
tar -xf archive.tar --transform='s/^\///'
```

Prefix paths.
```
tar -xf archive.tar --transform='s/^/prefix_/'
```

Suffix paths.
```
tar -xf archive.tar --transform='s/$/.bak/'
```

Lowercase names.
```
tar -xf archive.tar --transform='s/.*/\L&/'
```

Uppercase names.
```
tar -xf archive.tar --transform='s/.*/\U&/'
```

Replace spaces.
```
tar -xf archive.tar --transform='s/ /_/g'
```

Remove extension.
```
tar -xf archive.tar --transform='s/\.[^.]*$//'
```

Add extension.
```
tar -xf archive.tar --transform='s/$/.txt/'
```

---

## 15. Troubleshooting

Get error details.
```
tar -xvf archive.tar -v 2>&1
```

Show all warnings.
```
tar -cvf archive.tar files/ -w
```

Ignore errors.
```
tar -xvf archive.tar -i
```

Skip bad blocks.
```
tar -xvf archive.tar -k
```

Continue on error.
```
tar -xvf archive.tar -C / --continue-on-error
```

Ignore unknown.
```
tar -xvf archive.tar --ignore-unknown
```

Show help.
```
tar --help | less
```

Show manual.
```
man tar
```

Show examples.
```
tar --help | grep -A 5 Example
```

Fix symbolic links.
```
tar -hxvf archive.tar
```

Fix hardlinks.
```
tar -xvf archive.tar -h
```

Unlink before extract.
```
tar -xvf archive.tar --unlink-first
```

Overwrite files.
```
tar -xvf archive.tar --overwrite
```

Don't overwrite.
```
tar -xvf archive.tar --no-overwrite
```

Skip existing.
```
tar -xvf archive.tar --skip-old-files
```

Keep newer.
```
tar -xvf archive.tar --keep-newer-files
```

Keep older.
```
tar -xvf archive.tar --keep-old-files
```

Preserve links.
```
tar -xvf archive.tar -h
```

Extract sparse files.
```
tar -xvf archive.tar --sparse
```

Ignore sizes.
```
tar -xvf archive.tar -s
```

Check free space.
```
df -h /destination
```

Check permissions.
```
ls -ld destination/
```

Check ownership.
```
ls -ln destination/
```

Check disk space.
```
du -sh destination/
```

Read error log.
```
tar -xvf archive.tar 2> error.log
```

Redirect errors.
```
tar -xvf archive.tar 2>&1 | tee log.txt
```

Verbose errors.
```
tar -tvf archive.tar -w 2>&1 | head -10
```

Debug mode.
```
tar -xvf archive.tar -v -v -v
```

Show tar options.
```
tar --show-options
```

Show defaults.
```
tar --show-defaults
```

List formats.
```
tar --list-formats
```

Check compression.
```
tar -tvf archive.tar
```

Verify tar version.
```
tar --version
```

Check gzip version.
```
gzip --version
```

Check bzip2 version.
```
bzip2 --version
```

Check xz version.
```
xz --version
```

Update tar.
```
apt update && apt install tar
```

Reinstall tar.
```
apt install --reinstall tar
```

Install tools.
```
apt install tar gzip bzip2 xz-utils
```

Install with contrib.
```
apt install tar contrib
```

Add to path.
```
export PATH=$PATH:/usr/local/bin
```

Use absolute path.
```
/bin/tar -cvf archive.tar files/
```

Check library.
```
ldd /bin/tar
```

Check dependencies.
```
ldd /bin/tar | grep not
```

Strace tar.
```
strace -e trace=open,read tar -xvf archive.tar
```

Trace with filter.
```
strace -f tar -xvf archive.tar 2>&1 | grep -i error
```

Profile tar.
```
time tar -cvf archive.tar files/
```

Measure speed.
```
time tar -xvf archive.tar
```

Show processes.
```
ps aux | grep tar
```

Show open files.
```
lsof | grep tar
```

Check locks.
```
lsof | grep archive.tar
```

Show file descriptors.
```
ls /proc/$(pgrep tar)/fd
```

Monitor system.
```
top
```

Check dmesg.
```
dmesg | tail -20
```

Check syslog.
```
tail -f /var/log/syslog
```

Check messages.
```
tail -f /var/log/messages
```

Show kernel ring.
```
dmesg -T
```

Clear cache.
```
sync && echo 3 > /proc/sys/vm/drop_caches
```

Increase buffer.
```
tar -cvf archive.tar files/ --record-size=512
```

Use large records.
```
tar -cvf archive.tar files/ -- Blocking-size=10240
```

Read with buffer.
```
tar -xvf archive.tar --read-full-records
```

---

## Quick Reference

### Create Archive
```
tar -cvf archive.tar files/
```

### Extract Archive
```
tar -xvf archive.tar
```

### List Contents
```
tar -tvf archive.tar
```

### Compress (gzip)
```
tar -czvf archive.tar.gz files/
```

### Compress (bzip2)
```
tar -cjvf archive.tar.bz2 files/
```

### Extract (gzip)
```
tar -xzvf archive.tar.gz
```

### Add File
```
tar -rvf archive.tar newfile
```

### Delete File
```
tar --delete -f archive.tar file
```

---

## Common Options

| Option | Description |
|--------|------------|
| `-c` | Create archive |
| `-x` | Extract archive |
| `-t` | List contents |
| `-v` | Verbose |
| `-f` | File name |
| `-z` | gzip compression |
| `-j` | bzip2 compression |
| `-J` | xz compression |
| `-p` | Preserve permissions |
| `-C` | Change directory |
| `-A` | Concatenate |
| `-d` | Diff |
| `-r` | Append |
| `-u` | Update |
| `-k` | Keep |
| `-M` | Multi-volume |
| `-N` | Newer than |
| `-exclude` | Exclude pattern |
| `--wildcards` | Use wildcards |

---

## Examples

### Backup /home
```
tar -cvzf /backup/home_$(date +%Y%m%d).tar.gz /home
```

### Backup /etc
```
tar -cvzf /backup/etc_$(date +%Y%m%d).tar.gz /etc
```

### Full system backup
```
tar -cvpzf /backup/full_$(date +%Y%m%d).tar.gz --exclude=/proc --exclude=/sys --exclude=/dev --exclude=/run --exclude=/tmp /
```

### Incremental backup
```
tar -cvzf /backup/incremental_$(date +%Y%m%d).tar.gz -N /backup/last.tar.gz /
```

### Remote backup
```
tar -cvzf - /home | ssh user@server "cat > /backup/home.tar.gz"
```

### Extract single file
```
tar -xzf archive.tar.gz path/to/file.txt
```

### Extract to USB
```
tar -xvf archive.tar -C /media/usb
```

### List compressed
```
tar -tzvf archive.tar.gz
```

### Create from find
```
find /home -mtime -7 | tar -cvzf backup.tar.gz -T -
```

### Verify backup
```
tar -tzf backup.tar.gz > /dev/null && echo "OK"
```

### Clone system
```
tar -cvpf - / | ssh user@newserver "tar -xvpf - -C /"
```

---

### Thank you 
Contact if you want to join our classes/support: 
[EFXTV](https://t.me/efxtv)

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/efxtv)
