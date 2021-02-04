---
title: "Ceph总结"
categories: ["云计算"]
tags: ["ceph", "存储"]
date: 2020-10-30T20:50:00+08:00
---

# Ceph架构

## 存储集群
一个Ceph存储集群包含两种后台服务：`Ceph monitor`和`Ceph OSD deamon`

一个`Ceph monitor`维持着集群视图的一份主要拷贝，Ceph集群的monitor节点保证了高可用性当其中的某个monitor挂掉的时候，存储集群客户端会通过Ceph monitor重试集群视图的拷贝。

每个Ceph OSD后台服务检查自己的状态和其他OSD上报给monitor的状态。存储集群客户端和和每个Ceph OSD后台服务使用`CRUSH`算法高效计算数据存储位置，避免依赖一个中心化的查询表。Ceph的高级特性包括通过`librados`来提供原始接口访问存储集群和一系列建立在`librados`之上的服务接口。

存储集群通过ceph客户端包括Ceph Block Devcie/Ceph Object Storage/Ceph File System或者在`librados`上定制的，它将数据保存为对象，每个对象对应为文件系统上的一个文件，存储在Object Storage Devce(OSD)设备上。`Ceph OSD Daemon`后台服务负责处理他们在磁盘上的读写操作。

Ceph OSD后台服务将所有数据存储为平铺结构（没有目录层次）。每个文件包含一个标识符、二进制文件和kv形式的元数据。这种语义对客户端来说是完全透明的。例如，`CephFS`使用metadata来存储文件属性包括文件所有者、创建时间、最后修改时间等。

## Crush介绍
`Ceph`客户端和Ceph OSD后台服务同时使用CRUSH算法来计算数据存储位置。CRUSH算法通过所有客户端和OSD后台服务的分布式计算来支持海量规模。`CRUSH`算法的论文看[这里](https://ceph.com/wp-content/uploads/2016/08/weil-crush-sc06.pdf)。

# 硬件需求
## CPU
Ceph OSD运行中`rados`服务，通过`CRUSH`算法计算数据分布，复制数据和操作他们在集群中的复本。因此OSD有足够的理由获取大量的算力。根据不同使用场景需要不同的CPU资源，普通的轻度如归档使用每个OSD需要一个CPU核，而像RBD挂卷给虚拟机这个的重载服务则需要两个CPU核。Monitor/Manager则没有太重的CPU负荷，一个普通的CPU核即可应付。同时还需要考虑其他后台服务如openstack对CPU的占用，建议为Ceph保留足够的CPU资源。

## 内存
内存当然是越多越好。Monitor/Manager通常需要64GB内存，对于一个拥有大量OSDs的集群128GB是一个合理的目标。BlueStor OSDs最小需要4GB内存，考虑到操作系统和其他任务因素，建议为每个BlueStor OSD配置8GB内存。

## 数据存储
简单的Ceph集群必须在将所有数据写入journal(或者WAL+DB)才能返回ack消息，所以保持metadata和OSD性能平衡非常重要。

OSDs需要大量的硬盘去保存对象数据，推荐每个最个硬盘最小为1TB。考虑到大硬盘单位容量的价格优势，比如3TB硬盘的单位容量价格要比1TB的单位容量价格要便宜40%，可以选择容量更大失硬盘。

在单块SAS/SATA上运行多个OSDs不是一个好主意，然后对于NVMe，将一块硬盘分成两个或者多个OSD却可以提升性能。

>在一块硬件上同时运行OSD和monitor/metadata server不是一个好主意。

推荐使用单独的硬盘运行操作系统和软件，每个运行OSD守护进程的主机上有一个NVMe以上的驱动。

>Ceph的最佳实践之一是将操作系统、OSD数据、OSD journal运行在不同的驱动上。

## 硬盘控制器
HBA卡对写吞吐量有显著的影响，谨慎选择确保不会产生性能瓶颈。RAID-mode(IR) HBA卡比普通JBOD模式HBA卡增加一些时延。

## 网卡
选择10Gbps以上的网卡。在1Gbps的网卡上复制1TB数据需要3个小时，10TB则需要30小时，而在10Gbps网卡上则减少到20分钟和1个小时。

# Ceph安装

## 添加节点

在待添加节点上执行以下操作：

- 执行`yum install -y lvm2 yum-utils python3`安装python3、yum-utils、lvm2
- 添加docker仓库`yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`
- 执行`yum install -y docker-ce docker-ce-cli containerd.io`安装docker
- 启动docker并设置开机自启动`systemctl start docker && systemctl enable docker`
- 拷贝公钥到待添加节点`ssh-copy-id -f -i /etc/ceph/ceph.pub root@ceph3`（如未开启root用户密码登陆，则可手动将/etc/ceph/ceph.pub拷贝到/root/.ssh/authorized_keys文件中）
- 执行`ceph orch host add ceph3`添加节点

# RBD管理

## 创建rbd pool
指定pg数和pgp数。
```shell
ceph osd pool create rbd  32 32
ceph osd pool application enable rbd rbd
rbd pool init -p rbd
```

## 查询rbd pool列表
```powershell
ceph osd pool ls
```

## 创建rbd image
```shell
rbd create --size 10240 my_image
```
校验镜像是否存在
```shell
rbd -p rbd ls
```
删除rbd image
```shell
rbd rm -p rbd my_image
```
### 特性列表

| 特性名称       | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| layering       | 分层，镜像image的clone操作时父子镜像使用COW技术。            |
| striping v2    | 条带化对象数据，类似raid0。                                  |
| exclusive-lock | 独占分布式锁，开启时确保只有一个客户端在访问image。          |
| object-map     | 依赖独占锁，对象是thin-provisioning瘦分配，开启后记录对象视图，标识对象是否真实存在。 |
| fast-diff      | 依赖object-map，快速比较镜像image和快照之间的差异。          |
| deep-flatten   | 扁平化操作，镜像clone时父子镜像是COW，flatten解除父子image的依赖，deep-flatten解除子镜像快照的依赖。 |

使用`rbd info --pool rbd my_image`查看镜像支持的特性。

# 挂载RBD卷到虚拟机

## 配置ceph
创建ceph认证
```shell
ceph auth get-or-create client.libvirt mon 'profile rbd' osd 'profile rbd pool=rbd'
```

## 通过`map`映射到本地
挂载
```powershell
rbd device map rbd/my_image --id admin
```
解挂载
```powershell
rbd device unmap rbd/my_image
```

## 通过virsh挂载

### 设置访问ceph凭据
创建`secret.xml`文件，内容如下：
```shell
<secret ephemeral='no' private='no'>
    <usage type='ceph'>
        <name>client.libvirt secret</name>
    </usage>
</secret>
```
定义`secret`：
```powershell
virsh secret-define --file secret.xml
Secret 4e302cf8-a4de-44e3-af37-b95c0da90d7b created
```
在ceph中导出`secret`配置文件:
```powershell
ceph auth get-key client.libvirt | sudo tee client.libvirt.key
```
导入`secret`
```powershell
virsh secret-set-value --secret 4e302cf8-a4de-44e3-af37-b95c0da90d7b --base64 $(cat client.libvirt.key)
```
获取openstack虚拟机的libvirt uuid
```powershell
openstack server show ubuntu | grep instance
instance-00000014
```

### 挂载设备
添加新磁盘配置文件`disk.xml`
```powershell
<disk type='network' device='disk'>
    <driver name='qemu' type='raw' cache='none'/>
    <source protocol='rbd' name='rbd/my_image'>
        <host name='192.168.5.24' port='6789'/>
    </source>
    <auth username='libvirt'>
        <secret type='ceph' uuid='4e302cf8-a4de-44e3-af37-b95c0da90d7b'/>
    </auth>
    <target dev='vdd' bus='virtio'/>
</disk>
```
通过`virsh`命令行挂载
```shell
virsh attach-device instance-00000014 disk.xml --persistent
virsh detach-disk instance-00000014 vdd --persistent
```
查询是否挂载成功
```shell
virsh qemu-monitor-command --hmp instance-00000014 'info block'
```

# 性能测试

## 使用rados bench
使用参数包括
```powershell
bench <seconds> write|seq|rand [-t concurrent_operations] [--no-cleanup] [--run-name run_name] [--no-hints] [--reuse-bench] [-p pool]
        default is 16 concurrent IOs and 4 MB ops
        default is to clean up after write benchmark
        default run-name is 'benchmark_last_metadata'
```
例如在rbd存储池上测试64K IO写性能，时长60秒。
```shell
rados bench -p rbd 60 write -t 16 -b 64K 64k --no-cleanup
```
测试结果如下：
```shell
Total time run:         60.0313
Total writes made:      12988
Write size:             65536
Object size:            65536
Bandwidth (MB/sec):     13.5221
Stddev Bandwidth:       2.54425
Max bandwidth (MB/sec): 17.125
Min bandwidth (MB/sec): 1.625
Average IOPS:           216
Stddev IOPS:            40.7081
Max IOPS:               274
Min IOPS:               26
Average Latency(s):     0.0739494
Stddev Latency(s):      0.0711021
Max latency(s):         0.859925
Min latency(s):         0.00281792
```

## map方式

创建1个分区，使用ext4文件系统，64K随机写IO，4线程60秒，带宽11.1MB/s，时延22ms
```powershell
[root@ceph1 ceph_disk]# fio -filename=fake_64k.dat -direct=1 -iodepth 1 -thread -rw=randwrite -ioengine=psync -bs=64k -size=1G -numjobs=4 -runtime=60 -group_reporting -name=rand_100write_64k
rand_100write_4k: (g=0): rw=randwrite, bs=(R) 64.0KiB-64.0KiB, (W) 64.0KiB-64.0KiB, (T) 64.0KiB-64.0KiB, ioengine=psync, iodepth=1
...
fio-3.7
Starting 4 threads
Jobs: 4 (f=4): [w(4)][100.0%][r=0KiB/s,w=9.94MiB/s][r=0,w=159 IOPS][eta 00m:00s]
rand_100write_4k: (groupid=0, jobs=4): err= 0: pid=12647: Tue Nov  3 08:35:04 2020
  write: IOPS=178, BW=11.1MiB/s (11.7MB/s)(669MiB/60038msec)
    clat (msec): min=2, max=781, avg=22.42, stdev=35.46
     lat (msec): min=2, max=781, avg=22.43, stdev=35.46
    clat percentiles (msec):
     |  1.00th=[    3],  5.00th=[    4], 10.00th=[    4], 20.00th=[    5],
     | 30.00th=[    7], 40.00th=[   11], 50.00th=[   19], 60.00th=[   22],
     | 70.00th=[   26], 80.00th=[   31], 90.00th=[   40], 95.00th=[   53],
     | 99.00th=[  167], 99.50th=[  241], 99.90th=[  502], 99.95th=[  542],
     | 99.99th=[  684]
   bw (  KiB/s): min=  128, max= 9472, per=25.06%, avg=2858.18, stdev=1391.44, samples=479
   iops        : min=    2, max=  148, avg=44.62, stdev=21.73, samples=479
  lat (msec)   : 4=15.24%, 10=24.54%, 20=15.04%, 50=39.57%, 100=3.78%
  lat (msec)   : 250=1.36%, 500=0.37%, 750=0.09%, 1000=0.01%
  cpu          : usr=0.06%, sys=0.28%, ctx=21418, majf=0, minf=10
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,10701,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=11.1MiB/s (11.7MB/s), 11.1MiB/s-11.1MiB/s (11.7MB/s-11.7MB/s), io=669MiB (701MB), run=60038-60038msec

Disk stats (read/write):
  rbd0: ios=0/10789, merge=0/1783, ticks=0/62336, in_queue=59609, util=96.74%
```

直接写裸盘方式，64K随机IO，4线程60秒，带宽15.2MB/s，时延16ms
```powershell
[root@ceph1 shoppon]# fio -filename=/dev/rbd0 -direct=1 -iodepth 1 -thread -rw=randwrite -ioengine=psync -bs=64K -size=1G -numjobs=4 -runtime=60 -group_reporting -name=rand_100write_64K
rand_100write_64K: (g=0): rw=randwrite, bs=(R) 64.0KiB-64.0KiB, (W) 64.0KiB-64.0KiB, (T) 64.0KiB-64.0KiB, ioengine=psync, iodepth=1
...
fio-3.7
Starting 4 threads
Jobs: 4 (f=4): [w(4)][100.0%][r=0KiB/s,w=14.3MiB/s][r=0,w=229 IOPS][eta 00m:00s]
rand_100write_64K: (groupid=0, jobs=4): err= 0: pid=13280: Tue Nov  3 08:46:41 2020
  write: IOPS=242, BW=15.2MiB/s (15.9MB/s)(910MiB/60003msec)
    clat (msec): min=2, max=283, avg=16.47, stdev=15.27
     lat (msec): min=2, max=283, avg=16.48, stdev=15.27
    clat percentiles (msec):
     |  1.00th=[    4],  5.00th=[    4], 10.00th=[    4], 20.00th=[    5],
     | 30.00th=[    7], 40.00th=[   12], 50.00th=[   16], 60.00th=[   19],
     | 70.00th=[   23], 80.00th=[   27], 90.00th=[   31], 95.00th=[   33],
     | 99.00th=[   45], 99.50th=[   85], 99.90th=[  222], 99.95th=[  275],
     | 99.99th=[  284]
   bw (  KiB/s): min= 2048, max=13184, per=24.99%, avg=3880.50, stdev=2128.65, samples=480
   iops        : min=   32, max=  206, avg=60.58, stdev=33.26, samples=480
  lat (msec)   : 4=12.28%, 10=24.72%, 20=27.41%, 50=34.83%, 100=0.36%
  lat (msec)   : 250=0.34%, 500=0.05%
  cpu          : usr=0.06%, sys=0.18%, ctx=14559, majf=0, minf=7
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,14558,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=15.2MiB/s (15.9MB/s), 15.2MiB/s-15.2MiB/s (15.9MB/s-15.9MB/s), io=910MiB (954MB), run=60003-60003msec

Disk stats (read/write):
  rbd0: ios=61/14529, merge=0/0, ticks=31/238712, in_queue=225866, util=94.87%
```

IO越小，带宽越小，IOPS越大。

# 快照管理

## 创建快照
```powershell
rbd snap create rbd/my_image@my_snapshot
```
## 快照查询
```powershell
rbd snap ls rbd/my_image
```

## 删除快照
```shell
rbd snap rm rbd/my_image@my_snapshot2
```

## 快照回滚
将指定快照的数据回滚到镜像上。
```powershell
rbd snap rollback rbd/my_image@my_snapshot
```

## 导出差异
导出指定快照和当前镜像数据之间的差异。
```powershell
rbd export-diff rbd/my_image@my_snapshot my_diff
```

## 导出增量差异
指定两个快照导出增量差异，基础快照不需要指定镜像名。
```powershell
rbd export-diff rbd/my_image@my_snapshot2 --from-snap my_snapshot1 my_diff_1_2
```

## 导入差异
```powershell
rbd import-diff my_diff rbd/my_image
```


# Ceph常用命令

停止所有`ceph`服务：`systemctl stop ceph\*.service ceph\*.target`

# 参考