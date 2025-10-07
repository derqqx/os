import re
from collections import Counter
import os
import stat
import sys

LOG_FILE = "/var/log/auth.log" 
PASSWD_FILE = "/etc/passwd"
OUTPUT_FILE = "suspicious_activity.txt"

print("Starting log analysis...")
failed_ips = []
failed_users = set()

ip_regex = re.compile(r'from (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})')
user_regex = re.compile(r'for (invalid user |)(\w+)')

try:
    with open(LOG_FILE, 'r', errors='ignore') as f:
        for line in f:
            if "Failed password" in line:
                
                ip_match = ip_regex.search(line)
                if ip_match:
                    failed_ips.append(ip_match.group(1))
                
                user_match = user_regex.search(line)
                if user_match:
                    failed_users.add(user_match.group(2))

except FileNotFoundError:
    print("Error: Log file not found at {}. Exiting.".format(LOG_FILE))
    sys.exit(1)
except PermissionError:
    print("Error: Permission denied for {}. Run with 'sudo python3 security_analyzer.py'".format(LOG_FILE))
    sys.exit(1)

ip_counts = Counter(failed_ips)

print("Creating {}...".format(OUTPUT_FILE))
with open(OUTPUT_FILE, 'w') as out:
    out.write("Suspicious IPs and number of failed attempts:\n")
    for ip, count in ip_counts.items():
        out.write("{}: {} attempts\n".format(ip, count))

print("Found failed login attempts for users: {}".format(", ".join(failed_users)))


print("\nChecking home directory permissions (looking for 777)...")
insecure_users = []

try:
    with open(PASSWD_FILE, 'r') as f:
        for line in f:
            parts = line.split(":")
            if len(parts) >= 6:
                username = parts[0]
                home_dir = parts[5].strip()
                
                if os.path.isdir(home_dir):
                    try:
                        perm_mode = os.stat(home_dir).st_mode
                        # Extract the last three octal digits (e.g., '777')
