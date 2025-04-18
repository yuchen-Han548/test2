
# 安装指南

## 在python中使用astra库

**系统要求**：Windows/Linux系统，使用64位Python 3.9-3.13，支持conda。

在指定的conda环境中执行：

```bash
conda install astra-toolbox -c astra-toolbox -c nvidia
```

也可以使用pip安装Python版


**系统要求**：配置64位Python 3.9-3.13环境。

```bash
pip install astra-toolbox
```
```{warning}
由于astra库可能和部分常用第三方库产生冲突，建议[新建虚拟环境](https://docs.python.org/zh-cn/3.13/library/venv.html)进行astra库的使用。
```

## 在matlab中使用astra库
**系统要求**：CUDA 11+、MATLAB R2012a+
### Windows系统
在windows系统内配置astra较为简单。只需从astra包[下载页面](https://astra-toolbox.com/downloads/index.html)选择ASTRA v2.3.0 for Windows 64 bit, CUDA/MATLAB: astra-toolbox-2.3.0-matlab-win-x64.zip下载，解压缩后运行vc_redist.x64.exe安装g++支持，之后把文件夹添加到matlab路径就可以正常调用。
### Linux系统
**系统要求**：g++编译器、CUDA 11+、MATLAB R2012a+
下载ASTRA v2.3.0 Source Code (.zip): astra-toolbox-2.3.0.zip源代码版本。
在解压后文件夹位置，在终端中依次执行

```bash
cd build/linux
./autogen.sh   # 适用于git版本构建
./configure --with-cuda=/usr/local/cuda \
            --with-matlab=/usr/local/MATLAB/R2012a \
            --prefix=$HOME/astra \
            --with-install-type=module
make
make install
```
这会编译Linux系统可用的mex文件。

```{warning}
各MATLAB版本仅支持特定g++编译器版本。若因新版g++报错缺失GLIBCXX_3.4.xx符号，可通过删除MATLAB自带libstdc++（位于``MATLAB路径/bin/glnx[a64|x86]``）或设置``LD_PRELOAD=/usr/lib64/libstdc++.so.6``（在终端启动MATLAB时）临时规避。
```



