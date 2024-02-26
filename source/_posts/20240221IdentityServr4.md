---
title: identity Server4基础入门
date: 2023-02-21 11:21:05
categories: CSharp
---
## 客户端应用（需要得到接口的一方）
## 受保护资源
1.资源服务器---api接口
2.授权服务器---identity data（身份数据）
因为token当中有用户的身份信息 比如邮箱等等
## 访问令牌
访问令牌可以参考jwttoken 短期 可撤销 具有一定范围性的
## 刷新令牌
等于就是获取访问令牌 但是可以随时控制权限 因为等于是给一个新的权限
## 范围
这就好比vue项目要访问后台接口 最开始是全部开放的 但是有一天可以设置不开放或者只开放部分接口给vue项目访问。
## JWT和OAuth2.0的区别
首先jwt是具体的包含逻辑的，token实现框架 oAuth2.0是规范不是实现
## Clients、IdentityResources 和 ApiResources 是三个核心概念，
它们用于定义身份认证和授权服务的配置信息。以下是它们的简要解释：
### 1.Clients:

Clients 表示需要通过 IdentityServer 进行认证的客户端应用程序。每个客户端都有一组属性，包括标识符、密钥、访问令牌过期时间等。客户端可以是 Web 应用程序、移动应用程序、单页应用程序等。
示例配置：
```csharp
Clients = new List<Client>
{
    new Client
    {
        ClientId = "client_id",
        ClientSecrets = { new Secret("client_secret".Sha256()) },
        AllowedGrantTypes = GrantTypes.ClientCredentials,
        AllowedScopes = { "api1" }
    }
};
```
### 2.IdentityResources:

IdentityResources 表示要包含在身份令牌中的用户身份信息。这些信息可以是用户的唯一标识符、姓名、电子邮件地址等。当客户端应用程序请求身份令牌时，这些信息将包含在令牌中。
示例配置：
```csharp
IdentityResources = new List<IdentityResource>
{
    new IdentityResources.OpenId(),
    new IdentityResources.Profile(),
};
```
### 3.ApiResources:

ApiResources 表示受保护的资源，可以是 Web API 或其他服务。配置这些资源允许客户端请求访问特定资源。每个 ApiResource 包括标识符、显示名称、范围等信息。
示例配置：
```csharp
ApiResources = new List<ApiResource>
{
    new ApiResource("api1", "My API"),
};
```
## 快速搭建identity Server4基础项目
[文档链接](https://identityserver4docs.readthedocs.io/zh-cn/latest/quickstarts/3_aspnetcore_and_apis.html)

### step1 安装id4模板
``` bash
dotnet new -i IdentityServer4.Templates
```
### step2 创建id4模板
``` bash
dotnet new is4empty -n IdentityServer
```
### step 创建web api项目 引入identityServer4包
1.注册IdentityServer服务
2.
