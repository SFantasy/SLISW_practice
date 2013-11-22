# Haskell Day 1

Ubuntu 12.04 下可用apt-get安装Haskell的编译器(Glasgow Haskell Compiler):
    
    apt-get install ghc          

接下来就可以在终端中使用Haskell解释器了. (type in: ghci, exit: Ctrl + d)

## 有关Haskell

* Haskell Wiki: <http://www.haskell.org/haskellwiki/Haskell>(其实就是Haskell的官方网站) 
* Try Haskell: [Try Haskell](http://tryhaskell.org)（一个非常好的在线Haskell教程，输入chat可以连接到Haskell的IRC频道，不过有点慢） 

Haskell与之前的语言都有点不同,  
i.e.  
连接两个字符串需要++:
    
    Prelude> "hello" ++ "world"
    "helloworld"              

由于Haskell是强类型（同时也是静态类型）语言，所以if函数只能严格地接受Boolean型的参数, 所以不能有:
    
    Prelude> if 1 then "true" else "false"   

这样的语句.

* 多种实现allEven的方法 

1. 哨兵表达式 

```
module Main where
allEven :: [Integer] -> [Integer]
allEven [] = []
allEven (h:t)
  | even h = h:allEven(t)
  | otherwise = allEven(t)
```
                    

2. 列表解析 

```     
module Main where
allEven :: [Integer] -> [Integer]
allEven numbers = [ evens | evens <- numbers, even evens]
```                 

3. Filter: 

```   
module Main where
allEven :: [Integer] -> [Integer]
allEven numbers = filter even numbers
```                    

* 编写一个函数，它以一个列表作为参数并返回逆序后的列表 

```  
module Main where
reverseList :: [Integer] -> [Integer]
reverseList [h] = [h]
reverseList (h:t) = reverseList(t) ++ [h]
```          

事实上，Haskell已经提供了reverse这个函数:
    
    Prelude> reverse [1,2,3]
    [3,2,1]
            

* 从五个颜色黑、白、蓝、黄和红任选出两个组合在一起，编写一个函数，计算出所有可能的组合。

注意，你只能包含(black, blue)和(blue, black)两者中的一个 
    
    Prelude> let colors = ["black", "white", "yellow", "blue", "red"]
    Prelude> let choose1 = [(a, b) | a <- colors, b <- colors, a < b]
    Prelude> choose1
    [("black","white"),("black","yellow"),("black","blue"),("black","red"),
    ("white","yellow"),("blue","white"),("blue","yellow"),("blue","red"),
    ("red","white"),("red","yellow")] 
            

* 编写一个列表推导来构建一个儿童乘法表。

这个表应该是一个由三元组组成的列表，三元组的前两个整数元素的取值范围是1～12，第三个元素为前两个元素的乘积. 
    
    Prelude> let numbers = [1..12]
    Prelude> [(a, b, a*b) | a <- numbers, b <- numbers, a <= b]
    [(1,1,1),(1,2,2),(1,3,3),(1,4,4),(1,5,5),(1,6,6),(1,7,7),(1,8,8),(1,9,9),
    (1,10,10),(1,11,11),(1,12,12),(2,2,4),(2,3,6),(2,4,8),(2,5,10),(2,6,12),
    (2,7,14),(2,8,16),(2,9,18),(2,10,20),(2,11,22),(2,12,24),(3,3,9),(3,4,12),
    (3,5,15),(3,6,18),(3,7,21),(3,8,24),(3,9,27),(3,10,30),(3,11,33),(3,12,36),
    (4,4,16),(4,5,20),(4,6,24),(4,7,28),(4,8,32),(4,9,36),(4,10,40),(4,11,44),
    (4,12,48),(5,5,25),(5,6,30),(5,7,35),(5,8,40),(5,9,45),(5,10,50),(5,11,55),
    (5,12,60),(6,6,36),(6,7,42),(6,8,48),(6,9,54),(6,10,60),(6,11,66),(6,12,72),
    (7,7,49),(7,8,56),(7,9,63),(7,10,70),(7,11,77),(7,12,84),(8,8,64),(8,9,72),
    (8,10,80),(8,11,88),(8,12,96),(9,9,81),(9,10,90),(9,11,99),(9,12,108),
    (10,10,100),(10,11,110),(10,12,120),(11,11,121),(11,12,132),(12,12,144)]
            

* 使用Haskell解决地图着色问题

这个问题在学习Prolog的时候解决过，但是在Haskell的时候觉得又是不同的感觉了.  
先回顾一下Prolog中的解决方法，先描述different(color1, color2)；然后再描述相邻的states.

    
    Prelude> let colors = ["red", "blue", "green"]
    Prelude> [(alabama, mississippi, georgria, tennessee, florida) | 
               alabama <- colors, mississippi <- colors, georgria <- colors, 
               tennessee <- colors, florida <- colors, 
               mississippi /= tennessee, mississippi /= alabama, 
               alabama /= tennessee, alabama /= florida, 
               alabama /= georgria, georgria /= florida, georgria /= tennessee]
    [("red","green","green","blue","blue"),("red","blue","blue","green","green"),
    ("green","red","red","blue","blue"),("green","blue","blue","red","red"),
    ("blue","red","red","green","green"),("blue","green","green","red","red")]
            

一开始还把州名都写成首字母大写的了，然后就各种报错了 :(

[Haskell day 2](Haksell_day_2.md)
