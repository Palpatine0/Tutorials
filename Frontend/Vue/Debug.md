# <span style="color: #990000">Vue</span>


###### _Project-Zillow_
## <span style="color: red">npm start: Permission denied</span>

**Content:**
```shell
➜  livegoods-vue sudo npm start
> livegoods@0.1.0 start
> concurrently "npm run serve" " nodemon mock/index.js "
[1] /bin/sh: /Users/A-FILES/Coding/Temp/APPS/livegoods-vue/node_modules/.bin/nodemon: Permission denied
[1]  nodemon mock/index.js  exited with code 126
[0]
[0] > livegoods@0.1.0 serve
[0] > vue-cli-service serve
[0]
[0] sh: /Users/A-FILES/Coding/Temp/APPS/livegoods-vue/node_modules/.bin/vue-cli-service: Permission denied
[0] npm run serve exited with code 126
```

**Solution**
```shell
sudo chmod -R u+rwx
```


## <span style="color: red">errors after some commands execute</span>
**Solution**

1. remove node_modules 

<img style="height: 30px" src="https://assets.leetcode.com/users/images/845b194a-7d4b-4d60-a266-5ef4796aeb35_1714122604.064862.jpeg">

2. reinstall npm
```shell
npm install
```
## <span style="color: red">having trouble running project downloads from other sources</span>

**Solution**
```shell
rm -rf node_modules
rm package-lock.json
npm install
npm run serve
```

