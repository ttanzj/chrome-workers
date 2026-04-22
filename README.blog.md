[workers1-3](README.md)  | [workers-domain](README.domain.md) | workers-blog


## 本workers由AI创建的用D1数据库做存储的一个简易文档管理系统，主要为自己保存一些想法和技术资料，写法采用markdown，适配桌面和移动端。以下是该文档系统在 Cloudflare 上的完整部署指南、配置说明以及 Markdown 语法详细手册。

### 一、 部署过程 (Cloudflare 网页端)
1. 创建 D1 数据库
登录 Cloudflare 控制台，点击左侧菜单栏的 Storage & Databases (存储与数据库) -> D1。

点击 Create database -> Dashboard。

输入数据库名称（建议命名为 DB），点击 Create。

进入新建的数据库页面，点击 Console 标签页，在输入框中粘贴以下 SQL 语句并点击 Execute 执行（用于初始化表结构）：
```
CREATE TABLE docs (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT NOT NULL,
  category TEXT,
  tags TEXT,
  content TEXT,
  pinned INTEGER DEFAULT 0,
  created_at TEXT,
  updated_at TEXT
);
```
2. 创建 Workers 并部署代码
点击左侧菜单栏的 Compute (计算) -> Workers & Pages -> Create application。

点击 Create Worker，随便起个名字（如 my-doc-system），点击 Deploy。

点击 Edit Code，将编辑器中原有的代码全部删除，粘贴我刚才提供给你的完整代码。

点击右上角的 Save and deploy。

3. 绑定数据库与环境变量 (关键步骤)
代码部署后还不能直接运行，需要连接数据库和设置密码：

回到该 Worker 的详情页面，点击 Settings (设置) 选项卡。

绑定数据库：

点击左侧的 Bindings (绑定)。

点击 Add binding -> D1 database。

Variable name (变量名) 必须填写：DB（代码中通过 env.DB 调用）。

D1 database 选择你刚才创建的数据库。

点击 Save。

设置登录密码：

点击左侧的 Variables (变量)。

在 Environment Variables 处点击 Add variable。

Variable name 填写：PASSWORD。

Value 填写你想要的登录密码。

点击 Save and deploy。

现在，你可以通过 Worker 提供的 *.workers.dev 域名访问你的文档系统了。
### 二、 Markdown 支持的正则列表
当前代码通过原生正则表达式实现了一套轻量级解析引擎，其支持情况如下：
1. 已支持语法 (Supported)
```
   功能            写法示例                        文字解释                          易错写法/注意事项
多级标题           # 标题1 到 ###### 标题6          行首以 1-6 个 # 开头              后跟一个空格。,错误： #标题 (没空格) 或 ###标题 ### (闭合符)。
代码块             ```code```                      使用前后各三个反引号包裹多行文字。  注意： 仅支持简单显示，无语法高亮。
引用块             > 引用文字                       行首以 > 开头，后跟空格。          注意： 仅支持单层引用，不支持嵌套引用。
粗体               **文字**                         双星号包裹。                      注意： 中间不能有空格，如 ** 文字 ** 可能失效。
斜体              *文字*                           单星号包裹。                       注意： 容易与无序列表混淆，建议紧贴文字。
粗斜体            ***粗斜体***                     三星号包裹。                        -
删除线            ~~删除内容~~                      双波浪线包裹。                     -
行内代码          `code`                           单反引号包裹。                     错误： 使用了中文引号或单引号。
超链接            [百度](https://baidu.com)        方括号存标题                       圆括号存链接。,错误： (标题)[链接] (顺序反了)。
图片              ![说明](图片URL)                 感叹号开头                         随后同链接写法。,注意： URL 必须是可直连的公网地址。
无序列表           * 列表 或 - 列表                 行首以 * 或 - 加空格。             注意： 必须位于新行。
换行              直接回车                         渲染时自动将换行符转为 <br>。        -
```
2. 不支持的语法 (Unsupported)
由于是基于正则的精简版解析，以下高级 Markdown 功能目前不支持：

有序列表： 如 1. 第一点, 2. 第二点（会被视为普通文本）。

表格： 如 | 标题 | 标题 |（不会渲染为 HTML 表格）。

嵌套列表： 缩进形式的二级列表无法识别。

数学公式： 不支持 LaTeX 渲染（如 $E=mc^2$）。

任务列表： 不支持 - [ ] 任务 复选框。

水平分割线： 不支持 --- 或 *** 单独成行。

脚注/锚点： 不支持 [^1] 或特殊的 ID 链接。
3.本程序内行与行之间不用空一行。

