---
title: erp项目的部署
categories: 
    - [学习备忘笔记]
single_column: true
cover: https://pic-bed-6vd.pages.dev/img/bg15.webp
banner:
  type: img
  bgurl: https://pic-bed-6vd.pages.dev/img/bg2.webp
  banner_text: 
---

# ERP系统部署流程

## 前言
下载完安装包后进行解压。

## 一、后端编译

将文件夹中的 `jshERP-boot` 文件夹在 IDEA 中打开，运行 Maven 的生存期的 `package`。
![erp](https://pic-bed-6vd.pages.dev/img/erp1.png)

### 常见问题及解决办法：

1. **Lombok 与 Java 版本不符**
   - 解决办法：该程序自带 Java 版本为 22，升级 Lombok 版本，将版本改为 1.18.30 后 Maven 重新同步项目即可。

2. **Maven 插件或依赖项等爆红**
   - 解决办法：网络问题，Maven 设置里更改 Maven 主路径进行重新下载，下载后再改回 IDEA 自带版本（或者自己下载的版本）。

### 编译结果
后端编译后会在根目录下生成 `dist` 文件夹，文件夹下有 `jshERP-bin.zip` 压缩包。

## 二、前端编译

将文件夹中的 `jshERP-web` 文件夹在 VSCode（推荐）中打开，如果根目录下无 `node_modules` 文件夹，那么说明没有安装依赖。

### 环境准备
下载 Node.js，版本为 **16.20.2**（版本太高无法运行）。

### 依赖安装

1. **使用淘宝镜像安装**（推荐）
   - 一次性使用：
     ```bash
     npm install --registry=https://registry.npmmirror.com
     ```
   - 永久使用淘宝镜像：
     ```bash
     npm config set registry https://registry.npmmirror.com
     ```

2. **处理依赖版本冲突**
   - ERP（jsh-erp-web）的 `package.json` 中依赖版本比较旧，即使换了 Node16，npm 默认的严格检查机制依然会拦截 `css-loader` 和 `webpack` 的版本冲突。
   - 使用以下命令即可正常安装：
     ```bash
     npm install --legacy-peer-deps
     ```

3. **构建项目**
   - 在 `package.json` 下选中 `build`，如有依赖会正常显示运行脚本，点击运行。

### 编译结果
运行成功之后同样会在根目录下创建 `dist` 文件夹，将该文件夹压缩为 `dist.zip`。

## 三、整合前后端文件

1. 创建新文件夹 `jshERP`
2. 将两个压缩文件（`jshERP-bin.zip` 和 `dist.zip`）移动到该文件夹内
3. 再将 `jshERP` 文件夹压缩为 `jshERP.zip`
4. 后续会使用到

## 四、环境搭建（Linux 系统）

自己使用可以使用 VMware 虚拟机创建 Linux 系统，这里演示使用购买的服务器进行搭建，步骤都一样。

### 安装宝塔面板

下载宝塔面板（9.6.0 版本）（便于环境的搭建）。

- 在终端中粘贴执行（确保服务器或虚拟机网络正常）
- 输入 `y` 确认安装

#### 可能问题
- 使用的用户并非管理员用户，可以在命令前加入 `sudo`（将普通用户使用超级管理员权限）。

安装时间较长，安装完成后会显示面板账户登录信息（一定记住密码，虽然后续可更改）。

- 服务器使用 IPv4 面板地址访问（需要在服务器中放行端口号）
- 虚拟机使用内网面板访问

进入宝塔面板登录，首次登录没有宝塔账户可注册。

### 下载环境组件

只需下载 Nginx 与 MySQL，其他用不到，点击一键安装。

- 点击软件商店，下载 Redis
- 点击网站，选择上方的 JAVA 项目，在 Java 环境管理中选择 JDK 管理，选择安装 1.8 版本的 JDK
- 点击文件，将先前的 `jshERP.zip` 压缩包上传到宝塔面板，然后解压
- 再将文件夹内的 `jshERP-bin.zip` 和 `dist.zip` 进行解压

### 正式的环境搭建

#### 1. 添加数据库
点击数据库，点击添加数据库，输入数据库名之后点击确定。

##### 常见问题
会显示：`127.0.0.1 状态：root 用户连接失败，请尝试重置数据库 root 密码后重新添加。`
- 该问题是由于第一次添加数据库，只需点击添加数据库旁的「root 密码」，再重新添加数据库即可。

#### 2. 导入数据库
点击导入，点击从本机导入，选择解压出来的数据库文件，路径一般为 `……/jshERP/docs/jsh_erp.sql`。

#### 3. 修改配置文件
复制刚才的数据库密码，然后进入 `/jshERP/config/application.properties` 文件，修改数据库地址以及账户密码。

#### 4. 复制 Java 路径
进入 `/jshERP/bin/run-manage.sh` 文件，修改 Java 地址。

#### 5. 启动后端
进入 `/jshERP` 根目录，点击终端，输入命令 `./start.sh` 启动，显示 `success` 启动成功。

#### 6. 启动 Web 端
- 点击网站，选择上方的 HTML 项目，点击添加 HTML 项目，在绑定域名中输入服务器 ip 地址即可，根目录选择之前的 `dist`。
- 继续点击后方的设置按钮，点击配置文件。
- 在该处输入内容，将需要服务端处理的请求转发到服务端：
  ```nginx
  location /jshERP-boot/ {
        proxy_pass http://localhost:9999/jshERP-boot/;
        proxy_set_header Host $host:$server_port;
      }
  ```

#### 7. 打开网站
复制地址即可进入。

**测试用户：**
- 用户名：`jsh`，密码：`123456`
- 管理员：`admin`，密码：`123456`

---

## 关机后重新启动 ERP 系统流程

管理员用户可直接使用命令，否则在使用命令前加入 `sudo`。

1. 使用命令 `bt`
2. 显示宝塔面板，再次输入 `14`，查看宝塔面板的地址与用户名（密码不可见），可自行更改密码。
3. 进入宝塔面板
4. 在文件中 `jshERP` 终端输入启动命令 `./start.sh` 启动
5. 进入网站

### 可能问题
如果输入关闭命令 `./stop.sh` 可能会导致之前的配置信息重置，只需重新填写即可（数据库地址、Java 路径）。
