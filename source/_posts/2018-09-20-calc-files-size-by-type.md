---
title: 计算同一类型文件的占用多少空间
date: 2018-09-20 15:35:24
tags:
---

假设有一个文件夹C:\content，里面有10W个文件，包含几万个PDF文件，还有几万个XML文件，如何统计它们占用的磁盘空间呢？

Windows下可以用PowerShell来统计，命令如下:

```powershell
"{0:N2}MB" -f ((gci -path C:\content -recurse -include *.pdf | Measure-Object length -Sum).Sum/ 1mb)
```

Linux下可以用du来统计:

```shell
du -ch -- **/*.pdf | tail -n 1
```