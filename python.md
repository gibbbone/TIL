## File management

#### Move a file
[SO answer](https://stackoverflow.com/a/8858026/6332373)
```python
import os
import shutil

os.rename("path/to/current/file.foo", "path/to/new/destination/for/file.foo")
shutil.move("path/to/current/file.foo", "path/to/new/destination/for/file.foo")
```

