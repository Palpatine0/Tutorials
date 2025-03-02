# Python

## Install Python 11
```bash
wget https://mirrors.sonic.net/python/3.11.0/Python-3.11.0.tgz
```

```bash
tar xzf Python-3.11.0.tgz
```

```bash
cd Python-3.11.0
```

```bash
sudo yum install -y gcc gcc-c++ make zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel xz xz-devel libffi-devel wget
```

```bash
./configure --enable-optimizations
```

```bash
make
```

## Update default Python version
**Example: Update to python 3.11**
```bash
sudo ln -sf /usr/local/bin/python3.10 /usr/local/bin/python
```