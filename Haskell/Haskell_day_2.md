# Haskell Day 2

* 编写函数sort，接受一个列表作为参数并且返回一个有序的列表

这里我选择的是选择排序，即选择列表中最小的元素然后放在第一位.

    
      module MySort where
      
      import Data.List
    
      mySort list
        | null list = list
        | otherwise = [x] ++ mySort (delete x list) 
            where x = minimum list 
    		

* 编写函数sort，接受一个列表和一个比较两个参数大小的函数作为参数，然后返回一个有序列表

这个函数可以参阅一下[参考]中的博客。

    
      module Main where
      
      import Data.Ord
    
      mySortBy f (x:xs) = myInsertBy f x $ mySortBy f xs
        where myInsertBy _ x [] = []
              myInsertBy f x (y:ys) | f x y == GT = y:myInsertBy f x ys
                                    | otherwise   = x:y:ys
    
    		

* 编写一个Haskell函数，将字符串转换为数字。

字符串应该以$2, 245, 678, .99形式提供，并且可以包含前导零 
    
      module Main where
    
      import Data.Char
    
      parser :: String -> Float
    
      parser value = read strippedValue :: Float
        where 
          strippedValue = 
            foldl 
              (\ newString c ->
                if (isDigit c || c == '.' ) then
                  newString ++ [c]
                else
                  newString
              )
              " "
              value
    		

这样解决的问题是，还没有解决.99这样的字符串转换问题（由于本身不提供将.99 -> 0.99的parse）  
这里有一个更健全的版本:

    
    module Parse where
    
    import Data.Char
    import Data.List
    
    parse str =
        let (dig, frac) = break (== '.') str
        in foldl' pDig 0 (clean dig) + foldr pFrac 0 (clean frac) / 10
    
    clean s   = filter isDigit s
    pDig  a b = 10*a + toNum b
    pFrac a b = toNum a + b/10
    toNum c   = fromIntegral $ ord c - ord '0'  
    

* 编写一个函数，该函数接受一个参数x,并返回从x起始，每两个元素间相隔差值为2的惰性序列。

然后，编写另外一个函数，返回从y开始，每两个元素间相隔差值为4的惰性序列。通过组合将两个函数合并在一起返回一个从x+y开始，每两个元素间隔差值为7的惰性序列   

要做到这个非常简单，在ghci中我们可以测试发现:

    
    Prelude> [1..5]
    [1,2,3,4,5]
    Prelude> [1,2..5]
    [1,2,3,4,5]
    -- 这两者的效果是等价的
    Prelude> [1,2..]
    -- 这个输出的就是一个无穷序列
    Prelude> take 5 [1,2..]
    [1,2,3,4,5]
    

所以要做到间隔相差n, 只需要在序列的前两个数字上下功夫即可

    
    module Main where
    two x = [x, x+2..]
    three y = [y, y+3..]
    seven x y = zipWith (+) (two x) (three y)
    -- Output
    Prelude> :load "lazy.hs"
    [1 of 1] Compiling Main             ( lazy.hs, interpreted )
    Ok, modules loaded: Main.
    *Main> take 5 (two 3)
    [3,5,7,9,11]
    *Main> take 5 (three 4)
    [4,7,10,13,16]
    *Main> take 5 (seven 2)
    *Main> take 5 (seven 2 3)
    [5,10,15,20,25]
    

* 使用偏应用函数定义两个函数，其中一个将返回一个数值的一半.

另外一个函数将会将\n附加到任意字符串的末尾. 
    
      module Main where
      half = (/ 2)
      newLine = (++ "/n")  
    

## 参考

1. [wakatta's blog](http://blog.wakatta.jp/blog/2011/11/18/seven-languages-in-seven-weeks-haskell-day-2/)
2. [Data.List](http://www.haskell.org/ghc/docs/7.4-latest/html/libraries/base-4.5.1.0/Data-List.html)
3. [Data.Char](http://www.haskell.org/ghc/docs/7.4-latest/html/libraries/base-4.5.1.0/Data-Char.html)


<- [ Haskell day 1](Haksell_day_1.md) | [Haskell day 3](Haskell_day_3.md) ->

