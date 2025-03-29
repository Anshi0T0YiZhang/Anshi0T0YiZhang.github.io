1. 确保依赖项已安装
静态网站生成器（如 Jekyll）需要依赖 Ruby 和 Node.js 环境。根据项目中的 Gemfile 和 package.json，需先安装依赖：

bash
Copy
# 安装 Ruby 依赖（Jekyll 相关）
bundle install

# 安装 Node.js 依赖（如 Grunt、Less 等）
npm install
2. 修正目录和文件名格式
目录名称：
Jekyll 默认使用 _posts 目录存放文章。

选项 1：将 posts 目录重命名为 _posts。

选项 2：在 config.yml 中自定义目录（添加以下配置）：

yaml
Copy
collections:
  posts:
    output: true
    permalink: /:title/
文件名格式：
文件名需使用连字符分隔日期和标题，例如：
2025-03-29-PointCloud-vs-Mesh.md（原文件中的下划线需改为连字符）。

3. 运行构建命令
根据项目工具链选择以下一种方式：

使用 Jekyll 直接构建：

bash
Copy
# 生成静态文件到 _site 目录
bundle exec jekyll build

# 启动本地服务器并实时预览（自动监听文件变化）
bundle exec jekyll serve --watch
使用 Grunt 或 Rake 任务：
如果项目配置了自动化工具（如 Grunt），可运行：

bash
Copy
# 执行 Grunt 默认任务（需查看 Gruntfile.js 定义）
grunt

# 或执行 Rake 任务（需查看 Rakefile 定义）
rake
4. 清理缓存与验证输出
清理旧构建缓存：
手动删除 _site 目录（Jekyll 默认输出目录），确保全新构建：

bash
Copy
rm -rf _site
检查输出内容：
构建完成后，检查 _site 目录中是否包含新文章的 HTML 文件（如 _site/2025/03/29/PointCloud-vs-Mesh.html）。
若未生成，需检查构建日志中的错误提示。

5. 本地测试与部署
本地预览：
运行 bundle exec jekyll serve 后，访问 http://localhost:4000 查看更新。
若图片未显示，检查 Markdown 中的图片路径是否指向 img 目录（例如：![图片描述](/img/example.png)）。

部署到服务器：

若使用 GitHub Pages，提交代码到仓库后等待自动构建（约 1-2 分钟）。

若使用其他服务（如 Netlify），手动触发构建或等待自动检测更新。

常见问题排查
构建失败：

检查终端日志，常见问题包括：

依赖未安装（如 bundle install 未执行）。

Markdown 文件语法错误（如 YAML 头信息格式错误）。

图片路径错误（需使用绝对路径 /img/xxx.png 而非相对路径）。

网页未更新：

浏览器缓存问题：强制刷新页面（Ctrl + F5 或 Cmd + Shift + R）。

服务器未重启：本地开发时需确保 jekyll serve --watch 在运行。

总结
重新构建项目的核心步骤为：修正配置 → 安装依赖 → 执行构建命令 → 清理缓存 → 验证输出。
若仍不生效，优先检查终端报错和文件路径格式，确保每一步操作均符合静态生成器的规则。