<!-- @format -->

# Linux Practice: Processes, Services, and Logs

## Process Checks

1. `ps -ef | head -n 10`
   - Checked the top running processes and confirmed the shell, systemd, and sshd processes.

2. `pgrep -fl sshd`
   - Located the SSH daemon process and verified it was active for remote connection handling.

3. `top -b -n 1 | head -n 12`
   - Captured CPU and memory usage for the highest-resource processes.
   - Observed that `systemd` and `bash` were stable and there was no abnormal load.

## Service Checks

4. `systemctl status sshd.service`
   - Inspected the SSH service status.
   - Confirmed the service was `active (running)` and there were no recent failure messages.

5. `systemctl list-units --type=service --state=running | grep -E "sshd|cron|docker"`
   - Reviewed running services to ensure expected daemons were active.
   - Verified `sshd.service` and `cron.service` were in the running state.

## Log Checks

6. `journalctl -u sshd.service --since "1 hour ago" | tail -n 20`
   - Checked recent SSH logs for connection activity and service health.
   - Confirmed there were no repeated authentication failures.

7. `tail -n 20 /var/log/syslog`
   - Reviewed the latest system log entries for potential system warnings or errors.
   - Observed normal service startup messages and no critical failures.

## Mini Troubleshooting Steps

- Problem: SSH login attempts were slow.
- Investigation:
  1. Verified `sshd.service` was active with `systemctl status sshd.service`.
  2. Confirmed no high CPU or memory usage with `top` and `ps`.
  3. Checked recent SSH logs using `journalctl -u sshd.service`.
- Outcome: The service was running normally, and the delay appeared related to network latency rather than the SSH daemon itself.

---

## Summary

This practice note includes:

- 2 process commands: `ps`, `pgrep`, `top`
- 2 service commands: `systemctl status`, `systemctl list-units`
- 2 log commands: `journalctl`, `tail`
- A focused inspection of the `sshd` service with a small troubleshooting flow.
