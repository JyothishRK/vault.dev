**2. System Load Metrics**  
Tracks the _load on the OS_.  
Monitor:  

- **Load average (1m, 5m, 15m)**
- **Runnable processes**
- **Blocked processes** (D state)
- **Zombie processes**

These reveal:  

- Thread exhaustion
- Disk I/O blocking
- Kernel-level stalls

**3. I/O & File System Operations (Beyond simple disk usage)**  
Monitor:  

- **Disk I/O wait %**
- **Read/write IOPS**
- **Disk queue length**
- **File descriptor usage**
- **Inode usage**
- **Filesystem errors**
- **Mount failures / unmounted partitions**

These catch:  

- Disk bottlenecks
- Exhausted file handles
- FS corruption

**4. Process & Resource Limits**  
Monitor:  

- **Top resource-hogging processes**
- **Number of running processes**
- **Open file limits (ulimit)**
- **Context switches (high = problem)**
- **Thread count**

These help detect:  

- Leaky processes
- Fork bombs
- High context switching

**5. Kernel & OS Health**  
Monitor:  

- **Kernel errors in syslog**
- **OOM-killer events**
- **Kernel panics**
- **Segfaults**
- **System reboots**
- **Dmesg warnings**

Catches:  

- Kernel crashes
- Unstable drivers
- Hardware errors

**7. System Availability Metrics**  
Monitor:  

- **Server uptime**
- **Reboot counts**
- **Unreachable periods**
- **Cloud instance status checks (AWS, GCP, Azure)**

E.g., AWS EC2 has:  

- System status check
- Instance status check

**9. Virtualization/Hypervisor Metrics (Cloud-specific)**  
Monitor:  

- **Steal time (CPU steal%)**
- **Hypervisor throttling**
- **Burst credit usage (AWS T-series)**

High steal time = noisy neighbor issue.**10. Backup & Storage Health**  
Monitor:  

- **Backup success/failure**
- **Snapshot age & consistency**
- **Volume performance tier**
- **Storage attachment errors (EBS, GCP PD)**







----
- CPU Utilization
- Memory Usage
- Disk space usage
- Top resource hogging processes
- No of running processes
- Health
	- Server Uptime
	- Reboot count
	- Unreachable periods
- 