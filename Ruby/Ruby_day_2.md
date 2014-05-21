# Ruby Day 2

## 找

* Ruby用代码块和不用代码块读取文件的方法，用代码块有什么好处？  

用代码块读取文件的方法：  
    
    File.open("text.txt") do |f|
    puts f.gets
    end
            

不用代码块读取文件的方法：  

    f = File.new("text.txt","r")
    puts f.gets
    f.close            

使用代码块，是一种用来打开一个单独文件的方法，而且可以在一个地方（代码块内）对该文件进行操作。相比与不用代码块而言，使用代码块显得比较干净利落。

* 如何把散列表转换成数组？数组能转换成散列表吗？  

散列表转换成数组：
    
    hash.to_a #其中每个元素是一个key,value组成的数组            

数组转换成散列表：
    
    array = %w[a b c d]
    => ["a","b","c","d"]
    h = Hash[*array]
    => ["a"=>"b","c"=>"d"]            

当然，所用到的数组必须是偶数个的，不然无法进行Hash。

* 循环遍历散列表： 
    
    h.each{ |k,v| puts "#k"=>"#v"}
            

* Ruby的数组能当作栈来用，它还能作哪些常用的数据结构？  

Ruby的数组还可以用作队列、稀疏矩阵等数据结构。

## 做

* 有一个数组，包含16个数字。仅用each方法打印数组中的内容，一次打印4个数字。然后，用可枚举模块的each_slice方法重做一遍。  

```  
#生成一个数组 （来一个简单的方法：arr=(0..15).to_a）
i = 0
arr = []
while i < 16
  arr.insert(i,i)
  i = i+1
end
#用each打印数组中的内容，一次4个
t = 0 #用来计数
arr.each{
  |x|
  if t == 4
  t = 0
  print "\n"
end
print x," "
t = t + 1
}
#用each_slice方法
arr.each_slice(4) {|x| p x}
```            

* 我们前面实现了一个有趣的树类Tree，但它不是具有简洁的用户接口，来设置一棵新树，为它写一个初始化的方法，接受散列表和数组嵌套的结构，写好之后，你可以这样设置新树`{'grandpa' => {'dad' => {'child 1'=>{}, child 2' => {}} 'uncle' => {'child3'=>{}, 'child 4' => {} }}}`

```    
#!/usr/bin/ruby
#Filename: tree.rb
#Usage: Accept a Nested structure with hashes and arrays, and it can visit the tree.

class Tree
attr_accessor :children, :node_name

def initialize(tree)
tree.each do |key, value|
  @node_name = key
  @children = value.map {|(key,value)| Tree.new(key => value)}
end
end

def visit_all(&block;)
visit &block;
children.each {|c| c.visit_all &block;}
end

def visit(&block;)
block.call self
end
end

ruby_tree = Tree.new(
{'grandpa' => {'dad' => {'child 1' => {}, 'child 2' => {}}, 'uncle' => {'child 3' => {}, 'child 4' => {} }}})


puts "Visiting a node"
ruby_tree.visit {|node| puts node.node_name}
puts

puts "Visiting entire tree"
ruby_tree.visit_all {|node| puts node.node_name}
```	

这个树的问题着实让我苦恼……所以不得不在[本书的论坛上](http://forums.pragprog.com/)寻找众人讨论的果实，最终参考了以后总算完成
了这个练习。  
同时，该论坛上还有很多有趣、古怪的解答，有些让人着实摸不着头脑，有些让人拍案叫绝。

* 写一个简单的grep程序，把文件中出现的某词组的行全部打印出来。这需要使用简单的正则表达式匹配，并从文件中读取各行。如果你愿意的话，还可以加上行号。  

```    
def grepTest(fileName,wordName)
lineNum = 1
lineCount = 0
pattern = Regexp.new(wordName)
File.open fileName, 'r' do |file|
file.each_line do |line|
  if pattern =~ line
        puts "#{lineNum}:#{line}"
        lineCount = lineCount + 1
  end
  lineNum = lineNum + 1
end
if lineCount == 0
  puts "No such a word in the file."
end
end
end
print "input the word you are looking for:"
wordName = gets
grepTest("test.rb",wordName)
```    	

对于这个程序还有一个问题：当我使用gets获取用户输入文件名并将其传入grepTest()的时候，ruby就会提示我test.rb:no such file
or directory.  
但是直接将文件名作为参数传入的时候却是正确的。不知道这是什么原因？！  

> 通过gets得到wordName的时候,提示no such file or directory.是由于多读取了一个`\n`,使用`wordName.chomp`可以去掉wordName的`\n`,成功找到. -- by flyyuan

<- [Ruby Day 1](Ruby_day_1.md) | [Ruby Day 3](Ruby_day_3.md) ->
