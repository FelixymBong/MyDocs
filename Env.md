# 环境初始化步骤

1. 时间调整

```
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```

2. 查看显卡相关设置

```
nvidia-smi
```

3. 修改 apt 源（备份并更新）

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo nano /etc/apt/sources.list
sudo apt update
```

4. 安装常用工具

```
sudo apt install terminator
sudo apt install ssh
```

5. Git 与 SSH 配置

```
sudo apt-get install git
git config --global user.name "FelixymBong"
git config --global user.email "3230100841@zju.edu.cn"
ssh-keygen -t rsa -C "3230100841@zju.edu.cn"
# 私钥：/home/bong/.ssh/id_rsa
# 公钥：/home/bong/.ssh/id_rsa.pub
```

步骤说明：打开公钥文件（例如使用 `gedit /home/bong/.ssh/id_rsa.pub`），全选并复制公钥内容，登录 GitHub → Settings → SSH and GPG keys → New SSH key，粘贴并保存。

验证 GitHub SSH 连接：

```
ssh -T git@github.com
```

本地仓库关联远程库：

```
git remote add origin git@github.com:FelixymBong/MyDocs.git
```

push报错密钥什么的问题：可能是SSH 代理（ssh-agent）没有正确加载私钥，或者私钥的权限设置不对，解决方法：

1.启动代理：
```
eval "$(ssh-agent -s)"
```

2.添加私钥：
```
ssh-add ~/.ssh/id_rsa
```

查看隐藏文件/文件夹：

```
ls -a
```

6. 查看内存

```
free -h
```

7. 检查磁盘空间

```
df -h /
```

8. 检查 Ubuntu 版本与内核

```
lsb_release -a
uname -r
```

9. 安装下载的 .deb 包（示例）

```
sudo apt install ./clash-verge_1.7.7_amd64.deb
```

10. 解决 Terminator 未设为默认终端的问题

```
which terminator
sudo update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator /usr/bin/terminator 50
sudo update-alternatives --config x-terminal-emulator
```

11. VSCode配置

下载以下的Extensions:



12. Miniconda配置

```
sudo apt update
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
source ~/.bashrc
```
配置清华源：
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --set show_channel_urls yes
```
取消默认自动激活 base 环境:
```
conda config --set auto_activate_base false
```
conda常用命令：
```
conda create -n ENV_NAME python=3.9
conda activate ENV_NAME
conda install PACKAGE_NAME  
conda remove PACKAGE_NAME
conda list
conda deactivate
conda env remove -n ENV_NAME
conda env list
```

