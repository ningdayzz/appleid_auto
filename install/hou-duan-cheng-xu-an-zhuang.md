# 后端程序安装

## 注意事项

### 环境需求

后端运行基于Docker，安装脚本会自动检测Docker是否安装。

### Selenium

后端执行任务需要调用Webdriver，有两种部署方式：单机节点和集群。

#### 单机节点（standalone）

请根据需求修改参数

```bash
docker run -d --name=webdriver --log-opt max-size=1m --log-opt max-file=1 --shm-size="2g" --restart=always -e SE_NODE_MAX_SESSIONS=10 -e SE_NODE_OVERRIDE_MAX_SESSIONS=true -e SE_SESSION_RETRY_INTERVAL=1 -e SE_VNC_VIEW_ONLY=1 -p 4444:4444 -p 5900:5900 selenium/standalone-chrome
```

其中：\
SE\_VNC\_VIEW\_ONLY=1  表示VNC仅允许查看，不允许操作。\
SE\_NODE\_MAX\_SESSIONS=10  表示最多允许10个会话数。\
4444端口用于执行任务和面板，5900端口用于VNC。

VNC默认密码secret

如需在ARM设备上使用，请参阅[seleniarm/standalone-chromium](https://hub.docker.com/r/seleniarm/standalone-chromium)

#### 集群（grid）

Selenium Grid需要一个中心控制器（Hub），并允许在多台服务器上部署节点（Node）。Hub收到请求后会自动分配Node，实现负载均衡，多IP访问等功能。

快速部署脚本请参考[sahuidhsu/selenium-grid-docker](https://github.com/sahuidhsu/selenium-grid-docker) (这个脚本提供x86\_64和arm部署支持)

## 一键部署后端

```bash
bash <(curl -Ls https://raw.githubusercontent.com/pplulee/appleid_auto/backend/backend/install_unblocker.sh)
```

安装时按照提示输入参数即可。\
默认会以**appleauto**为容器名部署一个Docker容器。\
部署完成后可通过**docker logs appleauto**查看管理容器日志。\
通过**docker ps | grep apple-auto | awk '{print $1}' | xargs docker logs $1**查看后端容器日志。





