# 校园招聘经验交流平台

📌**项目简介**
---
> 校园招聘经验交流平台是一套前后端不分离的开源社区系统，基于目前主流 Java Web 技术栈（SpringBoot + MyBatis + MySQL + Redis + Kafka + Elasticsearch + Spring Security），并提供详细的开发文档和配套教程。包含帖子、评论、私信、系统通知、点赞、关注、搜索、用户设置、数据统计等模块。


## 🔍核心技术栈

**后端**：

- Spring
- Spring Boot 2.1.5 RELEASE
- Spring MVC
- ORM：MyBatis
- 数据库：MySQL 5.7
- 分布式缓存：Redis
- 本地缓存：Caffeine
- 消息队列：Kafka 2.13-2.7.0
- 搜索引擎：Elasticsearch 6.4.3
- 安全：Spring Security
- 邮件任务：Spring Mail
- 分布式定时任务：Spring Quartz
- 日志：SLF4J（日志接口） + Logback（日志实现）

**前端**：

- Thymeleaf
- Bootstrap 4.x
- Jquery
- Ajax

## 💻 开发环境

- 操作系统：Windows 11
- 构建工具：Apache Maven
- 集成开发工具：Intellij IDEA
- 应用服务器：Apache Tomcat
- 接口测试工具：Postman
- 压力测试工具：Apache JMeter
- 版本控制工具：Git
- Java 版本：8

## 🧷系统功能逻辑
> 为了便于大家理解，画了每个功能的逻辑图
- **注册**

![image.png](https://s2.loli.net/2023/03/26/kJfFZoS2Tjpt8q7.png)

- **登录、登出**

> - 进入登录界面，后台随机生成字符串标识用户，将字符串短暂的存入cookie中 （60s）
> - 动态获取验证码，将验证码以及标识该用户的字符串短暂的存入到redis  (60s)
> - 用户提交登录信息，当验证通过，进而随机生成登录凭证状态设置为有效（0），并存到redis中，再在cookie中存一份登录凭证。（过期时间根据用户是否**勾选记住我**来判断）
> - 使用拦截器，在所有请求之前，从cookie中获取登录凭证，进而到redis中查询是否有效，有效则持有用户信息（使用了**ThreadLocal**，保证服务器之间的用户登录状态同步）
> - 用户登出，根据id，从redis中取出ticket,根据ticket修改用户激活状态再放回redis即可（修改为1）


![image.png](https://s2.loli.net/2023/03/26/IYsXPiJfb2ntDoR.png)
- **账号设置**


> - 上传用户头像（异步请求）
>
> 用户头像上传到七牛云服务器
>
> - 查看用户头像
> - 修改密码
>
> 通过发送邮件修改密码
>
> - 个人主页展示
> 关注数量、点赞数量、粉丝数量

- **核心功能**

  - 发布帖子
 
> - 在新增帖子的时候就开始计算帖子分数  放到缓存里
> - 过滤敏感词
> - 触发发帖事件,将帖子异步的提交到Elasticsearch服务器
> - 持久化到数据库


![image.png](https://s2.loli.net/2023/03/26/NAHsIiEQ6hWrFw8.png)

  - 分页展示帖子

  > - 支持帖子按发帖时间排序展示
  > - 支持按照热度排行展示，热度根据定时任务计算帖子的分数，进而进行排序
  > - 热帖列表缓存到caffeine
  > - 帖子的总数也放在caffeine里

![image.png](https://s2.loli.net/2023/03/26/mxX2SAJH6W8fPbU.png)

  - 过滤敏感词




 
## 持续更新中
✔️面试题总结

