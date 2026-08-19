# Linux Lab 03 — Users, Processes & Process Monitoring

## Objective

Learn how Linux/macOS identifies users and running processes, how Process IDs (PIDs) work, and how command-line tools can be used to inspect active processes.

---

## User Identity

### `whoami`

I used:

```bash
whoami
```

to display the username of the account currently running the terminal session.

This helps identify which user is performing actions on the system.

In a security context, this matters because different users can have different permissions and levels of access.

---

### `id`

I also used:

```bash
id
```

to view more detailed information about my current user.

This can include:

- User ID (UID)
- Group ID (GID)
- Group memberships

This connects to Linux permissions because access decisions can depend on both the user and the groups they belong to.

---

## Processes

A process is a running program or task.

For example, applications such as:

- Terminal shells
- Python programs
- Browsers
- Spotify
- VS Code

can all appear as running processes.

---

## `ps`

I used:

```bash
ps
```

to view processes associated with my current terminal session.

The output includes a column called:

```text
PID
```

which stands for:

**Process ID**

Each running process is assigned its own PID so the operating system can identify and manage it.

---

## Viewing More Processes

I used:

```bash
ps aux
```

to display a larger list of running processes.

This produced a lot of output because many applications and background services were running on the system.

Instead of manually reading every line, I reused `grep` and pipes from Linux Lab 01.

---

## Filtering Processes

For example:

```bash
ps aux | grep "sleep"
```

means:

1. `ps aux` displays running processes.
2. `|` sends that output to another command.
3. `grep "sleep"` searches for lines containing the word `sleep`.

This made it easier to search for a specific process.

---

## Important Observation — `grep` Appears in the Results

When I searched:

```bash
ps aux | grep "sleep"
```

I saw a line containing:

```text
grep sleep
```

At first, this can look like the process being searched for.

However, the `grep` command itself contains the word `sleep`, so it can appear in its own search results.

This taught me to check the full command information instead of assuming every matching line represents the process I am looking for.

---

## Background Processes

I practiced starting a harmless background process using:

```bash
sleep 300 &
```

The `sleep` command waits for a specified number of seconds.

The:

```text
&
```

symbol tells the shell to run the process in the background.

This means the terminal prompt becomes available again while the process continues running.

---

## Foreground vs Background

Without `&`:

```bash
sleep 300
```

the process runs in the foreground and occupies the terminal.

With:

```bash
sleep 300 &
```

the process runs in the background.

This helped me understand that a shell can manage processes in different ways.

---

## `Ctrl + C`

I also learned that:

```text
Control + C
```

can interrupt a foreground process.

For example, if:

```bash
sleep 300
```

is running in the foreground, pressing `Control + C` stops it and returns control of the terminal.

---

## Process Investigation

At one point, I searched using:

```bash
ps aux | grep "process"
```

This returned a very large number of results.

The reason was that the word `process` appeared inside the command information of many running applications and helper services.

I then changed the search to something more specific:

```bash
ps aux | grep "sleep"
```

This produced much more relevant results.

This taught me that search terms should be specific enough to reduce irrelevant output.

---

## Checking Whether a Process Is Still Running

Later, I ran:

```bash
ps aux | grep "sleep"
```

and only saw:

```text
grep sleep
```

There was no:

```text
sleep 300
```

entry.

This meant the actual `sleep` process was no longer running.

The search command itself was still visible because `grep` contained the word `sleep`.

---

## Security Connection

Process monitoring is important in cybersecurity because running processes can reveal what is currently active on a system.

Security analysts may inspect processes to identify:

- Unexpected programs
- Suspicious applications
- Resource-heavy processes
- Unknown background activity
- Processes running under unexpected users

Before taking action on a process, it is important to identify it correctly.

A wrong PID or incorrect assumption could affect the wrong program.

---

## Commands Practiced

```bash
whoami
id
ps
ps aux
grep
sleep
sleep 300 &
```

I also reused:

```text
|
```

for piping output between commands.

---

## What I Learned

This lab helped me understand how the operating system identifies both users and processes.

I learned that each running process has a Process ID (PID), and tools such as `ps` can be used to inspect processes on the system.

I also reused `grep` and pipes to filter large amounts of process information instead of reading everything manually.

One useful lesson was that the search command itself can appear in `grep` results, so I need to inspect the complete output carefully before deciding whether a process is actually running.

I also learned that using more specific search terms makes process investigation easier and reduces irrelevant results.

This lab showed me how basic Linux commands can begin to support system monitoring and security investigation.
