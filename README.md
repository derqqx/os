sudo nano /etc/systemd/system/hello.service

[Unit]
Description=Custom Hello Service

[Service]
User=Zhumash
Group=Zhumash
ExecStart=/home/Zhumash/hello.sh
Restart=always

[Install]
WantedBy=multi-user.target


sudo systemctl daemon-reload
sudo systemctl restart hello.service
systemctl status hello.service


humash@Adilzhan:~$ systemctl status hello.service
● hello.service - Custom Hello Service
     Loaded: loaded (/etc/systemd/system/hello.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-10-15 02:07:01 +05; 6s ago
   Main PID: 7249 (hello.sh)
      Tasks: 2 (limit: 7671)
     Memory: 576.0K (peak: 1.0M)
        CPU: 20ms
     CGroup: /system.slice/hello.service
             ├─7249 /bin/bash /home/Zhumash/hello.sh
             └─7253 sleep 5

Oct 15 02:07:01 Adilzhan hello.sh[7249]: /home/Zhumash/hello.sh: line 3: /home/Zhumash/hello.log: Permission denied
Oct 15 02:07:06 Adilzhan hello.sh[7249]: /home/Zhumash/hello.sh: line 3: /home/Zhumash/hello.log: Permission denied
Zhumash@Adilzhan:~$ sudo kill 7249
Zhumash@Adilzhan:~$ ps aux | grep hello.sh
Zhumash     7271  0.3  0.0   9940  3504 ?        Ss   02:07   0:00 /bin/bash /home/Zhumash/hello.sh
Zhumash     7278  0.0  0.0   9276  2248 pts/1    S+   02:07   0:00 grep --color=auto hello.sh
Zhumash@Adilzhan:~$ 
