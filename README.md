# MySQL

[MySQL](https://MySQL.org) 是最流行的关系型数据库管理系统，在 WEB 应用方面 MySQL 是最好的关系数据库管理系统应用软件之一。

## 简介

此模板使用 [Helm](https://helm.sh) 工具以单节点的MySQL部署在 [Kubernetes](http://kubernetes.io) 集群中。

## 先决条件

- Kubernetes 1.10+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## 部署应用

```bash
  helm install --name my-release stable/mysql
```

该命令以默认配置在Kubernetes集群上部署MySQL。参数配置部分列出了可以在安装期间配置的参数。
默认情况下，将为root用户生成一个随机密码。如果您想设置自己的密码，请设置mysqlRootPassword参数值。
您可以通过运行以下命令来获取您的root密码。注意替换[YOUR_RELEASE_NAME]：
	printf $(printf '\%o' `kubectl get secret [YOUR_RELEASE_NAME]-mysql -o jsonpath="{.data.mysql-root-password[*]}"`)

## 卸载/删除my-release

卸载命令:

```bash
  helm delete --purge my-release
```

## 参数配置

下表列出了MySQL模板的可配置参数及其默认值。

| 参数                                         | 描述                                                                                         | 默认值                                               |
| -------------------------------------------- | -------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| `image`                                      | `mysql` 镜像仓库                                                                             | `mysql`                                              |
| `imageTag`                                   | `mysql` 镜像版本                                                                             | `5.7.30`                                             |
| `mysqlRootPassword`                          | `root` 用户密码                                                                              | Random 10 characters                                 |
| `mysqlUser`                                  | 要创建的新用户的用户名                                                                       | `nil`                                                |
| `mysqlPassword`                              | 新用户的密码                                                                                 | Random 10 characters                                 |
| `mysqlDatabase`                              | 要创建的新数据库的名称                                                                       | `nil`                                                |
| `service.type`                               | Kubernetes 网络服务的类型                                                                    | ClusterIP                                            |
| `timezone`                                   | 容器和mysqld时区                                                                             | `nil` (UTC depending on image)                       |

上面的某些参数映射到 [MySQL DockerHub image](https://hub.docker.com/_/mysql/) 映像中定义的env变量。

helm install的时候使用--set key=value[,key=value]参数指定每个参数。例如，

```bash
  helm install --name my-release \
  --set mysqlRootPassword=secretpassword,mysqlUser=my-user,mysqlPassword=my-password,mysqlDatabase=my-database \
    stable/mysql
```

上面的命令将MySQLroot帐户密码设置为secretpassword。此外，它还会创建一个标准的数据库用户my-user，其密码为my-password，可以访问名为的数据库，该用户的密码为my-database。

或者，可以在部署chart时提供指定参数值的YAML文件。例如，

```bash
  helm install --name my-release -f values.yaml stable/mysql
```