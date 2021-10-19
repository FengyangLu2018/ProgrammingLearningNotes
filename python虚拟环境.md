# virtualenv
## 安装
`pip install virtualenv`
## 创建虚拟环境
`virtualenv [-p <intepreterPath>] <envName>`
- `-p <intepreterPath>`指定创建的环境使用的python解释器，例如`/usr/bin/python2.7`
## 启用虚拟环境
`source <envPath>/bin/activate`
## 退出虚拟环境
`deactivate`
## 删除虚拟环境
`rm -rf <envPath>`
## 其他命令
`virtualenv [options] [<destPath>]` 
- `–version` 显示当前版本号。 
- `-h, –help` 显示帮助信息。 
- `-v, –verbose` 显示详细信息。 
- `-q, –quiet` 不显示详细信息。 
- `-p PYTHON_EXE, –python=PYTHON_EXE` 指定所用的python解析器的版本，比如`–python=python2.5`就使用2.5版本的解析器创建新的隔离环境。默认使用的是当前系统安装(/usr/bin/python)的python解析器。
- `–clear` 清空非root用户的安装，并重头开始创建隔离环境。 
- `–no-site-packages` 令隔离环境不能访问系统全局的site-packages目录。 
- `–system-site-packages` 令隔离环境可以访问系统全局的site-packages目录。 
- `–unzip-setuptools` 安装时解压Setuptools或Distribute 
- `–relocatable` 重定位某个已存在的隔离环境。使用该选项将修正脚本并令所有.pth文件使用相当路径。 
- `–distribute` 使用Distribute代替Setuptools，也可设置环境变量VIRTUALENV_DISTRIBUTE达到同样效要。 
- `–extra-search-dir=SEARCH_DIRS` 用于查找setuptools/distribute/pip发布包的目录。可以添加任意数量的–extra-search-dir路径。 
- `–never-download` 禁止从网上下载任何数据。此时，如果在本地搜索发布包失败，virtualenv就会报错。 
- `–prompt==PROMPT` 定义隔离环境的命令行前缀。

# virtualenvwrapper
- virtualenv的扩展包，能方便的管理virtualenv
## 安装
- 环境变量`WORKON_HOME`指定虚拟环境位置
```bash
(Linux)
pip install virtualenvwrapper
export WORKON_HOME=~/Envs #指定创建的环境存放的目录
source /usr/local/bin/virtualenvwrapper.sh
source ~/.bashrc　　　　#读入配置文件，立即生效
(Windows)
pip install virtualenvwrapper-win
WORKON_HOME 默认值是 %USERPROFILE%Envs
```
## 创建虚拟环境
`mkvirtualenv <envName>`
## 切换到虚拟环境
`workon <envName>`
## 退出虚拟环境
`deactivate`
## 删除虚拟环境
`rmvirtualenv myenv`
## 其它用法
- `lsvirtualenv`或`workon` 列举所有的环境。
- `cdvirtualenv [subdir]` 导航到当前激活的虚拟环境的目录中
- `cdsitepackages [subdir]` 和上面的类似，但是是直接进入到site-packages目录中
- `lssitepackages` 显示site-packages目录中的内容
- `showvirtualenv [env]` 显示指定环境的详情
- `cpvirtualenv [source] [dest]` 复制一份虚拟环境
- `allvirtualenv` 对当前虚拟环境执行统一的命令
- `add2virtualenv [dir] [dir]` 把指定的目录加入当前使用的环境的path中，这常使用于在多个project里面同时使用一个较大的库的情况
- `toggleglobalsitepackages` 控制当前的环境是否使用全局的sitepackages目录