<div align="center">

<img src="public/logo.png" width="100" height="100" alt="super-api">

# [🥳 Super-API](https://api.cngov.top)

#### 🚀Super-API由new-api的基础上二次开发


![image](https://github.com/user-attachments/assets/37b3d1dd-7e2c-45f5-ad31-3d3af1b5ba5e)
</div>

# 🚀 Super-API 系统说明
> [!NOTE]  
> 本项目在 [NewAPI](https://github.com/Calcium-Ion/new-api) & [OneAPI](https://github.com/songquanpeng/one-api) 的基础上进行二次开发

> [!IMPORTANT]  
> 使用者必须在遵循上游AI模型服务提供商以及**法律法规**的情况下使用，不得用于非法用途。  
> 本项目仅供个人学习使用，不保证稳定性，且不提供任何技术支持。  
> 根据[《生成式人工智能服务管理暂行办法》](http://www.cac.gov.cn/2023-07/13/c_1690898327029107.htm)的要求，请勿对中国地区公众提供一切未经备案的生成式人工智能服务。

> [!WARNING]  
> **本系统为闭源免授权使用，仅供个人学习使用，请勿用于任何商业用途。**

## 使用说明 🚀

在您的服务器新建`docker-compose.yml`文件，内容如下：
```
version: '3.4'

services:
  SuperApi:
    image: registry.cn-hangzhou.aliyuncs.com/super-api/super-api:latest
    container_name: super-api
    restart: always
    command: --log-dir /app/logs
    ports:
      - "3000:3000"
    volumes:
      - ./data:/data
      - ./logs:/app/logs
    extra_hosts:
      - "host.docker.internal:host-gateway"
    environment:
      - SQL_DSN=root:123456@tcp(host.docker.internal:3306)/SuperApi?charset=utf8mb4&parseTime=True&loc=Local  # 修改此行，或注释掉以使用 SQLite 作为数据库
      - REDIS_CONN_STRING=redis://redis
      - SESSION_SECRET=random_string  # 启动前必须手动修改此值为随机字符串
      - TZ=Asia/Shanghai

    depends_on:
      - redis
    healthcheck:
      test: [ "CMD-SHELL", "wget -q -O - http://localhost:3000/api/status | grep -o '\"success\":\\s*true' | awk -F: '{print $2}'" ]
      interval: 30s
      timeout: 10s
      retries: 3

  redis:
    image: redis:latest
    container_name: redis
    restart: always
```

### 启动服务 ▶️

```bash
docker-compose up -d
```

访问 `http://ip:3000` 即可看到登录界面，输入账号密码即可登录（默认账号：`root`，默认密码：`123456`）。

### 更新服务 🔄

若有版本更新，您可以通过以下命令更新并重启服务：

```bash
docker-compose pull && docker-compose up -d
```

## 1. 界面全面重构 🎨

### 1.1 整体优化 ✨
- 🔄 全面重构UI，优化显示逻辑，加快网页访问速度
- ⚡ 优化后端响应性能
- 📱 统一应用页面布局，增加页面容器通用样式
- 🌈 优化页面加载动画，提升用户体验

### 1.2 导航栏重构 🧭
- 🌟 重构顶部和侧边栏，采用毛玻璃效果设计
- 🔧 支持自定义菜单功能，可设置显示位置（顶部/侧边栏）
- 📱 优化移动端适配，改进滚动体验
- 📢 增加公告按钮，便于用户随时查看系统通知
- 🌓 去除黑夜模式功能

## 2. 模型广场页面升级 🤖

- 🎯 重构模型广场页面，提升用户体验和功能完整性
- 🏷️ 增加模型标签系统，便于快速筛选和识别模型类型
- 📝 添加详细模型说明，帮助用户了解模型能力和使用场景
- 🎮 新增模型体验功能（游乐场），支持直接测试模型效果
- 🏢 展示模型厂商信息，便于用户识别不同来源的模型

## 3. 对话功能增强 💬

- 🔄 对话页面重构，界面更加简洁直观
- ⚡ 新增一键配置功能，快速设置模型参数
- 📜 优化对话历史记录展示
- ⚙️ 支持更多模型参数设置

## 4. 工作台页面 📊

全新工作台页面支持多款卡片组件：
- 🔌 API地址卡片：显示和管理API接口信息
- 📡 线路监控卡片：实时监控API服务状态
- 📂 折叠面板卡片：组织显示复杂信息
- 🖼️ iframe内嵌卡片：支持嵌入外部网页内容
- 📈 模型消耗图表：可视化展示模型使用情况
- 📊 调用次数图表：统计API调用频率
- 📢 系统公告卡片：展示重要系统通知
- 🌐 自定义HTML卡片：支持自定义内容
- 📄 自定义Markdown卡片：支持富文本内容展示
- ✅ 签到卡片：用户每日签到获取奖励

卡片支持全面自定义：
- 📐 可设置卡片大小
- 🔄 可调整卡片方向
- ✏️ 可修改卡片名称
- 💡 可添加卡片说明

## 5. MidJourney功能增强 🎨

### 5.1 模式选择优化 🎛️
- 🆕 新增多种模式选择方式：
  - ⚙️ 支持在令牌设置中配置默认模式
  - 🔍 自动识别提示词内的模式参数
  - 🛣️ 支持使用特定路径（如：/mj-fast/mj）指定模式
- 🔝 模式优先级：路径模式 > 令牌模式 > 提示词模式 > 默认fast模式

### 5.2 图片代理功能 🖼️
- 🔄 增加MJ图片代理地址配置
- 🔄 支持在后台配置多个代理地址轮换使用
- 🔑 支持令牌级别代理设置
- 🔍 绘图日志增加使用代理查看图片功能
- ⚡ 优化图片加载体验

## 6. Suno音乐功能优化 🎵

- 🔄 重构人物查询返回格式，适配大部分AI系统
- 📤 增加音乐上传接口（suno_upload）
  - 🔗 支持通过URL上传音频文件
  - 📁 支持直接上传音频文件
- 🔔 增加回调功能（notifyhook）
- 💰 支持在运营设置中配置收费价格
- 🎵 优化音乐生成和查询体验

## 7. 公告系统升级 📢

- 📑 支持设置多个公告
- ⏱️ 公告可配置开始时间/结束时间
- 📌 可设置公告标题和类型（信息、成功、警告、错误）
- 👁️ 可控制公告显示状态
- ⏰ 支持"24小时内不自动弹出"功能
- 🌐 公告支持HTML富文本内容

## 8. 钱包与支付优化 💰

- 🔄 全面重构钱包页面，增加宣传栏
- 🎁 新增套餐支付功能，支持设置折扣
- 🎫 增加兑换码购买入口
- 💳 优化充值和消费流程
- 🖥️ 可视化配置套餐界面，无需编辑JSON

## 9. 其他新功能 🎯

### 9.1 签到系统 📅
- ✅ 新增每日签到功能
- 🎮 支持在工作台配置签到卡片
- 🎁 签到可获取奖励

### 9.2 用户协议与隐私 📜
- ✓ 登录时需要勾选隐私策略和服务条款
- 📃 优化协议展示

### 9.3 邮件优化 📧
- 🎨 优化邮件模板样式
- 📨 提升邮件通知体验

### 9.4 标签功能 🏷️
- 🆕 增加标签系统，用于分类和标记
- 🎨 标签支持自定义设置
  - 📝 可设置文字内容
  - 🎨 可配置标签颜色
  - 🌈 可设置背景色
  - 🔠 可定义文字颜色

## 10. SEO优化 🔍

- 📋 添加SEO描述和关键字设置
- 🎨 支持自定义全局顶部样式
- 📝 支持全局底部脚本设置
- 📊 优化页面元数据


# 预览图：
![image](https://github.com/user-attachments/assets/e59acf87-4b64-4f43-b648-3bf03752ce18)

![image](https://github.com/user-attachments/assets/4f742cf1-f68e-4b11-b0b4-783b30aa9080)
![image](https://github.com/user-attachments/assets/6f3c7c3b-a058-491d-9b72-621e5d1204a1)
![image](https://github.com/user-attachments/assets/ea3b4c75-f527-4676-8fdb-3784b98b2106)
![image](https://github.com/user-attachments/assets/d9df0104-9501-49da-9d0e-c14fcc67a0cd)

![image](https://github.com/user-attachments/assets/0172008f-6c56-41c7-adde-d3ff20eaf046)
![image](https://github.com/user-attachments/assets/91d81c57-8b03-44c4-b52c-1e7ac2b85d01)
![image](https://github.com/user-attachments/assets/a5679279-5159-4ee4-80fa-70ea0e72659b)

![image](https://github.com/user-attachments/assets/15035468-dc49-49e8-98e7-df332793d97a)
![image](https://github.com/user-attachments/assets/861a7605-83ab-4907-9e5c-a57bb720a1ed)
![image](https://github.com/user-attachments/assets/3080811c-c994-49a8-8f93-c37f1eba3819)
![image](https://github.com/user-attachments/assets/3c957691-cacb-4d45-8cd1-2ade46a6e981)


![image](https://github.com/user-attachments/assets/eefb57af-2af8-422c-a06f-6613c1b72bc4)

![image](https://github.com/user-attachments/assets/1c008704-29a7-46b7-9980-b4b099c67a22)

