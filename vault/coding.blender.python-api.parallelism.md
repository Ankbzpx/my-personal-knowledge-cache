---
id: mehe0995foclne4lxh1ncqc
title: Parallelism
desc: ''
updated: 1646716542662
created: 1646715342469
---
> Reference: https://blender.stackexchange.com/questions/28304/using-multiprocessing-to-speed-up-multiple-sound-bakings

Python `multiprocessing` alone won't work as Blender's python is merely wrapper for C api. Instead, one could run multiple blender C thread as subprocesses to achieve parallelism.

> blender_main.py

```
import bpy
import multiprocessing
import subprocess

def call_subprocess(folder, name):
    ...
    subprocess.run([
        "blender", "-b", "-P", "blender_subprocess.py", "--", "{folder}", f"{name}"
    ])


if __name__ == "__main__":
    ...
    with multiprocessing.Pool(multiprocessing.cpu_count()) as p:
        # call subprocesses in parallel
        p.starmap(call_subprocess, zip(folders, names))
        # append result to current blender project
        for obj_name in names:
            bpy.ops.wm.append(
                filepath=f"tmp/{obj_name}.blend\\Object\\{obj_name}",
                filename=f"{obj_name}",
                directory=f"tmp/{obj_name}.blend\\Object\\")
            obj = bpy.data.objects[obj_name]
            obj.select_set(False)
```

> blender_subprocess.py

```
import bpy
import os

if __name__ == "__main__":

    argv = sys.argv
    argv = argv[argv.index("--") + 1:]

    folder = argv[0]
    name = argv[1]

    ...

    bpy.ops.wm.save_as_mainfile(filepath=os.path.join(folder, f"{name}.blend"),
                                check_existing=False)
```
