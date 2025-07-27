---
title: 个人Blog部署及域名解析(Vercel + Cloudflare)
published: 2025-07-27
description: '关于本站的搭建，部署，以及域名DNS解析方面的探索心得'
image: 'https://s3.bmp.ovh/imgs/2025/07/27/eb2d76c5a4373a34.png'
tags: [Blog, Vercel, Cloudflare]
category: '记录'
draft: false 
lang: ''
---

## 0. 准备

+ 一台能上网的电脑，上面装了[Git](https://git-scm.com/)和好用的IDE（比如[VSCode](https://code.visualstudio.com/)）
+ 哦对了你的电脑上记得装[NodeJS](nodejs.org/en)
+ [Github](https://github.com/)账号
+ 科学上网的工具（可能非必须，但是没有绝对会很难受！）
+ 一颗决心 (heart

## 1. 本地网页部署

我直接套了Fuwari的壳子，操作如下：

  1. Fork仓库到你的仓库下：[Fuwari](https://github.com/saicaca/fuwari)

  2. 使用`git clone <你的git链接>`克隆到本地

  3. 用VSCode(或者任何其他的IDE)打开它

  4. 根据Fuwari的README文件更改网站内容: [这里](https://github.com/saicaca/fuwari/blob/main/docs/README.zh-CN.md)

  5. (步骤5-6在上面的README文件中亦有记载)

     安装pnpm：`npm install -g pnpm`

  6. 在项目根目录安装依赖：`pnpm install` 和 `pnpm add sharp`

  7. 本地的博客部署基本完毕，你可以使用`pnpm dev`在本地运行，在localhost:4321中查看效果

## 2. 个性化

  1. 修改`src/config.ts`中的内容来修改部分内容显示

     可以参照注释进行修改

     ```typescript
     // config.ts
     ...
     
     export const siteConfig: SiteConfig = { // Line 10
     	title: "Fuwari", // 网页最上方显示的标题
     	subtitle: "Demo Site", // 副标题
     	lang: "en", // 语言：'en', 'zh_CN', 'zh_TW', 'ja', 'ko', 'es', 'th', 'vi'
     	themeColor: {
              // 默认主题颜色：可以参考页面右上角调色盘按钮修改
     		hue: 250, // Default hue for the theme color, from 0 to 360. e.g. red: 0, teal: 200, cyan: 250, pink: 345
     		fixed: false, // Hide the theme color picker for visitors
     	},
     	banner: {
     		enable: false, // 若开启，在主页会有一张背景图
     		src: "assets/images/demo-banner.png", // Relative to the /src directory. Relative to the /public directory if it starts with '/'
     		position: "center", // Equivalent to object-position, only supports 'top', 'center', 'bottom'. 'center' by default
     		credit: {
     			enable: false, // Display the credit text of the banner image
     			text: "", // Credit text to be displayed
     			url: "", // (Optional) URL link to the original artwork or artist's page
     		},
     	},
     	toc: {
     		enable: true, // Display the table of contents on the right side of the post
     		depth: 2, // Maximum heading depth to show in the table, from 1 to 3
     	},
     	favicon: [
              // 此处是网页icon设置，详情见下面的
     		// Leave this array empty to use the default favicon
     		// {
     		//   src: '/favicon/icon.png',    // Path of the favicon, relative to the /public directory
     		//   theme: 'light',              // (Optional) Either 'light' or 'dark', set only if you have different favicons for light and dark mode
     		//   sizes: '32x32',              // (Optional) Size of the favicon, set only if you have favicons of different sizes
     		// }
     	],
     };
     
     export const navBarConfig: NavBarConfig = {
     	links: [
              // 上方导航栏的按钮
     		LinkPreset.Home,
     		LinkPreset.Archive,
     		LinkPreset.About,
     		{
     			name: "GitHub",
     			url: "https://github.com/saicaca/fuwari", // Internal links should not include the base path, as it is automatically added
     			external: true, // Show an external link icon and will open in a new tab
     		},
     	],
     };
     
     export const profileConfig: ProfileConfig = {
         // 这里是左侧侧头像框的内容
     	avatar: "assets/images/demo-avatar.png", // 头像： Relative to the /src directory. Relative to the /public directory if it starts with '/'
     	name: "Lorem Ipsum", // 显示的用户名
     	bio: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.", // 简介
     	links: [
             // 底下三个按钮
     		{
     			name: "Twitter",
     			icon: "fa6-brands:twitter", // Visit https://icones.js.org/ for icon codes
     			// You will need to install the corresponding icon set if it's not already included
     			// `pnpm add @iconify-json/<icon-set-name>`
     			url: "https://twitter.com",
     		},
     		{
     			name: "Steam",
     			icon: "fa6-brands:steam",
     			url: "https://store.steampowered.com",
     		},
     		{
     			name: "GitHub",
     			icon: "fa6-brands:github",
     			url: "https://github.com/saicaca/fuwari",
     		},
     	],
     };
     
     ...
     ```

2. 添加博客内容：在根目录下输入`pnpm new-post <文章标题>`创建文字模板，此时会在`src/content/posts`创建一个markdown文件，你可以在markdown中撰写你的内容！

3. 注意，自动生成的md有一定格式的文件开头format

   ```markdown
   ---
   title: 你的Blog标题
   published: 2023-09-09       # 创建时间
   description: '描述'
   image: './cover.jpg'        # 封面图
   tags: [标签1, 标签2]
   category: 分类
   draft: false                # 是否为草稿
   lang: jp      # Set only if the post's language differs from the site's language in `config.ts`
   ---
   ```

   记得要按正确格式填写，不然可能报错

4. 如果你不会markdown语法，建议找个视频光速学一下，不难，而且markdown真的很常用

## 3. 上传你的更改到Github

我强烈推荐使用VSCode或者IDEA自带的Git界面！不然命令行真的会看着令人头大！

1. 关联你本地的git和Github账号

```verilog
git config --global user.name "XXXX"  用户名标识  ---- 实际也可以填写您的github仓库的名称
git config --global user.email "xxxx@xxx.com"  邮箱标识  -------可以填写github仓库的邮箱
```

2. add&commit

点击左侧的“源代码管理”按钮（长的像个树的分叉），暂存所有更改后输入commit message并commit

![git.png](https://s3.bmp.ovh/imgs/2025/07/27/f8116bdb07bb0930.png)

![commit.png](https://s3.bmp.ovh/imgs/2025/07/27/ae973b3c4ef1479b.png)

3. 点击“同步更改”，把你本地的代码push到远程仓库

这样一来，Github仓库应该就有了你的最新更改！

## 4. 网页托管

有几个选项可供选择：

+ Github Page: 部署方便，但是构建速度慢
+ Vercel: 极致性能，但是国内不能直接访问部署的站点（但是在用Cloudflare代理时可以解决这一问题，甚至访问速度很不错）
+ Netlify: 和Vercel差不多，听说构建速度上笔Vercel略逊一筹（但也没啥大区别）

如果你并不打算放到自己的域名，只想要找个网站托管，推荐Github

如果你想要把网站放到**自己的域名**下，可以使用Vercel

此处就以Vercel为例：

1. 进入[Vercel](vercel.com/signup)并注册（最好以Github注册，托管起来方便一些）

2. 左侧会显示import按钮，授权Github并导入你的博客的仓库

3. 导入完成后，界面长这样：

   ![vercel.png](https://s3.bmp.ovh/imgs/2025/07/27/fa5539ba5f892d98.png) 

   此处，domains后面的网址就是托管到的网址（这里我有两个是因为我自行添加了另一个我买的域名，不添加的话只有xxxxxx.vercel.app这样的网址)

至此，你的网页就托管好了，但是还不能在国内访问，需要用自己的域名来指向这个网址，并通过cloudflare的代理使国内可以顺利访问

## 5. 买个域名

搜索下来，得到了几个不知道是真是假的建议：

1. 阿里云，腾讯云等域名注册平台比较正规，但是优惠少一点，不过以后国内备案也方便（如果你想要拿域名做点大事情的话），Namecheap和porkbun也是不错的选择（比较便宜），不过需要你有国外支付方式
2. '.top'和'.xyz'比较便宜，自己小项目用用比较合适，但是不建议用于大项目；'.com'看上去比较专业，不过也因此比较贵
3. 国内的许多域名注册平台需要实名，不实名会导致域名被暂停解析(ServerHold)，在下一步前，请确保你的域名状态正常

## 6. DNS解析

1. 有域名之后来到Vercel，找到你刚托管的网页，最上方点击settings -> Domains -> Add Domain

   ![domain.png](https://s3.bmp.ovh/imgs/2025/07/27/2a16d3c46601093e.png)

   在中间的输入栏里面输入你的域名，其他的不用改

   ![add-domain.png](https://s3.bmp.ovh/imgs/2025/07/27/4c4cd8baae89d5e3.png)

   此时会有**Invalid Configuration**提示，不要惊慌，接下来解决这一问题

2. 打开并注册[Cloudflare](www.cloudflare.com/zh-cn)，点击右上角的添加**Add**，选择连接域

3. 在底下输入你的域名

4. 选择免费的计划

5. 接着cloudflare会告诉你添加DNS记录，此时找到刚才Vercel界面的新添加的域名哪里，会有一个表格

   其中有www开头的长这样：

   | Type  | Name | Value |
   | ----- | ---- | ----- |
   | CNAME | www  | ...   |

   没有www的长这样：

   | Type | Name | Value |
   | ---- | ---- | ----- |
   | A    | @    | ...   |

   在cloudflare中添加DNS记录，并一一对应

6. 接着会提示更换DNS服务器，下面会为你提供两个服务器地址，复制他们，在你的域名注册商的平台里找到DNS服务器选项（比如阿里云：域名列表->(你的域名)->基本信息->注册信息->DNS服务器->旁边有个修改DNS服务器按钮），把DNS服务器改为cloudflare提供的那两个

7. 更改DNS服务器之后需要等待一段时间，这里我只等了半小时左右就行了
8. DNS服务器更改成功后，回到Vercel的界面，点击Refresh，不出意外就是**Valid Configuration**了，在DNS解析成功后，Vercal会自动生成SSL证书，无须担心网页证书的问题！

到此为止，一个自己的域名，自己的博客终于全部搞定！

## 7. 后记

关于在搭博客的过程中，遇到的许多概念性的问题，以及技术的问题，都能询问AI解决，**AI真是个很好的工具**！

+ 感谢DeepSeek

搭博客的过程以及本文撰写时也参考了许多大佬的博客：[使用Astro+Vercel+Cloudflare一天时间开发部署上线一个知识博客网站](https://juejin.cn/post/7395866920896118836), [Fuwari静态博客搭建教程](https://2x.nz/posts/fuwari/)

+ 感谢大佬们



