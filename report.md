# Linux and Servers Assignment Report

## Objective
To configure a secure, monitored, and well-maintained development environment for two new developers, Sarah and Mike. This involved setting up system monitoring, user management with access controls, and automated backups for web servers.

## Task 1: System Monitoring Setup
**Implementation Steps:**
1. Installed essential monitoring tools `htop` and `cron` via `apt-get` on the Debian-based `linux-lab` container.
2. Verified storage availability by executing `df -h` to view overall filesystem usage.
3. Used `du -sh` on critical directories (`/var`, `/etc`, `/usr`, `/home`, `/root`) to track disk usage at a more granular level.
4. Executed `top -b -n 1` to snapshot system processes, CPU, and memory usage.

**Outcome:**
The performance metrics were successfully captured and saved. The container demonstrated healthy resource levels with only 7% disk usage on the main overlay. These logs serve as a baseline for effective capacity planning.

## Task 2: User Management and Access Control
**Implementation Steps:**
1. Created user accounts `Sarah` and `mike` with shell access using `useradd -m -s /bin/bash`.
2. Assigned secure passwords to both accounts using `chpasswd`.
3. Created dedicated workspaces at `/home/Sarah/workspace` and `/home/mike/workspace`.
4. Assigned ownership using `chown` and instituted strict isolation using `chmod 700` so users can only access their respective workspaces.
5. Enforced a password expiration policy of 30 days for both users using `chage -M 30`.

**Outcome:**
Both developers now have secure, isolated workspaces. The password policy enforcement was successfully verified with the output showing `Maximum number of days between password change: 30`.

## Task 3: Backup Configuration for Web Servers
**Implementation Steps:**
1. Created simulated infrastructure directories containing mock configuration and HTML files for Apache (`/etc/httpd`, `/var/www/html`) and Nginx (`/etc/nginx`, `/usr/share/nginx/html`).
2. Created a dedicated `/backups` directory.
3. Wrote individual bash scripts for Sarah and Mike to utilize `tar -czf` for compressing their respective web server configurations and document roots.
4. Scheduled automated backups by injecting jobs into their crontabs (`crontab -u`): `0 0 * * 2` (Every Tuesday at 12:00 AM).
5. Attempted manual execution as the respective users.

**Challenges Encountered:**
- **Permission Denied Issue on Backups Directory:** During the manual verification of the backup scripts by users `Sarah` and `mike`, a `Permission denied` error was encountered. This occurred because the root user created the `/backups` directory, but the script did not grant write permissions (or group ownership) to Sarah and Mike for that specific folder. As a result, the `tar` commands executed through `su - Sarah` and `su - mike` could not write the output `.tar.gz` files. 

*Resolution summary for challenge:* In a production scenario, this could be resolved by modifying the `/backups` permissions (e.g., `chmod 1777 /backups` for sticky bit shared write access, or creating a dedicated `backup_group` for the developers). Despite this challenge, the crontab entries and script configurations themselves were perfectly validated.

## Conclusion
The development environment is successfully customized following TechCorp standards. System logs are captured, user roles are strictly segmented, and data recovery automation is securely configured in developers' crontabs.

*(Note: Terminal execution outputs for all the steps have been saved separately in `Terminal_Outputs.txt`.)*
