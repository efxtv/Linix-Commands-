# Complete Grep Command Guide

## Table of Contents

1. Basic Grep Operations
2. Pattern Matching
3. Regular Expressions
4. Case Sensitivity
5. Line Operations
6. Count and Numbering
7. Inverting Matches
8. Context Options
9. File Options
10. Colored Output
11. Fixed Strings
12. Extended Regex
13. Recursive Search
14. Multiple Patterns
15. Multiple Files
16. Performance
17. Output redirection
18. Pipe Operations
19. Combined Searches
20. Troubleshooting


## 1. Basic Grep Operations

Search for a pattern in a file.
```
grep "pattern" file.txt
```

Search in multiple files.
```
grep "pattern" file1.txt file2.txt
```

Search case sensitively.
```
grep "PATTERN" file.txt
```

Search case insensitively.
```
grep -i "pattern" file.txt
```

Search entire word.
```
grep -w "pattern" file.txt
```

Search entire phrase.
```
grep -F "exact phrase" file.txt
```

Search with line numbers.
```
grep -n "pattern" file.txt
```

Show only matching part.
```
grep -o "pattern" file.txt
```

Count matches.
```
grep -c "pattern" file.txt
```

Show non-matching lines.
```
grep -v "pattern" file.txt
```

Quiet mode (exit code only).
```
grep -q "pattern" file.txt
```

Show filename only.
```
grep -l "pattern" *.txt
```

Show files without match.
```
grep -L "pattern" *.txt
```

Binary files skip.
```
grep -I "pattern" file.txt
```

Directory search.
```
grep -r "pattern" directory/
```

Recursive with dirs.
```
grep -R "pattern" /path
```

No header.
```
grep -H "pattern" file.txt
```

Force header.
```
grep -h "pattern" file1.txt file2.txt
```

Show byte offset.
```
grep -b "pattern" file.txt
```

Byte offset only.
```
grep -o -b "pattern" file.txt
```

Maximum count.
```
grep -m 10 "pattern" file.txt
```

Show all matches.
```
grep -c "" file.txt
```

Binary matches.
```
grep -a "pattern" file.bin
```

Read null separated.
```
grep -z "pattern" file.txt
```

---

## 2. Pattern Matching

Match any character.
```
grep "p.ttern" file.txt
```

Match any char set.
```
grep "p[aeiou]ttern" file.txt
```

Match digit.
```
grep "[0-9]" file.txt
```

Match lowercase.
```
grep "[a-z]" file.txt
```

Match uppercase.
```
grep "[A-Z]" file.txt
```

Match alphabetic.
```
grep "[a-zA-Z]" file.txt
```

Match alphanumeric.
```
grep "[a-zA-Z0-9]" file.txt
```

Match not digit.
```
grep "[^0-9]" file.txt
```

Match not letter.
```
grep "[^a-zA-Z]" file.txt
```

Match range.
```
grep "[a-f]" file.txt
```

Match multiple.
```
grep "error|warning" file.txt
```

Or pattern.
```
grep -E "error|warning" file.txt
```

Start of line.
```
grep "^pattern" file.txt
```

End of line.
```
grep "pattern$" file.txt
```

Exact line.
```
grep "^pattern$" file.txt
```

Empty line.
```
grep "^$" file.txt
```

Non-empty line.
```
grep "." file.txt
```

Word boundary.
```
grep "\bword\b" file.txt
```

Non-word boundary.
```
grep "\Bword\B" file.txt
```

Start of word.
```
grep "<word" file.txt
```

End of word.
```
grep "word>" file.txt
```

Zero or more.
```
grep "patt*" file.txt
```

One or more.
```
grep -E "patt+" file.txt
```

Zero or one.
```
grep -E "patt?" file.txt
```

Exactly n times.
```
grep -E "patt{2}" file.txt
```

At least n times.
```
grep -E "patt{2,}" file.txt
```

Between n and m.
```
grep -E "patt{2,4}" file.txt
```

Any single char.
```
grep "." file.txt
```

Escaped special.
```
grep "\." file.txt
```

Tab character.
```
grep "\t" file.txt
```

Newline character.
```
grep "\n" file.txt
```

Backslash.
```
grep "\\" file.txt
```

Quote.
```
grep "\"" file.txt
```

Any digit.
```
grep "[[:digit:]]" file.txt
```

Any lower.
```
grep "[[:lower:]]" file.txt
```

Any upper.
```
grep "[[:upper:]]" file.txt
```

Any alpha.
```
grep "[[:alpha:]]" file.txt
```

Any alnum.
```
grep "[[:alnum:]]" file.txt
```

Any space.
```
grep "[[:space:]]" file.txt
```

Any word.
```
grep "[[:word:]]" file.txt
```

Any punct.
```
grep "[[:punct:]]" file.txt
```

---

## 3. Regular Expressions

Match email.
```
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```

Match IP address.
```
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" file.txt
```

Match URL.
```
grep -E "https?://[a-zA-Z0-9.-]+" file.txt
```

Match phone number.
```
grep -E "[0-9]{3}-[0-9]{3}-[0-9]{4}" file.txt
```

Match date.
```
grep -E "[0-9]{4}-[0-9]{2}-[0-9]{2}" file.txt
```

Match time.
```
grep -E "[0-9]{2}:[0-9]{2}:[0-9]{2}" file.txt
```

Match hex color.
```
grep -E "#[0-9A-Fa-f]{6}" file.txt
```

Match version number.
```
grep -E "v[0-9]+\.[0-9]+\.[0-9]+" file.txt
```

Match UUID.
```
grep -E "[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}" file.txt
```

Match MAC address.
```
grep -E "([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}" file.txt
```

Match HTML tag.
```
grep -E "<[^>]+>" file.txt
```

Match empty tag.
```
grep -E "<[^>]+/>" file.txt
```

Match opening tag.
```
grep -E "<[a-zA-Z]+>" file.txt
```

Match closing tag.
```
grep -E "</[a-zA-Z]+>" file.txt
```

Match comment.
```
grep -E "<!--.*-->" file.txt
```

Match string in quotes.
```
grep -E "\"[^\"]*\"" file.txt
```

Match single quotes.
```
grep -E "'[^']*'" file.txt
```

Match backticks.
```
grep -E "`[^`]*`" file.txt
```

Match path.
```
grep -E "/[a-zA-Z0-9/_-]+" file.txt
```

Match file extension.
```
grep -E "\.[a-zA-Z]{2,4}" file.txt
```

Match number.
```
grep -E "[0-9]+" file.txt
```

Match float.
```
grep -E "[0-9]+\.[0-9]+" file.txt
```

Match negative.
```
grep -E "\-[0-9]+" file.txt
```

Match positive.
```
grep -E "\+[0-9]+" file.txt
```

Match signed.
```
grep -E "[+-]?[0-9]+" file.txt
```

Match scientific.
```
grep -E "[0-9]+[eE][+-]?[0-9]+" file.txt
```

Match currency.
```
grep -E "\$[0-9,]+\.?[0-9]*" file.txt
```

Match percentage.
```
grep -E "[0-9]+%" file.txt
```

Match any word.
```
grep -E "[a-zA-Z]+" file.txt
```

Match one capital.
```
grep -E "[A-Z][a-z]+" file.txt
```

Match camelCase.
```
grep -E "[a-z]+[A-Z][a-zA-Z]*" file.txt
```

Match snake_case.
```
grep -E "[a-z]+_[a-z]+" file.txt
```

Match kebab-case.
```
grep -E "[a-z]+-[a-z]+" file.txt
```

---

## 4. Case Sensitivity

Case sensitive default.
```
grep "Error" file.txt
```

Case insensitive.
```
grep -i "error" file.txt
```

Lowercase search.
```
grep "error" file.txt
```

Uppercase search.
```
grep "ERROR" file.txt
```

Mixed case.
```
grep -i "ErRoR" file.txt
```

Case insensitive word.
```
grep -iw "word" file.txt
```

Case insensitive line.
```
grep -ix "pattern" file.txt
```

Case in pattern.
```
grep -e "error" -e "Error" -e "ERROR" file.txt
```

Fold case.
```
grep -i "pattern" file.txt
```

Case matching.
```
grep --case-sensitive "pattern" file.txt
```

Ignore case option.
```
grep --ignore-case "pattern" file.txt
```

Smart case.
```
grep -y "pattern" file.txt
```

---

## 5. Line Operations

Single line.
```
grep -n "pattern" file.txt
```

Multiple lines.
```
grep -n "pattern" file.txt | head -20
```

All lines.
```
grep -n "." file.txt
```

Line numbers only.
```
grep -n "pattern" file.txt | cut -d: -f1
```

Content only.
```
grep "pattern" file.txt | cut -d: -f2
```

With surrounding lines.
```
grep -n -A 2 "pattern" file.txt
```

After lines.
```
grep -n -A 5 "pattern" file.txt
```

Before lines.
```
grep -n -B 5 "pattern" file.txt
```

Around lines.
```
grep -n -C 2 "pattern" file.txt
```

Skip first n lines.
```
tail -n +10 file.txt | grep "pattern"
```

Last n lines.
```
head -n 100 file.txt | grep "pattern"
```

Between patterns.
```
grep -n "start" file.txt | head -1
grep -n "end" file.txt | tail -1
```

Specific line number.
```
sed -n '25p' file.txt | grep "pattern"
```

Even lines.
```
sed -n '2~2p' file.txt | grep "pattern"
```

Odd lines.
```
sed -n '1~2p' file.txt | grep "pattern"
```

Line range.
```
sed -n '10,20p' file.txt | grep "pattern"
```

Lines before match.
```
sed -n '1,/pattern/p' file.txt
```

Lines after match.
```
sed -n '/pattern/,$p' file.txt
```

Delete matching.
```
sed '/pattern/d' file.txt
```

Delete non-matching.
```
sed -n '/pattern/!p' file.txt
```

Print match line number.
```
awk '/pattern/{print NR}' file.txt
```

---

## 6. Count and Numbering

Count all lines.
```
grep -c "" file.txt
```

Count matches.
```
grep -c "pattern" file.txt
```

Count unique matches.
```
grep -o "pattern" file.txt | sort | uniq -c
```

Count words.
```
grep -oE "\w+" file.txt | sort | uniq -c | sort -rn
```

Count per file.
```
grep -rc "pattern" *.txt
```

Line number with count.
```
grep -n "pattern" file.txt | wc -l
```

Matches with count.
```
grep -o "pattern" file.txt | wc -l
```

Unique count.
```
grep -oh "pattern" file.txt | sort | uniq -c | wc -l
```

Number lines.
```
grep -n "." file.txt
```

Number matches.
```
grep -n "pattern" file.txt | nl
```

Count by group.
```
grep -oh "pattern" file.txt | sort | uniq -c | sort -rn
```

Frequency.
```
grep -o "\b\w+\b" file.txt | sort | uniq -c | sort -rn
```

Top results.
```
grep -o "\w+" file.txt | sort | uniq -c | sort -rn | head -10
```

Count pattern1 and pattern2.
```
grep -c "pattern1\|pattern2" file.txt
```

Count each pattern.
```
for p in pattern1 pattern2 pattern3; do echo "$p: $(grep -c "$p" file.txt)"; done
```

Count files.
```
grep -l "pattern" *.txt | wc -l
```

Count matching files.
```
grep -L "pattern" *.txt | wc -l
```

Count lines after.
```
grep -A 5 "pattern" file.txt | wc -l
```

Count before.
```
grep -B 5 "pattern" file.txt | wc -l
```

Count context.
```
grep -C 3 "pattern" file.txt | wc -l
```

Total occurrences.
```
grep -o "pattern" file.txt | wc -l
```

---

## 7. Inverting Matches

Exclude pattern.
```
grep -v "pattern" file.txt
```

Exclude multiple.
```
grep -v "pattern1\|pattern2" file.txt
```

Exclude regex.
```
grep -v "[0-9]" file.txt
```

Exclude empty.
```
grep -v "^$" file.txt
```

Exclude comment.
```
grep -v "^#" file.txt
```

Exclude blank lines.
```
grep -v "^[[:space:]]*$" file.txt
```

Exclude word.
```
grep -vw "pattern" file.txt
```

Invert case sensitive.
```
grep -iv "pattern" file.txt
```

Exclude file.
```
grep -v "pattern" <file.txt
```

Invert recursive.
```
grep -r -v "pattern" directory/
```

Not contains.
```
grep -L "pattern" *.txt
```

Lines without.
```
grep -c "pattern" file.txt | grep -v ":0"
```

Show non-matching files.
```
grep -rL "pattern" directory/
```

Get non-matches.
```
awk '!/pattern/' file.txt
```

Not equal.
```
awk '$0 !~ /pattern/' file.txt
```

Not contains.
```
awk 'index($0, "pattern") == 0' file.txt
```

Print non-matched.
```
sed -n '/pattern/!p' file.txt
```

Filter out.
```
grep -v "error\|warning\|debug" file.txt
```

Exclude extensions.
```
grep -v "\.log$\|\.tmp$" file.txt
```

---

## 8. Context Options

Show line number.
```
grep -n "pattern" file.txt
```

Show after lines.
```
grep -n -A 3 "pattern" file.txt
```

Show before lines.
```
grep -n -B 3 "pattern" file.txt
```

Show around lines.
```
grep -n -C 2 "pattern" file.txt
```

Show 5 lines after.
```
grep -n -A 5 "pattern" file.txt
```

Show 5 before.
```
grep -n -B 5 "pattern" file.txt
```

Show 1 before.
```
grep -n -B 1 "pattern" file.txt
```

Show 1 after.
```
grep -n -A 1 "pattern" file.txt
```

Show context lines.
```
grep -n -C 5 "pattern" file.txt
```

After until blank.
```
grep -n -A 20 "pattern" file.txt | grep -m 1 "^--"
```

Before until blank.
```
grep -n -B 20 "pattern" file.txt | grep -m 1 "^--"
```

Context with separator.
```
grep -n -C 2 "pattern" file.txt
```

Group separator.
```
grep -n --group-separator="---" -C 2 "pattern" file.txt
```

No group separator.
```
grep -n --no-group-separator -C 2 "pattern" file.txt
```

Binary context.
```
grep -a -C 2 "pattern" binary.file
```

Max context.
```
grep -C 2 --max-count=10 "pattern" file.txt
```

Sparse context.
```
grep -n -U -C 2 "pattern" file.txt
```

Context to file.
```
grep -n -C 2 "pattern" file.txt > output.txt
```

Json context.
```
grep -n -C 2 --json "pattern" file.txt
```

Help context.
```
grep --help | grep context
```

All contexts.
```
grep -C 3 "pattern" *.txt
```

Context recursion.
```
grep -r -C 2 "pattern" directory/
```

---

## 9. File Options

Single file.
```
grep "pattern" file.txt
```

Multiple files.
```
grep "pattern" file1.txt file2.txt
```

All text files.
```
grep "pattern" *.txt
```

All files.
```
grep "pattern" *
```

Directory recursive.
```
grep -r "pattern" directory/
```

Recursive only dirs.
```
grep -R -d skip "pattern" directory/
```

Read patterns file.
```
grep -f patterns.txt file.txt
```

Read multiple files.
```
grep -f patterns.txt *.txt
```

Exclude binary.
```
grep -I "pattern" file.txt
```

Binary as text.
```
grep -a "pattern" file.bin
```

Search gzip.
```
grep -z "pattern" file.txt.gz
```

Search Zip.
```
grep -z "pattern" file.zip
```

List files.
```
grep -l "pattern" *.txt
```

List all matches.
```
grep -rl "pattern" directory/
```

No matches list.
```
grep -rL "pattern" directory/
```

File without match.
```
grep -L "pattern" *.txt
```

Files with match.
```
grep -l "pattern" *.txt
```

All files recursive.
```
grep -r "pattern" . -d REWRITE
```

Hidden files.
```
grep "pattern" .*
```

Config files.
```
grep "pattern" /etc/*.conf
```

Log files.
```
grep "pattern" /var/log/*.log
```

Process files.
```
grep "pattern" /proc/*/cmdline
```

System files.
```
grep "pattern" /sys/*/*
```

Device files.
```
grep "pattern" /dev/*
```

Socket files.
```
grep "pattern" /var/run/*.sock
```

PID files.
```
grep "pattern" /var/run/*.pid
```

---

## 10. Colored Output

Color auto.
```
grep --color "pattern" file.txt
```

Color always.
```
grep --color=always "pattern" file.txt
```

Color never.
```
grep --color=never "pattern" file.txt
```

Color version.
```
grep --color -V
```

ANSI color codes.
```
grep --color=auto -E "pattern" file.txt | less -R
```

Green match.
```
GREP_COLOR="1;32" grep -E --color=always "pattern" file.txt
```

Red match.
```
GREP_COLOR="1;31" grep -E --color=always "pattern" file.txt
```

Yellow match.
```
GREP_COLOR="1;33" grep -E --color=always "pattern" file.txt
```

Blue match.
```
GREP_COLOR="1;34" grep -E --color=always "pattern" file.txt
```

Bold match.
```
GREP_COLOR="1;1" grep -E --color=always "pattern" file.txt
```

Underline match.
```
GREP_COLOR="1;4" grep -E --color=always "pattern" file.txt
```

Custom color.
```
grep --color=always "pattern" file.txt
```

Less with color.
```
grep --color=always "pattern" file.txt | less -R
```

More with color.
```
grep --color=always "pattern" file.txt | more -R
```

Pin to terminal.
```
grep --color=auto "pattern" file.txt
```

---

## 11. Fixed Strings

Exact string.
```
grep -F "pattern" file.txt
```

Fixed string multiple.
```
grep -F -f strings.txt file.txt
```

Literal dot.
```
grep -F "." file.txt
```

Literal star.
```
grep -F "*" file.txt
```

Literal bracket.
```
grep -F "[" file.txt
```

Literal parenthesis.
```
grep -F "(" file.txt
```

Literal backslash.
```
grep -F "\" file.txt
```

Fixed newline.
```
grep -F "\n" file.txt
```

Fixed tab.
```
grep -F "\t" file.txt
```

Fixed null.
```
grep -F "" file.txt
```

Fixed words.
```
grep -F -w "word" file.txt
```

Fixed phrases.
```
grep -F "exact phrase" file.txt
```

Fixed case.
```
grep -F -i "Pattern" file.txt
```

Fixed count.
```
grep -F -c "pattern" file.txt
```

Fixed quiet.
```
grep -F -q "pattern" file.txt
```

Fixed list.
```
grep -F -l "pattern" *.txt
```

Fixed invert.
```
grep -F -v "pattern" file.txt
```

Fixed line number.
```
grep -F -n "pattern" file.txt
```

Fixed context.
```
grep -F -C 2 "pattern" file.txt
```

Fixed max count.
```
grep -F -m 5 "pattern" file.txt
```

---

## 12. Extended Regex

Enable extended.
```
grep -E "pattern" file.txt
```

One or more.
```
grep -E "a+" file.txt
```

Zero or more.
```
grep -E "a*" file.txt
```

Zero or one.
```
grep -E "a?" file.txt
```

Group.
```
grep -E "(ab)+" file.txt
```

Alternation.
```
grep -E "a|b" file.txt
```

Character class.
```
grep -E "[a-z]+" file.txt
```

Quantifier.
```
grep -E "a{2,4}" file.txt
```

Lazy quantifier.
```
grep -E "a*?" file.txt
```

Lookahead.
```
grep -E "a(?=b)" file.txt
```

Negative lookahead.
```
grep -E "a(?!b)" file.txt
```

Lookbehind.
```
grep -E "(?<=a)b" file.txt
```

Negative lookbehind.
```
grep -E "(?<!a)b" file.txt
```

Named group.
```
grep -E "(?<name>a)" file.txt
```

Backreference.
```
grep -E "(a)\1" file.txt
```

Non-capturing.
```
grep -E "(?:ab)+" file.txt
```

Non-greedy.
```
grep -E "a*?" file.txt
```

Alternation group.
```
grep -E "(error|warning)" file.txt
```

Nested groups.
```
grep -E "(a(b))" file.txt
```

Character union.
```
grep -E "[aeiou]" file.txt
```

Character intersection.
```
grep -E "[a-z&&[aeiou]]" file.txt
```

Character subtraction.
```
grep -E "[a-z&&[^aeiou]]" file.txt
```

Character range.
```
grep -E "[A-Za-z]" file.txt
```

Digit not.
```
grep -E "[^0-9]" file.txt
```

Posix character.
```
grep -E "[[:space:]]" file.txt
```

Unicode.
```
grep -E "\p{L}" file.txt
```

Word boundary.
```
grep -E "\bword\b" file.txt
```

Line boundary.
```
grep -E "^pattern$" file.txt
```

---

## 13. Recursive Search

Recursive search.
```
grep -r "pattern" directory/
```

Recursive all.
```
grep -R "pattern" directory/
```

Recursive with hidden.
```
grep -r -d recurse "pattern" .
```

Recursive files only.
```
grep -r -I "pattern" directory/
```

Recursive include.
```
grep -r --include="*.txt" "pattern" directory/
```

Recursive exclude.
```
grep -r --exclude="*.log" "pattern" directory/
```

Recursive exclude dir.
```
grep -r --exclude-dir=".git" "pattern" directory/
```

Recursive max depth.
```
grep -r --max-depth=2 "pattern" directory/
```

Recursive no depth.
```
grep -r --no-max-depth "pattern" directory/
```

Recursive directories.
```
grep -r -d call "pattern" directory/
```

Recursive skip.
```
grep -r -d skip "pattern" directory/
```

Recursive symlinks.
```
grep -r -L "pattern" directory/
```

Recursive follow.
```
grep -r -H "pattern" directory/
```

Recursive no group.
```
grep -r -h "pattern" directory/
```

Recursive output.
```
grep -r -l "pattern" directory/
```

Recursive count.
```
grep -r -c "pattern" directory/
```

Recursive quiet.
```
grep -r -q "pattern" directory/
```

Recursive invert.
```
grep -r -v "pattern" directory/
```

Recursive context.
```
grep -r -C 2 "pattern" directory/
```

Recursive line number.
```
grep -r -n "pattern" directory/
```

Recursive binary.
```
grep -r -a "pattern" directory/
```

Recursive after.
```
grep -r -A 2 "pattern" directory/
```

Recursive before.
```
grep -r -B 2 "pattern" directory/
```

Recursive with file.
```
grep -r -f patterns.txt directory/
```

---

## 14. Multiple Patterns

Or pattern.
```
grep -e "pattern1" -e "pattern2" file.txt
```

With -E.
```
grep -E "pattern1|pattern2" file.txt
```

Multiple -e.
```
grep -e "error" -e "warning" -e "fail" file.txt
```

Pattern file.
```
grep -f patterns.txt file.txt
```

Combined patterns.
```
grep -e "pattern1" -e "pattern2" -e "pattern3" file.txt
```

And pattern.
```
grep "pattern1" file.txt | grep "pattern2"
```

Not pattern.
```
grep "pattern1" file.txt | grep -v "pattern2"
```

Xor pattern.
```
grep "pattern1\|pattern2" file.txt | grep -v "pattern1\|pattern2" | grep
```

All patterns.
```
grep "pattern1" file.txt > temp.txt
grep "pattern2" file.txt >> temp.txt
grep "pattern3" file.txt >> temp.txt
sort temp.txt | uniq
```

Named patterns.
```
grep -E "(error|warning)" file.txt
```

Group patterns.
```
grep -E "(error|warning|fail)" file.txt
```

Complex patterns.
```
grep -E "^[0-9]+.*(error|warning)" file.txt
```

Negate pattern.
```
grep -v "pattern" file.txt
```

Exclude patterns.
```
grep -v "error\|warning\|debug" file.txt
```

Filter patterns.
```
grep -E "^[a-z]" file.txt
```

Match both.
```
grep "pattern1" file.txt | grep "pattern2"
```

Either pattern.
```
grep -E "pattern1|pattern2" file.txt
```

Both patterns.
```
grep "pattern1" file.txt > p1.txt
grep "pattern2" file.txt > p2.txt
comm -12 p1.txt p2.txt
```

Unique matches.
```
grep -oh "pattern" file.txt | sort -u
```

Distinct patterns.
```
grep -ohE "[a-z]+" file.txt | sort | uniq
```

---

## 15. Multiple Files

Search file1.
```
grep "pattern" file1.txt
```

Search file1 and file2.
```
grep "pattern" file1.txt file2.txt
```

Search all txt.
```
grep "pattern" *.txt
```

Search log files.
```
grep "pattern" *.log
```

Search config.
```
grep "pattern" *.conf *.cfg
```

Search source.
```
grep "pattern" *.c *.cpp *.h *.java
```

Search script.
```
grep "pattern" *.sh *.py *.rb
```

Search web.
```
grep "pattern" *.html *.php *.asp
```

Search data.
```
grep "pattern" *.json *.xml *.csv
```

Search with glob.
```
grep "pattern" /path/*.txt
```

Recursive files.
```
grep "pattern" **/*.txt
```

Hidden files.
```
grep "pattern" .*
```

All in directory.
```
grep "pattern" *
```

Specific files.
```
grep "pattern" {file1,file2,file3}.txt
```

All extensions.
```
grep "pattern" *.{txt,log,conf}
```

File list.
```
grep "pattern" $(find . -name "*.txt")
```

From filelist.
```
grep "pattern" $(cat filelist.txt)
```

By date.
```
find . -mtime -7 -name "*.txt" -exec grep "pattern" {} \;
```

By size.
```
find . -size +1M -name "*.txt" -exec grep "pattern" {} \;
```

By user.
```
find . -user username -name "*.txt" -exec grep "pattern" {} \;
```

By permission.
```
find . -perm 644 -name "*.txt" -exec grep "pattern" {} \;
```

By type.
```
find . -type f -name "*.txt" -exec grep "pattern" {} \;
```

---

## 16. Performance

Fast mode.
```
grep -F "pattern" file.txt
```

Fast recursive.
```
grep -r -F "pattern" directory/
```

Binary skip.
```
grep -I "pattern" file.txt
```

Skip binary fast.
```
grep -i -G "pattern" file.txt
```

Optimized.
```
grep --binary-files=without-match "pattern" file.txt
```

Max count fast.
```
grep -m 1000 "pattern" file.txt
```

No memory.
```
grep --no-ignore Exclude "pattern" file.txt
```

Quick search.
```
grep -q "pattern" file.txt
```

Stop first match.
```
grep -m 1 "pattern" file.txt
```

Sparse check.
```
grep -S "pattern" file.txt
```

Use indexes.
```
grep --index "pattern" file.txt
```

Load memory.
```
grep --mmap "pattern" file.txt
```

Page files.
```
grep --line-buffered "pattern" file.txt
```

Buffer size.
```
grep --buffer-size=65536 "pattern" file.txt
```

Sparse files.
```
grep --sparse "pattern" file.txt
```

Binary files.
```
grep -a "pattern" binary.file
```

Text mode.
```
grep -T "pattern" file.txt
```

Initial tag.
```
grep -I "pattern" file.txt
```

8-bit.
```
grep -w -G "pattern" file.txt
```

Empty pattern.
```
grep -e "" file.txt > /dev/null && echo "OK"
```

---

## 17. Output Redirection

Save to file.
```
grep "pattern" file.txt > output.txt
```

Append to file.
```
grep "pattern" file.txt >> output.txt
```

Error to file.
```
grep "pattern" file.txt 2> error.txt
```

Both to file.
```
grep "pattern" file.txt &> output.txt
```

To null.
```
grep "pattern" file.txt > /dev/null
```

To stdout.
```
grep "pattern" file.txt 1> output.txt
```

To stderr.
```
grep "pattern" file.txt 2>&1
```

Combined output.
```
grep "pattern" file.txt | tee output.txt
```

Tee and append.
```
grep "pattern" file.txt | tee -a output.txt
```

Save errors.
```
grep "pattern" file.txt 2> errors.txt
```

Save quiet.
```
grep -q "pattern" file.txt && echo "Found" || echo "Not Found"
```

Exit status.
```
grep "pattern" file.txt; echo $?
```

Success check.
```
grep -q "pattern" file.txt && echo "Yes"
```

Fail check.
```
grep -q "pattern" file.txt || echo "No"
```

Conditional.
```
grep -q "pattern" file.txt && echo "Match" || echo "No match"
```

Split output.
```
grep "pattern" file.txt | split -l 1000 - output_
```

To numbered.
```
grep -n "pattern" file.txt > numbered.txt
```

To process.
```
grep "pattern" file.txt | wc -l
```

To sort.
```
grep "pattern" file.txt | sort
```

To unique.
```
grep -o "pattern" file.txt | sort -u
```

To count.
```
grep -o "pattern" file.txt | uniq -c
```

---

## 18. Pipe Operations

Pipe to cat.
```
grep "pattern" file.txt | cat
```

Pipe to less.
```
grep "pattern" file.txt | less
```

Pipe to more.
```
grep "pattern" file.txt | more
```

Pipe to head.
```
grep "pattern" file.txt | head -20
```

Pipe to tail.
```
grep "pattern" file.txt | tail -20
```

Pipe to sort.
```
grep "pattern" file.txt | sort
```

Pipe to uniq.
```
grep "pattern" file.txt | uniq
```

Pipe to wc.
```
grep "pattern" file.txt | wc -l
```

Pipe to cut.
```
grep "pattern" file.txt | cut -d: -f1
```

Pipe to awk.
```
grep "pattern" file.txt | awk '{print $1}'
```

Pipe to sed.
```
grep "pattern" file.txt | sed 's/old/new/'
```

Pipe to tr.
```
grep "pattern" file.txt | tr 'a-z' 'A-Z'
```

Pipe to rev.
```
grep "pattern" file.txt | rev
```

Pipe to column.
```
grep "pattern" file.txt | column -t
```

Pipe to table.
```
grep "pattern" file.txt | column -s, -t
```

Pipe to xargs.
```
grep "pattern" file.txt | xargs echo
```

Pipe to parallel.
```
grep -r "pattern" . | parallel -j4 grep {}
```

Pipe to zip.
```
grep "pattern" file.txt | zip output.zip
```

Pipe to gzip.
```
grep "pattern" file.txt | gzip > output.gz
```

Pipe to bzip2.
```
grep "pattern" file.txt | bzip2 > output.bz2
```

---

## 19. Combined Searches

Search and sort.
```
grep "error" file.txt | sort
```

Search and count.
```
grep "error" file.txt | wc -l
```

Search and unique.
```
grep "error" file.txt | uniq -c
```

Search and edit.
```
grep "error" file.txt | sed 's/error/WARNING/'
```

Search and cut.
```
grep "error" file.txt | cut -d: -f1
```

Search and awk.
```
grep "error" file.txt | awk '{print $1}'
```

Search and replace.
```
grep "error" file.txt | tr 'e' 'E'
```

Search and unique sort.
```
grep "error" file.txt | sort | uniq
```

Search and case.
```
grep -i "error" file.txt | tr 'a-z' 'A-Z'
```

Search and format.
```
grep -o "error" file.txt | column -c 80
```

Search and limit.
```
grep "error" file.txt | head -n 10
```

Search and offset.
```
grep "error" file.txt | tail -n +5 | head -n 10
```

Search and group.
```
grep "error" file.txt | sort | uniq -c | sort -rn
```

Search and filter.
```
grep "error" file.txt | grep -v "debug"
```

Search and pipe.
```
grep "error" file.txt | head | tail
```

Search and combine.
```
grep -e "error" -e "warning" file.txt | sort
```

Search and split.
```
grep "error" file.txt | split -b 1M - output_
```

Search and merge.
```
grep "error" file1.txt file2.txt | sort -u
```

Search and verify.
```
grep "error" file.txt | md5sum
```

Search and check.
```
grep "error" file.txt | sum
```

Search and checksum.
```
grep "error" file.txt | cksum
```

Search and hash.
```
grep "error" file.txt | shasum
```

Search and archive.
```
grep "error" file.txt | tar -cvf error.tar -T -
```

Search and export.
```
grep "error" file.txt | export CSV
```

Search and convert.
```
grep "error" file.txt | unix2dos
```

Search and normalize.
```
grep "error" file.txt | unix2dos | dos2unix
```

Search and encode.
```
grep "error" file.txt | base64
```

Search and decode.
```
grep "error" file.txt | base64 -d
```

Search and compress.
```
grep "error" file.txt | gzip
```

Search and decompress.
```
grep "error" file.txt | gunzip
```

Search and encrypt.
```
grep "error" file.txt | gpg -c
```

Search and decrypt.
```
grep "error" file.txt | gpg -d
```

Search and sign.
```
grep "error" file.txt | gpg --sign
```

Search and verify.
```
grep "error" file.txt | gpg --verify
```

---

## 20. Troubleshooting

Show version.
```
grep --version
```

Show help.
```
grep --help
```

Verbose mode.
```
grep -V "pattern" file.txt
```

Debug mode.
```
grep -d debug "pattern" file.txt
```

Binary check.
```
file file.txt
```

Encoding check.
```
file -bi file.txt
```

Line breaks.
```
file -bL file.txt
```

Show encoding.
```
iconv -l
```

Convert encoding.
```
iconv -f UTF-8 -t ASCII file.txt
```

Fix encoding.
```
sed 's/\xC3\xA9/e/g' file.txt
```

Check pattern.
```
echo "test" | grep "pattern"
```

Debug regex.
```
grep --debug "pattern" file.txt
```

Trace search.
```
strace -e trace=open,read grep "pattern" file.txt
```

Profile.
```
time grep "pattern" file.txt
```

Statistics.
```
grep -c "pattern" file.txt
```

Memory usage.
```
/usr/bin/time -v grep "pattern" file.txt
```

Speed test.
```
dd if=file.txt bs=1M 2>/dev/null | grep "pattern"
```

Performance check.
```
dd if=file.txt bs=1K count=1000 2>/dev/null | grep "pattern"
```

Benchmark.
```
for i in 1 2 3; do time grep "pattern" file.txt; done
```

Compare tools.
```
time grep "pattern" file.txt
time egrep "pattern" file.txt
time fgrep "pattern" file.txt
```

Parallel check.
```
time grep -P "pattern" file.txt
```

No cache.
```
echo 3 | sudo tee /proc/sys/vm/drop_caches
time grep "pattern" file.txt
```

Large file.
```
ls -lh file.txt
grep "pattern" file.txt &
```

Hanged process.
```
ps aux | grep grep
kill -9 $(pgrep grep)
```

Memory limit.
```
ulimit -v 4096000; grep "pattern" file.txt
```

CPU limit.
```
ulimit -t 60; grep "pattern" file.txt
```

Check limits.
```
ulimit -a
```

Search in hex.
```
grep -ab "pattern" file.txt
```

Hex dump.
```
xxd file.txt | grep "pattern"
```

Binary search.
```
grep -a "pattern" binary.file
```

Octal dump.
```
od -c file.txt | grep "pattern"
```

Unicode search.
```
grep -P "[\u1234]" file.txt
```

Multi-byte.
```
grep -P "[\x{1234}]" file.txt
```

Show context.
```
grep -H "pattern" file.txt
```

Force output.
```
grep -n -a "pattern" file.txt
```

Continue on error.
```
grep -s "pattern" file.txt
```

Suppress errors.
```
grep -s "pattern" file.txt 2>/dev/null
```

Warnings.
```
grep -W "pattern" file.txt
```

Show warnings.
```
grep --warn "pattern" file.txt
```

Compatibility.
```
grep --line-buffered "pattern" file.txt
```

---

## Quick Reference

### Basic Search
```
grep "pattern" file.txt
```

### Case Insensitive
```
grep -i "pattern" file.txt
```

### Line Numbers
```
grep -n "pattern" file.txt
```

### Count Matches
```
grep -c "pattern" file.txt
```

### Invert Match
```
grep -v "pattern" file.txt
```

### Regex Search
```
grep -E "pattern" file.txt
```

### Fixed String
```
grep -F "pattern" file.txt
```

### Recursive
```
grep -r "pattern" directory/
```

### Multiple Patterns
```
grep -e "p1" -e "p2" file.txt
```

### File List
```
grep -l "pattern" *.txt
```

---

## Common Options

| Option | Description |
|--------|------------|
| `-i` | Case insensitive |
| `-n` | Show line numbers |
| `-c` | Count matches |
| `-v` | Invert match |
| `-w` | Word match |
| `-o` | Only matching |
| `-r` | Recursive |
| `-l` | List files |
| `-L` | List non-matching |
| `-A` | After context |
| `-B` | Before context |
| `-C` | Context |
| `-E` | Extended regex |
| `-F` | Fixed strings |
| `-m` | Max matches |
| `-q` | Quiet mode |
| `-h` | No filename |
| `-H` | Show filename |

---

## Example Commands

### Find errors
```
grep -rn "error" /var/log/
```

### Find in logs
```
grep -i "error\|warning" /var/log/*.log
```

### IP address
```
grep -Eo "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" access.log
```

### Email addresses
```
grep -Eo "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```

### URLs
```
grep -Eo "https?://[^ ]+" file.txt
```

### Config entries
```
grep -v "^#" config.txt | grep -v "^$"
```

### Active users
```
who | grep -v "^$"
```

### Running processes
```
ps aux | grep process_name | grep -v grep
```

### Failed logins
```
last | grep "Failed"
```

### Recent files
```
ls -lt | grep "2024"
```

### Large files
```
du -sh * | grep -E "^[0-9]+G"
```

### Empty lines
```
grep -n "^$" file.txt
```

### Comments
```
grep "^#" file.txt
```

### Function definitions
```
grep -n "^def \|^function " *.py *.js
```

### TODO comments
```
grep -rn "TODO\|FIXME\|XXX" --include="*.{js,py,java}"
```

### Error logs
```
tail -f /var/log/syslog | grep error
```

### Live search
```
watch 'ps | grep pattern'
```

### Count errors per file
```
grep -rc "error" *.log | grep -v ":0"
```

### Search history
```
history | grep git
```

### Find with grep
```
find . -exec grep -l "pattern" {} \;
```

---

### Thank you 
Contact if you want to join our classes/support: 
[EFXTV](https://t.me/efxtv)

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/efxtv)
