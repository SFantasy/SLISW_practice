# Io Day 1

##答

* 对1+1求for(i, 2, 10, 1, array append(array at(i-1) + array at(i-2)) println) 值，然后对1＋"one"求值。Io是强类型还是弱类型？用代码验证。

```   
Io> 1+"one"
Exception: argument 0 to method '+' must be a Number, not a 'Seuqentce'
---------
message '+' in 'Command Line' on line 1
```   		

显然，Io是强类型语言。

* 0是true还是false? 空字符串是true还是false? nil是true还是false? 用代码证实。

```   
Io> 0 isTrue
==> true
Io> "" isTrue
==> true
Io> nil isTrue
==> false
```    

* 如何知道某个原型都支持哪些槽？
    
```
Object slotNames
......
```

* = 、:=、 ::= 之间有什么区别？你会在什么情况下使用它们？

在[Io Programming Guide](http://iolanguage.org/scm/io/docs/IoGuide.html#Syntax-
Operators)上找到了关于运算符的一些内容，其中关于这三个运算符是这样的：

```
operator action
::=  Creates slot, creates setter, assigns value 
 :=  Creates slot, assigns value 
  =  Assigns value to slot if it exists, otherwise raises exception 
```

##做

* 从文件中运行Io程序。 

和Ruby一样，创建一个.io的文件，写一点代码进去，比如保存为test.io

    	  echo '"hello io" println' > test.io
    	  io test.io
    	  hello io
    	

* 给定槽的名称，执行该槽中的代码。 

```   
//test.io
Vehicle := Object clone
Car := Vehicle clone
ferrari := Car clone

Car selfDescription := method("I am a car." println)

ferrari selfDescription
//bash output
I am a car.
```

[ Io day 2](Io_day_2.md) ->
