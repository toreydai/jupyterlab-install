# JupyterLab Install on Amazon EC2
## 启动EC2

* AMI选择Deep Learning AMI (Amazon Linux 2) Version 30.1
* 实例类型选择GPU实例，这个后期也可以修改
* 实例绑定弹性IP，以便每次使用同一IP访问
* 存储至少需要100GiB
* 安全组入站规则需要放开8888端口

## 安装配置JupyterLab
### ssh登录EC2
### 升级pip
sudo python -m pip install --upgrade pip
### 下载安装jupyterlab
pip install jupyterlab
### 修改Jupyter密码
jupyter notebook password
### 配置JupyterLab
修改文件~/.jupyter/jupyter_notebook_config.py

```
## 服务的端口，用默认的8888即可
c.NotebookApp.port = 8888
 
## 是否需要自动弹出浏览器，服务器端一般不需要
c.NotebookApp.open_browser = False
 
## The directory to use for notebooks and kernels.
c.NotebookApp.notebook_dir = '/home/ec2-user'

##  client ip 限制 
c.NotebookApp.ip='*'
```
### 启动JupyterLab
jupyter-lab  &
### 登录web
浏览器访问http://ip:8888/lab
## 验证GPU是否可用
新建Notebook，Kernel选择conda_tensorflow_p36，运行以下代码：

```
import tensorflow as tf
 
gpu_device_name = tf.test.gpu_device_name()
print("gpu_device_name:"+gpu_device_name)
print("tf.test.is_gpu_available():"+str(tf.test.is_gpu_available()))
```
如果支持GPU，输出为：

```
gpu_device_name:/device:GPU:0
tf.test.is_gpu_available():True
```
