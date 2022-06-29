# Windows系统目录迁移:Users,Program Files,ProgramData_Hansel的博客-CSDN博客_programdata 迁移
## Windows7 系统目录迁移: Users,Program Files,ProgramData

微软设计了比如：我的文档、我的 OOXX，之类的东西，在 WIN7 下面更连游戏、下载等等目录都设计好了，我也很乖巧的把各种文件都分门别类的放进去了。

同时也很厉害的设计在了 “%HOMEDRIVE%” 里面，各种的资料，这种软件的配置就全给放进去了

在 XP 的时候对于 C 盘的要求还不是那么大，但是在 win7 下就显的捉襟见肘了

再格式化，重装系统就全没了，囧，就也是必须移动出来的第二个理由

以前只是自己挪挪地儿，小改一下注册表，今天无意中参考了几篇文章，那搞的才是个全面啊，在膜拜只后就全给做笔记了，忽忽，先上牛人原文的[传送门](http://hi.baidu.com/umu618)

首先，不管你要怎么挪，请记住挪坏了我不会负责。其次，确定系统是刚刚安装好的，这样比较不会出现意外，也更有效优化，确定是用 Administrator 登录。

## 第一步，复制 Program Files 目录

不能直接用[资源管理器](https://so.csdn.net/so/search?q=%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E5%99%A8&spm=1001.2101.3001.7020)复制，我们需要保留此目录的所有权限设置，（以前我就是直接在资源管理器面弄到，现在严重怀疑，我的机器时不时的抽风是不是和这个有关）假设要从 C 盘移动到 D 盘：

xcopy "C:\\Program Files" "D:\\Program Files\\" /E /H /K /X /Y/C

## 第二步，修改注册表

Windows Registry Editor Version 5.00

\[ HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion ]  
"ProgramFilesDir" \\= "D:\\\\ProgramFiles"  
"CommonFilesDir" \\= "D:\\\\ProgramFiles\\\\Common Files"

## 第三步，重启

注意不是注销，因为 Program Files 里有很多文件是被系统服务使用的，注销并不能重启服务。

## 第四步，关闭 iphlpsvc 服务

因为它使用到了 “C:\\Program Files\\Internet Explorer\\sqmapi.dll”：

## 第五步，删除 “C:\\Program Files” 目录

要先取得所有权，添加改写权限后才可以删除。

## 第六步，创建 Junction 文件夹映射

把 “C:\\Program Files” 指向 “D:\\Program Files”，这是为了防止一些硬编码的 SB 程序不由分说地往 “C:\\Program Files” 里写东西。

mklink /J "C:\\Program Files" "D:\\Program Files"

至此，Program Files 目录的转移就结束了，与还不太放心的话，可以去注册表找找 “C:\\ProgramFiles” 这个字串符，手动改改了，呵呵，改坏了就是你人品问题了。

## 第一步，复制 ProgramData 目录

假设要从 C 盘移动到 D 盘：

xcopy C:\\ProgramData D:\\ProgramData\\ /E /H /K /X /Y /B /C

## 第二步，修改注册表

：

\[HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\WindowsNT\\CurrentVersion\\ProfileList] 下的 ProgramData 数据原为 %SystemDrive%\\ProgramData，改为 D:\\ProgramData。

\[HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\ShellFolders] 下的 “Common Administrative Tools”、”Common AppData”、”CommonPrograms”、”Common Startup”、”OEM Links”、”Common Templates” 等值的数据也相应地改改。如下：

Windows Registry Editor Version 5.00

\[ HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\ShellFolders ]  
"Common Start Menu" \\= "D:\\\\ProgramData\\\\Microsoft\\\\Windows\\\\StartMenu"  
"Common Programs" \\= "D:\\\\ProgramData\\\\Microsoft\\\\Windows\\\\StartMenu\\\\Programs"  
"Common Administrative Tools" \\= "D:\\\\ProgramData\\\\Microsoft\\\\Windows\\\\StartMenu\\\\Programs\\\\Administrative Tools"  
"Common Startup" \\= "D:\\\\ProgramData\\\\Microsoft\\\\Windows\\\\StartMenu\\\\Programs\\\\Startup"  
"OEM Links" \\= "D:\\\\ProgramData\\\\OEMLinks"  
"Common Templates" \\= "D:\\\\ProgramData\\\\Microsoft\\\\Windows\\\\Templates"  
"Common AppData" \\= "D:\\\\ProgramData"

## 第三步，重启

注销的话需要多加 net stop iphlpsvc 和 net stop BITS。

## 第四步，删除 C:\\ProgramData 目录

其中有两个无法直接删除的文件要先取得所有权，添加改写权限后才可以删除。

## 第五步，创建 Junction 文件夹映射

把 C:\\ProgramData 指向 D:\\ProgramData，这是为了防止一些硬编码的程序不由分说地往 “C:\\ProgramData” 里写东西。

mklink /J C:\\ProgramData D:\\ProgramData

## 第一步，修改注册表

和前面不同，因为 Users 目录下有一些系统占用的文件，复制不了。如果你和我一样有双系统或者用 U 盘启动 WinPE，那可以试试，不必按照这里写出的步骤做。但如果你没有相应的设备的话，那就继续 SBS 吧，先修改注册表，再复制文件。假设要移动到 E 盘：

\[HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\WindowsNT\\CurrentVersion\\ProfileList] 下的 Default、ProfilesDirectory、Public 三个值的数据改一下，把盘符都改为 E:。

\[HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\WindowsNT\\CurrentVersion\\ProfileList\\S-1-5-21-3843801140-3458922274-3296897442-500]下的 ProfileImagePath 数据改为 E:\\Users\\Administrator。

\[HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\ShellFolders] 下的 “Common Desktop”、”CommonDocuments”、CommonMusic、CommonPictures、CommonVideo 五个值的数据把盘符都改为 E:。

\[HKEY_CURRENT_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\ShellFolders] 下的值看到数据中有 C:\\Users 的都改为 E:\\Users。

## 第二步，注销，重新登录

会发现一些用户配置没正确加载的问题，但不用理会。

## 第三步，复制文件

xcopy C:\\Users E:\\Users\\ /E /H /K /X /Y /B /C

## 第四步，注销，重新登录

在第二步看到的问题，解决了。

## 第五步，删除 “E:\\Users\\Default User” 目录

因为这个目录被 xcopy 复制错误，重新创建：

mklink /J "E:\\Users\\Default User" E:\\Users\\Default

然后对照 “C:\\Users\\Default User” 的权限设置，把 “E:\\Users\\Default User” 设置成和它一样：

cacls "E:\\Users\\Default User"/S:"D:PAI (D;;CC;;;WD)(A;;0x1200a9;;;WD )(A;;FA;;;SY )(A;;FA;;;BA )"

## 第六步，删除 C:\\Users 目录

直接用资源管理器删除，很顺利。

## 第七步，创建映射

mklink /J C:\\Users E:\\User

好吧，我承认前面的你都移动了，其实系统还是会添加几个 G 文件区 C 盘，我们前面只是移动了应用软件的默认安装位置，和一些个人数据

这里要说的是，对于像 %SystemRoot%\\Installer、%SystemRoot%\\SoftwareDistribution 这类 “顽固” 的文件夹，不能通过修改注册表来定义路径的设置！

我们要先准备一个工具 Junction[传送门](http://download.sysinternals.com/Files/Junction.zip)

这两个文件夹比较 “常用”，MSI 安装程序会把安装文件缓存到 %SystemRoot%\\Installer，比如您装了 VS，会发现这个文件夹大了很多；而 %SystemRoot%\\SoftwareDistribution 是自动更新服务用来缓存更新程序的。我的这两个文件夹加起来就有 2GB 多，惆怅

　　假设要把 %SystemRoot%\\Installer 修改为 E:\\SysDir\\Installer，

首先通过资源管理器把 C:\\WINDOWS\\Installer 文件夹剪切到 E:\\SysDir\\ 下（这个文件夹是隐藏的）

然后在命令提示符下输入：

junction C:\\WINDOWS\\Installer E:\\SysDir\\Installer

对于 SoftwareDistribution 要多一步，要先停止自动更新服务：

后面步骤和 Installer 的一样，剪切 -> Junction：

junction C:\\WINDOWS\\SoftwareDistributionE:\\SysDir\\SoftwareDistribution

　　这样做完之后 C:\\WINDOWS\\ 下的 Installer 和 SoftwareDistribution 其实只是文件夹的映射，对他们的写入操作全部都会映射到 D:\\SysDir\\ 下的对应文件夹。本质上就是把 E 盘的空间拿到 C 盘使用，减少对 C 盘的写入。 
 [https://blog.csdn.net/hansel/article/details/50813797](https://blog.csdn.net/hansel/article/details/50813797)
