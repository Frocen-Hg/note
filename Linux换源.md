备份yum源文件

```shell
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```



常用的国内源

```shell
阿里云源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

清华大学源
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.tuna.tsinghua.edu.cn/repo/Centos-7.repo

网易源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo

中科大源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.ustc.edu.cn/centos/7/os/x86_64/
```

清理 YUM 缓存并更新

```shell
yum clean all
yum makecache
```

验证新源

```shell
yum repolist
```

