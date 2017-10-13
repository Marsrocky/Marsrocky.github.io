---
layout:     post
title:      "A Lua Primer plus Torch"
subtitle:   "Lua + Torch入门学习"
date:       2017-10-13 19:18
author:     "Jianfei"
header-img: "img/post-lua.jpg"
tags:        [Code, Lua]
category:   Programming
---

## Lua
- Lua是一个高性能的轻量级的脚本语言。
- 广泛应用于游戏脚本（暴雪WOW），nginx，wireshark的脚本。
- Lua由标准C编写而成，和C/C++语言有非常好的交互。
- Lua没有提供强大的库，不适合作为开发独立应用程序的语言。


#### lua的安装
{% highlight linux %}
curl -R -O http://www.lua.org/ftp/lua-5.3.0.tar.gz
tar zxf lua-5.3.0.tar.gz
cd lua-5.3.0
make macosx test
make install
{% endhighlight %}

Lua也有交互式编程模式，可以通过命令lua直接启动，也可以写好hello.lua文件然后执行。先来学习一波基本语法和特性！

#### Lua 基本语法
##### [注释]
{% highlight lua %}
-- 两个减号为注释
-- 书写时通过指定lua解释器来运行脚本
#!/usr/local/bin/lua
{% endhighlight %}
##### [标示符]
{% highlight lua %}
-- 用于定义一个变量，函数获取其他用户定义的项。标示符以一个字母 A 到 Z 或 a 到 z 或下划线 _ 开头后加上0个或多个字母，下划线，数字（0到9）。最好不要使用下划线加大写字母的标示符，因为Lua的保留字也是这样的。
-- 变量默认为全局变量
b = 10
print(b)
-- 删除变量，只需赋值nil
b = nil
-- 局部变量
local a = 10
{% endhighlight %}
##### [数据类型]
{% highlight lua %}
--nil: 无效值
--boolean: false/true
--number: 只有double
--string: 双引号/单引号
--function: 比如print
--userdata: 存储在变量中的C数据结构
--thread: 表示执行的独立线路
--table: 关联数组
{% endhighlight %}
##### [赋值]
{% highlight lua %}
--同时对多个赋值
a,b = 10,c
--对table索引
site = {}
site["key"] = "marsrock.net"
{% endhighlight %}
##### [循环]
{% highlight lua %}
--循环会在开始前一次性求值，之后不在求值
for i=1,10 do
    print(i)
end
{% endhighlight %}
##### [函数]
{% highlight lua %}
--[[ 函数返回两个值的最大值 --]]
function max(num1, num2)

   if (num1 > num2) then
      result = num1;
   else
      result = num2;
   end
   return result; 
end
-- 调用函数
print("两值比较最大值为 ",max(10,4))
print("两值比较最大值为 ",max(5,6))
{% endhighlight %}
##### [字符串]
{% highlight lua %}
--string functions
a = "lua"
string.upper(argument)
string.lower(argument)
string.gsub(mainString, findString, replaceString, num)
string.find("Hello Lua user", "Lua", 1)
string.reverse("Lua")
string.len("abc")
print("abc".."def") --连接字符串
{% endhighlight %}

##### [数组]
{% highlight lua %}
array = {"Lua", "Tutorial"}

for i= 0, 2 do
   print(array[i])
end
{% endhighlight %}

##### [面向对象]
{% highlight lua %}
-- Meta class
Shape = {area = 0}

-- 基础类方法 new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end
{% endhighlight %}
{% highlight lua %}
-- 基础类方法 printArea
function Shape:printArea ()
  print("面积为 ",self.area)
end
-- 创建对象
myshape = Shape:new(nil,10)
myshape:printArea()
{% endhighlight %}
## Torch
- Torch 是一个lua编写的十分老牌、对多维矩阵数据进行操作的张量（tensor ）库。
- Facebook的AI团队发布了Pytorch，针对GPU加速的DNN编程。

#### 安装
{% highlight shell %}
# in a terminal, run the commands WITHOUT sudo
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; bash install-deps;
./install.sh
{% endhighlight %}
将PATH生效后，就可以正常通过th启动Torch7了，也可以通过luarocks安装包。
Lua可以通过luarocks安装包。
{% highlight shell %}
luarocks install image
luarocks list
{% endhighlight %}