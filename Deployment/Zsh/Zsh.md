# Zsh

## Install Oh My Zsh
### Step 1: Install zsh (if not installed)
```zsh 
sudo yum install zsh
```

### Step 2: Install Git (if not installed)
```zsh 
sudo yum install git
```

### Step 3: Install Oh My Zsh
```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Step 4: Change the default shell as Zsh (AWS EC2 instance)
```zsh
sudo vim /etc/passwd
```

### Step 5: Locate the line for ec2-user
Find the line that corresponds to `ec2-user`. It will look something like this:
```text
ec2-user:x:1000:1000:EC2 Default User:/home/ec2-user:/bin/bash
```
Update to 
```text
ec2-user:x:1000:1000:EC2 Default User:/home/ec2-user:/bin/zsh
```

## Update Oh My Zsh theme
### Step 1: Open Zsh configurations
```zsh
vim ~/.zshrc
```

### Step 2: Set the theme 
```text
ZSH_THEME="your theme"
```

### Step 3: Apply changes
```zsh
source ~/.zshrc
```


## Install Plugin `zsh-autosuggestions`

### Step 1: Install the `zsh-autosuggestions`
```zsh
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
```

### Step 2: Open Zsh configurations
```zsh
vim ~/.zshrc
```

### Step 2: Add the plugin
Add `zsh-autosuggestions` into `plugins=()`
```text
plugins=(zsh-autosuggestions)
```

### Step 4: Apply changes
```zsh
source ~/.zshrc
```

## Install Plugin `sudo`

### Step 2: Open Zsh configurations
```zsh
vim ~/.zshrc
```

### Step 2: Add the plugin
Add `sudo` into `plugins=()`
```text
plugins=(sudo)
```

### Step 3: Apply changes
```zsh
source ~/.zshrc
```