# Linux

## Switch Java versions
```bash
sudo alternatives --config java
```

## File transmission

```bash
scp <file need to send out> root@<server IP>:<a dir on your server>
```

## Save a Maven to `/usr/local`
### Step 1: Chmod dir
```text
sudo chmod -R 777 /usr/local/
```

## Test if a port is available

```bash
nc -zv <server IP> <port number>
```

## Unzip a zip file
```bash
unzip <filename.zip>
```

## Kill process occupying port
### Step 1: Crate download `lsof`
```bash
sudo yum install lsof -y
```
### Step 2: Locate processing occupying the port
```bash
lsof -i :<port id>
```
### Step 3: Kill process
```bash
sudo kill -9 <pid>
```


## Ignore Termius login command
### Step 1: Edit `.bashrc`
```bash
nano ~/.bashrc
```

### Step 2: Add trap
```text
# Function to filter unwanted Termius login commands before they are written to history
filter_history() {
  # Escape sequence handling for printf and match for specific Termius patterns
  if [[ "$BASH_COMMAND" =~ "builtin printf \\e\\[\\?1049l" || "$BASH_COMMAND" =~ "builtin printf \\e\\[\\?1049h" || "$BASH_COMMAND" =~ "_termius_integration_installed" ]]; then
    # Clear the last history entry to prevent it from being saved
    history -d $(history 1)
  fi
}

# Trap DEBUG to execute filter_history before saving to history
trap 'filter_history' DEBUG
```

### Step 2: Source `.bashrc`
```bash
source ~/.bashrc
```

## Run commands on CentOS automatically at boot time
In this example, I will try to run `nohup java -jar /www/jar/xyp-api/xyp-api-0.0.1-SNAPSHOT.jar > xyp-api-nohup 2>&1 &`, a command to start a SpringBoot project when system started

### Step 1: Create a service file
```bash
sudo nano /etc/systemd/system/xyp-api.service
```
### Step 2: Add this into the file:
```ini
[Unit]
Description=XYP API Java Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/nohup java -jar /www/jar/xyp-api/xyp-api-0.0.1-SNAPSHOT.jar
WorkingDirectory=/www/jar/xyp-api
StandardOutput=append:/www/jar/xyp-api/xyp-api-nohup
StandardError=append:/www/jar/xyp-api/xyp-api-nohup
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```
### Step 3: Enable & start the service:
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable xyp-api
sudo systemctl start xyp-api
```