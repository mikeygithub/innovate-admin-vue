# innovate-admin-vue

#### 项目介绍
innovate-admin-vue

#### 软件架构
软件架构说明
vue + element UI

#### 安装教程

1. 安装依赖
控制台输入：npm install
2. 运行：npm run dev
3. 构建 npm run build
3. docker打包： docker build -t mikeyboom/innovate-admin-vue:v1.2.3 .
4. docker login 登入后推到仓库： docker push mikeyboom/innovate-admin-vue
5. 服务器拉取镜像： docker pull mikeyboom/innovate-admin-vue:v1.2.5
6. 部署：docker run --name innovate-admin-vue -d -p 8001:8001 innovate-admin-vue:v1.2.3

``
linux 前端项目vue无法热更新 在启动时请使用 sudo npm run dev
``


