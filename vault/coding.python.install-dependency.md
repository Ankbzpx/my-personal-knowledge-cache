---
id: UuQVS3jtitzbGoGwjT7Qq
title: Install Dependency
desc: ''
updated: 1642933008980
created: 1642932917749
---
## Command line
```
pip install numpy, opencv-python
```

## Python Script
Create `install dependency.py`
```
import subprocess
import sys

def install(package):
    subprocess.check_call([sys.executable, "-m", "pip", "install", package])

for pkg in ['numpy', 'opencv-python']:
    install(pkg)
```
Then
```
python dependency.py
```