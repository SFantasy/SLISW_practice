# Io Day 3


## 做

* 改进本节生成的XML程序，增加空格以显示缩进结构。

```   
Builder := Object clone
Builder depth ::= 0

Builder forward := method(

		#depth ::= 0
		prefix := ("  " repeated (self depth))

		writeln(prefix, "<", call message name, ">")
		
		#depth = (depth +1)

		self setDepth(self depth + 1)
		call message arguments foreach(
			 arg,
			 content := self doMessage(arg);
			 if(content type == "Sequence", writeln(prefix, "  ",content)))

	    #depth = (depth - 1)
	    self setDepth(self depth - 1)
		writeln(prefix, ""))

Builder ul(
		li("Io"),
		li("Lua"),
		li("JavaScript"))
```    	  

* 创建一种使用括号的列表语法

看到这道题目的时候比较奇怪……感觉没有表达清楚让我们要创建的是什么？乍一看，还以为要写一个类似Lisp的语法（难道是我理解的问题ORZ）？后来看到别人写的才
知道是什么（还是有点奇怪－ －）……

```    
curlyBrackets := method(
  mapList := call message arguments map(
  	  value,
	  self doMessage(value);
  )
  
  return(mapList)
)

example := {
	  "Item one",
	  "Item two",
	  "Item three",
	  "Final item"
}
("Size:" .. example size()) println

example println

example at( example size() - 1) println
```    	  

* 改进本节生成的XML程序，使其可处理属性：如果第一个参数是映射（用大括号表示语法），则为XML程序添加属性。例如：  
`book({"author": "Tate"}...))`将打印出`< book author="Tate"; >`

先贴一个实现的比较全面的:[addr](http://www.bennadel.com/blog/2067-Seven-Languages-In-
Seven-Weeks-Io-Day-3.htm)

然后是我写的……不幸的是怎么都显示不标签的属性来:(  
上面那个很全面，可是对于这题而言又太过复杂了（话说那位同学写的注释是真多啊！），看来对于Io的很多内容真的是知之略少啊：（

```   
newXML := Object clone
newXML depth ::= 0

OperatorTable addAssignOperator(":", "atPutNumber")

curlyBrackets := method(
  attributes := list()
  call message arguments foreach(
    attributePair,
	attributes append(
	  self doString(attributePair asString)
	)
  )
  return attributes
)
atPutNumber := method(name, value,
  attribute := Map clone
  attribute atPut("name", name)
  attribute atPut("value", value)
  
  return attribute	
  #self atPut(
    #call evalArgAt(0) atMutable removePrefix("\"") removeSuffix("\"")
	#call evalArgAt(1)
  #)
)
atPut := method(
  name, value,
  if(list("Sequence", "Number") contains( value type),
    super(atPut(name, value)))
  return self
)

newXML forward := method(
  prefix := ("  " repeated (self depth))
  writeln(prefix, "<", call message name, ">")
  self setDepth(self depth + 1)
  call message arguments foreach(
 		arg,
		content := self doMessage(arg);
		if(content type == "Sequence", writeln(prefix, "  ", content))
  )
  self setDepth(self depth - 1)
  writeln(prefix, "")
)

newXML ul(
  li("Io"),
  li(book({"author": "Tate"})),
  li("JS")
)
```

<- [ Io day 2](Io_day_2.md)
