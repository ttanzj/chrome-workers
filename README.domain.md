# domain-check
这是一个简洁高效的域名可视化展示面板，基于Cloudflare Workers构建。它提供了一个直观的界面，让用户能够一目了然地查看他们域名的状态、注册商、注册日期、过期日期和使用进度，并可在到期前通过TG机器人向用户推送通知。

## 2024-11-11 更新：每天只进行一次 TG 通知
- 创建一个KV命令空间：名称随意，假设为`DOMAINS_TG_KV`
- 在 workers 或 pages 的设置里，绑定 kv 空间，变量名为`DOMAINS_TG_KV`（不能修改），绑定上一步中新建的 kv 空间
- 最新的代码为仓库中的 `DOMAINS_TG_KV.js` 文件


## 部署方法

**worker 部署**

在cf中创建一个workers，复制`worker_kv`中的代码到workers中，点击保存并部署。

## 变量设置
```
变量名            类型                 作用说明  
SITENAME         String               自定义网站标题。如果设置了，会覆盖默认的 sitename。  
PASSWORD         String               访问密码。如果设置了该变量，则启用密码保护功能。  
DOMAINS_KV       KV Namespace         用于持久化存储域名列表数据（必须绑定）。  
TGID             String               Telegram 聊天 ID（可选）。  
TGTOKEN          String               Telegram Bot Token（可选）。  
DAYS             String               提前提醒天数（可选,会转为数字）。  
DOMAINS_TG_KV    KV Namespace         用于记录当天是否已发送过 Telegram 提醒，防止重复推送（可选）。
```

## 使用方法

双击标题区域，出现编辑、删除、添加新记录，拖动改变顺序，再次双击消失。也可双击域名，进入该域名编辑。
