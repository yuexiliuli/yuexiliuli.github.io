# #mklink

创建符号链接。
```
MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      创建目录符号链接。默认为文件
                符号链接。
        /H      创建硬链接而非符号链接。
        /J      创建目录联接。
        Link    指定新的符号链接名称。
        Target  指定新链接引用的路径
                (相对或绝对)。
				
```

将`C:\Users\iCloudDrive\iCloud~md~obsidian\myku` 
链接到`E:\OneDrive\Notebooks\Obsidian\myku` 命令如下：
```
C:\Users\20294>mklink /d "E:\OneDrive\Notebooks\Obsidian\myku" "C:\Users\iCloudDrive\iCloud~md~obsidian\myku"
```

