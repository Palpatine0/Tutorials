# Commands

## Switch Java versions

```bash
sudo alternatives --config java
```

## File transmission

```bash
scp <file need to send out> root@<server IP>:<a dir on your server>
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
