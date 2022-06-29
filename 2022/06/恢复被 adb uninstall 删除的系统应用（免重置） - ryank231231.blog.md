# 恢复被 adb uninstall 删除的系统应用（免重置） - ryank231231.blog
我们知道，可以使用 `adb uninstall --user 0 <name of package>` 或者 `adb shell pm uninstall --user 0 <name of package>` 来卸载系统内置的应用。但是我们该这么来找回这些被我们干掉的应用？重置手机固然是种办法，但过于繁杂。  
但好在还有一种办法。  
还是在命令提示符下，执行 `adb shell cmd package install-existing <name of package>`，其中`<name of package>` 为你所恢复应用的包名。若没报错，你的应用就又回来了。 
 [https://ryank231231.top/archives/restore-system-apps-those-uninstall-by-adb.html](https://ryank231231.top/archives/restore-system-apps-those-uninstall-by-adb.html)
