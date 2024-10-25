# Enterprise-Level DDoS Detection and Mitigation Script

This is an enterprise-level Bash script designed to detect and mitigate DDoS attacks on a server. The script monitors active connections and SYN packet activity on port 80 and takes action if predefined thresholds are exceeded.

## Features

- **Threshold-Based Detection**: Monitors active HTTP connections and SYN packets to detect potential DDoS attacks.
- **IP Blocking**: Identifies and blocks suspicious IPs via iptables, with an option to whitelist trusted IP addresses.
- **Logging**: Logs DDoS detection events to a specified log file.
- **Email Alerts**: Sends email alerts to the administrator if an attack is detected.
- **HTTPD Service Control**: Optionally stops and restarts the HTTPD service to mitigate heavy server load.

## Requirements

- Bash
- `netstat`
- `iptables`
- `mail` command (for email alerts)
- A running HTTPD service

## Usage

1. **Clone or Copy the Script**: Place the script on your server.

2. **Configure Thresholds**: Adjust the connection and SYN thresholds at the beginning of the script:
   ```bash
   ACTIVE_CONN_THRESHOLD=1000  # Active connections threshold
   SYN_ACTIVE_THRESHOLD=200    # SYN packets threshold
   ```

3. **Set Up Whitelist**:
   - Create a file named `whitelist.txt` and add one whitelisted IP per line.
   - Update the `WHITELIST_FILE` path in the script to point to `whitelist.txt`:
     ```bash
     WHITELIST_FILE="/path/to/whitelist.txt"
     ```

4. **Email Alerts**: Configure the alert email address and enable or disable alerts as needed:
   ```bash
   ALERT_EMAIL="admin@enterprise.com"
   SEND_ALERTS=true
   ```

5. **Run the Script**:
   - Execute the script manually or add it as a cron job for continuous monitoring.

## Script Overview

- **Threshold Checks**: The script monitors active connections and SYN packets on port 80.
- **Whitelisting**: IP addresses in `whitelist.txt` are exempt from blocking.
- **DDoS Detection**: If thresholds are exceeded, the script logs the event and identifies offending IPs.
- **IP Blocking**: Non-whitelisted IPs are blocked using `iptables`, and rules are saved.
- **Email Alert**: Sends a detailed alert email if IPs are blocked.
- **HTTPD Service Control**: Stops HTTPD during the attack and restarts it after a set period.

## Example whitelist.txt

The `whitelist.txt` file should contain IP addresses you want to exclude from blocking, with one IP per line:

```text
123.123.123.123
124.124.124.124
```

## Sample Output

When a DDoS attack is detected, you’ll see output like this in the terminal:

```bash
!!! ALERT !!! Possible DDoS Attack Detected
Active connections: 1050, SYN packets: 250
Identifying and blocking IP addresses...
Blocking IP: 192.168.1.10
Blocking IP: 192.168.1.15
...
Iptables rules saved.
Stopping HTTPD service...
Sending alert email to admin@enterprise.com
Restarting HTTPD service...
```

## Logging

DDoS detection events are logged to `/var/log/ddos_protection.log` by default. Modify the `LOG_FILE` variable if a different log path is needed.

## Notes

- Make sure to adjust the script’s parameters according to your server’s capacity and typical traffic.
- **Warning**: Be cautious when modifying iptables rules, especially on production servers.
- **Security**: The script should be run with root or sudo privileges to interact with `iptables` and control services.

## License

This script is released under the MIT License.

