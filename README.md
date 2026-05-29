# ResumeAI Pro - AI智能简历优化平台

一个基于React和AI技术的现代化简历优化平台，帮助求职者提升简历质量，提高面试机会。

## 功能特性

### 核心功能
- **AI智能分析** - 深度分析简历内容，识别优势与不足
- **职位匹配优化** - 根据目标职位精准优化关键词和技能描述
- **ATS系统友好** - 优化格式和关键词，提高通过招聘系统筛选的概率
- **语法检查** - 自动检测并修正语法错误
- **快速优化** - 30秒内完成优化
- **隐私保护** - 数据加密传输，优化后自动删除

### 商业模式
1. **免费版** - 基础简历分析，3次优化机会，含广告
2. **专业版** - ¥29/月，无限次优化，无广告体验
3. **高级精修** - ¥99/月，一对一专家服务，人工审核

## 技术栈

- **前端框架**: React 18
- **构建工具**: Vite 5
- **样式**: Tailwind CSS
- **图标**: Lucide React
- **支付**: Stripe / PayPal
- **AI集成**: OpenAI API / 自定义AI服务
- **文件处理**: mammoth (Word文档解析)

## 快速开始

### 安装依赖

```bash
cd resume-optimizer
npm install
```

### 配置环境变量

创建 `.env` 文件：

```env
# OpenAI API配置（可选）
VITE_OPENAI_API_KEY=your_openai_api_key

# 自定义AI服务（可选）
VITE_AI_API_URL=your_custom_ai_api_url
VITE_AI_API_KEY=your_custom_ai_api_key

# Stripe支付配置
VITE_STRIPE_PUBLIC_KEY=your_stripe_public_key
```

### 启动开发服务器

```bash
npm run dev
```

访问 http://localhost:3000

### 构建生产版本

```bash
npm run build
```

## 项目结构

```
resume-optimizer/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── Header.jsx          # 导航栏
│   │   ├── Hero.jsx            # 首页横幅
│   │   ├── Features.jsx        # 功能特性
│   │   ├── ResumeUploader.jsx  # 简历上传组件
│   │   ├── OptimizationResult.jsx # 优化结果展示
│   │   ├── Pricing.jsx         # 定价方案
│   │   ├── PaymentModal.jsx    # 支付弹窗
│   │   ├── Testimonials.jsx    # 用户评价
│   │   └── Footer.jsx          # 页脚
│   ├── services/
│   │   └── aiService.js        # AI服务集成
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── index.html
├── package.json
├── vite.config.js
├── tailwind.config.js
└── README.md
```

## API集成

### OpenAI API

项目支持直接调用OpenAI API进行简历优化：

```javascript
import { optimizeResume } from './services/aiService'

const result = await optimizeResume(resumeText, targetPosition)
```

### 自定义AI服务

也可以集成自定义的AI服务：

```javascript
// 在 .env 中配置
VITE_AI_API_URL=https://your-api.com/optimize
VITE_AI_API_KEY=your_api_key
```

## 支付集成

### Stripe集成

1. 注册Stripe账号并获取API密钥
2. 在后端创建支付意图API
3. 前端使用Stripe Elements收集支付信息

### PayPal集成

1. 注册PayPal开发者账号
2. 获取Client ID
3. 集成PayPal SDK

## 部署

### Vercel部署

```bash
npm i -g vercel
vercel
```

### GitHub Pages部署

1. 修改 `vite.config.js`:
```javascript
export default defineConfig({
  base: '/your-repo-name/',
  // ...
})
```

2. 构建并部署：
```bash
npm run build
# 将 dist 目录推送到 gh-pages 分支
```

## 盈利模式

### 1. 免费版（广告收入）
- 提供3次免费优化机会
- 页面展示Google AdSense或其他广告
- 通过广告点击获得收入

### 2. 专业版订阅
- 月付 ¥29 / 年付 ¥290
- 无限次优化
- 无广告体验
- 稳定的订阅收入

### 3. 高级精修服务
- 月付 ¥99 / 年付 ¥990
- 一对一专家服务
- 人工审核简历
- 高附加值服务

## 未来规划

- [ ] 多语言支持（英文、日文等）
- [ ] 简历模板库
- [ ] 求职信生成
- [ ] LinkedIn优化
- [ ] 面试模拟
- [ ] 职业规划建议
- [ ] 企业版API

## 许可证

MIT License

## 联系方式

- 邮箱: support@resumeai.pro
- 电话: 400-123-4567
- 地址: 北京市朝阳区科技园区


[DuMate AI生成]