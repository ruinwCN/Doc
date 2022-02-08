## pyinstaller

pyinstaller note

>  https://github.com/pyinstaller/pyinstaller/tree/d006b364cfabd1ac23f57c28b92d385331987edf/doc

### 1. install

```
pip install pyinstaller
```

### 2. pyinstaller options

    > https://pyinstaller.readthedocs.io/en/stable/usage.html#options

  ```bash
	pyinstaller --hidden-import=_cffi_backend -F main.py
  ```

### 3. get application path
```
import os
import sys


def getBasePath() -> str:
    return os.path.dirname(os.path.realpath(sys.argv[0]))
  

def checkPath():
    dirs = getBasePath() + '/log'
    if not os.path.exists(dirs):
        os.makedirs(dirs)

```
