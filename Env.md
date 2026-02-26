# 环境初始化步骤

## 1. 时间调整

```
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```

## 2. 查看显卡相关设置

```
nvidia-smi
```

## 3. 修改 apt 源（备份并更新）

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo nano /etc/apt/sources.list
sudo apt update
```

## 4. 安装常用工具

```
sudo apt install terminator
sudo apt install ssh
```

## 5. Git 与 SSH 配置

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

## 6.查看隐藏文件/文件夹：

```
ls -a
```

## 7. 查看内存

```
free -h
```

## 8. 检查磁盘空间

```
df -h /
```

## 9. 检查 Ubuntu 版本与内核

```
lsb_release -a
uname -r
```

## 10. 安装下载的 .deb 包（示例）

```
sudo apt install ./clash-verge_1.7.7_amd64.deb
```

## 11. 解决 Terminator 未设为默认终端的问题

```
which terminator
sudo update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator /usr/bin/terminator 50
sudo update-alternatives --config x-terminal-emulator
```

## 12. VSCode配置

下载以下的Extensions:



## 13. Miniconda配置

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
## 14. Docker配置

根据
https://docs.docker.com/engine/install/ubuntu/

```
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```
```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
尝试pull镜像，报错:
```
Error response from daemon: failed to resolve reference "docker.io/library/busybox:latest": failed to do request: Head "https://registry-1.docker.io/v2/library/busybox/manifests/latest": dial tcp 104.244.46.244:443: i/o timeout
```
原因:docker服务器被墙了；解决方案：配置docker代理：
```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo nano /etc/systemd/system/docker.service.d/http-proxy.conf
```
写入以下内容（端口号看你的代理软件）：
```
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```
重载配置并重启 Docker：
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```
检查配置是否生效：
```
sudo docker info | grep Proxy
```
再次pull后成功。

