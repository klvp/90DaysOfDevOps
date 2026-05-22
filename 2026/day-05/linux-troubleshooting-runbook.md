<!-- @format -->

# Linux Troubleshooting Runbook

## Target service / process

- Target: `sshd` service
- Purpose: Verify SSH daemon health and gather system resource context for a common remote access service.

## Environment basics

1. `uname -a`
   - Observed Linux kernel version and architecture. Confirmed system is Linux x86_64 and running a stable kernel.
2. `cat /etc/os-release`
   - Confirmed distribution details and version. System is Ubuntu-based with standard server release metadata.

## Filesystem sanity

3. `mkdir -p /tmp/runbook-demo && cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo`
   - Verified write access to `/tmp` and that file copy completed successfully.
   - Observed the copied `/etc/hosts` file present with expected permissions.
4. `df -h /tmp`
   - Confirmed `/tmp` is on the root filesystem and has healthy free space.
   - No low-disk warnings; available space is sufficient for temporary troubleshooting files.

## CPU & Memory snapshot

5. `ps -o pid,pcpu,pmem,comm -p $(pgrep -d',' sshd)`
   - Checked SSH daemon CPU and memory usage. `sshd` is using negligible CPU and only a small memory footprint.
6. `free -h`
   - Confirmed total memory, used memory, and available swap.
   - System has adequate free memory and swap is not heavily used.

## Disk / IO snapshot

7. `df -h`
   - Reviewed filesystem usage for root and key mount points.
   - Confirmed the root filesystem has safe headroom and no partitions are near 100%.
8. `du -sh /var/log`
   - Checked the size of log storage to ensure it is not growing unexpectedly.
   - `var/log` size is within normal bounds, indicating logs are not consuming excessive disk.

## Network snapshot

9. `ss -tulpn | grep sshd`
   - Verified SSHd is listening on the expected port (22) and bound to the active network interface.
   - Confirmed the process ID matches the running `sshd` service.
10. `ping -c 3 8.8.8.8`
    - Validated basic network connectivity from the host.
    - Observed low latency and no packet loss, indicating the network path is healthy.

## Logs reviewed

11. `journalctl -u sshd.service -n 50`
    - Reviewed the most recent SSH daemon log entries.
    - No recent errors or repeated authentication failures were found.
12. `tail -n 50 /var/log/auth.log`
    - Verified authentication log entries for recent SSH sessions.
    - Confirmed successful SSH logins with no suspicious failed attempts in the last 50 lines.

## Quick findings

- `sshd` service is active and running normally.
- CPU and memory for `sshd` are low and stable.
- Disk usage is healthy on root and log volumes.
- Network connectivity is functional and SSH is listening correctly.
- No critical SSH errors appeared in the latest logs.

## If this worsens

1. Restart `sshd` with `sudo systemctl restart sshd.service`, then immediately re-check status and logs.
2. Increase SSH logging verbosity in `/etc/ssh/sshd_config` and collect additional `journalctl` output if the issue reappears.
3. Use `strace -p <sshd-pid>` or `sshd -T` for deeper debugging if the service is failing to accept connections.
