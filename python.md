## File management

### Move a file
[SO answer](https://stackoverflow.com/a/8858026/6332373)
```python
import os
import shutil

os.rename("path/to/current/file.foo", "path/to/new/destination/for/file.foo")
shutil.move("path/to/current/file.foo", "path/to/new/destination/for/file.foo")
```

## Packaging
### Reload module entirely
[SO answer](https://stackoverflow.com/a/3194343/6332373)

When you type `import something` the module is imported only if not already there. Hence if you modify the module it will not be reimported with the new changes. 

There is a [`reload` method](https://stackoverflow.com/a/437591/6332373) that should work but I didn't manage to make it work (probably because I'm using a Jupyter configuration).

What worked for me was to delete the module from the system *with all its submodules*. The last important because they share the same reference code at import and hence they will refer to the old version of the module (if I understood importing correctly). 

Finally keep in mind that if the module previously used some common import from other modules those will be lost, hence if you were using 
`numpy` from another module import you have to reimport it at the start of the file (again if I understood importing correctly). 

```python
for mod in sys.modules.keys(): 
    if mod.startswith('mymodule.'): #or 'mymodule.mysbmodule.'
        del(sys.modules[mod])
# then reimport here
```
