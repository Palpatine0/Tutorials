# Java

## Install Java
Install Java on AWS EC2 
```
sudo dnf install java-1.8.0-amazon-corretto -y
```

## Version Shifting

##### Process
### Step 1: Locate your JDK
```shell
/usr/libexec/java_home -V
``` 
<img src="https://pic.leetcode.cn/1713839228-BZMHih-WeChat380c19b7731e728ba336174e753a5c62.jpg" width="400px">

### Step 1: Shift
Pick a version dir and export as "JAVA_HOME"
```shell
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
```
check if it is done

<img src="https://assets.leetcode.com/users/images/60568817-2b2e-451e-98d4-693bf1c0b5af_1713839912.1686397.jpeg" width="400px">

