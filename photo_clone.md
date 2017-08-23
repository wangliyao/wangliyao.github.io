## 关于个人写小爬虫的积累

### 所用gem 
  * gem 'nokogiri'
  * gem "http"  
----
#### 关于爬虫最简单的写法其实并没有想象中的难以理解，具体操作就是获取所需要的网页，解析html，获取你想要的元素，并保存，大部分问题gem包都已经为我们解决了，下面是一个简单的爬虫的代码
```ruby
 #获取所需的http页面tbody
    body = HTTP.get(name).body
      ans = []
     while body != '' 
     #遍历获取所有body页面
      html = body.readpartial
       if html ==nil
         break;
       end
       #获取页面中的html然后讲所需要的img抽取出来，存入数组当中
      doc = Nokogiri::HTML(html)
      doc.search('img').each do |i|
        ans << i.attr('src') if i.attr('src') != nil
      end
     end
```

```ruby
    #获取的所有html链接通过wget下载
    ans.each do |full_url|
      #system "mkdir #{Dir.getwd}/photo "
      cmd = system "wget -r #{Dir.getwd}/ #{full_url}"
      puts '-' * 20
      p cmd
    end
  puts 'success'
```
----
#### 项目地址为 
* > https://github.com/wangliyao/clone_photo
