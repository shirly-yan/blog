---
title: ActiveRecord(三)
date: 2016-04-05 14:31:10
categories: ruby
tags: [ruby,rails]
---
查询
<!-- more -->

<h2>从数据库中获取数据</h2>
+ find
+ first
+ take
+ last
+ find_each
```ruby
User.find_each(batch_size: 3) do |user|
  puts user.id
end
```
+ find_in_batches
```ruby
User.find_in_batches(batch_size: 3) do |users|
  puts users.count
end
```

<h2>条件查询</h2>
<h3>>where</h3>
```ruby
User.where("name = 'mazi'")

User.where("name = 'mazi' AND id = 39")

str = 'maizi'
User.where("name = ?", str)

User.where("id > 52")

num = 55
User.where("id < ?", num)

User.where("id < ? AND name = ?", num, str)

User.where(:name => 'maizi', :id => 55)

User.where.not(:name => 'maizi', :id => 55)
```

<h2>排序</h2>
升序
```ruby
User.where.not(:name => 'maizi', :id => 55).order(:created_at => :ASC)
```
降序
```ruby
User.where.not(:name => 'maizi', :id => 55).order(:created_at => :DESC)
```
<h2>查询指定字段</h2>
```ruby
User.where.not(:name => 'maizi', :id => 55).order(:created_at => :DESC).select(:name)
```
<h2>分页查询</h2>
+ limit
+ offset

```ruby
User.where.not(:name => 'maizi').order(:created_at => :DESC).select(:name, :created_at).limit(1).offset(1)
```

```ruby

time = 0
User.where.not(:name => 'maizi').order(:created_at => :DESC).select(:name, :created_at).limit(10).offset(time)

time = 1
User.where.not(:name => 'maizi').order(:created_at => :DESC).select(:name, :created_at).limit(10).offset(time)

time = 2
User.where.not(:name => 'maizi').order(:created_at => :DESC).select(:name, :created_at).limit(10).offset(time)
```
<h2>分组查询</h2>
```ruby
i = 0
User.find_each do |user|
  if i%2 == 0
    user.gender = '女'
  else
    user.gender = '男'
  end
  user.money = i+20
  user.save
  i += 1
end
```


```ruby
result = User.select("gender", "sum(money) as total_money").group("gender")

result.first

result.first.total_money
```

```ruby
result = User.select("gender", "sum(money) as total_money").group("gender")

result.first

result.first.total_money
```
```ruby
result = User.select("gender", "sum(money) as total_money").group("gender").having("sum(money) > ?", 120)
```

<h2>条件覆盖</h2>
+ unscope
+ only
+ reorder
+ reverse_order
+ rewhere
```ruby
User.select("id", "created_at").limit(5).unscope(:limit)

User.select("id").limit(5).offset(2).only(:limit, :offset)

User.select("id").order(:id => :DESC)

User.select("id").order(:id => :DESC).reorder(:id)


User.select("id").order(:id => :DESC).reorder(:id)

User.select("id").order(:id => :DESC).reverse_order

```
<h2>只读对象</h2>
```ruby
User.readonly.first
```








<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
