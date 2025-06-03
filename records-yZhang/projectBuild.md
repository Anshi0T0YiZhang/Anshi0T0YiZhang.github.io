# 项目构建(大致思路)

## 一.项目来源

项目模板(fork):
<https://github.com/Huxpro/huxpro.github.io>

项目迁移教程：  
>2.84 复制打开抖音，看看【技术爬爬虾的作品】不花一分钱,半分钟搭建自己的网站,手把手教程 # ... <https://v.douyin.com/UjmCNxOFoXM/> 12/27 yGV:/ <u@s.re>  

## 二.项目构建

### 1.初始化模板的样本内容(windows环境下)

### (1)清除照片  
>
>Remove-Item -Recurse -Path img\*  

### (2)清除内容  
>
>Remove-Item -Recurse -Path _posts\*

### (3)建立与远程仓库的链接(命令样本)
>
>git init  
>git remote add origin <https://github.com/yourusername/yourusername.github.io.git>  
>git pull origin master  

### (4)修改_config.yml文件  
>
>name: 你的名字  
>email: 你的邮箱  
>author: 你的名字  
>title: 你的博客标题  
>description: 你的博客描述  
>baseurl: "" # the subpath of your site, e.g. /blog  
>url: "<https://yourusername.github.io>" # the base hostname & protocol for your site  
>github_username: yourgithubusername  
>avatar: 你的头像路径  
>timezone: 你的时区  
>permalink: /:year/:month/:day/:title/  

### 2.执行项目

### 安装 Ruby 依赖（Jekyll 相关）
>
>cd D:\Git_clone\Anshi0T0YiZhangBlog  
>ruby -v  
>bundle install

### 安装 Node.js 依赖（如 Grunt、Less 等）
>
>npm install  

### 修正文件名格式
>
>文件名需使用连字符分隔日期和标题，例如：
2025-03-29-PointCloud-vs-Mesh.md（原文件中的下划线需改为连字符）。

### 运行构建命令

根据项目工具链我选择的方式：**使用 Jekyll 直接构建**

#### 生成静态文件到 _site 目录  
>
>bundle exec jekyll build  

#### 启动本地服务器并实时预览（自动监听文件变化）
>
>bundle exec jekyll serve --watch

+ *上述仅是在本地渲染界面，会生成一个地址为类似于 <http://127.0.0.1:4000/> 的网页，这是为了方便项目构建者方便查看效果;  
但最终发布到线上后，需要将项目发布到github上（仓库名必须与github用户名一致），然后在github上设置Pages服务(技术爬爬虾有讲)，等待几分钟后，项目便会被部署到线上。*

## 三.项目细节修改

### 1.浏览器开发者工具(F12)指导

### 1.1 常用功能和界面

**select an element in the page to inspect it：**
选择页面中的一个元素进行检查；图标形状：  

<p align="center">
  <img src="/img/InternetTech/selectButton.png" alt="alt text" />
</p>

**Elements/Styles：**  
查看和实时编辑网页的 ```HTML``` 结构和 ```CSS``` 样式。

**Elements/Computed：**  

+ 样式最终值计算
显示当前选中元素所有 CSS 属性的 最终生效值（包括继承、层叠、浏览器默认样式等）。

+ 盒模型可视化
以图形化方式展示元素的 width/height、padding、border、margin。

+ 样式来源追踪
点击任意属性可跳转到 Styles 面板中定义该属性的原始规则。

### 1.2 页面布局调整步骤

> 1. 用select an element in the page to inspect it，选中需要调整的元素。
> 2. 在Elements/Styles面板中调整元素的宽度和高度。
> 3. 在Elements/Computed面板中调整元素的边距、内边距、边框、外边距。

### 2.修改.min.css文件

(1) 使用cssnano压缩css文件，压缩后的文件名为.min.css

安装工具：

```bash
npm install -g cssnano  # 全局安装（需提前安装 Node.js）
```

这里有个小知识：
```powershell ≠ cmd窗口```
>PowerShell 默认执行策略可能禁止脚本运行：
npm 依赖脚本执行（如 npm install 会运行 .cmd 或 .ps1 脚本），若执行策略限制严格，会导致报错（如 无法加载文件，因为在此系统上禁止运行脚本）。
```Get-ExecutionPolicy```若结果为 Restricted（默认值）或 AllSigned，表示禁止运行未签名的脚本。

执行压缩命令：

```bash
cssnano themes/hux/source/css/hux-blog.css -o themes/hux/source/css/hux-blog.min.css
```

(2) 主题配置了自动化构建（如使用 Gulp/Grunt）

部分开发者可能为主题配置了构建工具（如 Gulp），修改 hux-blog.css 后只需运行构建命令即可自动生成 .min.css：

```bash
cd 博客根目录
gulp css  # 或其他主题定义的命令
```

(3)使用在线压缩工具压缩css文件（我的Edge浏览器已收藏）

### 3.修改成功 ！！！

删除```.min.css```文件，修改```.css```文件和```.less```文件中的对应内容（从```F12```浏览器开发工具找对应位置），用在线工具重新生成```.min.css```文件，刷新浏览器（```F12```-```Application```清除缓存-刷新页面）即可查看效果。
