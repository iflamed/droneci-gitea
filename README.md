# DroneCI-Gitea devops 基础服务
使用 docker-compose.yml 文件部署 DroneCI 和 Gitea 服务

## Docker 环境变量说明
部署前请先创建 `.env` 文件，可复制示例文件:
```shell
cp .env.example .env
```

创建 `.env` 文件之后，并根据各参数作用进行修改，参数说明如下:

```shell
# 时区配置
TIMEZONE=Asia/Shanghai

# Mysql 数据库服务器IP
DB_HOST=127.0.0.1
# Mysql 数据库服务器端口
DB_PORT=3306
# Gitea 数据库名称
GITEA_DB_NAME=database
# Gitea 数据库登录账户名
GITEA_DB_USER=username
# Gitea 数据库登录密码
GITEA_DB_PASSWD=password
# Gitea 创建应用 Drone 的 OAuth client_id
GITEA_CLIENT_ID=clientuuid
# Gitea 创建应用 Drone 的 OAuth client_secret
GITEA_CLIENT_SECRET=clientSecret
# Gitea 服务器访问地址，例如: https://gitea.com, https://git.example.com:2222
GITEA_SERVER=https://gitea.com
# Gitea 服务器主机中的 git 用户的UID
GITEA_USER_UID=1000
# Gitea 服务器主机中的 git 用户的GID
GITEA_USER_GID=1000

# Drone 服务的地址，域名:端口 or ip:port
DRONE_SERVER_HOST=drone.example.com
# Drone 服务的访问协议，https or http
DRONE_SERVER_PROTO=https
# Drone server 和 Drone runner 的 rpc 通信密钥
DRONE_RPC_SECRET=drone_rpc_secret
# Drone 服务的默认用户
DRONE_USER_ADMIN=drone_admin_username
# Drone runner 同时可运行的最大任务数量
DRONE_RUNNER_CAPACITY=2
```

## Gitea 部署说明
使用此服务需要先部署 `Gitea` 服务，然后部署 `DroneCI` 服务。 `Gitea` 分两种情况部署，分别有所不同如下：

1. 开发者机器部署本地 `Gitea` 服务，建议使用 `docker-compose-gitea-local.yml` 文件，通过 `docker compose up -d` 启动。启动之前，需要修改 `.env` 文件中，DB 与 Gitea的配置。
2. 服务器上单机部署 `Gitea` 远程服务，建议使用 `docker-compose-gitea-server.yml` 文件，通过 `docker compose up -d` 启动。启动之前，需要修改 `.env` 文件中，DB 与 Gitea的配置。注意 `Gitea` 中 `git` 用户的 `uid` 与 `gid` 应该与 `host` 机器上的一致。
3. 如需要通过 `ssh` 方式 `clone` `gitea` 中仓库，需要参考配置: <https://docs.gitea.io/en-us/install-with-docker/#ssh-container-passthrough>

## DroneCI 部署说明
部署DroneCI 比较简单，只需要使用 `docker-compose.yml` 文件，通过命令 `docker compose up -d` 启动即可。启动之前你可能需要做以下配置：

1. 先在 `Gitea` 服务你的账号下，创建一个 `OAuth` 应用，OAuth 回调地址一般为 `https://drone.example.com/login`;
2. 修改 `.env` 文件中 `Drone` 相关的配置;
3. 默认 `Drone` 的数据库是 `sqlite`，你可以根据文档 <https://docs.drone.io/server/storage/database/> 描述修改数据库配置。
