## 功能说明
HDFS TO COS 工具用于将 HDFS 上的数据拷贝到腾讯云对象存储（COS）上。
## 使用限制
适用于 COS V5 版本
## 使用环境
### 系统环境
Linux 或 Windows 系统
### 软件依赖
JDK 1.7 或1.8 
#### 安装与配置
具体环境安装与配置请参考 [Java 安装与配置](doc/product/436/10865)。
## 配置及使用方法
### 配置方法
1. 安装 Hadoop-2.7.2 及以上版本，具体安装步骤请参考 [Hadoop 安装与测试](/doc/product/436/10867)。
2. 通过 [GitHub 地址](https://github.com/tencentyun/hdfs_to_cos_tools) 下载 HDFS TO COS 工具并解压缩。
3. 将要同步的 HDFS 集群的 core-site.xml 拷贝到 conf 文件夹中，其中 core-site.xml 中包含 NameNode 的配置信息。
4. 编辑配置文件`cos_info.conf`， 包括 APPID、存储桶（Bucket）、地域（Region）以及 API 密钥信息。
5. 在命令行参数中指定配置文件位置， 默认位置 `conf/cos_info.conf`。
> <font color="#0000cc">**注意：** </font>
当命令行参数中的参数与配置文件重合时，以命令行为准。

### 使用方法
（以 Linux 为例）
#### 查看帮助
```
./hdfs_to_cos_cmd -h
```
执行结果如下图所示：
![微信图片_20170807163035](//mc.qcloudimg.com/static/img/dcff34d37928c0d8b9c4b45c25ac116e/image.png)

#### 文件拷贝
- 从 HDFS 拷贝到 COS，若 COS 上已存在同名文件， 则会覆盖原文件。
```
./hdfs_to_cos_cmd --hdfs_path=/tmp/hive --cos_path=/hdfs/20170224/
```
-  从 HDFS 拷贝到 COS，若 COS 上已存在同名且长度一致的文件时，则忽略上传（适用于拷贝一次后，重新拷贝）。
```
./hdfs_to_cos_cmd --hdfs_path=/tmp/hive --cos_path=/hdfs/20170224/ -skip_if_len_match
```
这里只做长度的判断，因为如果将 Hadoop 上的文件摘要算出，开销较大。

#### 目录信息
```
conf : 配置文件, 用于存放 core-site.xml 和 cos_info.conf
log  : 日志目录
src  : Java 源程序
dep  : 编译生成的可运行的 JAR 包
```
## 问题与帮助
#### 关于配置信息
请确保填写的配置信息正确，包括 APPID、存储桶（Bucket）、地域（Region）以及 API 密钥信息。并保证机器的时间和北京时间一致（相差 1 分钟左右是正常的），如果相差较大，请重新设置机器时间。
#### 关于 DateNode
请保证对于 DateNode，拷贝程序所在的机器也可以连接。NameNode 有外网 IP 可以连接，但获取的 block 所在的 DateNode 机器是内网 IP，是无法直接连接上的。因此建议同步程序放在 Hadoop 的某个节点上执行，保证对 NameNode 和 DateNode 皆可访问。
#### 关于权限
请使用 Hadoop 命令下载文件，检查是否正常， 再使用同步工具同步 Hadoop 上的数据支持。
#### 关于文件覆盖
对于 COS 上已存在的文件， 默认进行重传覆盖。除非用户明确的指定`-skip_if_len_match`，当文件长度一致时则跳过上传。
#### 关于 cos path
 cos path 默认为是目录， 最终从 HDFS 上拷贝的文件都会存放在该目录下。
