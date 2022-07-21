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