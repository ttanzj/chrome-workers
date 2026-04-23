[Workers1-3](README.md) | [workers-domain](README.domain.md)  | [workers-blog](README.blog.md) | workers-tuku

### 这是一个依托cloudflar的workers做前端，依托github存储建立的个人图库项目，由Ai制作而成。
一. 准备 GitHub 仓库<br>
你需要：<br>
一个 GitHub 仓库（例如：my-gallery）<br>
分支：main 或你自定义（代码里 GITHUB_BRANCH 要一致）<br>
用来存储图片文件<br>
2. 获取 GitHub Token<br>
进入：GitHub → Settings → Developer settings → Personal access tokens<br>
权限要勾：<br>
repo（必须）<br>
如果是 fine-grained token：<br>
Contents: Read & Write<br>
复制 token（只显示一次）<br>
二、创建 Worker（网页端）<br>
步骤 1：进入 Worker<br>
👉 选择“Hello World Worker”<br>
步骤 2：删除默认代码<br>
步骤 3：粘贴你的完整代码<br>
三、 创建 Cloudflare KV<br>
进入：Cloudflare Dashboard → Workers & Pages → KV<br>
创建 KV Namespace：<br>
GALLERY_KV<br>
绑定<br>
四、绑定环境变量（非常关键）<br>
进入 Worker：👉 Settings → Variables → Environment Variables<br>
添加以下变量：<br>

名称|值
---|---
GITHUB_OWNER|你的 GitHub 用户名
GITHUB_REPO|仓库名
GITHUB_BRANCH|main（或你的分支）
GITHUB_TOKEN|你的 GitHub Token
GALLERY_PASSWORD|登录密码
五、绑定自定义域名（可选）<br>
如果你想用自己的域名：<br>

