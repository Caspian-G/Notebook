#### needed

```
pip install requests beautifulsoup4 lxml selenium scrapy
```

`requests`：发送 HTTP 请求，模拟浏览器访问网站
`beautifulsoup4`（简称 bs4）：解析 HTML，提取数据
`lxml`：bs4 的高效解析器
`selenium`：处理动态页面（如 JavaScript 加载）
`scrapy`：专业爬虫框架

#### 基本流程

用 requests 获取网页内容

用 bs4 或 xpath 提取有用信息

存到文件、数据库或 Excel

**注** Headers：模拟浏览器（如 User-Agent）

#### Robots协议 (君子协议)

打开任何网站 后面加上 `/robots.txt`

