# 📁 NotePages

NotePages 是一个基于 Cloudflare Workers 和 S3 协议（如 Backblaze B2、AWS S3、Cloudflare R2 等）构建的**轻量级、无服务器（Serverless）双栏个人云笔记与文件上传分享系统**。

具备完整的持久化密码锁屏防护，支持全平台自适应高度、拖拽文件平滑上传、多文件列表管理以及无密码限制的单文件快速分享下载。

---

## ✨ 功能特性

* **🔒 全局密码锁屏**：首次进入页面或注销后显示锁屏，只有输入正确密码才能进入主界面进行查看与编辑。密码通过网络安全验证并加密存储于本地，长期有效。
* **📝 双栏云笔记**：左侧支持笔记列表（可按住直接**上下拖拽实时排序并同步云端**），右侧内置秒级无感自动保存、字符实时统计、新建及一键彻底删除。
* **📤 平滑进度条上传**：优化了小文件及高网络带宽下的视觉阻碍，通过原生事件捕获提供极致平滑的 `0% - 100%` 上传进度条过渡，支持点击或直接拖拽文件上传。
* **📋 多功能文件列表**：自动按上传时间倒序排列，集成一键复制外链、无限制直接下载、极速删除等核心管理操作。
* **🔗 独立安全分享链接**：通过复制链接生成的 `/download/:id` 分享文件，**外部访问者无需任何密码即可直接满速下载**，完美实现无阻碍分享。
* **📱 移动端自适应**：原生适配手机和 Pad 屏幕，自动切换为抽屉式单/双栏互补布局，编辑和查看操作依然丝滑。

---

## 🛠️ 环境变量配置 (Environment Variables)

在部署 Cloudflare Workers 时，请在 Settings -> Variables 中添加以下环境变量：

| 变量名称 | 必须 | 示例值 | 说明 |
| :--- | :---: | :--- | :--- |
| `ACCESS_PASSWORD` | **是** | `MySecRet999` | 进入页面的安全访问密码（若不填则默认为 `admin123`） |
| `S3_ENDPOINT` | **是** | `https://s3.us-east-1.backblazeb2.com` | 您的 S3 兼容服务的主 API 终结点 |
| `S3_BUCKET` | **是** | `my-notepages-bucket` | 桶（Bucket）的名称 |
| `S3_ACCESS_KEY` | **是** | `0058b76xxxxxxxxx000001` | S3 凭证密钥 ID |
| `S3_SECRET_KEY` | **是** | `K005Fxxxxxxxxxxxxxxxxxxxxxxx` | S3 凭证私钥（Secret Key） |
| `S3_ENDPOINT_BACKUP`| 否 | `https://s3.us-west-004.backblazeb2.com`| 备份 API 终结点（设置后将开启双终结点随机并发负载） |
| `STORAGE_PREFIX` | 否 | `notepages` | S3 桶内存储目录前缀（默认即为 `notepages/`） |

---

## 🚀 快速部署说明

### 第一步：准备 S3 兼容桶 (以 Backblaze B2 为例)
1. 登录您的存储提供商账户，创建一个**私有（Private）** 的新存储桶（Bucket）。
2. 前往密钥管理（Application Keys），生成一组拥有该桶读写权限的 `Application Key`（即得到 `keyID` 和 `applicationKey`）。

### 第二步：在 Cloudflare 创建 Worker
1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)，依次点击 **"Workers & Pages"** -> **"Create"** -> **"Create Worker"**。
2. 为你的 Worker 起一个名字（例如 `notepages`），点击 **"Deploy"**。
3. 部署成功后，点击 **"Edit Code"** 进入在线编辑器。
4. 清空编辑器中原有的默认代码，将本项目修改好的完整 `index.js` 代码全部复制并粘贴进去。
5. 点击右上角的 **"Save and Deploy"**。

### 第三步：配置环境变量
1. 返回该 Worker 的控制台主页面，点击 **"Settings"** 选项卡。
2. 在左侧菜单中选择 **"Variables"**。
3. 在 **"Environment Variables"** 区域点击 **"Add variable"**，根据上方的 [环境变量配置表](#-环境变量配置-environment-variables) 依次填入您的密码和 S3 配置。
4. 填写完成后，务必点击底部的 **"Save and deploy"** 按钮使其生效。

### 第四步：完成访问
* 点击 Worker 自带的 `*.workers.dev` 域名（或者您自己绑定的自定义独立域名），即可看到优雅的密码输入界面。
* 输入您在 `ACCESS_PASSWORD` 中配置的密码，开启您的专属云端笔记与高效文件分享之旅！

---

## 💡 使用小贴士

1. **登录有效期**：只要您不更换浏览器或者不点击标题栏右侧的 **"🔐 注销登录"** 按钮，系统密码在本地长期有效，免去频繁输入的烦恼。
2. **分享文件**：文件上传完成后，直接点击卡片上的 **"复制链接"**，将其发送给任何人，对方打开该链接可无缝直接下载，完全不受密码屏蔽阻碍。
3. **备份安全**：笔记目录和文件目录在 S3 桶内分别位于 `notes/`、`files/` 以及两个 `_index.json` 索引文件，请勿随意在存储桶后台手动删除这些索引，否则会导致列表无法正常在网页中读取。
