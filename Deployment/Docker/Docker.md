# Docker

## Install Docker (AWS EC2 instance only)
### Step 1: Update the system and Install the latest package of `Docker`.
```bash
sudo yum update -y
```
```bash
sudo yum install docker -y
```

### Step 2: Start the service of `Docker`.
```bash
sudo systemctl start docker
```

## Import a Docker Img
This is applicable if you're having trouble using "docker pull" on your remote server. 
Simply upload the image from your laptop to your server. 
```bash
docker load < <dir>
```

## Save a Docker Img
```bash
docker save <img_name> <img_name>.tar 
```
## Rename
```bash
docker rename <old_container_name> <new_container_name>
```

