# CUDCOS_framework_PrivateUniverse
私有Universe框架库


/docker 文件夹下包含容器化的私有镜像库

/package 文件夹下包含通用框架的配置文件

##安装Python3
```Bash
mkdir -p /usr/local/python3
wget https://www.python.org/ftp/python/3.4.2/Python-3.4.2.tgz
tar -zxf Python-3.4.2.tgz
cd Python-3.4.2
./configure --prefix=/usr/local/python3
make
make install
mv /usr/bin/python /usr/bin/python_old
ln -s /usr/local/python3/bin/python3 /usr/bin/python
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
python -m pip install jsonschema
```

从官网

https://github.com/mesosphere/universe/tree/version-3.x/repo/packages


或
  
https://github.com/chinaunicomri/CUDCOS_framework_PrivateUniverse/tree/master/packages

添加新的packages到：


10.1.131.36:/root/linkerUniverse-master/repo/packages

编译源代码
```Bash
cd /root/linkerUniverse-master/scripts
./build.sh
```

编译完成后，开始制作镜像
```Bash
cd /root/linkerUniverse-master/docker/local-universe
sudo make base
sudo make local-universe
```

查看编译好的镜像
```Bash
docker image
```
拷贝编译好的镜像
```Bash
docker save ID > image.tar
```
拷贝编译好的镜像到dcos集群里面master节点上
```Bash
docker load -i image.tar
```
按照github上面的说明启动
https://github.com/chinaunicomri/CUDCOS_framework_PrivateUniverse/tree/master/docker/local-universe

针对open dcos需要安装open dcos cli
```Bash
curl -fLsS --retry 20 -Y 100000 -y 60 https://downloads.dcos.io/binaries/cli/linux/x86-64/dcos-1.8/dcos -o dcos && 
 sudo mv dcos /usr/local/bin && 
 sudo chmod +x /usr/local/bin/dcos && 
 dcos config set core.dcos_url http://10.1.24.172 && 
 dcos
```
设置dcos cli的SSL认证
```Bash
dcos config set core.ssl_verify false
```
添加local repository
```Bash
dcos package repo add local-universe http://master.mesos:8082/repo
```
查看本地的local repository list
```Bash
dcos package  repo list
```
在线框架库地址如下
名称：Universe
地址：https://universe.mesosphere.com/repo
