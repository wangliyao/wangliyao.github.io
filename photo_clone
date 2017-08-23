```ruby
    body = HTTP.get(name).body
      ans = []
       while body != ''
     html = body.readpartial
     if html ==nil
      break;
     end
     doc = Nokogiri::HTML(html)
     doc.search('img').each do |i|
      ans << i.attr('src') if i.attr('src') != nil
     end
   end
```
