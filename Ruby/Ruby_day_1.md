# Ruby Day 1

## 找

* Ruby API 文档：<http://ruby-doc.org/core-1.9.3/>

* Programming Ruby: The Pragmatic Programmer's Guide [TFH08]的免费在线版本：[http://www.ruby-doc.org/docs/ProgrammingRuby/](http://www.ruby-doc.org/docs/ProgrammingRuby/)

* 替换字符串某一部分的方法： 

替换一次:

    
    "Hello World".sub('World','Ruby')
    => Hello Ruby
            

替换所有:

    
    "This is not right".gsub('i',' ')
    => Th s  s not r ght
            

* 有关Ruby正则表达式的资料：[http://ruby-doc.org/core-1.9.3/Regexp.html](http://ruby-doc.org/core-1.9.3/Regexp.html)

* 有关Ruby区间(range)的资料：[http://ruby-doc.org/core-1.9.3/Range.html(http://ruby-doc.org/core-1.9.3/Range.html)

## 做

* 打印字符串"Hello World.": 
   
``` 
irb>puts "Hello World."
Hello World.
=> nil
```            

* 在字符串"Hello Ruby."中，找出"Ruby."所在下标。 

```    
irb>"Hello Ruby.".scan(/[Ruby\.]/) { |x| puts "Hello Ruby.".index(x)}
6
7
8
9
10
=> "Hello Ruby."
```            

其中，scan为Ruby在字符串中扫描的函数，寻找到匹配"Ruby."中任意字符的字符，然后再用index方法求下标。

* 打印你的名字十遍 

```    
irb>10.times do puts "fanTasy" end
fanTasy
fanTasy
fanTasy
fanTasy
fanTasy
fanTasy
fanTasy
fanTasy
fanTasy
fanTasy
=> 10
```            

* 从打印字符串"This is sentence number 1."，其中的数字1会一直变化到10。 

```    
irb>n = 0
while (n<10)
puts "This is sentence number"+(n+1).to_s+"."
end
This is sentence number 1.
This is sentence number 2.
This is sentence number 3.
This is sentence number 4.
This is sentence number 5.
This is sentence number 6.
This is sentence number 7.
This is sentence number 8.
This is sentence number 9.
This is sentence number 10
```            

* 从文件运行Ruby程序 

用自己喜爱的文本编辑器写好一个Ruby程序，保存之后在终端输入:`ruby fileName.rb`
            

* 猜数字程序： 

```    
puts "Guess a number:"
a = gets()
b = rand(10)
while true
   if a.to_i > b
      puts "Too large\n"
   elsif a.to_i < b
      puts "Too small\n"
   else
      puts "all right\n"
      exit
   end
   puts "Guess again:\n"
   a = gets()
end
```
        

[Ruby Day 2](Ruby_day_2.md) ->

