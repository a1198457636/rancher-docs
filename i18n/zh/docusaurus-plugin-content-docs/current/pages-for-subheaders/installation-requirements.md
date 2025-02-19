---
title: 安装要求
description: 如果 Rancher 配置在 Docker 或 Kubernetes 中运行时，了解运行 Rancher Server 的每个节点的节点要求
---

本文描述了对需要安装 Rancher Server 的节点的软件、硬件和网络要求。Rancher Server 可以安装在单个节点或高可用的 Kubernetes 集群上。

:::note 重要提示：

如果你需要在 Kubernetes 集群上安装 Rancher，该节点的要求与用于运行应用和服务的[下游集群的节点要求](../how-to-guides/new-user-guides/kubernetes-clusters-in-rancher-setup/node-requirements-for-rancher-managed-clusters.md)不同。

:::

请确保安装 Rancher Server 的节点满足以下要求：

- [操作系统和容器运行时要求](#操作系统和容器运行时要求)
   - [RKE 要求](#rke-要求)
   - [K3s 要求](#k3s-要求)
   - [RKE2 要求](#rke2-要求)
   - [安装 Docker](#安装-docker)
- [硬件要求](#硬件要求)
- [CPU 和内存](#cpu-和内存)
   - [RKE 和托管 Kubernetes](#rke-和托管-kubernetes)
   - [K3s Kubernetes](#k3s-kubernetes)
   - [RKE2 Kubernetes](#rke2-kubernetes)
   - [Docker](#docker)
- [Ingress](#ingress)
- [磁盘](#磁盘)
- [网络要求](#网络要求)
   - [节点 IP 地址](#节点-ip-地址)
   - [端口要求](#端口要求)
- [Dockershim 支持](#dockershim-支持)

如需获取在生产环境中运行 Rancher Server 的最佳实践列表，请参见[最佳实践](../reference-guides/best-practices/rancher-server/tips-for-running-rancher.md)。

Rancher UI 在 Firefox 或 Chrome 中效果更佳。

# 操作系统和容器运行时要求

Rancher 兼容当前所有的主流 Linux 发行版。

运行 RKE Kubernetes 集群的节点需要安装 Docker。Kubernetes 安装不需要 Docker。

Rancher 需要安装在支持的 Kubernetes 版本上。如需了解你使用的 Rancher 版本支持哪些 Kubernetes 版本，请参见[支持维护条款](https://rancher.com/support-maintenance-terms/)。

如需了解各个 Rancher 版本通过了哪些操作系统和 Docker 版本测试，请参见[支持和维护条款](https://rancher.com/support-maintenance-terms/)。

所有支持的操作系统都使用 64-bit x86 架构。

请安装 `ntp`（Network Time Protocol），以防止在客户端和服务器之间由于时间不同步造成的证书验证错误。

某些 Linux 发行版的默认防火墙规则可能会阻止与 Helm 的通信。我们建议禁用 firewalld。如果使用的是 Kubernetes 1.19，1.20 或 1.21，则必须关闭 firewalld。

如果你不太想这样做的话，你可以查看[相关问题](https://github.com/rancher/rancher/issues/28840)中的建议。某些用户已能成功[使用 ACCEPT 策略 为 Pod CIDR 创建一个独立的 firewalld 区域](https://github.com/rancher/rancher/issues/28840#issuecomment-787404822)。

如果你需要在 ARM64 上使用 Rancher，请参见[在 ARM64（实验功能）上运行 Rancher](../getting-started/installation-and-upgrade/advanced-options/enable-experimental-features/rancher-on-arm64.md)。

### RKE 要求

容器运行时方面，RKE 可以兼容当前的所有 Docker 版本。

请注意，必须应用以下 sysctl 设置：

```
net.bridge.bridge-nf-call-iptables=1
```

### K3s 要求

容器运行时方面，K3s 可以兼容当前的所有 Docker 版本。

Rancher 需要安装在支持的 Kubernetes 版本上。如需了解你使用的 Rancher 版本支持哪些 Kubernetes 版本，请参见[支持维护条款](https://rancher.com/support-maintenance-terms/)。如需指定 K3s 版本，请在运行 K3s 安装脚本时，使用 `INSTALL_K3S_VERSION` 环境变量。

如果你使用 **Raspbian Buster** 在 K3s 集群上安装 Rancher，请按照[这些步骤](https://rancher.com/docs/k3s/latest/en/advanced/#enabling-legacy-iptables-on-raspbian-buster)切换到旧版 iptables。

如果你使用 Alpine Linux 的 K3s 集群上安装 Rancher，请按照[这些步骤](https://rancher.com/docs/k3s/latest/en/advanced/#additional-preparation-for-alpine-linux-setup) 进行其他设置。



### RKE2 要求

如需了解 RKE2 通过了哪些操作系统版本的测试，请参见[支持和维护条款](https://rancher.com/support-maintenance-terms/)。

RKE2 安装不需要 Docker。

### 安装 Docker

Docker 是 Helm Chart 安装所必须的。你可以参见 [Docker 官方文档](https://docs.docker.com/)中的步骤进行安装。Rancher 也提供使用单条命令安装 Docker 的[脚本](../getting-started/installation-and-upgrade/installation-requirements/install-docker.md)。

# 硬件要求

本节描述安装 Rancher Server 的节点的 CPU、内存和磁盘要求。

# CPU 和内存

硬件要求根据你的 Rancher 部署规模而定。请根据要求配置每个节点。通过单节点容器安装 Rancher，和在 Kubernetes 集群上安装 Rancher 的要求有所不同。

### RKE 和托管 Kubernetes

这些 CPU 和内存要求适用于每个安装 Rancher Server 的 Kubernetes 集群中的主机。

这些要求适用于 RKE Kubernetes 集群以及托管的 Kubernetes 集群，例如 EKS。

| 部署规模 | 集群 | 节点 | vCPUs | 内存 |
| --------------- | ---------- | ------------ | -------| ------- |
| 小 | 最多 150 个 | 最多 1500 个 | 2 | 8 GB |
| 中 | 最多 300 个 | 最多 3,000 个 | 4 | 16 GB |
| 大 | 最多 500 个 | 最多 5,000 个 | 8 | 32 GB |
| 特大 | 最多 1,000 个 | 最多 10,000 个 | 16 | 64 GB |
| 超大 | 最多 2,000 个 | 最多 20,000 个 | 32 | 128 GB |

每个用例和环境都是不同的。请[联系 Rancher](https://rancher.com/contact/) 来审核你的情况。

### K3s Kubernetes

这些 CPU 和内存要求适用于每个[安装 Rancher Server 的 Kubernetes 集群](install-upgrade-on-a-kubernetes-cluster.md)中的主机。

| 部署规模 | 集群 | 节点 | vCPUs | 内存 | 数据库大小 |
| --------------- | ---------- | ------------ | -------| ---------| ------------------------- |
| 小 | 最多 150 个 | 最多 1500 个 | 2 | 8 GB | 2 核，4 GB + 1,000 IOPS |
| 中 | 最多 300 个 | 最多 3,000 个 | 4 | 16 GB | 2 核，4 GB + 1,000 IOPS |
| 大 | 最多 500 个 | 最多 5,000 个 | 8 | 32 GB | 2 核，4 GB + 1,000 IOPS |
| 特大 | 最多 1,000 个 | 最多 10,000 个 | 16 | 64 GB | 2 核，4 GB + 1,000 IOPS |
| 超大 | 最多 2,000 个 | 最多 20,000 个 | 32 | 128 GB | 2 核，4 GB + 1,000 IOPS |

每个用例和环境都是不同的。请[联系 Rancher](https://rancher.com/contact/) 来审核你的情况。


### RKE2 Kubernetes

这些 CPU 和内存要求适用于安装了 RKE2 的每个实例。最低配置要求如下：

| 部署规模 | 集群 | 节点 | vCPUs | 内存 |
| --------------- | -------- | --------- | ----- | ---- |
| 小 | 最多 5 个 | 最多 50 个 | 2 | 5 GB |
| 中 | 最多 15 个 | 最多 200 个 | 3 | 9 GB |

### Docker

这些 CPU 和内存要求适用于[单节点](rancher-on-a-single-node-with-docker.md)安装 Rancher 的主机。

| 部署规模 | 集群 | 节点 | vCPUs | 内存 |
| --------------- | -------- | --------- | ----- | ---- |
| 小 | 最多 5 个 | 最多 50 个 | 1 | 4 GB |
| 中 | 最多 15 个 | 最多 200 个 | 2 | 8 GB |

# Ingress

安装 Rancher 的 Kubernetes 集群中的每个节点都应该运行一个 Ingress。

Ingress 需要部署为 DaemonSet 以确保负载均衡器能成功把流量转发到各个节点。

如果是 RKE 和 K3s 安装，你不需要手动安装 Ingress，因为它是默认安装的。

如果是托管 Kubernetes 集群（EKS、GKE、AKS）和 RKE2 Kubernetes 安装，你需要设置 Ingress。

- **Amazon EKS**：[在 Amazon EKS 上安装 Rancher 以及如何安装 Ingress 以访问 Rancher Server](../getting-started/installation-and-upgrade/install-upgrade-on-a-kubernetes-cluster/rancher-on-amazon-eks.md)。
- **AKS**：[使用 Azure Kubernetes 服务安装 Rancher 以及如何安装 Ingress 以访问 Rancher Server](../getting-started/installation-and-upgrade/install-upgrade-on-a-kubernetes-cluster/rancher-on-aks.md)。
- **GKE**：[使用 GKE 安装 Rancher 以及如何安装 Ingress 以访问 Rancher Server](../getting-started/installation-and-upgrade/install-upgrade-on-a-kubernetes-cluster/rancher-on-gke.md)。

# 磁盘

etcd 在集群中的性能决定了 Rancher 的性能。因此，为了获得最佳速度，我们建议使用 SSD 磁盘来支持 Rancher 管理的 Kubernetes 集群。在云提供商上，你还需使用能获得最大 IOPS 的最小大小。在较大的集群中，请考虑使用专用存储设备存储 etcd 数据和 wal 目录。

# 网络要求

本节描述了安装 Rancher Server 的节点的网络要求。

:::caution

如果包含 Rancher 的服务器带有 `X-Frame-Options=DENY` 标头，在升级旧版 UI 之后，Rancher UI 中的某些页面可能无法渲染。这是因为某些旧版页面在新 UI 中是以 iFrames 模式嵌入的。

:::

### 节点 IP 地址

无论你是在单个节点还是高可用集群上安装 Rancher，每个节点都应配置一个静态 IP。如果使用 DHCP，则每个节点都应该有一个 DHCP 预留，以确保节点分配到相同的 IP 地址。

### 端口要求

为了确保能正常运行，Rancher 需要在 Rancher 节点和下游 Kubernetes 集群节点上开放一些端口。不同集群类型的 Rancher 和下游集群的所有必要端口，请参见[端口要求](../getting-started/installation-and-upgrade/installation-requirements/port-requirements.md)。

# Dockershim 支持

有关 Dockershim 支持的详情，请参见[此页面](../getting-started/installation-and-upgrade/installation-requirements/dockershim.md)。
