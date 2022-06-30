# 分享一下自己将容器作为开发环境的方案 - V2EX
在过去几年中我需要在不同的机器上开发 Python 项目、训练模型（主要基于 torch cuda ），为了解决环境、依赖、工具差异，提升干活效率，于是自己封一个专门用于 Python 开发的 docker 镜像。在此分享给大家

典型的使用场景：

-   本地 docker run 跑起来一个独立容器环境，自动帮我配好 bash ，处理好挂载文件路径权限，处理好好 SSH agent forwarding 等常见开发问题。然后命令行测试
-   本地 / 远端跑起来一个容器，然后 ssh 过去执行一些耗时较长的任务（比如训练模型）。ssh 过去后会自动进入 sreen session （这样不需要考虑什么 nohup &），且 bash 也已经配好
-   本地 / 远端跑起来一个容器，然后 vscode ssh remote 过去，容器内部已经装好常见 Python 插件，且 bash 也已配好

传送门： [https://github.com/vkit-x/wden](https://github.com/vkit-x/wden) 
 [https://www.v2ex.com/t/862569#reply11](https://www.v2ex.com/t/862569#reply11)
