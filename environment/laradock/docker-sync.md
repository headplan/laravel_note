# Docker-Sync

Mac上的Docker速度会有些慢 , 特别是在项目较大的时候  . 问题源自MacOS上的绑定挂载性能 . 因为默认情况下 , Mac使用osxfs , 与Linux相比共享代码到Docker的性能较差 , 当然它还是又很多优点的 .

这个问题可以查看官方社区的帖子 :

[https://github.com/docker/for-mac/issues/77\#issuecomment-283996750](https://github.com/docker/for-mac/issues/77#issuecomment-283996750)

当然选择使用NFS也是很简单的 , 下面有文章记录了两个方案使用Dinghy或D4m-NFS

**或者直接使用Docker-Sync**

[https://github.com/EugenMayer/docker-sync](https://github.com/EugenMayer/docker-sync)

[https://github.com/EugenMayer/docker-sync-boilerplate](https://github.com/EugenMayer/docker-sync-boilerplate)

