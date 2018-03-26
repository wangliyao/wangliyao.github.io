## actioncable 在rails api 中使用
```ruby 
class PostChannel < ApplicationCable::Channel
  def subscribed
    logger.warn "current connections: #{ActionCable.server.connections.count}"
    stream_from "post:#{params[:id]}:channel"
    $redis.zadd("post_#{params[:id]}_channel", Time.now.to_i, params[:username])
    broadcast_to(params[:id])
  end

  def unsubscribed
    $redis.zrem("post_#{params[:id]}_channel", params[:username])
    broadcast_to(params[:id])
  end

  private

  def broadcast_to(id)
    num = $redis.zrange("post_#{id}_channel", 0, -1)
    $redis.del("post_#{id}_channel") if num.blank?
    ActionCable.server.broadcast "post:#{id}:channel", num
  end
end
```
### 其他Gem 使用 有很好的文章参阅
[Rails 365 websocket](https://www.rails365.net/articles/websocket-xu-lie-wen-zhang-mu-lu)
