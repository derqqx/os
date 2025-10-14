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
