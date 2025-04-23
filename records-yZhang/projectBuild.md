# 项目构建(大致思路)
## 项目来源
项目模板(fork):   
<https://github.com/Huxpro/huxpro.github.io>

项目迁移教程：  
>2.84 复制打开抖音，看看【技术爬爬虾的作品】不花一分钱,半分钟搭建自己的网站,手把手教程 # ... https://v.douyin.com/UjmCNxOFoXM/ 12/27 yGV:/ u@s.re  

## 项目构建
### 1.初始化模板的样本内容(windows环境下)

#### (1)清除照片  
>Remove-Item -Recurse -Path img\*  


#### (2)清除内容  
>Remove-Item -Recurse -Path _posts\*


#### (3)建立与远程仓库的链接(命令样本)
>git init  
>git remote add origin https://github.com/yourusername/yourusername.github.io.git  
>git pull origin master  

#### (4)修改_config.yml文件  
>name: 你的名字  
>email: 你的邮箱  
>author: 你的名字  
>title: 你的博客标题  
>description: 你的博客描述  
>baseurl: "" # the subpath of your site, e.g. /blog  
>url: "https://yourusername.github.io" # the base hostname & protocol for your site  
>github_username: yourgithubusername  
>avatar: 你的头像路径  
>timezone: 你的时区  
>permalink: /:year/:month/:day/:title/  

### 2.执行项目
#### 安装 Ruby 依赖（Jekyll 相关）
>cd D:\Git_clone\Anshi0T0YiZhangBlog  
>ruby -v  
>bundle install

#### 安装 Node.js 依赖（如 Grunt、Less 等）
>npm install  

#### 修正文件名格式
>文件名需使用连字符分隔日期和标题，例如：
2025-03-29-PointCloud-vs-Mesh.md（原文件中的下划线需改为连字符）。

#### 运行构建命令
根据项目工具链我选择的方式：**使用 Jekyll 直接构建**
##### 生成静态文件到 _site 目录  
>bundle exec jekyll build  

##### 启动本地服务器并实时预览（自动监听文件变化）
>bundle exec jekyll serve --watch

+ *上述仅是在本地渲染界面，会生成一个地址为类似于 http://127.0.0.1:4000/ 的网页，这是为了方便项目构建者方便查看效果;  
但最终发布到线上后，需要将项目发布到github上（仓库名必须与github用户名一致），然后在github上设置Pages服务(技术爬爬虾有讲)，等待几分钟后，项目便会被部署到线上。*