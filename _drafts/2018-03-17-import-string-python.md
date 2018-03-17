---
layout: post
title: "Importing using string - Python"
---

It's too convenient for importing libraries in Python using `importlib` where strings can be used to import any library., especially in Machine learning where you need multiple of imports.

Even, Django uses this to reduce cyclomatic dependency.


```python
def import_these(*pkg_args):
    return [import_this(pkg_str) for pkg_str in pkg_args]

def import_this(pkg_str: str):
    import importlib
    try:
        return importlib.import_module(pkg_str)
    except ModuleNotFoundError:
        print(f"The '{pkg_str}' module wasn't found."
              "Probably, it's not installed or it may not be in the path.")
        raise
    except AttributeError:
        print("Only strings are supported for import."
              f"'{type(pkg_str).__name__}' isn't supported.")
        raise
```
1. Importing a single module
```python
pathlib = import_this('pathlib')
```
Similar to 
```python
import pathlib
```

2. Importing a module from package
```python
keras_datasets = import_these('keras' '.' 'datasets')
```
Similar to 
```python
from keras import datasets as keras_datasets
```

3. Importing multiple packages/modules
```python
keras, sklearn, np, pd, pplot = import_these('keras', 'sklearn', 'numpy', 'pandas', 'matplotlib.pyplot')
```
Similar to:
```python
 import keras
 import sklearn
 import numpy as np
 import pandas as pd
 from matplotlib import pyplot as pplot
 ```


4. Oops, I don't need all at present.
```python
keras, _, np, pd, plot = import_these('keras', 'sklearn', 'numpy', 'pandas', 'matplotlib.pyplot')
```
Similar to not importing at all. :P

5. Importing from a tuple/list of *to be imported* packages/modules
```python
to_import = ['sys', 'time', 'os', 'pprint']
sys, time, os, pprint = import_these(*to_import)
pprint
```
Similar to 
```python
import sys, time, os, pprint
```

6. Importing from a string separated by space
```python
to_import = 'sys time os pprint'.split(' ')
import_these(*to_import)
```
Similar to 
```python
import sys, time, os, pprint
```

But, 5 and 6 are a lot powerful. You can import 
**dynamicallly** (*even from user's input*).
