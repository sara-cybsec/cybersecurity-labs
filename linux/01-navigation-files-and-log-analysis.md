# Linux Lab 01 — Navigation, Files & Basic Log Analysis

## Objective

Practice basic Linux/macOS terminal commands and learn how command-line tools can be combined to analyze simple security log data.

## Commands Practiced

### `pwd`

Purpose:

`pwd` shows me the directory I am currently working inside. I can use it when I want to check where I am in the terminal.

### `ls`

Purpose:

`ls` shows me the files and folders inside my current directory.

### `cd`

Purpose:

`cd` lets me move from one directory to another. For example, I can use it to move from my home directory to Downloads or Desktop.

### `mkdir`

Purpose:

`mkdir` creates a new directory or folder.

### `touch`

Purpose:

`touch` can be used to create a new empty file. I learned that this is different from `mkdir`, which creates a directory.

### `cat`

Purpose:

`cat` lets me display and read the contents of a text file directly in the terminal.

### `grep`

Purpose:

`grep` searches text for something specific. For example, I used it to search a fake security log and display only the lines containing `FAILED`.

---

## Output Redirection

### `>`

My explanation:

`>` sends output into a file, but it overwrites what was already inside the file. I need to be careful when using it because existing content can be replaced.

### `>>`

My explanation:

`>>` also sends output into a file, but instead of replacing the existing content, it adds the new content to the end of the file.

---

## Pipes

### `|`

My explanation:

A pipe `|` takes the output from one command and sends it to another command. This lets me combine different Linux commands to perform more useful tasks.

For example:

```bash
grep "FAILED" notes.txt | wc -l
```

First, `grep` finds the failed login lines. Then the pipe sends those results to `wc -l`, which counts how many lines were found.

---

## Mini Security Log Exercise

I created a small fake login log containing successful and failed login attempts.

Example:

```text
FAILED LOGIN admin
SUCCESS LOGIN sara
FAILED LOGIN root
SUCCESS LOGIN guest
FAILED LOGIN admin
```

### Finding Failed Logins

I used:

```bash
grep "FAILED" notes.txt
```

Result:

```text
FAILED LOGIN admin
FAILED LOGIN root
FAILED LOGIN admin
```

### Counting Failed Logins

```bash
grep "FAILED" notes.txt | wc -l
```

Result:

```text
3
```

### Extracting Usernames

```bash
grep "FAILED" notes.txt | awk '{print $3}'
```

Result:

```text
admin
root
admin
```

### Counting Failed Attempts Per User

```bash
grep "FAILED" notes.txt | awk '{print $3}' | sort | uniq -c | sort -nr
```

Result:

```text
2 admin
1 root
```

---

## Mistake I Made

While creating `passwords.txt`, I accidentally created it as a directory instead of a file.

When I tried to write text into it, I received:

```text
zsh: is a directory: passwords.txt
```

Instead of assuming the command was broken, I checked what `passwords.txt` actually was using:

```bash
ls -l
```

I learned that the first character can help identify the type:

```text
d = directory
- = regular file
```

I removed the empty directory using:

```bash
rmdir passwords.txt
```

and recreated it correctly as a file using:

```bash
touch passwords.txt
```

This mistake helped me understand the difference between creating a directory and creating a file.

---

## What I Learned

In this lab, I learned how to navigate through directories, create files and folders, read files, and search through text using the terminal.

One of the most useful things I learned was that Linux commands can be combined using pipes. Instead of using one command for everything, I can take the output from one command and process it with another.

I also learned that checking the data is important when analyzing results. At one point, I expected two failed login attempts for `admin`, but my command only showed one. The command was working correctly — I had simply forgotten to add the second log entry.

The `passwords.txt` mistake also taught me how to use `ls -l` to distinguish between a regular file and a directory.

Finally, I saw how commands such as `grep`, `awk`, `sort`, `uniq`, and `wc` can be combined to perform basic log analysis. Even though this was a small fake security log, the same idea can be used to filter and investigate much larger sets of security data.
