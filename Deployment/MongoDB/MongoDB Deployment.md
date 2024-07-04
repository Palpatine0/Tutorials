# MongoDB Deployment

### Step 1: Crate container

```bash
docker run -d -p 27017:27017 --name <your_mongodb_name> mongo:4.4.19-rc2 --auth
```

### Step 2: Enter mongodb

```bash
docker exec -it <your_mongodb_name> bash
mongo
```

### Step 3: Set auth

```bash
use admin
db.createUser({ user: "root", pwd: "  
root", roles: [{ role: "root", db: "admin" }] })
db.auth("root", "root");
```

