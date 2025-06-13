# Commands

## Prompting `(git@github.com) Password`

##### Process
**S1: run command**
```bash
ssh -T -p 443 git@ssh.github.com
```

**S2: Edit `~/.ssh/config`**

Edit `~/.ssh/config` so that when you talk to “github.com” it actually goes to ssh.github.com on port 443:
```text
Host github.com
  HostName ssh.github.com
  User git
  Port 443
  IdentityFile /Users/A-FILES/Coding/Github/Keys/Palpatine0/key
  IdentitiesOnly yes
  AddKeysToAgent yes
  UseKeychain yes  
```