---
title: ActiveRecord(二)
date: 2016-04-04 14:38:26
categories: ruby
tags: [ruby,rails]
---
ActiveRecord 回调
<!-- more -->

<h2>回调</h2>

<h3>创建对象顺序</h3>

```ruby
class User < ApplicationRecord
  before_validation :beforeValidation
  after_validation :afterValidation
  before_save :beforeSave
  before_create :beforeCreate
  after_create :afterCreate
  after_save :afterSave



  def beforeValidation

  end
  def afterValidation

  end
  def beforeSave

  end
  def beforeCreate

  end
  def afterCreate

  end
  def afterSave
    
  end
end
```

<h3>更新对象顺序</h3>

```ruby
class User < ApplicationRecord
  before_validation :beforeValidation
  after_validation :afterValidation
  before_save :beforeSave
  before_update :beforeUpdate
  after_update :afterUpdate
  after_save :afterSave



  def beforeValidation

  end
  def afterValidation

  end
  def beforeSave

  end
  def beforeUpdate

  end
  def afterUpdate

  end
  def afterSave
    
  end
end
```
<h3>销毁对象顺序</h3>

```ruby
class User < ApplicationRecord
  before_destroy :beforeDestroy
  after_destroy :afterDestroy



  def beforeDestroy

  end
  def afterDestroy

  end
  
end
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->

