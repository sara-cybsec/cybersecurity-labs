# Linux Lab 04 — Users, Groups & Privilege Concepts

## Objective

Understand how Linux uses users, groups, privileges, and administrative access to control what people and processes are allowed to do on a system.

---

## Users

Linux is a multi-user operating system.

Different users can have different:

- Files
- Permissions
- Groups
- Processes
- Access privileges

The command:

```bash
whoami
```

shows the currently active user.

Another useful command is:

```bash
id
```

which can display information such as:

- User ID (UID)
- Group ID (GID)
- Group memberships

Linux internally uses numerical identifiers in addition to usernames.

---

## User ID — UID

Each user has a User ID.

For example:

```text
username → sara
UID      → numerical identifier
```

The operating system can use the UID when determining ownership and access.

A particularly important UID on Linux systems is:

```text
UID 0
```

which belongs to the `root` account.

---

## Groups

Users can belong to groups.

Groups make it easier to give several users access to the same resources.

For example, an organization might conceptually have groups such as:

```text
developers
security
finance
students
administrators
```

Instead of configuring every file separately for every person, permissions can be assigned to a group.

This connects directly to the permissions from Lab 02:

```text
OWNER | GROUP | OTHERS
```

---

## Root

`root` is the superuser on Linux systems.

The root account has extremely powerful privileges and can perform administrative actions that normal users may not be allowed to perform.

Examples can include:

- Modifying protected system files
- Managing users
- Changing system configuration
- Installing or removing software
- Managing services
- Changing ownership and permissions
- Accessing resources unavailable to normal users

Because root has extensive privileges, using it unnecessarily increases security risk.

---

## `sudo`

`sudo` is commonly used to run an individual command with elevated privileges when the current user is authorized to do so.

Conceptually:

```text
Normal user
     ↓
sudo command
     ↓
Authorization check
     ↓
Command runs with elevated privileges
```

This is different from simply performing all daily activity as the root user.

---

## Why Elevated Privileges Matter

Imagine a command accidentally modifies the wrong file.

With limited permissions:

```text
User attempts dangerous change
        ↓
Permission denied
```

The operating system may prevent the damage.

With unnecessary administrative privileges:

```text
Privileged user
        ↓
Dangerous command
        ↓
System allows operation
        ↓
Potential system damage
```

This is one reason security professionals avoid using elevated privileges when they are not necessary.

---

## Principle of Least Privilege

The Principle of Least Privilege means:

> A user, application, or process should receive only the permissions required to perform its intended task.

This principle applies to much more than Linux.

It is used throughout cybersecurity in areas such as:

- Operating systems
- Cloud environments
- Databases
- Applications
- Network administration
- Identity and Access Management (IAM)

Giving unnecessary privileges increases the potential impact of mistakes or compromised accounts.

---

## Privilege Escalation

Privilege escalation refers to gaining a higher level of access than originally available.

Conceptually:

```text
Normal User
    ↓
Limited Permissions
    ↓
Privilege Escalation
    ↓
Higher Privileges
```

In legitimate administration, authorized users may intentionally elevate privileges when required.

In cybersecurity, attackers may attempt to exploit vulnerabilities or misconfigurations to obtain unauthorized elevated privileges.

This is why permissions, configuration, patching, and access control are important.

---

## Horizontal vs Vertical Privilege Escalation

### Vertical Privilege Escalation

A user obtains permissions belonging to a more privileged role.

Conceptually:

```text
Normal User
     ↓
Administrator / Root
```

### Horizontal Privilege Escalation

A user accesses resources belonging to another user with a similar privilege level.

Conceptually:

```text
User A
  ↓
User B's resources
```

Both represent failures of access control when performed without authorization.

---

## File Ownership

Files have both:

- An owner
- A group

This can be viewed using:

```bash
ls -l
```

Example:

```text
-rw-r----- sara security report.txt
```

Conceptually:

```text
Owner → sara
Group → security
```

Permissions can then determine what the owner, group members, and everyone else can do.

---

## `chown`

On Linux, `chown` is used to change file ownership.

General syntax:

```bash
chown USER FILE
```

Ownership can also include a group:

```bash
chown USER:GROUP FILE
```

Changing ownership normally requires appropriate privileges.

---

## Authentication vs Authorization

This topic also helped me separate two important security concepts.

### Authentication

Authentication asks:

> Who are you?

Examples:

- Password
- Security key
- Biometrics
- Multi-factor authentication

### Authorization

Authorization asks:

> What are you allowed to do?

Linux permissions are primarily related to authorization.

For example:

```text
Authentication
     ↓
User identified as Sara
     ↓
Authorization
     ↓
Can Sara read this file?
Can Sara modify this file?
Can Sara execute this program?
```

Being successfully authenticated does not automatically mean a user should have access to everything.

---

## Security Takeaways

The main security concepts from this topic are:

- Linux supports multiple users with different privileges.
- Users can belong to groups.
- UIDs and GIDs help identify users and groups.
- `root` is the highly privileged Linux superuser.
- Administrative privileges should not be used unnecessarily.
- `sudo` can provide controlled elevated access when authorized.
- File ownership works together with permissions.
- Least privilege reduces unnecessary access.
- Authentication identifies a user.
- Authorization determines what that user can access.
- Privilege escalation is an important security concept because increased access can greatly increase the impact of an account compromise.

---

## Commands / Concepts

```bash
whoami
id
ls -l
sudo
chown
```

Concepts:

```text
Users
Groups
UID
GID
Root
Privileges
Least Privilege
Privilege Escalation
Authentication
Authorization
File Ownership
```

---

## What I Learned

This topic helped me understand that Linux security is not only about whether a file has `r`, `w`, or `x` permissions.

The operating system also needs to know which user is requesting access, which groups that user belongs to, who owns the resource, and what level of privilege the user has.

I also learned why the root account needs to be treated carefully. Having more privileges is not always better because unnecessary privileges can increase the impact of mistakes or security compromises.

The distinction between authentication and authorization was also important: proving who I am does not automatically mean I should be allowed to access every resource.

These ideas connect Linux permissions to broader cybersecurity concepts such as access control, identity management, and the Principle of Least Privilege.
