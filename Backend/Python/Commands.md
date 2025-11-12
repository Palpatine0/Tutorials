# Commands

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

## Create a Virtual Environment
**Create environment** 
```bash
python -m venv <env name>
```

**Activate environment** 
```bash
conda activate <your proj name>source <env name>/bin/activate
```

**Install dependencies** 
```bash
pip install -r requirements.txt
```

## Create a Virtual Environment (Conda)
**Create conda environment**
```bash
conda create -n <your proj name> python=<python version>
```

**Activate environment**
```bash
conda activate <your proj name>
```

**Install dependencies**
```bash
pip install -r requirements.txt
```

**Deactivate**
```bash
conda deactivate           
```