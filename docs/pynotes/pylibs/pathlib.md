---
title: pathlib之使用面向对象的方式操作路径
date: 2024-11-06 22:24:22
tags:
  - Python
  - Python类库
---

> Reference：  

> - [官方文档](https://docs.python.org/3/library/pathlib.html)  

> - [pep-0428](https://peps.python.org/pep-0428/)


## 模块概述
### 应用场景：
如果你厌倦了使用os包提供的接口去操作文件路径……

如果你更喜欢以面向对象的方式去操作文件路径……

那么，pathlib 也许是一个不错的选择！！！ 


### 模块介绍
pathlib（Object-oriented filesystem paths）是python官方封装提供的一种使用面向对象的方式操作文件路径的工具包，在python3.4中发布，可以支持跨平台（Unix和Windows），pathlib基于是否有I/O操作提供了两种方式来操作文件路径,一种是提供纯计算操作而没有I/O的**纯路径**，另一种是提供I/O操作的**具体路径**。
```python
         +----------+
                |          |
       ---------| PurePath |--------
       |        |          |       |
       |        +----------+       |
       |             |             |
       |             |             |
       v             |             v
+---------------+    |    +-----------------+
|               |    |    |                 |
| PurePosixPath |    |    | PureWindowsPath |
|               |    |    |                 |
+---------------+    |    +-----------------+
       |             v             |
       |          +------+         |
       |          |      |         |
       |   -------| Path |------   |
       |   |      |      |     |   |
       |   |      +------+     |   |
       |   |                   |   |
       |   |                   |   |
       v   v                   v   v
  +-----------+           +-------------+
  |           |           |             |
  | PosixPath |           | WindowsPath |
  |           |           |             |
  +-----------+           +-------------+
```
### 实现原理：

pathlib包主要组成结构：

- **PurePath(object): 该类提供一套基于非I/O操作的文件路径的接口。**
- PurePosixPath(PurePath): 该提供一套在Unix环境中基于非I/O操作的文件路径接口。
- PureWindowsPath(PurePath): 该提供一套在Windows环境中基于非I/O操作的文件路径接口。

- **Path(PurePath): 该类提供一套基于I/O操作的文件路径接口。**
- PosixPath(Path, PurePosixPath): 该类提供一套在Unix环境中基于I/O操作的文件路径接口。
- WindowsPath(Path, PureWindowsPath): 该类提供一套在windows环境中基于I/O操作的文件路径接口。

**通常，Path是在日常开发中使用频率最高的类，可以类比os.path。**

## Path类的常用接口
> 备注：下列仅列出了日常开发中常用的接口，更全的接口清单请见官方文档

### 路径（查找）接口：

- Path.cwd()： 
    - 返回一个新的表示当前目录的路径对象（和 [os.getcwd()](https://docs.python.org/zh-cn/3/library/os.html#os.getcwd) 返回的相同）

- Path.home()：
    - 返回一个表示用户家目录的新路径对象（与带 ~ 构造的 [os.path.expanduser()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.expanduser) 所返回的相同）。 如果无法解析家目录，则会引发 [RuntimeError](https://docs.python.org/zh-cn/3/library/exceptions.html#RuntimeError)

- Path.exists(*, follow_symlinks=True):
    - 如果路径指向现有的文件或目录则返回 True

- Path.glob(pattern, *, case_sensitive=None):
    - 解析相对于此路径的通配符 _pattern_，产生所有匹配的文件，pattern 的形式与 [fnmatch --- Unix 文件名模式匹配](https://docs.python.org/zh-cn/3/library/fnmatch.html#module-fnmatch) 的相同，还增加了 "**" 表示 "此目录以及所有子目录，递归"。 换句话说，它启用递归通配
    - 在一个较大的目录树中使用 "**" 模式可能会消耗非常多的时间。

- Path.rglob(pattern, *, case_sensitive=None):
    - 递归地对给定的相对 _pattern_ 执行 glob 通配。 这类似于当调用 [Path.glob()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.glob) 时在 _pattern_ 之前加上 "**/"，其中 _patterns_ 与 [fnmatch](https://docs.python.org/zh-cn/3/library/fnmatch.html#module-fnmatch) 中的相同:

-  Path.iterdir():
    - 当路径指向一个目录时，产生该路径下的对象的路径， 子条目会以任意顺序生成，并且不包括特殊条目 '.' 和 '..'。 如果迭代器创建之后有文件在目录中被移除或添加，是否要包括该文件所对应的路径对象并没有明确规定。

- Path.walk(top_down=True, on_error=None, follow_symlinks=False)
    - 通过对目录树自上而下或自下而上的遍历来生成其中的文件名。对于根位置为 _self_ 的目录树中的每个目录（包括 _self_ 但不包括 '.' 和 '..'），该方法会产生一个 3 元组 (dirpath, dirnames, filenames)。
        - dirpath_ 是指向当前正被遍历到的目录的 [Path](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path)，
        - dirnames_ 是由表示 _dirpath_ 中子目录名称的字符串组成的列表 (不包括 '.' 和 '..')，
        - filenames_ 是由表示 _dirpath_ 中非目录文件名称的字符串组成的列表。 要获取 _dirpath_ 中文件或目录的完整路径 (以 _self_ 开头)，可使用 dirpath / name。 这些列表是否排序取决于具体的文件系统。

-  Path.absolute():
    - 改为绝对路径

- Path.resolve(strict=False):
    - 将路径绝对化


### 路径（判断）接口
- Path.is_dir():
    - 如果路径指向一个目录（或者一个指向目录的符号链接）则返回 True，如果指向其他类型的文件则返回 False。

- Path.is_file():
    - 如果路径指向一个正常的文件（或者一个指向正常文件的符号链接）则返回 True，如果指向其他类型的文件则返回 False。

- Path.**samefile**(_other_path_):
    - 返回此目录是否指向与可能是字符串或者另一个路径对象的 _other_path_ 相同的文件。语义类似于 [os.path.samefile()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.samefile) 与 [os.path.samestat()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.samestat)。

### 路径（读写）接口
- Path.mkdir(mode=0o777, parents=False, exist_ok=False):
    - 新建给定路径的目录。如果给出了 _mode_ ，它将与当前进程的 umask 值合并来决定文件模式和访问标志。如果路径已经存在，则抛出 [FileExistsError](https://docs.python.org/zh-cn/3/library/exceptions.html#FileExistsError)。
    - _parents_ ->True: 任何找不到的父目录都会伴随着此路径被创建；它们会以默认权限被创建，而不考虑 _mode_ 设置
    - _parents_ ->False，则找不到的父级目录会引发 [FileNotFoundError](https://docs.python.org/zh-cn/3/library/exceptions.html#FileNotFoundError)。
    - _exist_ok_ 为 false（默认），则在目标已存在的情况下抛出 [FileExistsError](https://docs.python.org/zh-cn/3/library/exceptions.html#FileExistsError).
    - _exist_ok_ 为 true， 则 [FileExistsError](https://docs.python.org/zh-cn/3/library/exceptions.html#FileExistsError) 异常将被忽略（和 POSIX mkdir -p 命令行为相同），但是只有在最后一个路径组件不是现存的非目录文件时才生效。

- Path.rmdir():
    - 移除此目录。此目录必须为空的。

- Path.touch(mode=0o666, exist_ok=True)
    - 将给定的路径创建为文件。如果给出了 _mode_ 它将与当前进程的 umask 值合并以确定文件的模式和访问标志。如果文件已经存在，则当 _exist_ok_ 为 true 则函数仍会成功（并且将它的修改事件更新为当前事件），否则抛出 [FileExistsError](https://docs.python.org/zh-cn/3/library/exceptions.html#FileExistsError)。

- Path.rename(target):
    - 将文件名目录重命名为给定的 _target_，并返回一个新的指向 _target_ 的 Path 实例。 在 Unix 上，如果 _target_ 存在且为一个文件，如果用户有足够权限则它将被静默地替换。 在 Windows 上，如果 _target_ 存在，则将会引发 [FileExistsError](https://docs.python.org/zh-cn/3/library/exceptions.html#FileExistsError)。 _target_ 可以是一个字符串或者另一个路径对象:

- Path.open(mode='r', buffering=- 1, encoding=None, errors=None, newline=None):
    - 打开路径指向的文件，就像内置的 [open()](https://docs.python.org/zh-cn/3/library/functions.html#open) 函数所做的一样

- Path.read_bytes():
    - 以字节对象的形式返回路径指向的文件的二进制内容

- Path.read_text(encoding=None, errors=None):
    - 以字符串形式返回路径指向的文件的解码后文本内容。

- Path.write_bytes(data):
  - 将文件以二进制模式打开，写入 _data_ 并关闭,一个同名的现存文件将被覆盖。

- Path.write_text(data, encoding=None, errors=None, newline=None):
    - 将文件以文本模式打开，写入 _data_ 并关闭,同名的现有文件会被覆盖。 可选形参的含义与 [open()](https://docs.python.org/zh-cn/3/library/functions.html#open) 的相同。

- Path.stat(*, follow_symlinks=True):
    - 返回一个 [os.stat_result](https://docs.python.org/zh-cn/3/library/os.html#os.stat_result) 对象，其中包含有关此路径的信息，例如 [os.stat()](https://docs.python.org/zh-cn/3/library/os.html#os.stat)。 结果会在每次调用此方法时重新搜索。

### 路径（权限）接口
- Path.chmod(mode, *, follow_symlinks=True)
    - 改变文件模式和权限，和 [os.chmod()](https://docs.python.org/zh-cn/3/library/os.html#os.chmod) 一样。

- Path.owner():
    - 返回拥有此文件的用户名。如果文件的 UID 无法在系统数据库中找到，则抛出 [KeyError](https://docs.python.org/zh-cn/3/library/exceptions.html#KeyError)。

- Path.group():
    - 返回拥有此文件的用户组。如果文件的 GID 无法在系统数据库中找到，将抛出 [KeyError](https://docs.python.org/zh-cn/3/library/exceptions.html#KeyError) 。

## pathlib和os接口对照
| [pathlib](https://docs.python.org/zh-cn/3/library/pathlib.html#module-pathlib) |  | [os](https://docs.python.org/zh-cn/3/library/os.html#module-os) 和 [os.path](https://docs.python.org/zh-cn/3/library/os.path.html#module-os.path) |
| --- | --- | --- |
| [Path.absolute()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.absolute) [1](https://docs.python.org/zh-cn/3/library/pathlib.html#id5) | 改为绝对路径，返回新的路径对象 | [os.path.abspath()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.abspath) |
| [Path.resolve()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.resolve) | 将路径绝对化，返回新的路径对象 | [os.path.realpath()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.realpath) |
| [Path.chmod()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.chmod) | 改变文件模式和权限 | [os.chmod()](https://docs.python.org/zh-cn/3/library/os.html#os.chmod) |
| [Path.mkdir()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.mkdir) | 新建给定的路径 | [os.mkdir()](https://docs.python.org/zh-cn/3/library/os.html#os.mkdir) |
| [Path.mkdir()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.mkdir) | 同上 | [os.makedirs()](https://docs.python.org/zh-cn/3/library/os.html#os.makedirs) |
| [Path.rename()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.rename) | 将文件名目录重命名为给定的 _target，需要权限_ | [os.rename()](https://docs.python.org/zh-cn/3/library/os.html#os.rename) |
| [Path.replace()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.replace) | 将文件或目录重命名为给定的 _target，强制替换_ | [os.replace()](https://docs.python.org/zh-cn/3/library/os.html#os.replace) |
| [Path.rmdir()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.rmdir) | 移除一个空目录 | [os.rmdir()](https://docs.python.org/zh-cn/3/library/os.html#os.rmdir) |
| [Path.unlink()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.unlink) | 移除一个文件 | [os.remove()](https://docs.python.org/zh-cn/3/library/os.html#os.remove), [os.unlink()](https://docs.python.org/zh-cn/3/library/os.html#os.unlink) |
| [Path.cwd()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.cwd) | 放回当前路径的路径对象 | [os.getcwd()](https://docs.python.org/zh-cn/3/library/os.html#os.getcwd) |
| [Path.exists()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.exists) | 判断文件或目录是否存在 | [os.path.exists()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.exists) |
| [Path.expanduser()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.expanduser) 和 [Path.home()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.home) | 获取表示用户家目录的新路径对象 | [os.path.expanduser()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.expanduser) |
| [Path.iterdir()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.iterdir) | 遍历当前路径 | [os.listdir()](https://docs.python.org/zh-cn/3/library/os.html#os.listdir) |
| [Path.walk()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.walk) | 通过对目录树自上而下或自下而上的遍历来生成其中的文件名 | [os.walk()](https://docs.python.org/zh-cn/3/library/os.html#os.walk) |
| [Path.is_dir()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.is_dir) | 是否是目录 | [os.path.isdir()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.isdir) |
| [Path.is_file()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.is_file) | 是否文件 | [os.path.isfile()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.isfile) |
| [Path.is_symlink()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.is_symlink) | - | [os.path.islink()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.islink) |
| [Path.hardlink_to()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.hardlink_to) | - | [os.link()](https://docs.python.org/zh-cn/3/library/os.html#os.link) |
| [Path.symlink_to()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.symlink_to) | - | [os.symlink()](https://docs.python.org/zh-cn/3/library/os.html#os.symlink) |
| [Path.readlink()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.readlink) | - | [os.readlink()](https://docs.python.org/zh-cn/3/library/os.html#os.readlink) |
| [PurePath.relative_to()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.relative_to) [2](https://docs.python.org/zh-cn/3/library/pathlib.html#id6) | - | [os.path.relpath()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.relpath) |
| [Path.stat()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.stat), [Path.owner()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.owner), [Path.group()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.group) | 获取文件和路径信息、权限 | [os.stat()](https://docs.python.org/zh-cn/3/library/os.html#os.stat) |
| [PurePath.is_absolute()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.is_absolute) | 是否是觉得路径 | [os.path.isabs()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.isabs) |
| [PurePath.joinpath()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.joinpath) | 路径拼接 | [os.path.join()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.join) |
| [PurePath.name](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.name) | 获取文件或目录名 | [os.path.basename()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.basename) |
| [PurePath.parent](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.parent) | 获取上级目录 | [os.path.dirname()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.dirname) |
| [Path.samefile()](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.samefile) | 判断是相同文件 | [os.path.samefile()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.samefile) |
| [PurePath.stem](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.stem) 和 [PurePath.suffix](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.suffix) | 获取文件名称，获取文件后缀 | [os.path.splitext()](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.splitext) |


## 实践应用
> 前置说明：方便起见，导包路径不在下列案例中重复添加

```python
from pathlib import *

import os
```
### 获取文件的父级目录
```python
# pathlib写法
pathlib.Path.cwd().parent

# os写法
os.path.dirname(os.path.dirname(os.getcwd()))
```
###  路径拼接
```python
# pathlib写法
paths = ('ssw', 'test')
pathlib.Path.cwd().parent.joinpath(*paths) 
# 或
pathlib.Path.cwd().parent / 'ssw' / 'test'
# 或
pathlib.Path.cwd().parent / 'ssw/test'

# os写法
os.path.join(os.path.dirname(os.getcwd()), 'ssw', 'test')
```
### 判断文件类别
```python
In [5]: path_tmp = Path('/data/sswang/projects/test/opserver/manage.py')

In [6]: Path(path_tmp).parent
Out[6]: PosixPath('/data/sswang/projects/test/opserver')

In [7]: Path(path_tmp).parents
Out[7]: <PosixPath.parents>

In [10]: list(Path(path_tmp).parents)
Out[10]: 
[PosixPath('/data/sswang/projects/test/opserver'),
 PosixPath('/data/sswang/projects/test'),
 PosixPath('/data/sswang/projects'),
 PosixPath('/data/sswang'),
 PosixPath('/data'),
 PosixPath('/')]

In [11]: Path(path_tmp).name
Out[11]: 'manage.py'

In [12]: Path(path_tmp).stem
Out[12]: 'manage'

In [13]: Path(path_tmp).suffix
Out[13]: '.py'

In [14]: Path(path_tmp).suffixes
Out[14]: ['.py']
```
### 打开文件
```python
In [39]: with p.open() as f:
    ...:     f.readline()
    ...: 

In [40]: p.read_text()
Out[40]: '#!/usr/bin/env python\n"""Django\'s command-line utility for administrative tasks."""\nimport os\nimport sys\n\n\ndef main():\n    """Run administrative tasks."""\n    os.environ.setdefault(\'DJANGO_SETTINGS_MODULE\', \'main.settings\')\n    try:\n        from django.core.management import execute_from_command_line\n    except ImportError as exc:\n        raise ImportError(\n            "Couldn\'t import Django. Are you sure it\'s installed and "\n            "available on your PYTHONPATH environment variable? Did you "\n            "forget to activate a virtual environment?"\n        ) from exc\n    execute_from_command_line(sys.argv)\n\n\nif __name__ == \'__main__\':\n    main()\n'

```

### 获取文件信息
```python
In [1]: from pathlib import *

In [2]: p1= Path.cwd()

In [3]: p1
Out[3]: PosixPath('/data/sswang/projects/test/opserver')

In [6]: p2 = Path('/data/sswang/projects/test/opserver/manage.py')

In [7]: p2.stat()
Out[7]: os.stat_result(st_mode=33188, st_ino=219153651, st_dev=2081, st_nlink=1, st_uid=0, st_gid=0, st_size=682, st_atime=1619528866, st_mtime=1619528322, st_ctime=1619528322)

In [9]: p2.stat().st_size
Out[9]: 682

In [10]: p2.stat().st_atime
Out[10]: 1619528866.8391194

In [11]: p2.stat().st_mode
Out[11]: 33188
```
### 查找文件
```python
In [12]: p3= Path('.').glob("*.py")

In [13]: p
Out[13]: <generator object Path.glob at 0x7f7baf232810>

In [15]: p= Path('.').glob("*.py")

In [16]: list(p)
Out[16]: 
[PosixPath('config_local.py'),
 PosixPath('config_public.py'),
 PosixPath('config.py'),
 PosixPath('manage.py')]

In [17]: p1= Path('.').glob("**/*.py")

In [18]: list(p1)
Out[18]: 
[PosixPath('config_local.py'),
 PosixPath('config_public.py'),
 PosixPath('config.py'),
 PosixPath('manage.py'),  
 PosixPath('opserver/apps.py'),
 ......
]

```









