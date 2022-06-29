# WordPress网站与Google Blogger数据互相导入_M@ple的博客-CSDN博客
### **WordPress 网站数据导入到 Google Blogger**

可以将博文和评论从[WordPress](https://so.csdn.net/so/search?q=WordPress&spm=1001.2101.3001.7020)网站导入到 Google Blogger。具体操作步骤如下：

1、首先从 WordPress 站点导出[XML](https://so.csdn.net/so/search?q=XML&spm=1001.2101.3001.7020)文件（**Tools** → **Export** → **Download Export File**）。  
2、然后登录到 Google Blogger。  
3、选择您要导入博文和评论的目标博客。  
4、在左侧的菜单中，点击**设置** → **其他**。  
5、在 “**导入和备份**” 部分，点击**导入内容**。  
6、从计算机上选择 **.xml** 文件。  
7、点击**发布**。  
注意：导入的博客没有文件大小限制，但 Google Blogger 会限制一天内导入的博客数量。如果文章数量较多，可以使用[WXR File Splitter](http://rangerpretzel.com/content/view/20/1/)将 XML 文件分成数个小文件分批导入。

或者使用在线工具（[https://wordpress-to-blogger-converter.appspot.com](https://wordpress-to-blogger-converter.appspot.com/)）将 Wordpress 的 xml 转换为 blogger 格式，编辑导出的文件，把附件链接修改下。

提供免费 Google Blogger 模板（Templates）的网站：[https://btemplates.com](https://btemplates.com/)

Google Blogger 官网说明：[https://support.google.com/blogger/answer/41387](https://support.google.com/blogger/answer/41387)

### **Google Blogger 数据导入到 WordPress 网站**

可以将 posts、comments 和 users 从 Google Blogger 导入到 WordPress 网站。具体操作步骤如下：

1、首先从 Google Blogger 导出 XML 文件（**设置** → **其他** → **导入和备份** → **备份内容** → **保存到计算机**）。  
2、然后以管理员登录 WordPress 站点。  
3、从 Dashboard 界面进入**Tools** → **Import**。  
4、点击”**Blogger**“下方的”**Install Now**“安装导入工具。  
5、点击”**Run Importer**“链接。  
6、点击”**Browse**“按钮，选择从 Google Blogger 导出的 XML 文件， 点击 “**Upload file and import**“。

WordPress 官网说明：[https://codex.wordpress.org/Importing_Content](https://codex.wordpress.org/Importing_Content) 
 [https://blog.csdn.net/qq_34673086/article/details/103148855](https://blog.csdn.net/qq_34673086/article/details/103148855)
