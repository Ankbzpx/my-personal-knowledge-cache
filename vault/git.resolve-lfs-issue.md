---
id: FO60MeJ4Kfx7IA8rDev2F
title: Resolve Lfs Issue
desc: ''
updated: 1640017538106
created: 1640017530018
---

```
git rm --cached -r .
git reset --hard
git rm .gitattributes
git reset .
git checkout .
```