# Vue Deployment

### Step 1: Crate a `Dockerfile` on your vue project root dir

```dockerfile
# Use the node image from official Docker Hub
FROM node:<node version>-alpine as build-stage

# Set the working directory
WORKDIR /app

# Copy the working directory in the container
COPY package*.json ./

# Install the project dependencies
# if you npm then npm install
RUN yarn install

# Copy the rest of the project files to the container
COPY . .

# Build the Vue.js application to the production mode to dist folder
# here also if you use npm then npm run build
RUN yarn build

# Use the lightweight Nginx image from the previous state to the Nginx container
FROM nginx:stable-alpine as production-stage

# Copy the build application from the previous state to the Nginx container
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Copy the Nginx configuration file
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

# Expose the port 80 (for simplicity)
EXPOSE 80

# Start nginx to serve the application
CMD ["nginx", "-g", "daemon off;"]
```

### Step 2: Crate a `docker-compose` on your vue project root dir

```yaml
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: <vue project name>
    ports:
      - <port for vue project>:80 # Maps host port to container port 80  

  nginx:
    image: nginx:alpine
    container_name: prd_hazelgrove_svc_nginx
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    ports:
      - <port for vue nginx>:80  # Maps host port to container port 80
    depends_on:
      - app
```

### Step 3: Crate `nginx/default.conf` on your vue project root dir

```
server {
    listen 80;
    server_name <server IP addr>;
    root /usr/share/nginx/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```
### Step 4: Upload your vue project to your server
```bash
cd <project root dir>
scp <file need to send out> root@<server IP>:<a dir on your server>
```

### Step 5: Go to your vue project dir

### Step 6: Compose
```bash
docker-compose up -d
```

