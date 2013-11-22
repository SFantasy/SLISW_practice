# Haskell Day 3

最后一天所谓的［特殊能力］即Haskell的类型系统--支持类型推断.

一些有关monad的教程

1. Haskell Wiki: [monad](http://www.haskell.org/haskellwiki/Monad)
2. Haskell Tuitorial: [monad](http://www.haskell.org/tutorial/monads.html)
3. Wiki 教科书: [理解monad](http://zh.wikibooks.org/zh-cn/Haskell/%E7%90%86%E8%A7%A3monads)

突然出现的Monad概念有点手足无措啊……虽然monad的设计原由、使用也不是很难，但是回过头来一时之间还真理解不了.
做了蛮久只完成了一个题目，还是有参考的情况下.

* 编写一个函数，该函数使用Maybe monad查找一个散列值表。编写一个散列表，该散列表可以多级存储其他的散列表。

使用Maybe monad检索一个散列key对应的多级散列表中的元素. 
    
    module Main where
      
    my_lookup key [] = Nothing
    my_lookup key ((k, value):rest)
      | key == k = Just value
      | otherwise = lookup key rest
    
    testData = [(1, []), 
                (2, [("a", [("i", "tada!")]), ("b", [("j", "nope")])]), 
                (3, [("c", [("k", "tada!")])])]
    
    
    *Main> my_lookup 2 testData >>= my_lookup "a" >>= my_lookup "i"
    Just "tada!"
    *Main> my_lookup 2 testData >>= my_lookup "b" >>= my_lookup "j"
    Just "nope"
    *Main> my_lookup 2 testData >>= my_lookup "b" >>= my_lookup "t"
    Nothing
    		

* 用Haskell表示一个迷宫。你将需要一个Maze类型，一个Node类型以及一个返回给定坐标节点的函数。这个节点应该具有一个到其他节点的出口列表. 
* 使用一个列表monad解决迷宫问题 
* 在非函数式语言中实现一个monad

在Haskell Wiki上就有一个monad in other languages的部分：[here](http://www.haskell.org/ha
skellwiki/Monad#Monads_in_other_languages)

## 参考

1. [Wakatta](http://blog.wakatta.jp/blog/2011/11/19/seven-languages-in-seven-weeks-haskell-day-3/)


<- [ Haskell day 2](Haksell_day_2.md)

