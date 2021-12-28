---
title:  How to copy folder content including hidden files
description: In this post, we are going to see how to copy folder content including hidden files in linux. 
tags: 
 - how-to
---

# Copy folder content including hidden files

You can **copy the content** of a folder `/source` to **another existing** folder `/dest`, **including hidden files**, with the command:

```bash
$ cp -a /source/. /dest/
```

 - The `-a` option **is an improved recursive option, that preserve all file attributes, and also preserve symlinks.**
 - The `.` at end of the source path is a specific `cp` syntax that **allowes to copy all files and folders, including hidden ones.**
