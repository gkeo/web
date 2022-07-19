# semanage 命令，Linux semanage 命令详解：默认目录的安全上下文查询与修改 - Linux 命令搜索引擎
[纠正错误](https://github.com/jaywcjlove/linux-command/edit/master/command/semanage.md) [添加实例](https://github.com/jaywcjlove/linux-command/edit/master/command/semanage.md)

默认目录的安全上下文查询与修改

## 补充说明

**semanage 命令** 是用来查询与修改 SELinux 默认目录的安全上下文。SELinux 的策略与规则管理相关命令：seinfo 命令、sesearch 命令、getsebool 命令、setsebool 命令、semanage 命令。

### 语法

    semanage {login|user|port|interface|fcontext|translation} -l semanage fcontext -{a|d|m} [-frst] file_spec 

### 选项

    -l：查询。 fcontext：主要用在安全上下文方面。 -a：增加，你可以增加一些目录的默认安全上下文类型设置。 -m：修改。 -d：删除。 

### 实例

查询一下`/var/www/html`的默认安全性本文的设置：

    semanage fcontext -l SELinux fcontext    type          Context ....(前面省略).... /var/www(/.*)?      all files     system_u:object_r:httpd_sys_content_t:s0 ....(後面省略).... 

如上面例子所示，我们可以查询的到每个目录的安全性本文！而目录的设定可以使用正则表达式去指定一个范围。那么如果我们想要增加某些自定义目录的安全性本文呢？举例来说，我想要色设置`/srv/samba`成为 `public_content_t`的类型时，应该如何设置呢？

用 semanage 命令设置`/srv/samba`目录的默认安全性本文为`public_content_t`：

    mkdir /srv/samba ll -Zd /srv/samba drwxr-xr-x  root root root:object_r:var_t    /srv/samba 

如上所示，默认的情况应该是`var_t`这个咚咚的！

    semanage fcontext -l | grep '/srv' /srv/.*                     all files   system_u:object_r:var_t:s0 /srv/([^/]*/)?ftp(/.*)?     all files   system_u:object_r:public_content_t:s0 /srv/([^/]*/)?www(/.*)?     all files   system_u:object_r:httpd_sys_content_t:s0 /srv/([^/]*/)?rsync(/.*)?   all files   system_u:object_r:public_content_t:s0 /srv/gallery2(/.*)?         all files   system_u:object_r:httpd_sys_content_t:s0 /srv                        directory   system_u:object_r:var_t:s0   //看这里！ 

上面则是默认的`/srv`底下的安全性本文资料，不过，并没有指定到`/srv/samba`。

    semanage fcontext -a -t public_content_t "/srv/samba(/.*)?" semanage fcontext -l | grep '/srv/samba' /srv/samba(/.*)?            all files   system_u:object_r:public_content_t:s0 

    cat /etc/selinux/targeted/contexts/files/file_contexts.local     /srv/samba(/.*)?    system_u:object_r:public_content_t:s0 

    restorecon -Rv /srv/samba* ll -Zd /srv/samba drwxr-xr-x  root root system_u:object_r:public_content_t /srv/samba/ 

semanage 命令的功能很多，这里主要用到的仅有 fcontext 这个选项的用法而已。如上所示，你可以使用 semanage 来查询所有的目录默认值，也能够使用它来增加默认值的设置！ 
 [https://wangchujiang.com/linux-command/c/semanage.html](https://wangchujiang.com/linux-command/c/semanage.html)
