## vmware-exporter
这是一个简单的 Prometheus 导出器，可从 vCenter 收集各种指标。

## 如何使用

当使用 /metcis 路径时，Exporter 会采集单个 vCenter 主机。 使用 /probe 可以抓取多个 vCenter 主机，但这些 vCenter 主机必须共享凭据（用户名和密码必须相同）。

### 二进制运行

下载二进制包，解压包，把二进制文件 vmware-exporter 放入到 `/usr/bin` 目录下，然后新建目录 `/etc/vmware-exporter/`

```bash
wget https://github.com/robotneo/vmware-exporter/releases/download/v0.1.12/vmware-exporter-v0.1.12-linux-amd64.tar.gz

mkdir -pv /opt/vmware
mkdir -pv /etc/vmware-exporter/

tar -zxvf vmware-exporter-v0.1.12-linux-amd64.tar.gz -C /opt/vmware
cd /opt/vmware
mv vmware-exporter /usr/bin

# 把 vmware.conf 文件放入 /etc/vmware-exporter/ 目录中，vmware.conf 通过命令行选项加载参数
ARGS="-vmware.username=administrator@vsphere.local -vmware.password=public@123 -vmware.vcenter=172.16.10.1:443"
# 更多参数 可通过空格进行添加 

# 复制项目中 system 目录下的 vmware-exporter.service 文件到 /etc/systemd/system/ 目录中，实现 systemd 管理 vmware-exporter 服务。
```

system 目录下的 vmware-exporter.service 文件需要放入 `/etc/systemd/system/` 目录中，并可通过 systemctl 命令进行管理。

### Docker 运行


## 参数设置

可以通过命令行选项、环境变量、yaml 配置文件或三者的组合来配置输出程序，设置的环境变量将被配置文件的内容覆盖，然后被启动时设置的任何命令行选项覆盖，可用的选项如下：

> 通用参数

| Option      | Description |
| ----------- | ----------- |
| -envflag.enable   | 告诉导出器在配置中使用环境变量  |
| -envflag.prefix	| 如果开启 -envflag.enable 可以设置环境变量的前缀    |
| -file	            | 沿用命令行选项结构的 yaml 配置文件的路径  |
| -http.address		| 导出器将绑定的地址和端口，格式为 host:端口（默认：":9169）  |
| -log.format	    | 日志输出格式可以是 json 或 logfmt（默认值：logfmt）   |
| -log.level	    | 日志输出级别可以是 debug、info、warn 或 error 之一（默认值：debug）  |
| -prim.maxRequests	| 最大并行抓取请求数（默认值：20）  |
| -disable.exporter.metrics	| 禁用 /metrics 路径中的导出器指标。如果 /metrics 目标被禁用，则始终启用（默认值为 true）   |
| -disable.exporter.target	| 禁用 /metrics 路径的默认目标  |
| -disable.default.collectors	| 如果设置此选项，则只有明确显示启用的采集器才会被启用  |

> 指标参数

| Option      | Description |
| ----------- | ----------- |
| -collector.datacenter | 启用或禁用 数据中心 指标收集（默认值：启用）  |
| -collector.cluster    | 启用或禁用 集群 指标收集（默认：启用）  |
| -collector.datastore	| 启用或禁用 存储 指标收集（默认：启用）  |
| -collector.host		| 启用或禁用 主机 指标收集（默认：启用）  |
| -collector.vm		    | 启用或禁用 虚拟机 指标收集（默认：已启用）  |
| -collector.esxcli.host.nic    | 使用通过 vCenter 调用的 esxcli 收集 ESXi NIC 固件信息（默认：禁用）   |
| -collector.esxcli.storage		| 使用通过 vCenter 调用的 esxcli 收集 ESXi 存储固件信息（默认：已禁用） |

> vCenter 信息参数

| Option      | Description |
| ----------- | ----------- |
| -vmware.granularity   | 采样数据的频率。默认值为20秒（默认为20）  |
| -vmware.interval      | 数据收集的频率。默认每20秒收集一次（默认值为20）  |
| -vmware.vcenter       | vCenter 服务器地址，格式为 host:port  |
| -vmware.username      | vCenter 用户名  |
| -vmware.password		| vCenter 密码  |
| -vmware.schema        | HTTP 或 HTTPS （默认：HTTPS）|
| -vmware.ssl		    | 验证 vCenter 的 SSL 证书或信任 |

