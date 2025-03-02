# Teamcity

## Install Teamcity
### Step 1: Chown directories
```bash
sudo chown -R 1000:1000 /data/teamcity_server/datadir
sudo chown -R 1000:1000 /opt/teamcity/logs
```

### Step 2: Pull from docker and run
```bash
sudo docker run -it --name teamcity-server \
-v <path to data directory>:/data/teamcity_server/datadir \
-v <path to logs directory>:/opt/teamcity/logs -p <port on host>:8111 \
jetbrains/teamcity-server
```
**Example Command**
```bash
sudo docker run -it --name teamcity-server \
-v /data/teamcity_server/datadir:/data/teamcity_server/datadir \
-v /opt/teamcity/logs:/opt/teamcity/logs -p 8111:8111 \
jetbrains/teamcity-server
```

### Step 3: Access Teamcity
Access Teamcity by the port you just designated in pull command
```
http://<your server ip addr>:<teamcity port on host>
```
<img src="https://i.imghippo.com/files/iDow4817j.jpg" alt="" border="0">

### Step 4: Configure basic settings
<div align="center">
<img src="https://i.imghippo.com/files/cMEo9838b.jpg" alt="" border="0" width="400px">
<img src="https://i.imghippo.com/files/zGfs9851qjE.jpg" alt="" border="0" width="400px">
</div>


## Create an Agent

### Step 1: Create an agent 

**Create using docker**

Your app will be running inside the agent docker container in this way.

Example code to create an agent based on `JDK 1.8`

```bash
sudo docker run -it \
  --name linkup-agent \
  -p 8090:8090 \
  --user root \
  -e SERVER_URL="http://3.82.238.126:8111" \
  -e TEAMCITY_AGENT_DISABLE_BUNDLED_JRE=true \
  -v team_city_agent_config:/data/teamcity_agent/conf \
  jetbrains/teamcity-agent:latest \
  /bin/bash -c "
    apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 && \
    export PATH=\$JAVA_HOME/bin:\$PATH && \
    /run-agent.sh
  "
```

- `-p 8090:8090`: if your project have mapped at port 8090, for example in your `application.yml` there is a line `server:
  port: 8090`. Then it the designated port for this agent should be `-p 8090:8090`
- `-e SERVER_URL="http://3.82.238.126:8111"`: This is the URL to TeamCity in your server

**Download an agent**

Download an agent manually. Configure its `buildAgent.properties` file and then store it somewhere at your server.

Boot command:
```bash
cd buildAgentFull/bin/
./agent.sh start
```


### Step 2: Authorize this agent

<div align="center"><img src="https://i.imghippo.com/files/OzYm4399dYs.jpg" alt="" border="0"></div>

## Create and Run a Project
This tutorial will take a **SpringBoot web app project** `https://github.com/Palpatine0/LinkUp.git` as an example

### Step 1: Create a project
<div align="center"><img src="https://i.imghippo.com/files/lqtn4657DY.jpg" alt="" border="0"></div>

Click `Create project..`

<div align="center"><img src="https://i.imghippo.com/files/bauZ9712TDo.jpg" alt="" border="0"></div>

Enter the repository URL and your credential then clicked `Processed` 

<div align="center"><img src="https://i.imghippo.com/files/CzN3321ntQ.jpg" alt="" border="0"></div>

Confirm the project information then clicked `Processed` again

### Step 2: Create an agent

Your app will be running inside the agent docker container in this way.

Example code to create an agent based on `JDK 1.8`

```bash
sudo docker run -it \
  --name linkup-agent \
  -p 8090:8090 \
  --user root \
  -e SERVER_URL="http://3.82.238.126:8111" \
  -e TEAMCITY_AGENT_DISABLE_BUNDLED_JRE=true \
  -v team_city_agent_config:/data/teamcity_agent/conf \
  jetbrains/teamcity-agent:latest \
  /bin/bash -c "
    apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 && \
    export PATH=\$JAVA_HOME/bin:\$PATH && \
    /run-agent.sh
  "
```

- `-p 8090:8090`: if your project have mapped at port 8090, for example in your `application.yml` there is a line `server:
  port: 8090`. Then it the designated port for this agent should be `-p 8090:8090`
- `-e SERVER_URL="http://3.82.238.126:8111"`: This is the URL to TeamCity in your server

<div align="center"><img src="https://i.imghippo.com/files/OzYm4399dYs.jpg" alt="" border="0"></div>


### Step 3: Configure build steps

<div align="center"><img src="https://i.imghippo.com/files/RRp4674qU.jpg" alt="" border="0"></div>

Go to `Edit configuration`

#### Create `Step 1` 

<div align="center"><img src="https://i.imghippo.com/files/lvmE3950Co.jpg" alt="" border="0"></div>

First, Click `Add build step` 

<div align="center"><img src="https://i.imghippo.com/files/jkEx8810jy.jpg" alt="" border="0"></div>

Then select `Maven`

<div align="center"><img src="https://i.imghippo.com/files/nBgs1667Qk.jpg" alt="" border="0"></div>

- `Goals`: What command will Maven execute. In step one this is `clean`

- `Path to POM file`: Path to POM file from the SpringBoot app you gonna run 

- `Maven Settings > Maven`: Designate a compatible Maven version with your project (if your agent is running on your server machine, not as a Docker container. You will have to choose `Custom` option and enter the location of your `Maven` file in your server)

- `Java Parameters > JDK`: Designate a compatible JDK version with your project (if your agent is running on your server machine, not as a Docker container. You will have to choose `Custom` option and enter the location of your JDK in your server)

Scroll to the button of this page and click **`Save`** after all critical options above is selected

#### Create `Step 2`

<div align="center"><img src="https://i.imghippo.com/files/qx3682LYc.jpg" alt="" border="0"></div>

Click button `Add build step` again

<div align="center"><img src="https://i.imghippo.com/files/jkEx8810jy.jpg" alt="" border="0"></div>

Select `Maven`

<div align="center"><img src="https://i.imghippo.com/files/Tmlb2414Nw.jpg" alt="" border="0"></div>

The `Goals` is package this time 

Scroll to the button of this page and click **`Save`** after all critical options above is selected

#### Create `Step 3`

<div align="center"><img src="https://i.imghippo.com/files/qx3682LYc.jpg" alt="" border="0"></div>

Click button `Add build step` again

<div align="center"><img src="https://i.imghippo.com/files/aLM1549Bng.jpg" alt="" border="0"></div>

Select `Command Line`

<div align="center"><img src="https://i.imghippo.com/files/aph8927URY.jpg" alt="" border="0"></div>

We will run the command line this time after the previous compile step is done. 
Change to the target dir (absolute path of the project dir) and run the jar file.

### Step 4: Configure general setting

<div align="center"><img src="https://i.imghippo.com/files/vJO1315OXg.jpg" alt="" border="0"></div>

### Step 5: Run the project

<div align="center"><img src="https://i.imghippo.com/files/VeK4138eBw.jpg" alt="" border="0"></div>