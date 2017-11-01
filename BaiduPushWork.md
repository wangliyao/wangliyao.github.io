## 百度站长推送，推送文章信息给百度，由百度抓取

```ruby 
require 'net/http'
urls = ['http://www.example.com/1.html', 'http://www.example.com/2.html']
uri = URI.parse('http://data.zz.baidu.com/urls?appid=1536768227928825&token=dpVt55TFcSh1xIue&type=realtime')
req = Net::HTTP::Post.new(uri.request_uri)
req.body = urls.join("\n")
req.content_type = 'text/plain'
res = Net::HTTP.start(uri.hostname, uri.port) { |http| http.request(req) }
puts res.body
```
```ruby
 # 返回参数格式
 {
    "remain": 4999999, #当天剩余可推送到新增内容的url条数 
    "success": 1, # 当天成功推送到新增内容的url条数
    "remain_realtime": 9,
    "success_realtime": 1
}
```

