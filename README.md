# Install Tremol ZFPLabServer on CentOS

## Add user

``` bash
sudo useradd -r -d /opt/zfplabserver -m -k /dev/zero -s /sbin/bologin zfplabserver
```

## Copy ZFPLabServer installation

``` bash
cp -r ${HOME}/temp/ZFPLABServerLinux/* /opt/zfplabserver
sudo chown -R zfplabserver:zfplabserver /opt/zfplabserver
sudo chmod u+x /opt/zfplabserver/zfplabserver
```

## Setup zfplabserver as service

* Create sysmtemd control script

``` bash
[Unit]
Description=ZFPLab Server
After=network.target

[Service]
Type=simple
WorkingDirectory=/opt/zfplabserver
User=zfplabserver
ExecStart=/opt/zfplabserver/zfplabserver
ExecStop=/usr/bin/curl -s http://localhost:4444/exit
Restart=always

[Install]
WantedBy=multi-user.target
' | sudo tee /etc/systemd/system/zfplabserver.service
```

* Reload systemd daemon
After ading new file, it is required to relod the systemd daemon in order the changes to take effect

``` bash
sudo systemctl daemon-reload
```

* Start zfplabserver service

``` bash
sudo systemctl enable --now zfplabserver
```
