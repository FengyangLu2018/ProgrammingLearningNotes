# python cli
`python [-bBdEhiIOqsSuvVWx?] [-c command | -m module-name | script | - ] [args]`

## python -c command
1. 释义
- 执行-c后面的python代码，command可以是由换行符分隔的一条或多条语句。
2. 示例
- 单行代码
```bash
python3 -c "print('hello world')"
```
- 多行代码

```bash
pi@raspberrypi:~ $ python3 -d -c '''
def sum(a, b):
>     s=a+b
>     return s
> print(sum(1, 2))
> '''
3
```

## python -m module-name
1. 释义
- 在`sys.path`中寻找名为module-name的模块，将其内容作为__main__模块执行。
2. 示例

```bash
python3 -m pip install numpy
```

## python script
1. 释义
- 执行脚本中包含的Python代码，script必须是一个Python文件路径、包含__main__.py文件的目录或包含__main__.py文件的zipfile文件。
2. 示例
```bash
python3 setup.py
```
## 通用选项
1. 帮助选项`-? -h --help`

```bash
pi@raspberrypi:~ $ python3 -?
usage: python3 [option] ... [-c cmd | -m mod | file | -] [arg] ...
Options and arguments (and corresponding environment variables):
-b     : issue warnings about str(bytes_instance), str(bytearray_instance)
         and comparing bytes/bytearray with str. (-bb: issue errors)
-B     : don't write .pyc files on import; also PYTHONDONTWRITEBYTECODE=x
-c cmd : program passed in as string (terminates option list)
-d     : debug output from parser; also PYTHONDEBUG=x
-E     : ignore PYTHON* environment variables (such as PYTHONPATH)
-h     : print this help message and exit (also --help)
-i     : inspect interactively after running script; forces a prompt even
         if stdin does not appear to be a terminal; also PYTHONINSPECT=x
-I     : isolate Python from the user's environment (implies -E and -s)
-m mod : run library module as a script (terminates option list)
-O     : remove assert and __debug__-dependent statements; add .opt-1 before
         .pyc extension; also PYTHONOPTIMIZE=x
-OO    : do -O changes and also discard docstrings; add .opt-2 before
         .pyc extension
-q     : don't print version and copyright messages on interactive startup
-s     : don't add user site directory to sys.path; also PYTHONNOUSERSITE
-S     : don't imply 'import site' on initialization
-u     : force the stdout and stderr streams to be unbuffered;
         this option has no effect on stdin; also PYTHONUNBUFFERED=x
-v     : verbose (trace import statements); also PYTHONVERBOSE=x
         can be supplied multiple times to increase verbosity
-V     : print the Python version number and exit (also --version)
         when given twice, print more information about the build
-W arg : warning control; arg is action:message:category:module:lineno
         also PYTHONWARNINGS=arg
-x     : skip first line of source, allowing use of non-Unix forms of #!cmd
-X opt : set implementation-specific option
--check-hash-based-pycs always|default|never:
    control how Python invalidates hash-based .pyc files
file   : program read from script file
-      : program read from stdin (default; interactive mode if a tty)
arg ...: arguments passed to program in sys.argv[1:]

Other environment variables:
PYTHONSTARTUP: file executed on interactive startup (no default)
PYTHONPATH   : ':'-separated list of directories prefixed to the
               default module search path.  The result is sys.path.
PYTHONHOME   : alternate <prefix> directory (or <prefix>:<exec_prefix>).
               The default module search path uses <prefix>/lib/pythonX.X.
PYTHONCASEOK : ignore case in 'import' statements (Windows).
PYTHONIOENCODING: Encoding[:errors] used for stdin/stdout/stderr.
PYTHONFAULTHANDLER: dump the Python traceback on fatal errors.
PYTHONHASHSEED: if this variable is set to 'random', a random value is used
   to seed the hashes of str, bytes and datetime objects.  It can also be
   set to an integer in the range [0,4294967295] to get hash values with a
   predictable seed.
PYTHONMALLOC: set the Python memory allocators and/or install debug hooks
   on Python memory allocators. Use PYTHONMALLOC=debug to install debug
   hooks.
PYTHONCOERCECLOCALE: if this variable is set to 0, it disables the locale
   coercion behavior. Use PYTHONCOERCECLOCALE=warn to request display of
   locale coercion and locale compatibility warnings on stderr.
PYTHONBREAKPOINT: if this variable is set to 0, it disables the default
   debugger. It can be set to the callable of your debugger of choice.
PYTHONDEVMODE: enable the development mode.

```
2. 版本信息`-V --version`
```bash
pi@raspberrypi:~ $ python3 --version
Python 3.7.3
```
3. 其他选项
- -d 打开解析器调试输出。同PYTHONDEBUG。
- -E 忽略所有PYTHON*环境变量。
- -i 当一个脚本作为第一个参数传递或使用-c选项时，在执行脚本或命令后进入交互模式。同PYTHONINSPECT。
```bash
pi@raspberrypi:~ $ python3 -i -c '''
def sum(a, b):
    s=a+b
    return s
print(sum(1, 2))
'''
3
>>> sum(3, 4)
7
>>> sum(10, 50)
60
>>> 
```
- -I 以隔离模式运行Python，意味着-E和-s。在隔离模式下，`sys.path`既不包含脚本的目录，也不包含用户的site-packages目录。所有PYTHON*环境变量也会被忽略。为了防止用户注入恶意代码，可能会施加进一步的限制。

- -q 即使在交互模式下，也不要显示版权和版本信息。
```bash
pi@raspberrypi:~ $ python3
Python 3.7.3 (default, Dec 20 2019, 18:57:59) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
KeyboardInterrupt
>>> 
pi@raspberrypi:~ $ python3 -q
>>> 
```
- -s 不将用户site-packages目录添加到`sys.path`。
- -u 强制取消stdout和stderr的缓冲。此选项对stdin流没有影响。同PYTHONUNBUFFERED。
- -v 每次初始化模块时打印一条消息，显示加载模块的位置(文件名或内置模块)。当给定两次(-vv)时，为搜索模块时检查的每个文件打印一条消息。还提供了退出时模块清理的信息。同PYTHONVERBOSE。
- -W 警告信息控制。同PYTHONWARNINGS。具体设置如下，可以简写为-Wi, -Wd, -Wa, -We等。
  - -Wdefault  # Warn once per call location
  - -Werror    # Convert to exceptions
  - -Walways   # Warn every time
  - -Wmodule   # Warn once per calling module
  - -Wonce     # Warn once per Python process
  - -Wignore   # Never warn

- -x 跳过源代码的第一行，允许使用非unix形式的#!cmd。这是专为DOS黑客设置的功能。