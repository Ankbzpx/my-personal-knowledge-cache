---
attachments: [Clipboard_2021-08-26-10-43-24.png]
tags: [Rendering]
title: Android Graphics Architecture
created: '2021-08-26T02:42:22.240Z'
modified: '2021-08-26T03:03:28.840Z'
---

# Android Graphics Architecture

- OpenGL: Graphics API (standard/specification)
- OpenGL loading library: Loads pointers to OpenGL functions from OS specific drivers
- OpenGL ES: OpenGL for embedded system
- EGL: Interface between OpenGL ES and native platform window system (provide OpenGL context and an application window to draw in)

Reference: https://source.android.com/devices/graphics/architecture

![](@attachment/Clipboard_2021-08-26-10-43-24.png)
