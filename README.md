Workers1-3 | [workers-domain](README.domain.md)  | [workers-blog](README.blog.md)| [workers-blog](README.tuku.md)


### Workers1\2\3是三个chromego节点提取工具


## workers-1

部署提醒在 Cloudflare 仪表盘创建 KV Namespace（名称随意）  
在 Worker → Settings → Bindings → KV Namespace Bindings 添加：Variable name：PROXY_KV  
KV namespace：选择你创建的那个  
chromego地址为26.3.6下载最新版新提取地址  
首次访问会较慢（抓取几十个源），之后从 KV 读取几乎瞬时。  

## workers-2  

chromego地址为原其他作者旧地址

## workers-3

代码最长，chromego地址为原其他作者旧地址加入了其他地址，得到节点不少，但 chromego节点一般，其他节点能用的不多
