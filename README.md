# Nginx Web Server Troubleshooting (LEMP Stack) on Linux Ubuntu/Debian
A hands-on case study simulating a critical configuration failure on an Nginx web server, analyzing system logs, and successfully restoring infrastructure uptime.

## Project Objective
To demonstrate core Linux system administration skills, including package management, Nginx host configuration file logic, and debugging tools ('systemd', 'journalctl').

## Environment & Tech Stack
* **OS:** Linux (Ubuntu Server) deployed via VirtualBox
* **Web Server:** Nginx (LEMP Stack)
* **Diagnostic Tools:** 'nginx -t', 'systemctl', 'journalctl', 'nano', 'ls'
---
## Walkthrough & Troubleshooting Steps
### Step 1: Locating the Active Configuration
During initial infrastructure mapping, it was identified that the default Nginx configuration file was disabled. The live website is managed via a custom virtual host configuration file named 'mytest', located at:
'/etc/nginx/sites-available/mytest'
### Step 2: Simulating a Critical Failure (Sabotage)
To the test the logging and recovery workflow, a syntax error was intentionally introduced info the 'mytest' configuration file inside the 'server {}' block by adding a non-existent directive: 'brocken_test;'.
After saving the modifications and attempting to restart the service ('sudo systemctl restart nginx'), the web server completely failed to start, throwing a **failed** status.
### Step 3: Diagnostics & Log Analysis
To isolate the root cause, two essential sysadmin debugging methods were utilized:
1. **Configuration Syntax Testing:**
   Running 'sudo nginx -t' triggered the built-in Nginx parser, immediately pointing to a syntax issue within the virtual host file.
2. **Systemd Journal Analysis:**
   The 'sudo journalctl -xeu nginx' utility was used to inspect the detailed system log. The crash logs explicitly pointed to the 'ExecStartPre=' stage and triggerd an emergency '[emerg]' log level, registering the unknow directive.
### Step 4: Incident Resolution & Recovery
1. The virtual host file '/etc/nginx/sites-available/mytest' was reopened using the 'nano' editor, and the invalid line 'brocken_test' was removed.
2. A secondary check using 'sudo nginx -t' confirmed that the configuration syntax was fully validated ('syntax is ok', 'test is successful').
3. The service was successfully restarted using ' sudo systemctl restart nginx'.
4. The wev server daemon status successfully reverted to **active (running):**
---
