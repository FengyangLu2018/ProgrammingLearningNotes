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

# pip的使用
## 配置
### 配置文件
1. 虚拟环境级
- 在Unix和macOS上，该文件为`$VIRTUAL_ENV/pip.conf`
- 在Windows下，该文件为`%VIRTUAL_ENV%\pip.ini`
2. 用户级
- 在Unix上，默认的配置文件是:`$HOME/.config/pip/pip.conf`，它涉及了`XDG_CONFIG_HOME`环境变量。
- 在macOS上，如果目录`$HOME/Library/Application Support/pip`存在，则配置文件为`$HOME/Library/Application Support/pip/pip.conf`。
- 在Windows上，配置文件是`%APPDATA%\pip\pip.ini`。
- 还有一个遗留的单用户级配置文件也被认可。位置如下:
  - 在Unix和macOS上，配置文件为`$HOME/.pip/pip.conf`
  - 在Windows上，配置文件是`%HOME% pip\pip.ini`
3. 全局级
- 在Unix中，该文件可能位于`/etc/pip.conf`中。或者，它可能位于环境变量`XDG_CONFIG_DIRS`(如果存在)中设置的任何路径的“pip”子目录中，例如`/etc/xdg/pip/pip.conf`。
- 在macOS上，这个文件是`/Library/Application Support/pip/pip.conf`
- 在Windows XP上，这个文件是`C:\Documents&nbsp;and&nbsp;Settings\All Users\Application Data\pip\pip.ini`
- 在Windows 7和更高版本中，该文件是隐藏的，但可以在`C: ProgramData\pip\pip.ini`中写入
- Windows Vista不支持全局配置。
4. 读取顺序
- 按全局、用户、虚拟环境的顺序读取，后读取的覆盖先读取的
5. 示例
```python
[global]
timeout = 60
quiet = 0
verbose = 2
find-links =
    http://download.example.com


[install]
no-compile = no
no-warn-script-location = false
ignore-installed = true
no-dependencies = yes
find-links =
    http://mirror1.example.com
    http://mirror2.example.com
trusted-host =
    mirror1.example.com
    mirror2.example.com


[freeze]
timeout = 10
```
### 命令行选项
- 配置文件中的配置项也可以以命令行选项的形式指定。

### 环境变量
- pip的命令行选项也可以使用环境变量设置，格式为`PIP_<UPPER_LONG_NAME>`
  - `PIP_DEFAULT_TIMEOUT=60`与`--default-timeout=60`效果相同。
  - `PIP_FIND_LINKS="http://mirror1.example.com http://mirror2.example.com"`与`--find-links=http://mirror1.example.com --find-links=http://mirror2.example.com`效果相同。
  - 没有值的可重复的配置项(如——verbose)可以使用重复次数指定，`PIP_VERBOSE=3`与`pip install -vvv`效果相同。
  - 设置为空字符串的环境变量(如Unix上的export X=)不会被视为false。必须使用no, false或0代替。

### 优先级顺序
- 命令行选项覆盖环境变量，环境变量覆盖配置文件。在配置文件中，命令特定节中的值高于全局节中的值。
  - `--host=foo`高于`PIP_HOST=foo`
  - `PIP_HOST=foo`高于配置文件中的`[global] host = foo`
  - 配置文件中`[<command>] host = bar`高于`[global] host = bar`

## 解决“Permission denied:”错误
错误出现原因可能是：当命令仅为另一个用户安装，而当前用户没有执行另一个用户命令的权限时，就会发生此错误。
为了解决这个问题，可以尝试一下方法：
1、为自己安装这个命令；
2、请求管理员更改指令的权限，使所有用户都可以使用这个命令；
3、查看你所在环境的`PATH`变量；
4、查看此命令的ACL权限表。

## pip cli
### pip
```bash
python -m pip [options]
```
- `-h, --help` 显示帮助信息。
- `--isolated` 在隔离模式下运行pip，忽略环境变量和用户配置。
- `-v, --verbose` 显示更多的输出信息，这个选项是可以叠加的，最高到重复3次。
- `-V, --version` 显示版本并退出。
- `-q, --quiet` 显示更少的输出信息，这个选项可以叠加，最高到重复3次(对应WARNING, ERROR, and CRITICAL logging三个级别)。
- `--log <path>` 详细附加日志的路径。
- `--no-input` 禁止输入提示。
- `--proxy <proxy>` 指定一个代理，以[user:passwd@]proxy.server:port的格式。
- `--retries <retries>` 每个连接应尝试的最大重试次数(默认5次)。
- `--timeout <sec>` 设置套接字超时(默认15秒)。
- `--exists-action <action>` 当路径存在时的默认行为：(s)witch, (i)gnore, (w)ipe, (b)ackup, (a)bort.
- `--trusted-host <hostname>` 将这个host或host:port标记为可信的，尽管它没有可靠的HTTPS。
- `--cert <path>` pem编码的CA证书包路径。如果提供，则覆盖默认值。有关更多信息，请参阅pip文档中的“SSL证书验证”。
- `--client-cert <path>` SSL客户端证书的路径，包含私钥和PEM格式的证书的单个文件。
- `--cache-dir <dir>` 将缓存数据放在`dir`目录。
- `--no-cache-dir` 禁用缓存功能。
- `--disable-pip-version-check` 不要定期检查PyPI以确定是否有新的pip版本可供下载。隐含着`--no-index`。
- `--no-color` 禁止彩色输出。
- `--no-python-version-warning` 针对即将出现的不支持的python的静默弃用警告。
- `--use-feature <feature>` 启用可能向后不兼容的新功能。
- `--use-deprecated <feature>` 启用在将来会被删除的已弃用的功能。

### pip install
1. 用法
```bash
python -m pip install [options] <requirement specifier> [package-index-options] ...
python -m pip install [options] -r <requirements file> [package-index-options] ...
python -m pip install [options] [-e] <vcs project url> ...
python -m pip install [options] [-e] <local project path> ...
python -m pip install [options] <archive url/path> ...
```
- `-r, --requirement <file>` 从需求文件安装，这个选项可以被使用多次。
- `-c, --constraint <file>` 使用约束文件约束版本，这个选项可以被使用多次。
- `--no-deps` 不安装依赖项。
- `--pre` 包含预发布版本和开发版本。默认情况下，pip只搜寻稳定版本。
- `-e, --editable <path url>` 从本地目录或版本控制系统url以编辑模式(即setuptools的“develop mode”)安装一个项目。
- `-t, --target <dir>` 将包安装到`dir`目录下。默认情况下这不会取代`dir`中的文件或文件夹。使用`--upgrade`选项可以将`dir`中的旧版本替换为新版本。
- `--platform <platform>` 只使用与`platform`兼容的wheel文件。默认为运行系统的平台。多次使用此选项以指定目标解释器支持的多个平台。
- `--python-version <python_version>` 用于wheel和“require -Python”兼容性检查的Python解释器版本。默认为从正在运行的解释器派生的版本。版本可以使用最多三个点分隔的整数来指定(例如，" 3 "代表3.0.0，" 3.7 "代表3.7.0，或" 3.7.3 ")。major-minor版本也可以用不带点的字符串给出(例如3.7.0中的“37”)。
- `--implementation <implementation>` 只使用与Python实现`implementation`兼容的wheel文件，例如'pp'，'jy'，'cp'或'ip'。如果未指定，则使用当前解释器实现。
- `--abi <abi>` 只使用与Python abi `abi`兼容的wheel，例如'pypy_41'。如果未指定，则使用当前解释器abi标记。多次使用此选项以指定目标解释器支持的多个abi。通常，在使用此选项时，需要指定`--implementation`、`--platform`和`--python-version`。
- `--user` 安装到你平台的python用户安装地址。典型地如`~/.local/`或windows系统中`%APPDATA%Python`。(详情参考python文档中的site.USER_BASE变量)。
- `--root <dir>` 将所有包安装到以`dir`为根目录的相对路径中。
- `--prefix <dir>` 在lib、bin和其他顶级文件夹的目录前缀`dir`。
- `--src <dir>` 将可编辑项目签入的目录。virtualenv中的默认地址是“`venv path`/src”。全局的默认地址是“`current dir`/src”。
- `-U, --upgrade` 将所有指定的包升级到最新版本。是否升级依赖项取决于`upgrade-strategy`选项。
- `--upgrade-strategy <upgrade_strategy>` 决定了升级时如何处理依赖项：
  - `only-if-needed`：默认行为，只有不满足需求才升级依赖项。
  - `eager`：所有依赖项都被升级，无论它们是否仍然满足需求项。
- `--force-reinstall` 重新安装所有软件包，即使它们已经是最新的。
- `-I, --ignore-installed` 忽略已安装的包，覆盖它们。如果现有的包是不同版本的，或者安装了不同的包管理器，这可能会破坏您的系统!
- `--ignore-requires-python` 忽略`Requires-Python`信息。
- `--no-build-isolation` 在构建现代源代码发行版时禁用隔离。如果使用此选项，则必须已经安装PEP 518指定的构建依赖项。
- `--use-pep517` 使用PEP 517构建源代码发行版(使用`--no-use-pep517`强制执行遗留行为)。
- `--install-option <options>` 要提供给`setup.py`安装命令的额外参数(使用类似`--install-option= "--install-scripts=/usr/local/bin"`)。使用多个`--install-option`选项将多个选项传递给`setup.py install`。如果您使用带有目录路径的选项，请确保使用绝对路径。
- `--global-option <options>` 在`install`或`bdist_wheel`命令之前提供给`setup.py`调用的额外全局选项。
- `--compile` 将python源码编译为二进制码。
- `--no-compile` 不将python源码编译为二进制码。
- `--no-warn-script-location` 在`PATH`之外安装脚本时不显示警告。
- `--no-warn-conflicts` 依赖项冲突时不显示警告。
- `--no-binary <format_control>` 不要使用二进制包。可以提供多次，每次都添加到现有值。接受":all: "以禁用所有二进制包，":none: "以清空集合(注意冒号)，或者一个或多个包名之间有逗号(没有冒号)。注意，有些包很难编译，在使用此选项时可能无法安装。
- `--only-binary <format_control>` 不要使用源包。可以提供多次，每次都添加到现有值。接受":all: "以禁用所有源包，":none: "以清空集合，或者一个或多个包名之间用逗号隔开。当在没有二进制发行版的包上使用此选项时，它们将无法安装。
- `--prefer-binary` 喜欢较旧的二进制包而不是较新的源包。
- `--require-hashes` 对于可重复安装，需要一个哈希来检查每个需求。当需求文件中的任何一个包具有`--hash`选项时，就会隐含此选项。
- `--progress-bar <progress_bar>` 指定要显示的进度条类型`off|on|ascii|pretty|emoji`(默认为开启)。
- `--no-clean` 不要清理构建目录。
- `-i, --index-url <url>` Python包索引的基础URL(默认为https://pypi.org/simple)。这应该指向符合PEP 503(简单存储库API)的存储库，或者以相同格式布局的本地目录。
- `--extra-index-url <url>` 除了`--index-url`之外要使用的包索引的额外url。应该遵循与`--index-url`相同的规则。
- `--no-index` 忽略包索引(只查看`--find-links url`)。
- `-f, --find-links <url>` 如果是一个html文件的URL或路径，那么解析指向归档文件的链接，比如sdist (.tar.gz)或wheel (.whl)文件。如果本地路径或文件:// URL是一个目录，那么在目录列表中查找存档。不支持到VCS项目url的链接。

2. 示例
```bash
#1、使用需求说明符从PyPI安装包和它的依赖项。
python -m pip install SomePackage            # latest version
python -m pip install SomePackage==1.0.4     # specific version
python -m pip install 'SomePackage>=1.0.4'   # minimum version

#2、安装需求文件指定的一系列需求项。
python -m pip install -r requirements.txt

#3、从PyPI将一个已经安装的包升级到最新版本。
python -m pip install --upgrade SomePackage

#4、以编辑模式安装一个本地项目。
python -m pip install -e .                # project in current directory
python -m pip install -e path/to/project  # project in another directory

#5、从VCS安装一个项目。
python -m pip install SomeProject@git+https://git.repo/some_pkg.git@1.3.1

#6、以编辑模式从VCS安装一个项目。
python -m pip install -e git+https://git.repo/some_pkg.git#egg=SomePackage          # from git
python -m pip install -e hg+https://hg.repo/some_pkg.git#egg=SomePackage            # from mercurial
python -m pip install -e svn+svn://svn.repo/some_pkg/trunk/#egg=SomePackage         # from svn
python -m pip install -e git+https://git.repo/some_pkg.git@feature#egg=SomePackage  # from 'feature' branch
python -m pip install -e "git+https://git.repo/some_repo.git#egg=subdir&subdirectory=subdir_path" # install a python package from a repo subdirectory

#7、安装一个拥有setuptools额外项的包。
python -m pip install SomePackage[PDF]
python -m pip install "SomePackage[PDF] @ git+https://git.repo/SomePackage@main#subdirectory=subdir_path"
python -m pip install .[PDF]  # project in current directory
python -m pip install SomePackage[PDF]==3.0
python -m pip install SomePackage[PDF,EPUB]  # multiple extras

#8、安装一个特定的源文件包。
python -m pip install ./downloads/SomePackage-1.0.4.tar.gz
python -m pip install http://my.package.repo/SomePackage-1.0.4.zip

#9、安装一个特定的源文件包，遵循PEP440。
python -m pip install SomeProject@http://my.package.repo/SomeProject-1.2.3-py33-none-any.whl
python -m pip install "SomeProject @ http://my.package.repo/SomeProject-1.2.3-py33-none-any.whl"
python -m pip install SomeProject@http://my.package.repo/1.2.3.tar.gz

#10、从替代的包仓库安装。
#1）从不是PyPI的另一个包索引安装。
python -m pip install --index-url http://my.package.repo/simple/ SomePackage
#2）从包含归档文件的本地目录安装(不要扫描索引)。
python -m pip install --no-index --find-links=file:///local/dir/ SomePackage
python -m pip install --no-index --find-links=/local/dir/ SomePackage
python -m pip install --no-index --find-links=relative/dir/ SomePackage
#3）安装期间除了搜索PyPI，还搜索另外的索引。
python -m pip install --extra-index-url http://my.package.repo/simple SomePackage

#11、除了稳定版本之外，也搜索预发布版本和开发版本。默认情况下，pip只找到稳定版本。
python -m pip install --pre SomePackage

#12、从源文件安装包。
#1）不使用任何二进制包。
python -m pip install SomePackage1 SomePackage2 --no-binary :all:
#2）规定`SomePackage1`从源文件安装。
python -m pip install SomePackage1 SomePackage2 --no-binary SomePackage1
```



### pip uninstall
1. 用法

```bash
python -m pip uninstall [options] <package> ...
python -m pip uninstall [options] -r <requirements file> ...
```
- `-r, --requirement <file>` 卸载给定的需求文件中列出的软件包，这个选项可以被使用多次。
- `-y, --yes` 直接卸载，不询问。

2. 示例
```bash
$ python -m pip uninstall simplejson
Uninstalling simplejson:
   /home/me/env/lib/python3.9/site-packages/simplejson
   /home/me/env/lib/python3.9/site-packages/simplejson-2.2.1-py3.9.egg-info
Proceed (y/n)? y
   Successfully uninstalled simplejson
```

### pip list
1. 用法
```bash
python -m pip list [options]
```
- `-o, --outdated` 列出已经过期的包。
- `-u, --uptodate` 列出最新版本的包。
- `-e, --editable` 列出可编辑的项目。
- `-l, --local` 如果在一个具有全局权限的虚拟环境中，不列出安装在全局的包。
- `--user` 只列出安装在用户地址的包。
- `--path <path>` 列出安装在`path`指定的安装位置中的包(可以被使用多次)。
- `--pre` 包含预发布和开发版本。默认情况下，pip只寻找稳定版。
- `--format <list_format>` 从以下几种格式选择输出格式：columns(默认格式)、freeze或json。
- `--not-required` 列出不是安装包依赖项的包。
- `--exclude-editable` 从输出中排除可编辑包。
- `--include-editable` 在输出中包含可编辑包。
- `--exclude <package>` 从输出中排除特定包。
- `-i, --index-url <url>` Python包索引的基础URL(默认为https://pypi.org/simple)。这应该指向符合PEP 503(简单存储库API)的存储库，或者以相同格式布局的本地目录。
- `--extra-index-url <url>` 除了`--index-url`之外要使用的包索引的额外url。应该遵循与`--index-url`相同的规则。
- `--no-index` 忽略包索引(只查看`--find-links url`)。
- `-f, --find-links <url>` 如果是一个html文件的URL或路径，那么解析指向归档文件的链接，比如sdist (.tar.gz)或wheel (.whl)文件。如果本地路径或文件:// URL是一个目录，那么在目录列表中查找存档。不支持到VCS项目url的链接。

2. 示例
```bash
#1、列出安装的包
$ python -m pip list
docutils (0.10)
Jinja2 (2.7.2)
MarkupSafe (0.18)
Pygments (1.6)
Sphinx (1.2.1)

#2、列出过时的包(不包括可编辑的包)和可用的最新版本。
$ python -m pip list --outdated
docutils (Current: 0.10 Latest: 0.11)
Sphinx (Current: 1.2.1 Latest: 1.2.2)

#3、以column格式列出安装的包。
$ python -m pip list --format columns
Package Version
------- -------
docopt  0.6.2
idlex   1.13
jedi    0.9.0

#4、以column格式列出过期包。
$ python -m pip list -o --format columns
Package    Version Latest Type
---------- ------- ------ -----
retry      0.8.1   0.9.1  wheel
setuptools 20.6.7  21.0.0 wheel

#5、列出不是其他包依赖项的包，可以与其他选项联合使用。
$ python -m pip list --outdated --not-required
docutils (Current: 0.10 Latest: 0.11)

#6、使用legacy格式。
$ python -m pip list --format=legacy
colorama (0.3.7)
docopt (0.6.2)
idlex (1.13)
jedi (0.9.0)

#7、使用json格式。
$ python -m pip list --format=json
[{'name': 'colorama', 'version': '0.3.7'}, {'name': 'docopt', 'version': '0.6.2'}, ...

#8、使用freeze格式。
$ python -m pip list --format=freeze
colorama==0.3.7
docopt==0.6.2
idlex==1.13
jedi==0.9.0
```

### pip show
1. 用法
```bash
python -m pip show [options] <package> ...
```
- `-f, --files` 显示每个包已安装文件的完整列表。

2. 示例
```bash
#1、显示一个包的信息。
$ python -m pip show sphinx
Name: Sphinx
Version: 1.4.5
Summary: Python documentation generator
Home-page: http://sphinx-doc.org/
Author: Georg Brandl
Author-email: georg@python.org
License: BSD
Location: /my/env/lib/python2.7/site-packages
Requires: docutils, snowballstemmer, alabaster, Pygments, imagesize, Jinja2, babel, six

#2、显示一个包的所有信息。
$ python -m pip show --verbose sphinx
Name: Sphinx
Version: 1.4.5
Summary: Python documentation generator
Home-page: http://sphinx-doc.org/
Author: Georg Brandl
Author-email: georg@python.org
License: BSD
Location: /my/env/lib/python2.7/site-packages
Requires: docutils, snowballstemmer, alabaster, Pygments, imagesize, Jinja2, babel, six
Metadata-Version: 2.0
Installer:
Classifiers:
   Development Status :: 5 - Production/Stable
   Environment :: Console
   Environment :: Web Environment
   Intended Audience :: Developers
   Intended Audience :: Education
   License :: OSI Approved :: BSD License
   Operating System :: OS Independent
   Programming Language :: Python
   Programming Language :: Python :: 2
   Programming Language :: Python :: 3
   Framework :: Sphinx
   Framework :: Sphinx :: Extension
   Framework :: Sphinx :: Theme
   Topic :: Documentation
   Topic :: Documentation :: Sphinx
   Topic :: Text Processing
   Topic :: Utilities
Entry-points:
   [console_scripts]
   sphinx-apidoc = sphinx.apidoc:main
   sphinx-autogen = sphinx.ext.autosummary.generate:main
   sphinx-build = sphinx:main
   sphinx-quickstart = sphinx.quickstart:main
   [distutils.commands]
   build_sphinx = sphinx.setup_command:BuildDoc
```

### pip freeze
1. 用法
```bash
python -m pip freeze [options]
```
- `-r, --requirement <file>` 在生成输出时，使用给定需求文件及其注释中的顺序。这个选项可以多次使用。
- `-l, --local` 如果在具有全局访问权限的virtualenv中，不要输出全局安装的包。
- `--user` 只输出安装在用户安装位置的包。
- `--path <path>` 只列出安装在`path`中的包。
- `--all` 不要在输出中跳过这些包:setuptools, distribute, pip, wheel。
- `--exclude-editable` 在输出中把可编辑包排除。
- `--exclude <package>` 把特定包从输出中排除。

2. 示例
```bash
#1、产生与需求文件格式匹配的输出信息。
$ python -m pip freeze
docutils==0.11
Jinja2==2.7.2
MarkupSafe==0.19
Pygments==1.6
Sphinx==1.2.2

#2、产生一个需求文件，按照它的要求在另一个环境安装包。
env1/bin/python -m pip freeze > requirements.txt
env2/bin/python -m pip install -r requirements.txt
```

### pip check
0. 作用
- 确认安装的包有兼容的依赖项。

1. 用法
```bash
python -m pip check [options]
```

2. 示例
```bash
#1、所有依赖项是兼容的
$ python -m pip check
No broken requirements found.
$ echo $?
0

#2、有一个包丢失
$ python -m pip check
pyramid 1.5.2 requires WebOb, which is not installed.
$ echo $?
1

#3、有一个包版本错误
$ python -m pip check
pyramid 1.5.2 has requirement WebOb>=1.3.1, but you have WebOb 0.8.
$ echo $?
1
```

### pip download
1. 用法
```bash
python -m pip download [options] <requirement specifier> [package-index-options] ...
python -m pip download [options] -r <requirements file> [package-index-options] ...
python -m pip download [options] <vcs project url> ...
python -m pip download [options] <local project path> ...
python -m pip download [options] <archive url/path> ...
```
- `-c, --constraint <file>` 使用给定的约束文件约束版本。这个选项可以多次使用。
- `-r, --requirement <file>` 从给定的需求文件进行安装。这个选项可以多次使用。
- `--no-deps` 不安装依赖项。
- `--global-option <options>` 在install或bdist_wheel命令之前提供给setup.py调用的额外全局选项。
- `--no-binary <format_control>` 不要使用二进制包。可以提供多次，每次都添加到现有值。接受":all: "以禁用所有二进制包，":none: "以清空集合(注意冒号)，或者一个或多个包名之间有逗号(没有冒号)。注意，有些包很难编译，在使用此选项时可能无法安装。
- `--only-binary <format_control>` 不要使用源包。可以提供多次，每次都添加到现有值。接受":all: "以禁用所有源包，":none: "以清空集合，或者一个或多个包名之间用逗号隔开。当在没有二进制发行版的包上使用此选项时，它们将无法安装。
- `--prefer-binary` 喜欢较旧的二进制包而不是较新的源包。
- `--src <dir>` 将可编辑项目签入的目录。virtualenv中的默认值是" venv path=/src "。全局安装的默认值是" current dir=/src "。
- `--pre` 包括预发布版本和开发版本。默认情况下，pip只找到稳定版本。
- `--require-hashes` 对于可重复安装，需要一个哈希来检查每个需求。当需求文件中的任何包都具有`--hash`选项时，就会隐含此选项。
- `--progress-bar <progress_bar>` 指定要显示的进度类型`off|on|ascii|pretty|emoji`(默认为开启)
- `--no-build-isolation` 在构建现代源代码发行版时禁用隔离。如果使用此选项，则必须已经安装PEP 518指定的构建依赖项。
- `--use-pep517` 同上文。
- `--ignore-requires-python` 同上文。
- `-d, --dest <dir>` 包下载目录。
- `--platform <platform>` 同上文。
- `--python-version <python_version>` 同上文。
- `--implementation <implementation>` 同上文。
- `--abi <abi>` 同上文。
- `--no-clean` 不清除构建目录。
- `-i, --index-url <url>` 同上文。
- `--extra-index-url <url>` 同上文。
- `--no-index` 同上文。
- `-f, --find-links <url>` 同上文。

2. 示例
```bash
#1、下载一个包和它所有依赖项
python -m pip download SomePackage
python -m pip download -d . SomePackage  # equivalent to above
python -m pip download --no-index --find-links=/tmp/wheelhouse -d /tmp/otherwheelhouse SomePackage

#2、OSX系统
python -m pip download \
   --only-binary=:all: \
   --platform macosx-10_10_x86_64 \
   --python-version 27 \
   --implementation cp \
   SomePackage

#3、linux系统
python -m pip download \
   --only-binary=:all: \
   --platform linux_x86_64 \
   --python-version 3 \
   --implementation cp \
   --abi cp34m \
   SomePackage

#4、跨平台
python -m pip download \
   --only-binary=:all: \
   --platform any \
   --python-version 3 \
   --implementation py \
   --abi none \
   SomePackage

### 5、即使过度约束，这仍将正确地取回pip的通用wheel。
$ python -m pip download \
   --only-binary=:all: \
   --platform linux_x86_64 \
   --python-version 33 \
   --implementation cp \
   --abi cp34m \
   pip>=8

#6、下载一个支持几种abi和平台之一的包。当为定义良好的解释器获取hweel时，这是很有用的，解释器支持的abi和平台是已知和固定的。
$ python -m pip download \
   --only-binary=:all: \
   --platform manylinux1_x86_64 --platform linux_x86_64 --platform any \
   --python-version 36 \
   --implementation cp \
   --abi cp36m --abi cp36 --abi abi3 --abi none \
   SomePackage
```

### pip wheel
0. 作用
- 为需求包和依赖项创建wheel文件。

1. 用法
```bash
python -m pip wheel [options] <requirement specifier> ...
python -m pip wheel [options] -r <requirements file> ...
python -m pip wheel [options] [-e] <vcs project url> ...
python -m pip wheel [options] [-e] <local project path> ...
python -m pip wheel [options] <archive url/path> ...
```
- `-w, --wheel-dir <dir>`
生成wheel文件并放入`dir`，默认路径是当前工作目录。
### --no-binary \<format_control>
同上文。

### --only-binary \<format_control>
同上文。

### --prefer-binary
同上文。

### --no-build-isolation
同上文。

### --use-pep517
同上文。

### -c, --constraint \<file>
同上文。

### -e, --editable \<path/url>
同上文。

### -r, --requirement \<file>
同上文。

### --src \<dir>
同上文。

### --ignore-requires-python
同上文。

### --no-deps
同上文。

### --progress-bar <progress_bar>
同上文。

### --no-verify
不验证生成的wheel文件是否有效。

### --build-option \<options>
提供给‘setup.py bdist_wheel’的额外参数。

### --global-option \<options>
在install或bdist_wheel命令之前提供给setup.py调用的额外全局选项。

### --pre
同上文。

### --require-hashes
同上文。

### --no-clean
同上文。

### -i, --index-url \<url>
同上文。

### --extra-index-url \<url>
同上文。

### --no-index
同上文。

### -f, --find-links \<url>
同上文。

## 示例
### 1、为一个需求项创建wheel文件，然后安装。

```bash
python -m pip wheel --wheel-dir=/tmp/wheelhouse SomePackage
python -m pip install --no-index --find-links=/tmp/wheelhouse SomePackage
```

### 2、将一个包从源文件生成wheel文件。

```bash
python -m pip wheel --no-binary SomePackage SomePackage
```

# pip hash
## 用法

```bash
python -m pip hash [options] <file> ...
```

## 描述
计算本地包的哈希值。
这些可以与需求文件中的`--hash`一起使用，以完成可重复的安装。

## 选项
### -a, --algorithm \<algorithm>
使用哪一种哈希算法：sha256、sha384或sha512。

## 示例
### 计算下载文档的哈希值。

```bash
$ python -m pip download SomePackage
Collecting SomePackage
   Downloading SomePackage-2.2.tar.gz
   Saved ./pip_downloads/SomePackage-2.2.tar.gz
Successfully downloaded SomePackage
$ python -m pip hash ./pip_downloads/SomePackage-2.2.tar.gz
./pip_downloads/SomePackage-2.2.tar.gz:
--hash=sha256:93e62e05c7ad3da1a233def6731e8285156701e3419a5fe279017c429ec67ce0
```

# pip search
## 用法

```bash
python -m pip search [options] <query>
```

## 描述
在PyPI中搜索关键字包含`query`的包。

## 选项
### -i, --index <url>
搜索的索引地址，默认是[https://pypi.org/pypi](https://pypi.org/pypi)。

## 示例
### 搜索“peppercorn”

```bash
$ python -m pip search peppercorn
pepperedform    - Helpers for using peppercorn with formprocess.
peppercorn      - A library for converting a token stream into [...]
```

# pip cache
## 用法

```bash
python -m pip cache dir
python -m pip cache info
python -m pip cache list [<pattern>] [--format=[human, abspath]]
python -m pip cache remove <pattern>
python -m pip cache purge
```

## 描述
检查和管理pip的wheel缓存。

### 子命令
#### dir
显示缓存目录。
#### info
显示cache信息。
#### list
列出缓存中存储的包的文件名。
#### remove
从缓存中删除一个或多个包。
#### purge
从缓存中删除所有项目。
#### \<pattern>
可以是一个全局表达式或包名。

## 选项
## --format \<list_format>
选择输出格式：human(default)或abspath。

# pip config
## 用法

```bash
python -m pip config [<file-option>] list
python -m pip config [<file-option>] [--editor <editor-path>] edit

python -m pip config [<file-option>] get name
python -m pip config [<file-option>] set name value
python -m pip config [<file-option>] unset name
python -m pip config [<file-option>] debug
```

## 描述
管理本地和全局配置。
### 子命令
#### list
列出生效的配置(或从指定的文件)
#### edit
在编辑器中编辑配置文件
#### get
获取与name关联的值
#### set
设置name=value
#### unset
取消设置name相关的值
#### debug
列出配置文件及其下定义的值

如果没有传递`--user`、`--global`和`--site`，则使用虚拟环境配置文件(如果虚拟环境配置文件是有效的且文件存在)。否则，默认情况下，所有修改都发生在用户文件上。

## 选项
### --editor \<editor>
用于编辑文件的编辑器。如果没有提供，则使用VISUAL或EDITOR环境变量指定的值。
### --global
仅使用系统范围配置文件。
### --user
仅使用用户配置文件。
### --site
仅使用当前环境配置文件。

# pip debug
## 用法

```bash
python -m pip debug <options>
```

## 描述
显示调试信息。

## 选项
### --platform \<platform>
同上文。

### --python-version \<python_version>
同上文。

### --implementation \<implementation>
同上文。

### --abi \<abi>
同上文。