Zhumash@Adilzhan:~$ sudo nano /etc/systemd/system/hello.service
Zhumash@Adilzhan:~$ sudo systemctl daemon-reload
Zhumash@Adilzhan:~$ sudo systemctl restart hello.service
Zhumash@Adilzhan:~$ systemctl status hello.service
● hello.service - Custom Hello Service
     Loaded: loaded (/etc/systemd/system/hello.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-10-15 01:59:03 +05; 8s ago
   Main PID: 6483 (hello.sh)
      Tasks: 2 (limit: 7671)
     Memory: 572.0K (peak: 1.0M)
        CPU: 21ms
     CGroup: /system.slice/hello.service
             ├─6483 /bin/bash /home/Zhumash/hello.sh
             └─6494 sleep 5
Zhumash@Adilzhan:~$ sudo kill 6483
Zhumash@Adilzhan:~$ ps aux | grep hello.sh
root        6519  0.0  0.0   9940  3728 ?        Ss   01:59   0:00 /bin/bash /home/Zhumash/hello.sh
Zhumash     6533  0.0  0.0   9276  2248 pts/1    S+   01:59   0:00 grep --color=auto hello.sh
Zhumash@Adilzhan:~$ 
