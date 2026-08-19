# Linux Lab 02 — Permissions & Access Control

## Objective

Learn how Linux file permissions control who can read, modify, or execute files, and understand why permissions are important from a security perspective.

---

## Understanding `ls -l`

I used:

```bash
ls -l
```

to view detailed information about files and directories.

Example:

```text
-rw-r--r--  1 saras  staff  22 Aug 19 secret.txt
```

The first character identifies the type:

```text
- = regular file
d = directory
```

The next nine characters represent permissions.

They are divided into three groups:

```text
rw-   r--   r--
 |     |     |
user  group others
```

- **User / Owner** — the user who owns the file
- **Group** — users belonging to the file's group
- **Others** — everyone else

---

## Basic Permissions

Linux uses three main permissions:

```text
r = read
w = write
x = execute
```

### Read — `r`

Allows the contents of a file to be viewed.

### Write — `w`

Allows the file to be modified.

### Execute — `x`

Allows a file or program to be executed when applicable.

---

## Numeric Permissions

I learned that permissions can be represented using numbers:

```text
r = 4
w = 2
x = 1
```

The numbers are added together.

For example:

```text
7 = 4 + 2 + 1 = rwx
6 = 4 + 2     = rw-
5 = 4 + 1     = r-x
4 = 4         = r--
0             = ---
```

The three digits represent:

```text
Owner | Group | Others
```

---

## Example — `chmod 600`

I created a test file:

```bash
touch secret.txt
```

Then added fake confidential lab data:

```bash
echo "CONFIDENTIAL LAB DATA" > secret.txt
```

I changed its permissions using:

```bash
chmod 600 secret.txt
```

This produced:

```text
-rw-------
```

Meaning:

```text
Owner   → read + write
Group   → no permissions
Others  → no permissions
```

This would make more sense for a sensitive file than allowing everyone to access it.

---

## Example — `chmod 740`

```bash
chmod 740 secret.txt
```

means:

```text
7 = rwx
4 = r--
0 = ---
```

Therefore:

```text
Owner   → read + write + execute
Group   → read only
Others  → no permissions
```

The permission representation is:

```text
-rwxr-----
```

---

## Example — `chmod 754`

```bash
chmod 754 secret.txt
```

means:

```text
Owner   → rwx
Group   → r-x
Others  → r--
```

The permission representation is:

```text
-rwxr-xr--
```

---

## Why `chmod 777` Can Be Dangerous

```bash
chmod 777 secret.txt
```

gives:

```text
Owner   → rwx
Group   → rwx
Others  → rwx
```

This means everyone has read, write, and execute permissions.

For a sensitive file, this would be a security risk because users who do not need access could potentially read or modify the file.

Giving very broad permissions just to make something work can create unnecessary security exposure.

---

## Principle of Least Privilege

This lab introduced me to the **Principle of Least Privilege**.

The idea is to give users, applications, or processes only the permissions they actually need to perform their tasks.

For example, if only the owner needs to read and modify a sensitive file:

```bash
chmod 600 secret.txt
```

is more appropriate than:

```bash
chmod 777 secret.txt
```

because other users do not need access.

---

## Symbolic Permissions

I also learned another way to modify permissions.

```text
u = user / owner
g = group
o = others
a = all
```

And:

```text
+ = add permission
- = remove permission
= = set permissions
```

### Example

```bash
chmod g+x secret.txt
```

means:

> Add execute permission to the group.

### Example

```bash
chmod o-r secret.txt
```

means:

> Remove read permission from others.

### Example

```bash
chmod u-w secret.txt
```

means:

> Remove write permission from the owner.

---

## Permission Denied Experiment

I wanted to see what happens when I own a file but do not have permission to read it.

The file originally had:

```text
-rw-------
```

I removed my own read permission:

```bash
chmod u-r secret.txt
```

Then I checked:

```bash
ls -l secret.txt
```

The owner permissions changed from:

```text
rw-
```

to:

```text
-w-
```

When I tried:

```bash
cat secret.txt
```

Terminal returned:

```text
cat: secret.txt: Permission denied
```

The file had not been deleted and the data was still there.

The operating system simply prevented me from reading it because the required permission was missing.

I restored access using:

```bash
chmod u+r secret.txt
```

---

## Mistake / Troubleshooting

At first, I expected `cat secret.txt` to return `Permission denied`, but it still displayed:

```text
CONFIDENTIAL LAB DATA
```

Instead of assuming the permission system was not working, I checked:

```bash
ls -l secret.txt
```

and discovered that the permissions were still:

```text
-rw-------
```

which meant I still had read permission.

After correctly running:

```bash
chmod u-r secret.txt
```

I verified the permissions again and then received the expected:

```text
Permission denied
```

This taught me to verify the actual system state instead of assuming a command changed something successfully.

---

## Security Takeaways

From this lab, I learned that file permissions are an important part of operating system access control.

The main things I learned were:

- `r`, `w`, and `x` represent read, write, and execute permissions.
- Permissions are separately assigned to the owner, group, and others.
- `chmod` can change permissions using numeric or symbolic notation.
- `777` gives everyone broad access and can be dangerous when it is unnecessary.
- Sensitive files should have only the permissions actually required.
- The Principle of Least Privilege helps reduce unnecessary access.
- `Permission denied` can occur even when a file exists because the operating system checks whether the user has permission to perform the requested action.
- `ls -l` is useful for investigating permission-related problems.

---

## Commands Practiced

```bash
ls -l
touch
cat
chmod
chmod 600
chmod 740
chmod 754
chmod u-r
chmod u+r
chmod g+x
chmod o-r
chmod u-w
```

---

## What I Learned

This lab helped me understand that Linux permissions are not just numbers that need to be memorized. They represent access-control decisions.

I also learned why giving every user full permissions can be a security problem and how the Principle of Least Privilege applies to files.

The Permission Denied experiment made the concept easier to understand because I could directly see the operating system allow or deny access depending on the permissions I configured.

I also learned an important troubleshooting habit: when the result is different from what I expected, I should check the actual configuration and data before assuming that the command or system is broken.
