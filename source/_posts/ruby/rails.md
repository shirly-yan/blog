---
title: rails
date: 2016-11-18 22:01:18
categories: [ruby]
---
rails
<!-- more -->
<h2>常用命令</h2>
<h3>创建</h3>
```bash
rails new name
```
```bash
rails new name --skip-bundle
```

<h3>开启服务</h3>
```bash
rails server
```
```bash
rails server -p 2000
```

<h2>bundle</h2>
```bash
bundle install
```

<h2>路由routes</h2>
<h3>查看所有路由信息</h3>
```bash
rake routes
```
<h3>设置根路由</h3>
```ruby
root 'controller_name#action_name'
```
`
<h3>一般路由</h3>
创建
```ruby
  get 'names/:id', :to => 'names#actionname'
```
调用
```ruby
 <%= link_to 'xxxxxxx',{:controller => 'names', :action => 'actionname', :id => 1} %>
```
<h3>命名路由</h3>
创建
```ruby
  get 'names/:id', :to => 'names#actionname', :as => 'names_actionname'
```
调用
```ruby
<%= link_to 'xxxxxxx',names_actionname_path(1) %>
<%= link_to 'xxxxxxx',names_actionname_path %>
```
<h3>资源路由resources</h3>
        请求  URL
+ index get  /books
+ creat post /books
+ show  get  /books/[:id]  
+ new   get  /books/new

设置
```ruby
resources :names
```

```ruby
  resources :names, :except => :show
```
<h4>集合路由</h4>
```ruby
  resources :posts do
    get 'recent', :on => :collection
  end
```
```ruby
  resources :posts do
    collection do
      get 'recent'
    end
  end
```
<h4>成员路由</h4>
```ruby
  resources :posts do
    get 'recent', :on => :member
  end
```
```ruby
    member do
      get 'recent' #post/:id/recent
    end
  end
```

```ruby
  resources :posts do
    collection do
      get 'recent' #post/recent
    end
    member do
      get 'recent' #post/:id/recent
    end
  end
```
<h2>db</h2>
```ruby
rake db:migrate
```

<h2>generate</h2>
<h3>scaffold</h3>
```ruby
 rails generate scaffold name
```
```ruby
 rails generate scaffold product name price:decimal description:text
```
<h3>controller</h3>
生成
```ruby
rails generate controller controllername
```
删除
```ruby
rails destroy controller controllername
```
生成
```ruby
rails generate controller controllername actionname1 actionname2
```
<h3>model</h3>
生成
```ruby
rails generate model modelname name1 name2:string
```
添加关联字段
```ruby
rails generate migration add_user_id_to_posts user_id:id
```





<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
