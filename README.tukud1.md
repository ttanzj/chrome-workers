[Workers1-3](README.md) | [workers-domain](README.domain.md)  | [workers-blog](README.blog.md)  | [workers-tuku](README.tuku.md) | workers-tukud1

### 这是一个依托cloudflar的workers做前端，依托D1存储建立的个人图库项目，由Ai制作而成。因D1库限制，采用了分片上传突破单图2M上限，但太大运行也困难，多库链接可突破到3G总存储。

一、选择 Workers<br>
点击 Create Worker，将代码替换原模板代码。<br>
二、创建D1库<br>
左侧菜单 → 存储和数据库 <br>
点击 D1 SQL Databases  <br>
点击 Create database，自己起名，创建后在控制台粘进以下代码并执行。<br>
```
CREATE TABLE images (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  folder_id INTEGER,
  name TEXT,
  data BLOB,
  mime_type TEXT,
  created_at INTEGER,
  chunk_index INTEGER
);
```
再
```
CREATE TABLE folders (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT
);
```
本程序为突破D1库存储限制，扩容支持最多6个D1库，最少一个就能保证运行，创建后都要执行以上初始化。<br>
三、创建 Cloudflare KV<br>
左侧菜单 → 存储和数据库<br>  
点击 KV<br>
点击 Create a namespace <br>
四、绑定创建的D1库和KV<br>
- D1库绑定主库变量名`DB`，扩展依次`DB2`，`DB3`....`DB6`，绑定自己创建的对应数据库名。<br>
- KV库变量名`INDEX_KV`<br>
五、绑定环境变量（密码）<br>
进入 Worker：👉 Settings → Variables → Environment Variables<br>
添加以下变量：<br>
`GALLERY_PASSWORD`👉 登录密码<br>
六、绑定自定义域名（可选）<br>
如果你想用自己的域名：<br>

