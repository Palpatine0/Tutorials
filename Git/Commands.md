# Commands

## SSH Keys Generate

##### Process
**S1: generate the key**
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
![11021716289607_.pic.jpg](https://assets.leetcode.com/users/images/97c498cf-4595-495b-9803-7af26f6a731f_1716289619.5681717.jpeg)


**S2: copy your key from where it belongs**
```bash
cd /path/to/your/key
cat your_key.pub | pbcopy
```

## Remove File from Being Commit 
```bash
git rm --cached -r <your file name>
```