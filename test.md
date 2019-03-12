# Android Studio 和 Mac 的初次相遇

------

&emsp;&emsp;这两天湿气席卷了沿海小镇珠海，回南天提前发生，让原本冰冷的空气多了些冰镇汽水的味道，周围充满肥宅快乐的喜庆气氛。
&emsp;&emsp;然鹅肥宅如我却不能在这样的空气里轻松起来，有一个问题困扰我半个礼拜之久，曾两三个晚上的花时间在这个问题上，至凌晨一两点。问题大致如下：

----
###问题产生经过：
> * 上手新鲜多汁的Mac系统，一开始便想找一些Windows平台的替代品，例如Xshell这种Windows上超级好用的工具（Xshell，终端模拟工具，支持SSH...），最终找到了国产良心finalShell[^finalShell]，随即设置了SSH连接、建立Socket隧道，目的是...（你懂的），接着下载最新正式版Android Studio，安装成功之后打开HTTP Proxy（注：这里可能开始埋下隐患）下载SDK、各个版本的Android、Build-tool、Gradle，新建项目并Build它。。。失败。。。失败。。。失败！
> * 首先是各种`Could not GET https://dl.google.com/dl/android/maven2/com/android/
tools/build/gradle/xxx/` 错误，按照 Stack Overflow[^code] 上大神的说法，勾选`enable embedded maven repository`但是看不到任何效果，甚至带来永远停留在 `waiting for build finish...` 的情况 ......这对我造成了一万点伤害，因为看着比Windows快一百倍的AS自带的Android虚拟机，我没有理由放弃在Mac上做Android开发，无论什么代价都必须解决这个问题！
> * 还是按照 Stack Overflow 另一篇文章的做法，尝试使用 `sync project with gradle files` 来尝试Build项目，产生了各种 `Unable to resolve dependency for ':app@debug/compileClasspath': Could not resolve com.android.support:appcompat-v7:28.0.0.`，期间尝试了各种修改代理，把过去正确编译的项目拿过来编译、修改gradle文件、卸载重装AS，皆不成功，一声长叹！

---

###问题解决经过：

> 使用 `shift+cmmand+.` 组合键 让 `Finder` 显示隐藏文件，删除 `用户\.gradle` 文件夹，Android Studio —— HTTP Proxy 使用 No Proxy，关闭并重新打开，这时 AndroidStudio 去加载新的的gradle文件（事实上，这些都不用代理），之后开始去下载各种 `android.support:appcompat` 依赖文件，等它Build完就可以了。

---

###问题产生与解决的原因猜测：
> * 在设置代理后去下载gradle和各种android.support:appcompat依赖文件过程中，SSH连接可能断开过，这会造成android.support:appcompat等文件下载不完整，这是造成 `Unable to resolve dependency for ':app@debug/compileClasspath': Could not resolve com.android.support:appcompat-v7:28.0.0.` 的原因
> * 而设置代理的信息被写入 `.gradle` 文件夹的某些文件中，而且这些连接信息（IP，端口信息）记录错误，这是`Could not GET https://dl.google.com/dl/android/maven2/com/android/tools/build/gradle/xxx/` 错误产生的原因
> * 删除 `.gradle` 文件夹，关闭 AndroidStudio 代理，重新下载 gradle，解决上述代理错误的问题，正确的 gradle 文件能够下载到完整的 support:appcompat 依赖文件，至此解决到全部问题。

------

###<center>感谢您花费时间阅读这篇文章，祝您生活愉快！</center>

##<center>作者 [@AlexL][3]  </center>   
#####<center>2016 年 07月 07日   </center> 

[^finalShell]: FinalShell是一体化的的服务器,网络管理软件,不仅是ssh客户端,还是功能强大的开发,运维工具,充分满足开发,运维需求.特色功能:免费海外服务器远程桌面加速,ssh加速,双边tcp加速,内网穿透.。

[^code]: Stack Overflow是一个与程序相关的IT技术问答网站。用户可以在网站免费提交问题，浏览问题，索引相关内容。

[3]: http://weibo.com/ghosert

