---
title: 使用python字典get方法的默认值避免key error
date: 2019-01-20
tag: [2019年, python, python3网络爬虫]
category: 我爱编程
---

## python字典dict 

```python
seen = {}
seen.get(url,0)
```

使用if语句判断字典seen是不是空的，如果为空则将0赋值给seen[url];如果不添加if else判断，字典seen中没有url这个key值，则会出现key error的报错；具体代码如下：

```python
def link_crawler(seed_url, link_regex, delay=3, max_depth=2):
    max_depth = max_depth
    throttle = Throttle(delay)
    crawl_queue = [seed_url]
    #seen = set([])
    seen = {}
    while crawl_queue:
        print(crawl_queue)
        url = crawl_queue.pop()
        throttle.wait(url)
        html = download(url)
        print(url)
        # 当get方法获取不到值是使用默认值，这里的默认值是0
        depth = seen.get(url,0)
        if depth != max_depth:
            for link in get_links(html):
                if re.match(link_regex, link):
                    full_link = urllib.parse.urljoin(seed_url,link)
                    if full_link not in seen:
                        seen[full_link] = depth + 1
                        crawl_queue.append(full_link)
                        #print(full_link)
```

