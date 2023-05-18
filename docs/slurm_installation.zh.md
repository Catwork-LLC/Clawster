# Slurm 安装和升级

## slurmctld 主服务器的硬件优化

[SchedMD](https://www.schedmd.com/)建议slurmctld服务器配置为具有少量但高性能的 CPU 核心，以确保最佳的响应能力。

文件系统`/var/spool/slurmctld/`应安装在尽可能快的磁盘上（如SSD或NVMe）。

## 创建全局用户账户

整个集群必须具有统一的用户和组命名空间（包括UIDs和GIDs），请参考[Slurm Quick Start](https://slurm.schedmd.com/quickstart_admin.html)管理员指南。虽然无需允许用户登录到控制主机（ControlMachine或BackupController），但必须在这些主机上配置用户和组。若要限制用户通过 SSH 登录，请使用 `/etc/ssh/sshd_config` 中的 `AllowUsers` 参数。

[Slurm](https://www.schedmd.com/)和[MUNGE](https://dun.github.io/munge/)需要<strong>在集群中的所有服务器和节点上保持一致的UIDs和GIDs</strong>，包括<em>slurm</em>和<em>munge</em>用户。

重要提示：避免使用低于 1000 的 UID 和 GID。这在标准配置文件`/etc/login.defs`中由`UID_MIN、UID_MAX、GID_MIN、GID_MAX`等参数定义，详情请参阅<https://en.wikipedia.org/wiki/User_identifier>。

创建用于 Slurm 和 MUNGE 的用户和组，例如：

```bash
export MUNGEUSER=1005
groupadd -g $MUNGEUSER munge
useradd  -m -c "MUNGE Uid 'N' Gid Emporium" -d /var/lib/munge -u $MUNGEUSER -g munge  -s /sbin/nologin munge
export SlurmUSER=1001
groupadd -g $SlurmUSER slurm
useradd  -m -c "Slurm workload manager" -d /var/lib/slurm -u $SlurmUSER 
```

请确保在所有节点上以相同的方式创建这些用户。这必须在<strong>安装 RPM 包之前完成</strong>（如果这些用户不存在，则会创建随机的 UID/GID 对）。

请注意，UID 和 GID 在1000以下目前保留给 CentOS 系统用户，请参考[本文](https://unix.stackexchange.com/questions/343445/user-id-less-than-1000-on-centos-7)和文件 `/etc/login.defs`。