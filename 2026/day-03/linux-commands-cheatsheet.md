<!-- @format -->

# Linux Commands Cheat Sheet

## Process Management

- `ps aux` — list all running processes with user, CPU, MEM, and command details.
- User ID (UID): Type `id -u` to see your current user ID.
- Group ID (GID): Type `id -g` to see your primary group ID.
- `pgrep <process_name>` to check PID of process
- `top` — interactively monitor system processes and resource usage in real time.
- `htop` — enhanced interactive process viewer with easier navigation and sorting.
- `pstree` — show processes in a tree view to understand parent/child relationships.
- `kill <PID>` — send SIGTERM to gracefully stop a process by PID.
- `kill -9 <PID>` — force kill a stubborn process immediately.
- `pkill <name>` — kill processes by name instead of PID.
- `nice -n 10 <cmd>` — run a command with lower priority.
- `renice 5 -p <PID>` — change an existing process priority.
- `nohup <cmd> &` — run a command in the background and ignore hangup signals.

## File System

- `ls -lah` — list files with sizes, permissions, owners, and hidden entries.
- `cd /path/to/dir` — change the current working directory.
- `pwd` — print the full path of the current directory.
- `mkdir -p /path/to/newdir` — create directories, including parent directories.
- `rm -rf <file|dir>` — remove files or directories recursively and forcefully.
- `cp -r src dest` — copy files or directories recursively.
- `mv src dest` — move or rename files and directories.
- `find /path -type f -name "*.log"` — search for files matching a pattern.
- `du -sh /path` — show the size of a directory or file in human-readable form.
- `df -h` — display disk space usage for mounted filesystems.

## Networking

- `ping 8.8.8.8` — test network connectivity and packet round-trip time.
- `ip addr` — display IP addresses and network interfaces.
- `dig example.com` — query DNS records and verify name resolution.
- `curl -I https://example.com` — fetch HTTP headers for a URL.
- `ss -tulpn` — show listening sockets and the processes using them.
- `traceroute 8.8.8.8` — trace the network path packets take to a host.

## Text and Logs

- `cat file.txt` — output the full contents of a file.
- `less file.log` — page through a file interactively.
- `tail -n 50 /var/log/syslog` — show the last 50 lines of a log file.
- `tail -f /var/log/syslog` — follow a log file as new lines are appended.
- `grep -i "error" /var/log/app.log` — search case-insensitively for text in a file.
- `grep -R "TODO" .` — recursively search files for a string.
- `awk '{print $1,$2}' file.txt` — process and extract columns from text.
- `sed -n '1,10p' file.txt` — print specific lines from a file.

## Permissions and Ownership

- `chmod 755 script.sh` — set file permissions to owner rwx and group/others rx.
- `chown user:group file.txt` — change the owner and group of a file.
- `stat file.txt` — show file metadata, including permissions and timestamps.

## Compression and Archives

- `tar -czf archive.tar.gz /path/to/dir` — create a gzip-compressed tar archive.
- `tar -xzf archive.tar.gz` — extract a gzip-compressed tar archive.

## Useful System Commands

- `uname -a` — display kernel and system information.
- `uptime` — show how long the system has been running and load averages.
- `whoami` — print the current logged-in user.
- `sudo <command>` — run a command with root privileges.

## Quick Troubleshooting Workflow

1. Check system state: `uptime` and `top`.
2. Inspect network: `ip addr`, `ping`, and `ss -tulpn`.
3. Search logs: `tail -f /var/log/syslog` and `grep -i "error" /var/log/app.log`.
4. Find problem files: `du -sh /path` and `find /path -type f -name "*.log"`.
5. Stop or restart services: `systemctl status <svc>`, `sudo systemctl restart <svc>`.
