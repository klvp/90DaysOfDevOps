<!-- @format -->

# Linux Architecture, Processes, and systemd

## Linux Core Architecture

### Three Main Layers

1. **Kernel** – Manages hardware, memory, CPU, disk I/O, networking
2. **User Space** – Applications, shells, libraries run here (isolated from kernel)
3. **Init System (systemd)** – First process (PID 1) that starts all other services and manages the system

### Why This Matters

- Kernel protects system stability; user space apps can't crash the OS
- systemd manages service lifecycle, dependencies, and system boot

---

## Process Management

### Process States

| State            | Meaning                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Running (R)**  | Currently executing on CPU                                   |
| **Sleeping (S)** | Waiting for I/O or event to continue                         |
| **Zombie (Z)**   | Process finished but parent hasn't collected its exit status |
| **Stopped (T)**  | Suspended by signal (SIGSTOP)                                |
| **Defunct**      | Orphaned zombie waiting for parent cleanup                   |

### How Processes Are Created

1. Parent process calls `fork()` – creates child copy
2. Child calls `exec()` – replaces itself with new program
3. Parent can wait for child or run independently
4. When child exits, parent gets exit status; if parent ignores it → zombie

---

## systemd – The Init System

### What It Does

- Starts services in parallel (faster boot than old init)
- Manages dependencies (Service A needs Service B to start first)
- Monitors and restarts failed services
- Provides centralized logging (journald)

### Key Files

- **Unit files:** `/etc/systemd/system/` and `/lib/systemd/system/`
- **Format:** Service files define how to start/stop/restart a service

---

## 5 Daily Commands

1. **`ps aux`** – View all running processes with memory/CPU usage

   ```
   ps aux | grep <service_name>
   ```

2. **`top`** – Real-time system resource monitoring (CPU, memory, processes)

   ```
   top -u <username>  # Filter by user
   ```

3. **`systemctl status <service>`** – Check service status and recent logs

   ```
   systemctl status nginx
   ```

4. **`journalctl -u <service> -n 50`** – View last 50 logs for a service

   ```
   journalctl -u docker -n 50 -f  # Follow live logs
   ```

5. **`kill -9 <PID>`** – Force kill a process (use with caution)
   ```
   pkill -f <process_pattern>  # Kill by pattern
   ```

---

## Quick Reference: Service Management

```bash
systemctl start <service>      # Start a service
systemctl stop <service>       # Stop a service
systemctl restart <service>    # Restart a service
systemctl enable <service>     # Enable at boot
systemctl disable <service>    # Disable at boot
systemctl list-units --type=service  # List all services
```

---

## Why This Matters for DevOps

✅ Debug crashed services quickly  
✅ Identify zombie processes causing memory leaks  
✅ Understand why a deployment failed  
✅ Monitor and manage thousands of processes in production  
✅ Write and manage custom systemd services

---

## Key Takeaway

**Linux = Kernel (heart) + User Space (apps) + systemd (conductor)**  
Understanding this helps you troubleshoot, optimize, and operate systems confidently.

# Refer Link

- https://chatgpt.com/share/6a0cb6b9-ac9c-83a7-b397-51fc1a8412bf
