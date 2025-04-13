# docker安装的流程

本虚拟机为centos 7虚拟机，无法通过官方安装脚本自动安装。

官方脚本指令

```shell
$ curl -fsSL https://get.docker.com -o install-docker.sh
$ sudo sh install-docker.sh
```

## 手动安装

1. 卸载旧版本

   来自网络建议的卸载指令

   ```shell
   sudo dnf remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

   

2. 安装依赖

   ```shell
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

   

3. 添加 Docker 官方源

   ```shell
   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   ```

   

4. 安装Docker

   ```
   sudo yum install -y docker-ce docker-ce-cli containerd.io
   ```

   

5. 启动Docker

   ```
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

   

关于检查docker是否启动成功

可以使用

```
docker --version
```

```
docker run hello-world
```







## 过程中的问题

#### yum下载失败

> 已加载插件：fastestmirror, langpacks
> Determining fastest mirrors
>
> Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=stock error was
> 14: curl#6 - "Could not resolve host: mirrorlist.centos.org; 未知的错误" 

```
备份原有repo：
cp -r /etc/yum.repos.d /etc/yum.repos.d.bak

下载阿里源：
sudo curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

替换CentOS源（阿里）：
sudo tee /etc/yum.repos.d/docker-ce.repo <<EOF
[docker-ce-stable]
name=Docker CE Stable - Aliyun
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/x86_64/stable/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
EOF


重启：
sudo yum clean all
sudo yum makecache
sudo yum update -y
```

### docker无法拉取镜像

#### 检查 Docker 运行状态

```
systemctl status docker
```

#### 切换国内源

编辑 `/etc/docker/daemon.json`

```
mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": ["https://registry.docker-cn.com", "https://hub-mirror.c.163.com"]
}
EOF
```

手动版本

```
vim /etc/docker/daemon.json
添加
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

或者

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://do.nark.eu.org",
        "https://dc.j8.work",
        "https://docker.m.daocloud.io",
        "https://dockerproxy.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://docker.nju.edu.cn"
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

然后输入 `docker info` 进行检查。



另：DNS配置检查指令 `cat /etc/resolv.conf`
