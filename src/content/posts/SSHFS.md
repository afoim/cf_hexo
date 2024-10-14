---
title: 使用SSHFS将Linux目录挂载为Windows磁盘
published: 2024-10-12
description: 'SSHFS拥有图形界面，可以通过SFTP协议将Linux目录映射为一个本地磁盘，便于管理文件'
image: './assets/images/QmZZkpG3gkkQjGExXvG9vbuAJyBfHGWEFH2Bk8YzhSSqwx.webp'
tags: [SSHFS]
category: '实用工具'
draft: false 
lang: ''
---

### 有一款更好用的软件，支持多达42种服务或协议：[RaiDrive](https://onani.cn/RaiDrive)😋

---

### 优点

可以像管理Windows文件一样管理Linux文件，不需要敲代码

---

### 正式开始

1. 下载并安装下面的软件

https://github.com/billziss-gh/sshfs-win

https://github.com/billziss-gh/winfsp

https://github.com/evsar3/sshfs-win-manager

2. 打开SSHFS-Win Manager，添加你的SSH服务器（我这里使用私钥连接）
   ![image](https://ipfs.crossbell.io/ipfs/QmccEuSGovyVuJeTRYEUKNr2XSPpKTeoqq82rZYMyhazh2?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

3. 点击连接即可将Linux目录挂载到本地磁盘
   ![image](https://ipfs.crossbell.io/ipfs/QmQHZCeYbQ3GmjWDqWqEpdXWYnQy6KHnep5m6bu6X3eCNH?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

4. 效果
   ![image](https://ipfs.crossbell.io/ipfs/QmNjTfv6XncV2dVddt4k6E9XN6tjuXf5rmXPhh2ggcemkU?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)
   ![image](https://ipfs.crossbell.io/ipfs/QmWMQHNpJUUPg9B1Hdw2zmwLx9q6bcS52nUFiB3P9iYvU9?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

### 疑难解答

如果无法连接，请打开设置的Debug Panel，根据报错着手解决
![image](https://ipfs.crossbell.io/ipfs/QmWW58e68YLKFoKbZG63CwWVqgxkGfDiCJrM42Hj9hcN6M?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)
