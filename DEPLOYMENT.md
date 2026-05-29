# 部署指南

## 项目结构

```
resume-optimizer/
├── src/                    # 前端源代码
├── backend/                # 后端API服务
│   ├── src/
│   │   ├── routes/        # API路由
│   │   ├── services/      # 业务逻辑
│   │   ├── middleware/    # 中间件
│   │   └── utils/         # 工具函数
│   ├── database/          # 数据库脚本
│   └── uploads/           # 文件上传目录
└── README.md
```

## 本地开发

### 1. 启动后端

```bash
cd backend
npm install
cp .env.example .env
# 编辑.env文件，填写必要配置
npm run dev
```

后端将在 http://localhost:5000 启动

### 2. 启动前端

```bash
# 在项目根目录
npm install
npm run dev
```

前端将在 http://localhost:3000 启动

### 3. 配置环境变量

**前端 (.env)**
```env
VITE_API_URL=http://localhost:5000/api
VITE_STRIPE_PUBLIC_KEY=pk_test_xxx
```

**后端 (.env)**
```env
DATABASE_URL=postgresql://user:password@localhost:5432/resume_optimizer
JWT_SECRET=your-secret-key
OPENAI_API_KEY=sk-xxx
STRIPE_SECRET_KEY=sk_test_xxx
```

## 生产部署

### 方案一：Vercel + Railway

#### 前端部署到Vercel

1. 安装Vercel CLI
```bash
npm i -g vercel
```

2. 部署
```bash
vercel --prod
```

3. 配置环境变量
在Vercel控制台设置：
- `VITE_API_URL` - 后端API地址

#### 后端部署到Railway

1. 安装Railway CLI
```bash
npm i -g @railway/cli
```

2. 登录并创建项目
```bash
railway login
railway init
```

3. 添加PostgreSQL
```bash
railway add --plugin postgresql
```

4. 部署
```bash
cd backend
railway up
```

5. 配置环境变量
在Railway控制台设置所有必需的环境变量

### 方案二：Docker部署

#### 构建镜像

```bash
# 后端
cd backend
docker build -t resume-backend .

# 前端
docker build -t resume-frontend -f Dockerfile.frontend .
```

#### 使用Docker Compose

创建 `docker-compose.yml`:

```yaml
version: '3.8'

services:
  frontend:
    image: resume-frontend
    ports:
      - "3000:80"
    environment:
      - VITE_API_URL=http://api.example.com
    depends_on:
      - backend

  backend:
    image: resume-backend
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/resume_optimizer
      - JWT_SECRET=${JWT_SECRET}
      - OPENAI_API_KEY=${OPENAI_API_KEY}
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=resume_optimizer
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

启动：
```bash
docker-compose up -d
```

### 方案三：云服务器部署

#### 准备服务器

推荐配置：
- CPU: 2核
- 内存: 4GB
- 存储: 40GB SSD
- 系统: Ubuntu 22.04

#### 安装依赖

```bash
# Node.js 18
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# PostgreSQL
sudo apt-get install -y postgresql postgresql-contrib

# Nginx
sudo apt-get install -y nginx

# PM2
sudo npm install -g pm2
```

#### 配置PostgreSQL

```bash
sudo -u postgres psql
CREATE DATABASE resume_optimizer;
CREATE USER resume_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE resume_optimizer TO resume_user;
```

#### 部署后端

```bash
cd /var/www
git clone your-repo
cd resume-optimizer/backend
npm install --production
cp .env.example .env
# 编辑.env

# 运行数据库迁移
npm run db:migrate

# 使用PM2启动
pm2 start src/server.js --name resume-backend
pm2 save
pm2 startup
```

#### 部署前端

```bash
cd /var/www/resume-optimizer
npm install
npm run build
```

#### 配置Nginx

创建 `/etc/nginx/sites-available/resume-optimizer`:

```nginx
server {
    listen 80;
    server_name your-domain.com;

    # 前端
    location / {
        root /var/www/resume-optimizer/dist;
        try_files $uri $uri/ /index.html;
    }

    # 后端API
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

启用配置：
```bash
sudo ln -s /etc/nginx/sites-available/resume-optimizer /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

#### 配置SSL

```bash
sudo apt-get install -y certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

## 数据库迁移

### 初始化数据库

```bash
cd backend
psql -U resume_user -d resume_optimizer -f database/schema.sql
```

### 备份数据库

```bash
pg_dump -U resume_user resume_optimizer > backup.sql
```

### 恢复数据库

```bash
psql -U resume_user resume_optimizer < backup.sql
```

## 监控与日志

### PM2监控

```bash
pm2 monit
pm2 logs resume-backend
```

### 日志位置

- 后端日志: `/var/www/resume-optimizer/backend/logs/`
- Nginx日志: `/var/log/nginx/`

## 性能优化

### 后端优化

1. 启用Redis缓存
2. 数据库索引优化
3. API响应压缩
4. 连接池配置

### 前端优化

1. 代码分割
2. 图片懒加载
3. CDN加速
4. Gzip压缩

## 安全建议

1. 定期更新依赖
2. 启用防火墙
3. 配置速率限制
4. 定期备份数据库
5. 监控异常访问
6. 使用环境变量管理密钥

## 故障排查

### 后端无法启动

1. 检查环境变量配置
2. 检查数据库连接
3. 查看错误日志

### 前端无法访问API

1. 检查CORS配置
2. 检查API_URL配置
3. 检查Nginx代理配置

### 数据库连接失败

1. 检查PostgreSQL服务状态
2. 检查连接字符串
3. 检查防火墙规则
