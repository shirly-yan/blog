---
title: 用string或symbol访问hash
date: 2017-01-04 13:53:37
categories: ruby
tags: [ruby,rails]
---
用string或symbol访问hash
<!-- more -->
Accessing a hash with either string or symbol keys
For a normal Ruby hash, the following code is true:
```ruby
x = {"key1" => "value1"}
x["key1"] #=> "value1"
x[:key1] #=> nil
```
What if we want to use either x[:key1] or x["key1"] and get the same result?
Rails gives us a new Hash called HashWithIndifferentAccess that does just that. If we have a normal hash and we want to convert it into HashWithIndifferentAccess, we do the following:
```ruby
x = {"key1" => "val1"}
x = x.with_indifferent_access
x[:key1] #=> "val1"
x["key1"] #=> "val1"
```
To instantiate a HashWithIndifferentAccess from the beginning we do the following:
```ruby
x = HashWithIndifferentAccess.new #=> {}
```

将hash中string类型的key全部转为symbol
```ruby
hash = {"apple" => "banana", "coconut" => "domino"}
hash = Hash[hash.map{ |k, v| [k.to_sym, v] }]
```
将hash中string类型的key全部转为symbol
```ruby
hash.deep_symbolize_keys
hash.deep_symbolize_keys!
```
将hash中symbol类型的key全部转为string
```ruby
hash.deep_stringify_keys
hash.deep_stringify_keys!
```
<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
