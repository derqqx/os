import re             
from collections import Counter
import os

cd ~/security_lab
sudo cp /var/log/auth.log sample_log.txt
sudo chmod 644 sample_log.txt


log_file = "/var/log/auth.log"
output_file = "suspicious_activity.txt"

with open(log_file, "r", errors="ignore") as f:
    logs = f.readlines()

ip_pattern = re.compile(r"(\d{1,3}(?:\.\d{1,3}){3})")
user_pattern = re.compile(r"Failed password for (?:invalid user )?(\w+)")

ips = []
users = []

for line in logs:
    if "Failed password" in line:
        ip_match = ip_pattern.search(line)
        user_match = user_pattern.search(line)
        if ip_match:
            ips.append(ip_match.group(1))
        if user_match:
            users.append(user_match.group(1))

ip_counts = Counter(ips)

with open(output_file, "w") as out:
    out.write("Suspicious IPs and number of failed attempts:\n")
    out.write("=============================================\n")
    for ip, count in ip_counts.items():
        out.write(f"{ip} - {count} attempts\n")

print(f"Suspicious activity saved to {output_file}")

print("\nChecking home directories permissions:")
with open("/etc/passwd", "r") as f:
    for line in f:
        parts = line.split(":")
        username = parts[0]
        home_dir = parts[5]
        if os.path.isdir(home_dir):
            perm = oct(os.stat(home_dir).st_mode)[-3:]
            if perm == "777":
                print(f"User {username} has insecure permissions on {home_dir} ({perm})")
