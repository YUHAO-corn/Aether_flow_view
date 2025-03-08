# Aether Flow 项目设置指南

本指南将帮助你在新的项目或设备环境中正确设置和运行 AetherFlow_web_front 和 AetherFlow_plugin_front 两个前端项目。

## 目录

1. [环境准备](#环境准备)
2. [获取项目代码](#获取项目代码)
3. [Web前端项目设置](#web前端项目设置)
4. [插件前端项目设置](#插件前端项目设置)
5. [跨平台注意事项](#跨平台注意事项)
6. [常见问题解决](#常见问题解决)

## 环境准备

在开始之前，请确保你的系统已安装以下软件：

- **Node.js** (推荐 v16.0.0 或更高版本)
- **npm** (通常随Node.js一起安装)
- **Git** (用于版本控制)

可以通过以下命令检查是否已安装：

```bash
node -v
npm -v
git --version
```

## 获取项目代码

### 方法1：从GitHub克隆

```bash
# 克隆整个仓库
git clone https://github.com/YUHAO-corn/Aether_flow_view.git
cd Aether_flow_view
```

### 方法2：下载ZIP文件

1. 访问 https://github.com/YUHAO-corn/Aether_flow_view
2. 点击"Code"按钮，然后选择"Download ZIP"
3. 解压下载的ZIP文件到你的工作目录

### 方法3：复制现有项目文件

如果你已经有项目文件，只需将 `AetherFlow_web_front` 和 `AetherFlow_plugin_front` 文件夹复制到你的新工作目录即可。

## Web前端项目设置

### 安装依赖

```bash
cd AetherFlow_web_front
npm install
```

### 启动开发服务器

```bash
npm run dev
```

服务器启动后，通常会在 http://localhost:5173 (或类似端口) 上运行。

### 构建生产版本

```bash
npm run build
```

构建完成后，生成的文件将位于 `dist` 目录中。

### 部署到生产环境

1. 将 `dist` 目录中的文件复制到你的Web服务器
2. 确保服务器配置正确处理单页应用路由（例如，对于Nginx，需要配置所有请求重定向到index.html）

## 插件前端项目设置

### 安装依赖

```bash
cd AetherFlow_plugin_front
npm install
```

### 启动开发服务器

```bash
npm run dev
```

### 构建浏览器插件

```bash
npm run build
```

构建完成后，生成的文件将位于 `dist` 目录中。

### 在浏览器中加载插件

#### Chrome浏览器

1. 打开Chrome，访问 `chrome://extensions/`
2. 启用"开发者模式"（右上角开关）
3. 点击"加载已解压的扩展程序"
4. 选择 `AetherFlow_plugin_front/dist` 目录

#### Firefox浏览器

1. 打开Firefox，访问 `about:debugging#/runtime/this-firefox`
2. 点击"临时加载附加组件"
3. 选择 `AetherFlow_plugin_front/dist/manifest.json` 文件

## 跨平台注意事项

### Windows 特定设置

1. 路径分隔符：Windows使用反斜杠 `\`，而项目中的路径使用正斜杠 `/`。Git和Node.js通常会自动处理这个差异，但在自定义脚本中需要注意。

2. 长路径限制：Windows有260个字符的路径长度限制。如果遇到相关错误，可以：
   - 将项目放在根目录（如 `C:\Projects\`）而不是深层嵌套的目录
   - 启用Windows的长路径支持：以管理员身份运行PowerShell并执行 `Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1`

3. 权限问题：确保你有足够的权限访问项目目录

### Mac/Linux 特定设置

1. 权限问题：可能需要调整文件权限
   ```bash
   chmod -R 755 AetherFlow_web_front
   chmod -R 755 AetherFlow_plugin_front
   ```

2. 确保脚本可执行
   ```bash
   chmod +x AetherFlow_web_front/node_modules/.bin/*
   chmod +x AetherFlow_plugin_front/node_modules/.bin/*
   ```

## 常见问题解决

### 依赖安装失败

1. 尝试清除npm缓存：
   ```bash
   npm cache clean --force
   ```

2. 使用yarn替代npm：
   ```bash
   npm install -g yarn
   yarn install
   ```

3. 检查Node.js版本兼容性：
   ```bash
   nvm install 16  # 如果你使用nvm管理Node版本
   ```

### 开发服务器启动失败

1. 检查端口占用：
   - Windows: `netstat -ano | findstr 5173`
   - Mac/Linux: `lsof -i :5173`

2. 修改端口：在 `vite.config.js` 或 `vite.config.ts` 中添加：
   ```javascript
   server: {
     port: 3000  // 使用其他可用端口
   }
   ```

### 构建错误

1. 检查TypeScript错误：
   ```bash
   npm run tsc
   ```

2. 检查ESLint错误：
   ```bash
   npm run lint
   ```

3. 尝试清除构建缓存：
   ```bash
   rm -rf node_modules/.vite
   ```

### 浏览器兼容性问题

如果在特定浏览器中遇到渲染问题：

1. 检查 `package.json` 中的 `browserslist` 配置
2. 确保 `postcss.config.js` 中包含适当的浏览器前缀配置
3. 考虑添加特定的polyfill

## 项目间共享代码

如果需要在两个项目之间共享代码：

1. 创建一个公共目录：
   ```bash
   mkdir -p shared/utils shared/components shared/styles
   ```

2. 在各自项目中创建符号链接：
   ```bash
   # 在Windows中
   mklink /D AetherFlow_web_front\src\shared ..\..\shared
   mklink /D AetherFlow_plugin_front\src\shared ..\..\shared
   
   # 在Mac/Linux中
   ln -s ../../shared AetherFlow_web_front/src/shared
   ln -s ../../shared AetherFlow_plugin_front/src/shared
   ```

3. 在项目的构建配置中添加共享目录的处理规则

---

如有任何问题或需要进一步的帮助，请参考各项目中的README文件或提交GitHub issue。 