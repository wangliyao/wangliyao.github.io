##Rails N+1查询及解决方式

Rails在module层建立关联后，进行关联查询时会出现N+1查询的情况

```ruby
class Post < ApplicationRecord
  has_many :comments
end

class Comment < ApplicationRecord
  belongs_to :post
end

> Post.count
(0.1ms)  SELECT COUNT(*) FROM "posts"
=> 4

> Post.all.map{|post| post.comments}
  Post Load (0.3ms)  SELECT "posts".* FROM "posts"
  Comment Load (0.2ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = ?  [["post_id", 1]]
  Comment Load (0.4ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = ?  [["post_id", 2]]
  Comment Load (0.6ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = ?  [["post_id", 3]]
  Comment Load (0.6ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = ?  [["post_id", 4]]
```
`一共有4个查询结果，但是却查询了5次，这个就出现了N+1查询`

##解决方式

ActiveRecord提供了对module层预加载的功能

分别为

* `includes`
* `preload`
* `eager_load`

```ruby
> Post.includes(:comments).map{|post| post.comments}
  Post Load (0.2ms)  SELECT "posts".* FROM "posts"
  Comment Load (0.5ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" IN (1, 2, 3, 4)
```
`更优雅的方式参见gem goldiloader`

####关于rails N+1问题
 在rails处理关联关系的时候出现，如果使用普通的查询，会先去执行查找父的条件，然后在去逐一查询子表的关联数据，如果使用include查询，则会只进行两次的条件查询
 减少与数据库的交互，一定程度上增加程序的查询效率，是比较常见的开发中会出现的问题
