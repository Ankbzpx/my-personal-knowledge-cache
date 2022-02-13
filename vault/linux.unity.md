---
id: zIRqOZmIxWSvvv225aZx4
title: Unity
desc: ''
updated: 1644464644928
created: 1644407240687
---

## Gnome DPI Scaling
>References: https://wiki.archlinux.org/title/HiDPI
https://forum.unity.com/threads/unity-hub-and-hidpi.604378/

- scale UI elements by an integer factor: `GDK_SCALE=2.0`
- undo scaling of text, fractional scale can be used: `GDK_DPI_SCALE=0.5`

```
GDK_SCALE=2.0 GDK_DPI_SCALE=0.5 path/to/unity/binary -projectpath path/to/unity/project/folder
```
