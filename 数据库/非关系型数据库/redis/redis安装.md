Redis的下载地址:          redis.io(英文官网)                 redis.cn(中文官网)

Redis要部署在Linux环境下(这里以虚拟机中的ubuntu14.04.5为运行环境),首先上传压缩包,解压到相应的目录

然后进入到Redis目录下,一次输入以下命令

make

cd src

make install

为了让Redis能够在后台运行,需要修改配置文件

vi redis.conf

将daemonize  no修改为daemonize  yes

然后进入到redis的src目录下,指定配置文件的位置

redis-server  /opt/redis/redis.conf               *这里需要替换成你自己的redis.conf的位置

然后进行简单的测试

redis-cli

set k2 world

get k2

shutdown

exit

redis-benchmark:    可以对Redis性能进行测试

Redis的读写性能很高，能达到每秒钟13万次

Redis杂项基础知识:

1.Redis是单进程的,单进程模式来处理客户端的请求,对读写事件的响应是通过epoll函数的包装来做到的,Redis的     实际处理速度完全依赖主进程的执行效率,Epoll是Linux内核为了处理大批量文件描述符,是Linux下多路复用IO       接口select/poll的增强版本,能显著提高程序在大量兵法连接中只有少量活跃的情况下的系统CPU利用率

2.Redis默认有16个数据库,类似组下标从0开始,初始默认使用0号库

3.使用select命令切换数据库,dbsize可以查看当前数据库的key的个数,keys *可以查看所有的key,keys也支持占位符查询,例如 keys k?就可以查出含有k的所有key      (Redis支持tab键补全功能)

4.FLUSHALL:   删除所有库的数据                         FLUSHDB:    删除当前库的所有数据
