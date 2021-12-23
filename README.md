## Dockerfile
1、进入pyspider目录 （这个其实就是直接fork binux/pyspider的文件，然后在`requirements.txt`中添加了几个库的，目的是锁定版本号以免出错）
 在cmd中进入`pyspider`这个目录
 ```
 cd pyspider
 ```

2、然后新建pyspider的docker image：
```
docker build -t "pyspider" .
```

说明：其实这一步是多余，为什么要这么多此一举呢？　因为binux提供的默认docker image `binux/pyspider` 是基于python2.7制作的，导致了里面后续安装的库大部分情况下都会安装失败，非常蛋疼。所以我干脆就再自建一个。

## docker-compose up
3、返回到根目录`docker-pyspider`
目的是运行docker-compose.yml
在cmd输入：
```
docker-compose up
```

说明：这里的`docker-compose.yml`已经设置好加装`config.json`这个参数文件，主要用于设置登陆密码

## 最后
pyspider生成的数据会记录到`data`这个外挂的文件夹当中，这样方便我们保存、调取数据。
