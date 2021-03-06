﻿人的一生要经历太多的生离死别，那些突如其来的离别往往将人伤得措手不及。
人生何处不相逢，但有些转身，真的就是一生，从此后会无期，永不相见。
用力爱过的人，讲再见那一刻格外艰难。
世界上最遥远的距离不是生离死别，而是对方已经云淡风轻，你却念念不忘。

——网友评论

一直很喜欢这首歌。正好学习python ，想着把这首歌的热门评论爬下来，看看网友的故事。

网易云音乐是一个有情怀的地方。大多数想说却没有办法说出口的话都能留在评论里。

费话不说了，开工。

### 获取数据
打开网易云音乐，找到歌曲春分十里不如你，打开开发者工具，分析页面。
![页面内容](https://upload-images.jianshu.io/upload_images/13161325-a070dfc506dd8c88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过 搜索框 url 的 id 可以定位到评论的 url 。看到请求是 post 方法的，我们可以看到下边的两个参数。
```python
params: 12PSl54ZzScPr+B27R+RJ14gF4YwwNz8YqWdldaCKao1s1/JexmIcnpaQu7oAkXM96vPBpEo42vFSp3BydeeYs6TKv/72oKRITbhg8hUP2vwsNW+hq8VfDvmjcq+ceScl9wEb3Wh6Whnu85Th7jHK4lNNKxNSJakjxuVnNcCDteI76F2xviD4jDcz9upF8CY
encSecKey: 227faa18d4ac5f4dfa07b4f0664bcb181240fcfb74192d7ce86b19ce302c61c8a5f2cbf45fc8874b5d74f0f6320f7681eef36e3f3a4d8349eed908188aae9717dd64f4d678e1d15afb8f06b559ebd51b2bca7b225f274378d89c068e18f7f8d45f7019e6923c2a0da30a4b68ecdfe2d6dcb954c3cfb0ec8812da693944617678
```
至于这两个参数怎么破解，小弟确实不会，不过有知乎大神已经破解过了，想要了解的可以移步知乎获取方法。

我们就直接拿到这两个参数去请求就好了。
```python
# 获取评论数据
def get_data():
    url = 'https://music.163.com/weapi/v1/resource/comments/R_SO_4_38576323?csrf_token='
    headers = {'Host': 'music.163.com',
               'Origin': 'https://music.163.com',
               'Referer': 'https://music.163.com/song?id=38576323',
               'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36'
               }
    data = {'params': '12PSl54ZzScPr+B27R+RJ14gF4YwwNz8YqWdldaCKao1s1/JexmIcnpaQu7oAkXM96vPBpEo42vFSp3BydeeYs6TKv/72oKRITbhg8hUP2vwsNW+hq8VfDvmjcq+ceScl9wEb3Wh6Whnu85Th7jHK4lNNKxNSJakjxuVnNcCDteI76F2xviD4jDcz9upF8CY',
            'encSecKey': '227faa18d4ac5f4dfa07b4f0664bcb181240fcfb74192d7ce86b19ce302c61c8a5f2cbf45fc8874b5d74f0f6320f7681eef36e3f3a4d8349eed908188aae9717dd64f4d678e1d15afb8f06b559ebd51b2bca7b225f274378d89c068e18f7f8d45f7019e6923c2a0da30a4b68ecdfe2d6dcb954c3cfb0ec8812da693944617678'}
    response = requests.post(url,headers=headers,data=data)
```
请求完发现数据是 json 格式的，那么获取就没有什么难度了。
```python
    html = json.loads(response.text)
    result = []
    for item in html['hotComments']:
        content = {'user':item['user']['nickname'],
                  'likedCount':item['likedCount'],
                   'content':item['content']
                   }
        result.append(content)
    return result

```
### 可视化操作
获取数据之后，我们把数据做的直观一点，便于查看。

![直方图](https://upload-images.jianshu.io/upload_images/13161325-9290f8ec918e1c45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
看一下点赞首位的评论：

听完这首歌，立刻定了下午五点的机票去北京找你。所以这次我用尽全力想对你说，你可不可以答应我，在我考上研究生到北京找你之前，你先不要结婚。不管我多么不喜欢北京，可是北京有你。没有你的杭州再美，也比不上有你的北京。

因为一个人爱上一座城。没有你的城市再好的风景也淡然无味。

![词云图](https://upload-images.jianshu.io/upload_images/13161325-412e5e466e8cb70d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 结语

总有一天，会有一个人，看你写过的所有状态，读完写的所有微博，看你从小到大的所有照片，甚至去别的地方寻找关于你的信息，试着听你听的歌，走你走过的地方，看你喜欢看的书，品尝你总是大呼好吃的东西……只是想弥补上，他迟到的时光。

