---
title: Centos7的yum换源
categories: 
    - [环境搭建]
single_column: true
cover: https://pic-bed-6vd.pages.dev/img/fm3.webp
banner:
  type: img
  bgurl: https://pic-bed-6vd.pages.dev/img/bg6.webp
  banner_text: 
---
由于Centos7在2024年06月30日停止维护，所以最初的yum源已经失效，需要更换为国内的yum源。
以下为个人总结后的步骤：
1. 进入yum源目录
```shell
cd /etc/yum.repos.d/
```
2. 备份原有的yum源(将CentOS-Base.repo重命名为CentOS-Base.repo.bak)
```shell
mv CentOS-Base.repo CentOS-Base.repo.bak
```
3. 下载国内的yum源(这里的为阿里云源)
在下载前先确定网络能够正常访问，否则会下载失败。
网络有关内容可以参考[网络配置](AboutCentosNetwork.md)
```shell
wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
# 或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
4. 清除yum缓存
```shell
yum clean all
```
5. 生成新的yum缓存
```shell
yum makecache
```
6. 查看yum源是否更换成功
```shell
yum repolist
```

<script src="https://giscus.app/client.js"
        data-repo="gdjide/TheDiscussions"
        data-repo-id="R_kgDONP3u_g"
        data-category="Announcements"
        data-category-id="DIC_kwDONP3u_s4CkTPz"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme="transparent_dark"
        data-lang="zh-CN"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>