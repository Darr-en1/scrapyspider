### 命令行模式:

scrapy shell url

scrapy startproject mySpider

scrapy genspider zhihu "zhihu.com"

### setting 中的配置说明：

ROBOTSTXT_OBEY 是否遵循网站robot协议，遵循则部分不被允许的网页则不被爬取，一般都会 设置为False

ITEM_PIPELINES value指代的数字表示pipelines的优先级，越小权限越大

自动下载图片： https://docs.scrapy.org/en/latest/topics/media-pipeline.html

setting.py 设置

```python
IMAGES_STORE = os.path.join(PROJECT_DIR, 'images')
IMAGES_URLS_FIELD = 'thumbnail'
IMAGES_RESULT_FIELD = 'thumbnail_path'
```

注意：需要 `pip install Pillow` 否则不会下载

原声不支持阿里云oss ,可以实现，参考 https://brucedone.com/archives/1160

模拟登陆 需要使用cookie 则设置settings.py `COOKIES_ENABLED=True`,则一次爬虫的多次请求的cookie会被向下传递

### 爬虫管理部署

#### scrapyd

https://github.com/scrapy/scrapyd

下载：`pip install scrapyd`

运行：`scrapyd`

##### 部署

下载：`pip install scrapyd-client`

编辑 `scarpy.cfg`文件

```buildoutcfg
[settings]
default = scrapyspider.settings

[deploy]
url = http://localhost:6800/
project = scrapyspider
```

多台机器部署

```buildoutcfg
[settings]
default = scrapyspider.settings

[deploy：vm1]
url = http://192.168.1.1:6800/
project = scrapyspider
[deploy：vm2]
url = http://192.168.1.2:6800/
project = scrapyspider
[deploy：vm3]
url = http://192.168.1.3:6800/
project = scrapyspider
```

项目部署：`scrapyd-deploy scrapyspider -p scrapyspider`

运行spider：`curl http://localhost:6800/schedule.json -d project=scrapyspider -d spider=cnblogs`

#### Gerapy

下载：`pip install gerapy`

初始化: `gerapy init`

生成表
```shell
cd gerapy
gerapy migrate
```

创建用户：`gerapy initadmin`

运行：`gerapy runserver`

[Gerapy 分布式爬虫管理框架教程](https://cuiqingcai.com/4959.html)



