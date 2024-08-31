# 介绍

基于Cloudflare Workers的Telegraph图床目前提供D1和KV两个版本，二者的主要区别在于存储位置。D1版本使用Cloudflare D1数据库进行存储，而KV版本则使用Cloudflare KV空间。

# 功能

- 支持多文件上传。
- 支持上传大于5MB的图片。
- 在图床界面中可以直接粘贴上传。
- 选择图片后会自动上传，使用方便。
- 管理界面支持查看和播放MP4文件。
- 显示上传时间，并支持按上传时间排序。
- 支持修改后台路径为 /admin，可在代码的第二行进行调整。
- 图片管理功能可通过访问域名 /admin 实现，且图片支持懒加载。
- 仅允许代理自己上传的图片，无法访问通过其他TG图床上传的链接。
- 支持JPEG、JPG、PNG、GIF和MP4格式，GIF和MP4的大小需≤5MB。
- 支持URL、BBCode和Markdown格式，点击对应按钮可自动复制相应格式的链接。
- 选择图片后会自动压缩，以节省Cloudflare和Telegraph的存储空间，同时加快上传速度。
- 对于需要自定义用户界面的用户，您可以自行修改代码。在修改时希望您能**保留项目的开源地址**。

# 效果图
![](http://kycloud3.koyoo.cn/2024082909911202408291512219124.png)
![](http://kycloud3.koyoo.cn/2024082977e50202408291512218799.png)
![](http://kycloud3.koyoo.cn/20240829639cf202408291512214544.png)

# 1. D1数据库限制

- [D1数据库限制详情](https://developers.cloudflare.com/d1/platform/limits/)
- 对于个人用户，500MB的免费存储空间足够用于储存图片链接使用。

| 类别                                                         | 限制                                     |
| :----------------------------------------------------------- | :--------------------------------------- |
| 数据库数量                                                   | 50,000 (付费用户) beta / 10 (免费用户)   |
| 最大数据库大小                                               | 2 GB (付费用户) beta / 500 MB (免费用户) |
| 每个帐户的最大存储空间                                       | 50 GB (付费用户) beta / 5 GB (免费用户)  |
| [Time Travel](https://developers.cloudflare.com/d1/learning/time-travel/) 间隔时间 (时间点恢复) | 30 days (付费用户) / 7 days (免费用户)   |
| 最大 Time Travel 还原操作数                                  | 每 10 分钟 10 次还原（每个数据库）       |
| 每个工作线程调用的查询数（读取子请求限制）                   | 50 (**Bundled**) / 1000 (Unbound)        |
| 每个表的最大列数                                             | 100                                      |
| 每个表的最大行数                                             | 无限制（不包括每个数据库的存储限制）     |
| 最大字符串或 `BLOB` 表行大小                                 | 1,000,000 bytes (1 MB)                   |
| 最大 SQL 语句长度                                            | 100,000 bytes (100 KB)                   |
| 每个查询的最大绑定参数数                                     | 100                                      |
| 每个 SQL 函数的最大参数数                                    | 32                                       |
| `LIKE` 或 `GLOB` 模式中的最大字符数（字节）                  | 50 bytes                                 |
| 每个工作线程脚本的最大绑定数                                 | 约5,000 人                               |

# 2. KV限制

- [KV键值限制详情](https://developers.cloudflare.com/kv/platform/limits/)
- 对于个人用户，免费额度足够用于储存图片链接使用。

| 类别            | 免费用户            | 付费用户   |
| :-------------- | :------------------ | :--------- |
| 读              | 每天 100,000 次读取 | 无限       |
| 写入不同的键    | 每天 1,000 次写入   | 无限       |
| 写入同一密钥    | 每秒 1 次           | 每秒 1 次  |
| 操作/worker调用 | 1000                | 1000       |
| 命名空间        | 100                 | 100        |
| 存储/帐户       | 1 GB                | 无限       |
| 存储/命名空间   | 1 GB                | 无限       |
| 键/命名空间     | 无限                | 无限       |
| 键大小          | 512 bytes           | 512 bytes  |
| 键元数据        | 1024 bytes          | 1024 bytes |
| 值大小          | 25 MiB              | 25 MiB     |
