# Access Control

## Models and Policies

**Subjects** (who) - users on UNIX

**Objects** (what) - files, processes, devices on UNIX

### Security Policy
- defines an access control matrix that maps subjects and objects to allowed operations
- users can have access rights to certain objects

### Complete Mediation
- every access to every object must be checked for authority by a mediator
- you need a foolproof way to tell where every request is coming from
- be careful with caching checks, since authorities can change

### Unix Security Model
- users and groups
- every user has a unique username and user id
- a user can belong to several groups, which provide access control
- superuser (id 0) has authority to do anything
users in a specific group (adm) can temporarily elevate to root priveleges
- every file and directory has an owner and a group
- every file has different permissions for the owner user, users that are a member of the file's group, and other users
- processes have an effective user id (EUID) that determine their permissions
- processes inherit user and group from parent process that started them
- executable files can have a setUID bit that allows them to change the UID, these files have permissions of their owner not the active user

### Isolation
- we should be skeptical of all programs, and isolate them to acheive least privelege
- reference monitor (part of the UNIX kernel) is responsible for isolation, must check and enforce every request from applications
- `chroot` changes the root of the filesystem for confined applications, meaning they can't access files outside of that new root, essentially a filesystem jail

### System call interposition
- processes must send system calls in order to cause problems
- have a monitor check all system calls and block unauthorized ones

### Confinement
- make sure misbehaving process can't harm the rest of the rest of the system
- confine it in a container (at the level of the operating system)
- confine it in a virtual machine (confinement at the level of the hypervisor, effectively separate machines)
- VM Security: malware can infect guest OS, but can't escape the VM and effect the host system
- VMs aren't totally secure because VMM (virtual machine manager) is too big to be verified, so it can have bugs
- VMs can have covert channels that exploit the fact that they share hardware, can be used to communicate between VMs and break permissions
- Ideal confinement is to run applications on separate hardware 
