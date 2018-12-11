# 一、下载个anaconda：官网：https://www.anaconda.com/download/#linux、 
## 进入该软件所在的目录，输入命令：

    sh Anaconda3-4.2.0-Linux-x86_64.sh 
    
# 二、根据python版本 下载合适的opencv whl安装包，官网地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv
## 使用pip命令安装pip   

    pip install opencv_python-3.4.3.18-cp27-cp27mu-manylinux1_x86_64.whl
    
## 报错  

    ** is not a supported wheel on this platform
    
## 进入python 编辑框

    import pip
    
    print(pip.pep425tags.get_supported())
    
## 即可查看python 支持的版本
# 三、安装成功
## import cv2 报错：

    ImportError: libSM.so.6: cannot open shared object file: No such file or directory
    
## 执行yum install libSM 可以解决
## 验证
    >>> import cv2
    
    >>> cv2.__version__
    
    >>> '3.4.3'
    
## 完成
