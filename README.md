  GNU nano 7.2                                /etc/systemd/system/hello.service                                         
[Unit]
Description=Custom Hello Service

[Service]
ExecStart=/home/Zhumash/hello.sh
Restart=always

[Install]
WantedBy=multi-user.target

