# GitHub Pages 部署指南

## 问题分析

你遇到的 `main.js:1 Failed to load resource: the server responded with a status of 404 ()` 错误通常是由于资源路径配置不正确导致的。

## 已修复的问题

1. **修复了 base 路径拼写错误**：在 `vite.config.js` 中将 `base: '/myPeject/'` 改为 `base: '/myProject/'`
2. **添加了部署依赖**：安装了 `gh-pages` 包用于部署
3. **配置了部署脚本**：在 `package.json` 中添加了 `deploy` 命令

## 下一步操作

### 1. 在 GitHub 上创建仓库

1. 登录你的 GitHub 账户
2. 点击右上角的 "+" 图标，选择 "New repository"
3. 仓库名称填写为 `myProject`
4. 设置仓库为 public（如果需要公开访问）
5. 点击 "Create repository"

### 2. 推送本地项目到 GitHub

在项目根目录下执行以下命令：

```bash
# 初始化 Git（如果还没有初始化）
git init

# 添加远程仓库
git remote add origin https://github.com/你的用户名/myProject.git

# 添加所有文件
git add .

# 提交文件
git commit -m "Initial commit"

# 推送主分支
git push -u origin master
```

### 3. 部署到 GitHub Pages

```bash
# 构建项目
npm run build

# 部署到 GitHub Pages
npm run deploy
```

### 4. 配置 GitHub Pages

1. 进入你的 GitHub 仓库页面
2. 点击 "Settings" 选项卡
3. 在左侧菜单中选择 "Pages"
4. 在 "Source" 部分，选择 "gh-pages" 分支
5. 点击 "Save"

### 5. 验证部署

部署完成后，你可以通过以下 URL 访问你的网站：
`https://你的用户名.github.io/myProject/`

## 常见问题排查

1. **404 错误**：确保 `vite.config.js` 中的 `base` 路径与仓库名称一致
2. **资源加载错误**：检查 `dist` 目录中的资源路径是否正确包含了 base 路径
3. **部署失败**：确保 GitHub 仓库存在且可以访问

如果还有问题，请检查浏览器控制台的具体错误信息，确认是哪个资源无法加载。