# -author:  加v：wp-pro
​ 本文以获取图书易经（The Book of Changes）在亚马逊官网（Amazon.com）的评论数据为例，获取商品的评分、评论日期、评论内容等数据。首先进入商品详情页
​
下拉滚动条找到‘See all reviews’标签并点击
 打开开发者工具，点击Network选项卡，再点击Fetch/XHR选项卡进行抓包分析。连续点击Next page按钮，多抓取几个请求以便于全面分析。
 通过观察发现，每一页的评论内容都在我们抓取的请求所返回的response里，第二个请求之后的所有请求的Request URL规律非常明显
 ​
Request URL是由https://www.amazon.com/hz/reviews-render/ajax/reviews/get/ref=cm_cr_getr_d_paging_btm_next_ 加上一个从3开始的数字组成。接下来对从3开始的3个url发起请求（均为post请求），寻找请求所携带的cookies，headers以及data参数的规律，这里借助写爬虫网站（Convert curl commands to code）帮助我们写单一请求的爬虫程序。

1、选中请求，右键--Copy--Copy as cURL(bash)，在爬虫网站的输入框内粘贴，得到爬虫代码

​通过对比发现，cookies完全相同，只有headers变量中的referer（防盗链）参数以及data中的pageNumber、reftag、scope参数发生了变化，变化的规律一目了然，单纯的是数字的增加，这时我们便可以通过构造参数循环爬取评论内容了获取到response之后，接下来就要对数据进行解析和存储了。数据解析部分主要是对响应内容进行观察分析，对数据进行简单的处理，然后运用xpath对数据进行提取，这里就不再进行详细讲解。
