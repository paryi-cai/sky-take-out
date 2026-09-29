# 苍穹外卖 Sky Take-Out

基于黑马程序员《苍穹外卖》课程的学习项目，一个前后端分离的外卖点餐系统，包含**管理端（Vue）**和**用户端（微信小程序）**。

## 技术栈

- **后端**：Spring Boot 2.7.3、Spring MVC、MyBatis + PageHelper、MySQL、Redis、Druid
- **常用组件**：JWT 登录认证、阿里云 OSS 文件上传、Knife4j 接口文档、Spring Task 定时任务、WebSocket、Spring Cache、Apache POI、微信支付 APIv3
- **前端**：Vue 2 + Element UI（管理端）、微信小程序（用户端）、ECharts（数据统计图表）
- **工具/环境**：JDK 17、Maven 3.9、Nginx

## 模块结构

```
sky-take-out
├── sky-common    # 公共模块：常量、异常、工具类、配置属性
├── sky-pojo      # 实体模块：entity、DTO、VO
└── sky-server    # 服务端模块：controller、service、mapper、config、task、websocket
```

## 功能清单

### 管理端

- **员工管理**：登录（JWT + MD5）、分页查询、新增/编辑、启用禁用
- **分类管理**：增删改查、起售停售
- **菜品管理**：新增（含口味）、分页查询、修改、批量删除、起售停售，AOP 自动填充公共字段
- **套餐管理**：新增/修改/删除、起售停售，Spring Cache 缓存套餐
- **店铺营业状态**：Redis 存取营业状态
- **订单管理**：条件搜索、状态统计、接单/拒单/取消/派送/完成
- **数据统计**：营业额、用户、订单、销量 Top10（ECharts 图表）
- **导出运营数据报表**：Apache POI 按模板生成 Excel
- **来单提醒**：用户支付成功后 WebSocket 推送消息

### 用户端（微信小程序）

- 微信登录、店铺营业状态
- 分类、菜品、套餐浏览（Redis 缓存菜品）
- 地址簿管理
- 购物车：添加、减一、查看、清空
- 下单、订单支付（微信支付 JSAPI）、历史订单、订单详情、取消订单、再来一单、客户催单
- 超时未支付订单自动取消、派送中订单自动完成（Spring Task）

## 本地运行

### 1. 环境准备

- JDK 17、Maven 3.9、MySQL 8、Redis
- 可选：Nginx（托管管理端前端）、微信开发者工具（运行小程序）

### 2. 数据库

新建数据库 `sky_take_out`，导入课程提供的 SQL 脚本（脚本不在本仓库中）。

### 3. 配置

`application-dev.yml` 中只保留占位符，真实密钥放在本地 `application-local.yml`（已在 `.gitignore` 中忽略，不会提交）：

```yaml
# sky-server/src/main/resources/application-local.yml
sky:
  alioss:
    access-key-id: 你的AccessKeyId
    access-key-secret: 你的AccessKeySecret
  wechat:
    secret: 你的小程序Secret
```

`application.yml` 中激活的是 `dev,local` 两个 profile，本地配置会覆盖 dev 配置；没有 `application-local.yml` 时，OSS 上传、微信登录等功能不可用。

### 4. 启动

运行 `SkyApplication`（端口 8080），接口文档地址：

```
http://localhost:8080/doc.html
```

## 接口文档

使用 Knife4j 生成，启动后访问 `/doc.html`，包含「管理端接口」和「用户端接口」两个分组。

## 说明

- 本项目仅用于学习，微信支付、百度地图配送范围校验等功能需要自备商户号、证书和 AK；
- 管理端与用户端前端代码来自课程资料，本仓库主要记录后端功能实现。
