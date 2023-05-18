# 作业

## 作业提交流程

1. 编写作业脚本

```bash
vi test.slurm  # 根据需求，选择计算资源：CPU 或 GPU、所需核数、是否需要大内存
```

2. 提交作业

```bash
sbatch test.slurm
```

3. 查看作业和资源

```bash
squeue       # 查看正在排队或运行的作业
```

或

```bash
sacct        # 查看过去 24 小时内已完成的作业
```

集群资源实时状态查询

```bash
sinfo        # 若有 idle 或 mix 状态的节点，排队会比较快
```

## 集群队列介绍

`scontrol show partition` 查看集群队列介绍
`sinfo` 查看集群资源实时状态

## Job Array 阵列作业

一批作业，若所需资源和内容相似，可借助 Job Array 批量提交。Job Array 中的每一个作业在调度时视为独立的作业。

cpu 队列 slurm 脚本示例 array

```bash
#!/bin/bash

#SBATCH --job-name=test           # 作业名
#SBATCH --partition=small         # small 队列
#SBATCH -n 1                      # 总核数 1
#SBATCH --ntasks-per-node=1       # 每节点核数
#SBATCH --output=array_%A_%a.out
#SBATCH --output=array_%A_%a.err
#SBATCH --array=1-20%10           # 总共 20 个子任务，每次最多同时运行 10 个

echo $SLURM_ARRAY_TASK_ID
```

