本文将为您介绍自建的 K8s 如何接入 Prometheus 监控服务。

>?
- 若您使用了腾讯云的容器服务，可无缝衔接 Prometheus 监控服务，控制台直接关联集群，获取监控数据，可参考 [集成容器服务](https://cloud.tencent.com/document/product/1416/72037)。
>2. 若您自建了 K8s 集群，可以借助 [云原生分布式云中心](https://cloud.tencent.com/apply/p/897g10ltlv6) 的集群管理的能力，一键快速接入 Prometheus 监控服务。
>3. 若您自建 K8s 集群，也可以按照开源社区的使用方式，像接入A自建Prometheus一样接入Prometheus 监控服务。

##  前提条件
申请 [云原生分布式云中心](https://cloud.tencent.com/apply/p/897g10ltlv6) 使用权限。


## 操作步骤
### 步骤1：申请注册集群功能

用户完全搭建自己的 k8s 和 prometheus，这个和外部社区是一样的。

1. 登录 [容器服务控制台-注册集群](https://console.cloud.tencent.com/tke2/external)。
2. 在页面中单击**开通服务**。
3. 在跳转页面中填写相关网络配置、开通地域等信息。填写完后单击**完成**即可。
![](https://qcloudimg.tencent-cloud.cn/raw/727183c34bd25ad812942fc6681a4d61.png)
4. 开通成功后会生成 Hub 集群。
![](https://qcloudimg.tencent-cloud.cn/raw/da4e613142f6e57f7b2ecf2f2eb14b1c.png)


### 步骤2：新建注册集群
1. 进入 [容器服务控制台-注册集群](https://console.cloud.tencent.com/tke2/external/create)。
2. 在页面中单击**注册已有集群**。在页面中填写集群名称，选择集群所在地或腾讯云标签。
![](https://qcloudimg.tencent-cloud.cn/raw/ae374964587e577bfa5499ab40d426c6.png)
3. 若显示**待注册状态**则表示，创建成功。
![](https://qcloudimg.tencent-cloud.cn/raw/bb5c5395cde8c7d00eab0756ad7406f1.png)


### 步骤3：自建 Kubernetes 集群接入
<dx-tabs>
::: 步骤1：复制注册命令
1. 进入 [容器服务控制台-注册集群](https://console.cloud.tencent.com/tke2/external/create)。
2. 在操作列中单击 **查看注册命令**，在弹框中选择 **外网访问**。
3. 在外网访问页面复制注册命令。
<img src="https://qcloudimg.tencent-cloud.cn/raw/491fe2943b984cef9d1e4cab89492f14.png" width="80%">
:::
::: 步骤2：新建 agent.yaml 文件
1. 用户本地 K8s 集群存放 yaml 的目录在本地新建 agent.yaml 文件。
![](https://qcloudimg.tencent-cloud.cn/raw/dc09ecab309146ac7b776e6ccce5eeba.png)
2. 把命令粘贴至 agent.yaml。
![](https://qcloudimg.tencent-cloud.cn/raw/574afd0d496e7f77c61cb50a1b35e5ff.png)
:::
::: 步骤3：执行命令创建资源
待注册集群中执行 kubectl apply -f agent.yaml 命令。
![](https://qcloudimg.tencent-cloud.cn/raw/789912b855c1971e8a06f9a095846a11.png)
:::
::: 步骤4：查看是否注册成功
- 通过下面命令可查看本地注册 pod 的状态。示例如下：
```
# kubectl get pod -n clusternet-system
NAME                                READY   STATUS    RESTARTS   AGE
clusternet-agent-78444974d7-f6fsc   1/1     Running   0          7m32s
clusternet-agent-78444974d7-qjp2q   1/1     Running   0          7m32s
clusternet-agent-78444974d7-r575w   1/1     Running   0          7m32s
```
- 控制台查看
进入 [容器服务控制台-注册集群](https://console.cloud.tencent.com/tke2/external/create)。若状态显示为**运行中**则表示注册成功。
![](https://qcloudimg.tencent-cloud.cn/raw/3f2b71094d71e345d88c6dff0f6a63f8.png)
:::
</dx-tabs>


### 步骤4：Prometheus 实例关联注册集群
1. 进入 [Prometheus控制台](https://console.cloud.tencent.com/monitor/prometheus)。
2. 选择您需要对应的 Prometheus 实例，单击其实例名称。
3. 进入实例管理页，单击 **集成容器服务** > **集群监控**。关联对应的注册集群。完成后单击 **确认** 即可。
![](https://qcloudimg.tencent-cloud.cn/raw/28a58589a491f0cad8dd3c4ae521a376.png)


### 步骤5：验证接入是否成功
1. 进入 [Prometheus控制台](https://console.cloud.tencent.com/monitor/prometheus)。
2. 选择您需要对应的 Prometheus 实例，单击其实例名称。
3. 进入实例管理页，单击 **集成容器服务** > **集群监控**。
4. 在集群监控列表中，单击操作列的 **数据采集配置**。
![](https://qcloudimg.tencent-cloud.cn/raw/4f67513963a095a6900d33cce94388a3.png)
5. 若如下图所示，显示指标详情，则表示接入成功。
![](https://qcloudimg.tencent-cloud.cn/raw/9aeac0d9b24aed3f8b2c67ab6cb98892.png)

### 步骤6：查看监控数据
1. 进入 [Prometheus控制台](https://console.cloud.tencent.com/monitor/prometheus)。
2. 选择您需要对应的 Prometheus 实例，单击实例名称右侧的 **Grafana**。
3. 在 Grafana 登录界面输入账号密码，进入 Grafana 管理后台。
4. 在左侧菜单栏中选择 **manage > tps 文件夹**，再根据自己需要查看对应大盘。
![](https://qcloudimg.tencent-cloud.cn/raw/d2a1106648b613f529a4a64c3dc84c80.png)

下列以选择 resource cluster 大盘数据为例，模板变量 cluster 选择对应集群（集群 ID）即可查看监控数据。
![](https://qcloudimg.tencent-cloud.cn/raw/199dc62024a2e92d076009d04e9b6ad8.png)

