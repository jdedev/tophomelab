
## Create service

[https://github.com/jdedev/tophomelab/blob/main/linux/systemd.md](https://github.com/jdedev/tophomelab/blob/main/linux/systemd.md)

```shell
#!/bin/bash

SERVICE_NAME="myservice"
SERVICE_FILE="/etc/systemd/system/$SERVICE_NAME.service"
SCRIPT_PATH="/usr/local/bin/myscript.sh"

# Create the systemd service file using a here-document
cat <<EOF | sudo tee $SERVICE_FILE >/dev/null
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
ExecStart=$SCRIPT_PATH
Restart=always
User=root
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

# Reload systemd, enable, and start the service

sudo systemctl daemon-reload
sudo systemctl enable $SERVICE_NAME
sudo systemctl start $SERVICE_NAME

echo "$SERVICE_NAME service has been created and started successfully."
echo "Check logs with: journalctl -u $SERVICE_NAME -f"
```