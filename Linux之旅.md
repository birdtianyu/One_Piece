# Linux之旅



**`#`**  + 命令：代表root权限命令
**`$` ** + 命令：代表普通用户命令



【/】代表根目录

【~】代表用户的家目录

> 如果以root账号登陆，~代表/root/
> 如果以username登陆，~代表/home/username/



### Yum软件包管理器

---

>Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理（原Red-Hat Package Manager），能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

```bash
# 安装nginx软件包
yum install nginx
#  搜索软件包
yum search nginx

# 显示指定安装包安装软件详情
yum list nginx
# 显示所有已安装以及可以安装的软件包
yum list
# 显示所有软件包
yum list all

# 移除软件包
yum remove nginx
# 移除软件包
yum earse nginx

# 升级软件包
yum update nginx
# 检查可以更新的软件
yum check-update
# 显示安装包信息
yum info nginx

# 列出软件包提供哪些文件
yum provides
# 列出哪些软件包提供nginx
yum provides */nginx

# 查看nginx依赖
yum deplist nginx

# 查看软件仓库
yum repolist

# 清除缓存目录下的软件包
yum clear nginx
# 清除缓存目录下的header
yum clean header
# 清除所有的缓存
yum clean all
```