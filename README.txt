#htcap简单介绍

开源Web漏洞扫描工具，它通过拦截AJAX调用和页面DOM结构的变化并采用递归的形式来爬取单页面应用（SPA）。htcap并不是一款新型的漏洞扫描工具，因为它主要针对的是漏洞扫描点的爬取过程，然后使用外部工具来扫描安全漏洞。在htcap的帮助下，我们就可以通过手动或自动渗透测试来对现代Web应用进行漏洞扫描了。

htcap的扫描过程分为两步，htcap首先会尽可能地收集待测目标可以发送的请求，
例如url、表单和AJAX请求等等，然后将收集到的请求保存到一个SQLite数据库中。爬取工作完成之后，
我们就可以使用其他的安全扫描工具来测试这些搜集到的测试点，最后将扫描结果存储到刚才那个SQLite数据库之中。

htcap内置了sqlmap和arachni模块，sqlmap主要用来扫描SQL注入漏洞，而arachni可以发现XSS、XXE、代码执行和文件包含等漏洞。

htcap所采用的爬虫算法能够采用递归的方式爬取基于AJAX的页面，htcap可以捕获AJAX调用，然后映射出DOM结构的变化，
并对新增的对象进行递归扫描。当htcap加载了一个测试页面之后，htcap会尝试通过触发所有的事件和填充输入值来触发AJAX调用请求，
当htcap检测到了AJAX调用之后，htcap会等待请求和相关调用完成。如果之后页面的DOM结构发生了变化，
htcap便会用相同算法对新增元素再次进行计算和爬取，直到触发了所有的AJAX调用为止。

#爬取范围

htcap可以指定爬取范围，可选范围包括：域名、目录和url。如果范围是域名的话，htcap只会爬取给定的域名地址；
如果范围为目录，那么htcap将会爬取指定目录以及该目录下的所有子目录；如果设置的是url，那么htcap将只会分析单个页面。



#命令行参数

$ htcap crawl -h
usage: htcap [options] url outfile
Options:
-h 帮助菜单
-w 覆盖输出文件
-q 不显示处理过程信息
-m MODE 设置爬取模式：
- passive:不与页面交互
- active:触发事件
- aggressive:填写输入值并爬取表单 (默认)
-s SCOPE 设置爬取范围：
- domain:仅爬取当前域名 (默认)
- directory:仅爬取档期那目录 (以及子目录)
- url: 仅分析单一页面
-D 最大爬取深度 (默认: 100)
-P 连续表单的最大爬取深度 (默认: 10)
-F 主动模式下不爬取表单
-H 保存页面生成的HTML代码
-d DOMAINS 待扫描的域名，多个域名用逗号分隔 (例如*.target.com)
-c COOKIES 以JSON格式或name=value键值对的形式设置cookie，多个值用分号隔开
-C COOKIE_FILE 包含cookie的文件路径
-r REFERER 设置初始引用
-x EXCLUDED 不扫描的URL地址，多个地址用逗号隔开
-p PROXY 设置代理，protocol:host:port - 支持'http'或'socks5'
-n THREADS 爬虫线程数量 (默认: 10)
-A CREDENTIALS 用户HTTP验证的用户名和密码，例如username:password
-U USERAGENT 设置用户代理
-t TIMEOUT 分析一个页面最长可用时间 (默认300)
-S 跳过初始url检测
-G 分组query_string参数
-N 不使用标准化URL路径 (保留../../)
-R 最大重定向数量 (默认10)
-I 忽略robots.txt
