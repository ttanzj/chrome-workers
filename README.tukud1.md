[Workers1-3](README.md) | [workers-domain](README.domain.md)  | [workers-blog](README.blog.md)  | [workers-blog](README.tuku.md) | workers-tukud1

### 这是一个依托cloudflar的workers做前端，依托D1存储建立的个人图库项目，由Ai制作而成。因D1库限制，上传图片最好在2M以下，并且总量不能超先天限制。
一. 创建D1库
自己起名，创建后在控制台粘进以下代码并执行。
```
CREATE TABLE folders (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT NOT NULL UNIQUE);
CREATE TABLE images (id INTEGER PRIMARY KEY AUTOINCREMENT, folder_id INTEGER, name TEXT NOT NULL, data TEXT NOT NULL, mime_type TEXT, created_at INTEGER, FOREIGN KEY (folder_id) REFERENCES folders(id));
INSERT INTO folders (name) VALUES ('Default');
```

三、 创建 Cloudflare KV<br>
进入：Cloudflare Dashboard → Workers & Pages → KV<br>
创建 D1 Namespace：<br>
绑定D1库，
DB|自己建的库名
四、绑定环境变量（密码）<br>
进入 Worker：👉 Settings → Variables → Environment Variables<br>
添加以下变量：<br>
GALLERY_PASSWORD|登录密码
五、绑定自定义域名（可选）<br>
如果你想用自己的域名：<br>

