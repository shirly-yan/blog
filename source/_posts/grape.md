---
title: grape
date: 2016-11-22 14:24:15
categories:
---
grape
<!-- more -->
<h2>Gemfile中加</h2>
```ruby
gem 'grape'
```

<h2>资源路径</h2>
/resources/:resource
/resources/:resource/:action

例
/users/1
/users/1/logout
路径嵌套
/users/1/posts/2/like


<h2>请求参数</h2>
+ header
+ param( queryparam pathparam fieldparam) 

```ruby
params do
   requires :year, type: Integer
   optional :month, type: String, default: 'may'
end
```
<h2>相应</h2>
<h3>资源的表述方式</h3>
+ json
+ xml

```ruby
  format :json
```
<h3>标准时间戳</h3>

<h3>响应码</h3>
200 ：get请求完成
201 ：post，put,delete请求完成
202 ：post，delete，请求提交成功，稍后将异步的进行处理
400 ：bad request请求无效，请求参数包含错误
401 ：unauthorized请求无效，因为用户没有进行认证
403 ：forbidden请求无效，因为用户被认定没有访问特定资源的权限
429 ：too many requests,请求频率超配，稍后再试
500 ：internal server error,服务器出错了，检查网站的状态，或者报告的问题

<h3>错误信息</h3>


例：
```ruby
class User::API < Grape::API
  format :json

  resources ':users' do
    get 'test' do
      data = {
          :time => Time.new,
          :ip => request.ip,
      }
      present data
    end

    resources ':user_id' do
      get do
        data = {
            :users => params[:users],
            :user_id => params[:user_id],
            :time => Time.new,
            :ip => request.ip,
        }
        present data
      end

      params do
        requires :year, type: Integer
        optional :month, type: String, default: 'may'

      end
      get 'logout' do
        data = {
            :year => params[:year],
            :month => params[:month],
            :type => 'get',
            :time => Time.new,
            :ip => request.ip,

        }
        present data
      end
      post 'logout' do
        data = {
            :day => params[:day],
            :app_version => headers['App-Version'],
            :token => headers['Token'],
            :type => 'post',
            :users => params[:users],
            :user_id => params[:user_id],
            :time => Time.new,
            :ip => request.ip,
        }
        present data
      end
    end
  end



end
```

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
