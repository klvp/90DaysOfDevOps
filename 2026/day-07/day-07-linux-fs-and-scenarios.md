<!-- @format -->

# Day 07 — Linux File System Hierarchy & Scenario Practice

## Part 1 — File System Hierarchy

/ (root)

- Purpose: Top-level directory; all files and directories originate here.
- Example listing: `ls -l /` → `bin  boot  dev  etc  home  lib  proc  usr`
- I would use this when: locating mount points and system-wide folders.

/home

- Purpose: User home directories (personal files and configs).
- Example listing: `ls -l /home` → `alice  bob`
- I would use this when: checking user-specific config files or user data.

/root

- Purpose: Root user's home directory (administrator account).
- Example listing: `ls -la /root` → `.bashrc  .profile`
- I would use this when: performing root-only maintenance or private admin scripts.

/etc

- Purpose: System configuration files for services and system settings.
- Example listing: `ls -l /etc | head -n 5` → `hosts  hostname  ssh  systemd`
- I would use this when: editing service configs (nginx, sshd, systemd unit files).

/var/log

- Purpose: System and service logs — essential for troubleshooting.
- Example listing: `ls -lh /var/log | head -n 5` → `syslog  auth.log  kern.log`
- I would use this when: investigating errors, authentication failures, or service crashes.

/tmp

- Purpose: Temporary files; not preserved across reboots (often world-writable).
- Example listing: `ls -l /tmp` → `tmp1234  installer.log`
- I would use this when: creating throwaway files during debugging or scripts.

/bin, /usr/bin, /opt (additional)

- `/bin` and `/usr/bin`: essential and user binaries respectively (e.g., `ls`, `bash`).
- `/opt`: optional/third-party apps (e.g., custom tools installed by packages).
- I would use these when: locating executables or installing third-party tools.

### Hands-on commands to run now

```
# Find largest log files
du -sh /var/log/* 2>/dev/null | sort -h | tail -5

# View a config file
cat /etc/hostname

# Check home directory
ls -la ~
```

## Part 2 — Scenario-Based Practice (my solutions)

Scenario 1: Service Not Starting (service: `myapp`)

Step 1: `systemctl status myapp`
Why: Shows whether the service is `active`, `failed`, or `inactive`.

Step 2: `journalctl -u myapp -n 100 --no-pager`
Why: Reviews recent logs to find startup errors or stack traces.

Step 3: `systemctl is-enabled myapp && systemctl list-dependencies myapp`
Why: Verifies if it should start on boot and what unit files it depends on.

Step 4: `sudo systemctl restart myapp && sudo systemctl status myapp -l`
Why: Attempt a controlled restart and immediately observe detailed status.

Scenario 2: High CPU Usage (identify top CPU process)

Step 1: `top -b -n 1 | head -n 20`
Why: Quick snapshot showing processes sorted by CPU usage.

Step 2: `ps aux --sort=-%cpu | head -n 10`
Why: Provides a stable, sortable list of top CPU consumers with PID.

Step 3: `pidstat -p <PID> 1 3` (if available) or `ps -p <PID> -o pid,pcpu,pmem,cmd`
Why: Collect per-process CPU usage samples and confirm the culprit.

Scenario 3: Finding Service Logs (service: `docker`)

Step 1: `systemctl status docker`
Why: Quick check to see if `docker` is active and show recent log snippets.

Step 2: `journalctl -u docker -n 200 --no-pager`
Why: View the last 200 lines of docker logs managed by journald.

Step 3: `journalctl -u docker -f`
Why: Follow logs in real-time when reproducing issues.

Scenario 4: File Permissions Issue (`/home/user/backup.sh` not executable)

Step 1: `ls -l /home/user/backup.sh`
Why: Inspect current permission bits to confirm missing execute bit.

Step 2: `chmod +x /home/user/backup.sh`
Why: Add execute permission to allow running the script.

Step 3: `ls -l /home/user/backup.sh && /home/user/backup.sh`
Why: Verify permission change and run the script to ensure it executes.

## Final notes and quick checklist

- Always start with `systemctl status` for service issues, then check `journalctl`.
- For resource problems, capture a snapshot (`top`/`ps`) before restarting services.
- Use `/var/log` and `/etc` as primary investigation paths for logs and configs.

---

Prepared as a one-page practical note for Day 07: File system hierarchy summary and scenario solutions.
