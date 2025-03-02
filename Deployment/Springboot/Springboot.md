# Springboot

## Deployment
### Step 1: Lifecycle operating

Perform a `clean` then a `package` for your project in your IDEA.
Then you will find a jar file with your project name as the prefix on your project `targer` directory. This is the one
we gonna uplaod later.

### Step 2: Upload your springboot project to your server

```bash
cd <project target dir>
scp <your-application.jar> root@<server IP>:<a dir on your server>
```

> **BE NOTE THAT**, if your project is using `.env`, this file should be place on the same dir where u store your `jar` file

### Step 3: Run the app
```bash
cd <path to your jar file>
nohup java -jar <your application.jar> &
```
**check logs**
```bash
tail -f nohup.out
```

## Add Maven to system
### Step 1: Add Maven to the PATH
Open your terminal and edit your shell configuration file:

For zsh (default on macOS):
```bash
vim ~/.zshrc
```
If you use bash instead:
```bash
vim ~/.bashrc
```

### Step 2: Add the following lines at the end of the file
Assume your maven dir path in your system is `/usr/local/apache-maven-3.6.3` 
```bash
export MAVEN_HOME=/usr/local/apache-maven-3.6.3
export PATH=$MAVEN_HOME/bin:$PATH
```

### Step 3: Reload your shell configuration
```bash
source ~/.zshrc
```