---
title: ActiveRecord(一)
date: 2016-04-01 20:47:18
categories: ruby
tags: [ruby,rails]
---
ActiveRecord(一)
<!-- more -->

<img src="/images/42.png" width="800" height="680" />

<h1>命名和模式约定</h1>

<h3>命名约定</h3>
<img src="/images/43.png" width="800" height="680" />

<h3>模式约定</h3>
<img src="/images/44.png" width="800" height="680" />
<h3>不使用约定</h3>
指定表名
```ruby
self.table_name
```
指定主键
```ruby
self.primary_key
```

<h1>基本操作</h1>

<h3>创建</h3>
```ruby
 User.create(:name => 'xiaowang', :email => 'xiaowang@gmail.com')
 
 @user = User.new(:name => 'xiaowang', :email => 'xiaowang@gmail.com')
 @user.save
```
<h3>读取</h3>
```ruby
 User.all

 User.find  #（主键）
 
 User.find_by(:name => 'xiaowang') #第一个符合条件的
```
<h3>更新</h3>
```ruby
 @user.update(:name => 'xiaowang', :email => 'xiaowang@gmail.com')

 User.update_all(:name => 'nima')
```
<h3>删除</h3>
删除一个数据
```ruby
@user.destroy
```

删除表中所有数据
```ruby
User.destroy_all
```


<h1>数据迁移</h1>

迁移
```ruby
rake db:migrate
```
回滚
```ruby
rake db:rollback
```
```ruby
rake db:rollback STEP=2
```
<h2>单独创建迁移</h2>
<h3>Creatxxx</h3>
```ruby
rails g migration CreateWallets
```
<h3>AddxxxToyyy</h3>
```ruby
rails g migration AddLocationToUser location:string 
```
<h3>RemovexxxFromyyy</h3>
```ruby
rails g migration RemoveLocationFromUser location:string 
```
<h2>change方法</h2>
<h3>table</h3>
<h4>create_table</h4>

<h4>rename_table</h4>

<h4>drop_table</h4>

<h3>join_table</h3>
<h4>create_join_table</h4>

<h4>drop_join_table</h4>

<h3>column</h3>
<h4>add_column</h4>

<h4>rename_column</h4>

<h4>remove_column</h4>

<h3>index</h3>
<h4>add_index</h4>

<h4>rename_index</h4>

<h4>remove_index</h4>

<h3>reference</h3>
<h4>add_reference</h4>

<h4>remove_reference</h4>

<h3>timestamps</h3>
<h4>add_timestamps</h4>

<h4>remove_timestamps</h4>

<h2>up和down方法</h2>
<h2>revert方法</h2>
```ruby
require_relative "20161202100202_create_uesrs"

class RevertCreateUsers < ActiveRecord::Migration
  def change
    revert CreateUsers
  end
end
```

<h2>种子数据</h2>
db/seed.rb
```ruby
15.times do |n|
  User.create(:name => 'male_user_#{n}', :phone_number => '13012312311', :gender => 'male' )
  User.create(:name => 'female_user_#{n}', :phone_number => '13012312311', :gender => 'female' )

end
```
搭建数据库
```ruby
rake db:setup
```
填充种子数据
```ruby
rake db:seed
```
重建数据库
```ruby
rake db:reset
```
设置环境
```ruby
RAILS_ENV=test
```
导出数据库
```ruby
rake db:schema:load
```

<h2>数据验证</h2>
<h3>跳过数据验证</h3>
```ruby
@user.save(validate:false)
```
<img src="/images/45.png" width="800" height="763" />

<h3>是否合法</h3>
valid？
```ruby
@user.valid？
```
invalid？
```ruby
@user.invalid？
```

<h3>错误信息</h3>
```ruby
@user.errors.messages
```
```ruby
@user.errors[:name]
```

<h3>presence</h3>
不为空
```ruby
class User < ApplicationRecord

  validates :name, presence: true

end
```
<h3>absence</h3>
为空
```ruby
  validates :name, absence: true
```
<h3>length</h3>
长度
+ minimum
+ maximum
+ in
+ is
```ruby
  validates :name, length: {maximum: 3}
  validates :name, length: {in: 3..20}
  validates :name, length: {is: 6}
```
<h3>confirmation</h3>
确认
```ruby
  validates :password, confirmation: true
```
<h3>inclusion/exclusion</h3>
范围
```ruby
  validates :province, inclusion: {in: ['北京', '上海']}
  validates :age, exclusion: {in: 0..18}

```
<h3>format</h3>
格式
```ruby
  validates :phone_number, format: {with: /1[0-9]{10}\z/ }
```

<h3>uniqueness</h3>
唯一
```ruby
  validates :name, uniqueness: true
```
不区分大小写
```ruby
  validates :name, uniqueness: {case_sensitive: false}
```




<h3>常用验证选项</h3>
+ allow_nil
+ allow_blank
+ message
+ on
```ruby
  validates :password, confirmation: true
  validates :password, :password_confirmation, presence: {on: :create, message: '密码和确认密码不能为空'}
  validates :password, length: {minimum: 6}


   validates :name, uniqueness: {allow_nil: true}

```

<h3>条件验证</h3>
指定symbol
```ruby
 validates :name, uniqueness: true, :if => :test?
  
  def test?
    false
  end
```
指定字符串
```ruby
  validates :phone_number, uniqueness: true, :if => 'name.nil'
```
指定proc
```ruby
  validates :phone_number, uniqueness: true, :if => Proc.new {name.nil?}
```

```ruby
  with_options if: :test? do
    validates :name, uniqueness: true
    validates :name, uniqueness: true

  end

  def test?
    (1+1 == 2)
  end
```
联合条件
```ruby
  with_options if: :test?, unless: :test2? do
    validates :name, uniqueness: true
    validates :name, uniqueness: true

  end

  def test?
    (1+1 == 2)
  end
  
    def test2?
      (1+1 == 2)
    end
```

<h3>自定义验证方法</h3>
+ validates_with
+ validates_each
```ruby
class MyValidator < ActiveModel::Validator

  def validate(record)
    if record.name.nil?
      record.errors[:name] << '用户名不能为空'
    end
    if record.phone_number.nil?
      record.errors[:phone_number] << '手机号码不能为空'
    end

  end
end


class User < ApplicationRecord

  validates_with MyValidator

end

```

```ruby
class User < ApplicationRecord

  validate :my_validator

  def my_validator
    if name.nil?
      errors[:name] << '用户名不能为空'
    end
  end

end
```
<h3>验证错误</h3>
+ errors
+ errors[:attribute]
+ errors.add
+ errors.size




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
