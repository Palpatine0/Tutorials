# Commands

## Import a Docker Img
This is applicable if you're having trouble using "docker pull" on your remote server. 
Simply upload the image from your laptop to your server. 
```bash
docker load < <dir>
```

## Save a Docker Img
```bash
docker save <img_name> > <img_name>.tar 
```
## Rename
```bash
docker rename <old_container_name> <new_container_name>
```

