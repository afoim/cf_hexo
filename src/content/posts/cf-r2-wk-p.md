---
title: Cloudflare R2+Workers！马上搭建自己的云上图床！
published: 2024-10-14
description: '使用R2存储图片，通过Workers连接，最后使用a标签或img标签在网页中嵌入展示，全链路上云'
image: './assets/images/QmVgqgoC7G8NLS21WvR8j9gf5amu33XvuV68ZrgM5B9iFf.webp'
tags: [Cloudflare, Cloudflare R2, Cloudflare Workers]
category: '开发'
draft: false 
lang: ''
---

### **结果图**

![a8d420168e3aff48d16074b9d9570b50_720](https://ipfs.crossbell.io/ipfs/QmVgqgoC7G8NLS21WvR8j9gf5amu33XvuV68ZrgM5B9iFf?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

### **原理**

图源由 Cloudflare R2 托管，通过两个 Workers 连接 R2 以展示随机横屏/竖屏图片，静态页面引用 Workers 的 URL 以实现以上界面

### **创建 Cloudflare R2 存储桶**

R2 实际上是一个对象存储。Cloudflare 提供 10G 的免费存储和每月 1000 万次的免费访问

1. 进入[Cloudflare 仪表盘](https://dash.cloudflare.com/)，进入 R2 页面，如图![image](https://ipfs.crossbell.io/ipfs/QmU7u2JHUcevyHnwsCdAZfs7X7Fcdh3KJhn6eoy24Q5dGC?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

2. 选择创建存储桶![image](https://ipfs.crossbell.io/ipfs/QmX3eCaCVEgE8AN29D9t2VpQ5t5SrZGKb8EcZv9oKpCqf2?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

3. 为你的存储桶起一个名字，然后单击创建![image](https://ipfs.crossbell.io/ipfs/QmVad5eoJCLpSNZ4HCvTPJfD8rpg4aePMzZ7j2DZATn1XD?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

4. 进入如下页面就已经创建完毕了![image](https://ipfs.crossbell.io/ipfs/QmSdzwBJpw2L4a8LJ3eM3VMJs3d5oV5iFCxCMtv69VZmYH?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

5. 返回 R2 首页。因为在下文我们需要使用 API 来进行文件传输，所以需要创建你的 R2 API 令牌，单击管理 R2 API 令牌![image](https://ipfs.crossbell.io/ipfs/QmbS8zjJTESwsmycKBSC9kmabAA9dtSCUX8nbUDWg4BWRX?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

6. 单击创建 API 令牌，如图![image](https://ipfs.crossbell.io/ipfs/QmPzJEHVAm4z3S1SHY4k99TugrPyTB9DXpyRR8Loj22bz3?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

7. 因为我们需要该 API 来管理单个 R2 存储桶，所以选择**对象读和写**，详细配置如图![image](https://ipfs.crossbell.io/ipfs/QmNY9p8hksi18B9R8TVfdGgu336oQ3cPmghyfYXE9CDGD4?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

8. 创建 API 令牌后，新页面会展示令牌的详细信息，**仅会展示一次！！！** 保持这个页面，直到你将该页面的所有信息都已经妥善保存，不要关闭界面，否则，你需要轮转 API 令牌以禁用之前的旧密钥，如图![image](https://ipfs.crossbell.io/ipfs/QmZTUwbycqbJhVP6PatD3psYy7ej9PDDoiXbmDWoakPhwx?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

9. 确保你已经妥善保存你的 R2 API 令牌，然后进行下一步

### **为你的存储桶添加文件**

因为 Web 界面传输文件较慢且不支持传输大于 300MB 的文件。这里使用本地部署 AList 然后连接你的 R2 存储桶实现高速上传

1. 笔者使用 Windows。前往[AList - Github Release](https://github.com/alist-org/alist/releases)下载适用于 Windows 的最新可执行文件，如图![image](https://ipfs.crossbell.io/ipfs/QmPDRDJGeGStreyZMXVYofbE9FCs1T1MyDek3KUbB3Kk5b?img-quality=75&img-format=auto&img-onerror=redirect&img-width=640)

2. 将下载的压缩包解压，并将其中的`alist.exe`放入一个空文件夹

3. 单击搜索框，输入 cmd 并回车，如图![image](https://ipfs.crossbell.io/ipfs/QmSt8aFtaeEprJHASEiNPB67UHcHoSxsbhhHUPxW6QkWSo?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)![Snipaste_2024-08-27_05-03-46](https://ipfs.crossbell.io/ipfs/QmNkMhDhpPLkYCpVhE1ov7Q6A34uWDvraCqNvuTqaCkujT?img-quality=75&img-format=auto&img-onerror=redirect&img-width=2048)

4. 在 cmd 中输入`alist.exe server`并且不要关闭窗口，运行成功后如图![image](https://ipfs.crossbell.io/ipfs/QmdzyY8xbic8jdnZEXegefoZPeizqHa4ZkdMnRKoguBMkf?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

5. 打开浏览器，输入`localhost:5244`即可进入 AList 控制台，如图![image](https://ipfs.crossbell.io/ipfs/QmUBFKu7mCiRneCrsTNPxTH6S4gxwtXf9cwLzf4dKW9LLR?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

6. 用户名：`admin`密码：`在cmd窗口中，如图`。你可以使用鼠标左键在终端中框选内容然后单击鼠标右键进行复制操作![image](https://ipfs.crossbell.io/ipfs/QmVH3qZYo3QE6anNHymwkikq5MSeJphrZNR7RCH5jpP3wn?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

7. 注意，在 cmd 中，鼠标左键点击或拖动 cmd 的终端界面会导致进入选择状态，程序将会被系统阻塞，**需要在终端界面点按鼠标右键解除**。若进程被阻塞，cmd 的进程名会多一个**选择**，请注意。如图是程序被阻塞的例子，**在终端界面点按鼠标右键即可解除**
   ![image](ipfs://QmSNuHgfgdBTRLNNDWM8sD6W4q6c9HeMGw7x6GtP2iD15L)

8. 现在，你已经成功以管理员身份登入了 AList![image](https://ipfs.crossbell.io/ipfs/QmXKoURarhZ2hHSn3KHobRP46RuMx1h7eXh3zxJcDGuMoN?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

9. 单击最下面的**管理**![image](https://ipfs.crossbell.io/ipfs/QmfNE53GThdjVrh4q64MJcZqwcGPD7UtcYTNw9bVBaSEaF?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

10. 你会进入到如图界面。尽管 AList 运行在本地，也建议更改你的用户名和密码![image](https://ipfs.crossbell.io/ipfs/QmNdD8UU8fkVDBz5dXdJhCF2fZg8P1FwrcMaaTsG6a7ENy?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

11. 更改账密，重新以新账密登录![image](https://ipfs.crossbell.io/ipfs/Qmas7pMiPR2FNTXheBT1xGNUpzDiSzv7J7yd6oCuT17yad?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

12. 进入控制台，然后单击存储，如图![image](https://ipfs.crossbell.io/ipfs/QmS4gGyCM1j3RXgHEPuZ1zTbLAvGtVBEiPXJe9QMF3dD2D?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

13. 选择添加，如图![image](https://ipfs.crossbell.io/ipfs/QmRDVxt8WbrVkHavgFNXj3qC86ysw6sSZhPy3Uf2ixKp2E?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

14. 详细配置如图。挂载路径即 AList 展示路径，推荐使用`/R2/你的存储桶名字`，地区为`auto`，根文件夹路径为`/`（图上填反了 Orz）![image](https://ipfs.crossbell.io/ipfs/Qme78Vbc1xBynKCYGQjf6xGX455ze5nrfppzXEtYQjwW2K?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

15. 回到主页，如图![image](https://ipfs.crossbell.io/ipfs/QmSnR9Ptrssx4nqk9qCvhFUNKQyQqJiN7GRscwoj4Dczgj?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

16. 尝试上传文件，如图![image](https://ipfs.crossbell.io/ipfs/QmPqFsmZNNnh4jNyLS7X3h8Zr6ZCVqTqGVwTxmPDdbmrGW?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

17. 可以看到，速度非常快![image](https://ipfs.crossbell.io/ipfs/QmXfGK6aZjz741GrY8RfFfKMkUzDMB3xhx93PGZ9S1QycT?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

18. 为你的图床创建目录以分类横屏和竖屏图等，以便下文使用 Workers 连接 R2 来调用。后文我将使用`/ri/h`作为横屏随机图目录、`/ri/v`作为竖屏随机图目录

### **创建 Workers，连接 R2**

1. 进入[Cloudflare 仪表盘](https://dash.cloudflare.com/)，进入 Workers 和 Pages 页面，如图![image](https://ipfs.crossbell.io/ipfs/QmW5UaUap8T2R37u5dzmKGLmUgk4qKnSMFwHBVHqvVbkVA?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

2. 单击创建，选择创建 Workers，名称自取，单击部署![image](https://ipfs.crossbell.io/ipfs/QmVvLv5n41QQfDfYiVWYRpsfw7TVNGy1BYuv5e8vBRhKLA?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

3. 选择编辑代码![image](https://ipfs.crossbell.io/ipfs/QmTbRifzXQ593DGyjFQMbA9exyNp2iAeAg4zbVrfFimQc4?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

4. 粘贴代码（创建随机横屏图）：

```
export default {
  async fetch(request, env, ctx) {
    // R2 bucket 配置
    const bucket = env.MY_BUCKET;

    try {
      // 列出 /ri/h 目录下的所有对象
      const objects = await bucket.list({ prefix: 'ri/h/' });

      // 从列表中随机选择一个对象
      const items = objects.objects;
      if (items.length === 0) {
        return new Response('No images found', { status: 404 });
      }
      const randomItem = items[Math.floor(Math.random() * items.length)];

      // 获取选中对象
      const object = await bucket.get(randomItem.key);

      if (!object) {
        return new Response('Image not found', { status: 404 });
      }

      // 设置适当的 Content-Type
      const headers = new Headers();
      headers.set('Content-Type', object.httpMetadata.contentType || 'image/jpeg');

      // 返回图片内容
      return new Response(object.body, { headers });
    } catch (error) {
      console.error('Error:', error);
      return new Response('Internal Server Error', { status: 500 });
    }
  },
};
```

5. 点击左侧的文件图标![image](https://ipfs.crossbell.io/ipfs/QmQGQTiTXSESU2TSJ6tc3KrzWU4KABKqn6QZ1GdWqKnWmc?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

6. 在`wrangler.toml`中填入：

```
[[r2_buckets]]
binding = "MY_BUCKET"
bucket_name = "114514"
```

7. 保存修改，点击右上角的部署![image](https://ipfs.crossbell.io/ipfs/QmP7hXdtenrJrzJRRePHQATGtyAsZEr5MkMsboXvmNUxTx?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1080)

8. 在设置 - 变量找到 R2 存储桶绑定，添加你的存储桶，变量名即上文的`MY_BUCKET`![image](https://ipfs.crossbell.io/ipfs/QmStitSyATnA8sY9tTgZaXXqmqkGPUtZmMxn9KjbFQzgTc?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

9. 在设置 - 触发器添加你的自定义域名以便访问![image](https://ipfs.crossbell.io/ipfs/QmUMxtkCiKsgFw8afRUGREFztXE9D5W6FmCbAUB7DaVH5o?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)![image](https://ipfs.crossbell.io/ipfs/QmPF9iCoq6n8Jj2Z6kPkdJSCm45VJystZoYcir55yceCQo?img-quality=75&img-format=auto&img-onerror=redirect&img-width=2048)

10. 访问效果，每次刷新都不一样![Snipaste_2024-08-27_06-12-53](https://ipfs.crossbell.io/ipfs/QmQgEdjXxF9oph2jYKzFMJToX9WfG11jUmPiNJnjhYVN4N?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)![Snipaste_2024-08-27_06-12-43](https://ipfs.crossbell.io/ipfs/QmbjTj3CXhrwpbJitw6nR21jxXG9BeoKWvPv7YQy8sCE38?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

### **通过使用 HTML 的 <img> 标签引用即可达到开头的效果**

如：`<img src="你的域名" alt="">`
<img src="https://hrandom.onani.cn" alt="">
