# Clojure Day 1

到第六门语言了。Clojure是JVM上的Lisp实现，这是很让人振奋的。

Clojure的安装比之前的几门语言都要复杂，因为需要使用额外的[leiningen](https://github.com/technomancy/leiningen)工具，在Ubuntu 12.04下可以使用

    sudo apt-get install leiningen

安装。如果是其他版本或者GNU/Linux发行版可能需要通过项目主页上提供的脚本进行安装。

需要通过
    
    lein repl

启动Clojure的交互式解释器(可能需要一段等待的时间)。

* Clojure序列的例子(via OCIWEB)

What does the following code output?

```
(map #(println ％)［1 2 3］）
```

REPL outputs:

```
(1
2
3
nil nil nil)
```

因为map返回的是一个LazySeq，如果是在一个脚本中运行的话，就什么都不会输出。

举一个可以调用LazySeq的例子（更多相关内容可以查看"参考"）

```
(dorun (map #(println %) [1 2 3]))
```

REPL outputs:

```
1
2
3
nil
```

* 实现一个函数(big st n),当字符串st长度不超过n个字符时返回true。

```
user=> (defn big [st n] (< (count st) n))
#'user/big
#outputs
user=> (big "hello" 3)
false
user=> (big "clojure" 8)
true
```

Tip: 这个函数很简单，而且不必使用println之类的打印true or false，因为表达式自身返回的就是boolean值。

* 实现一个函数（collection-type col)，根据给定集合col的类型返回:list, :map或者:vector。

```
user=> (defn collection-type [col] (if (list? col) :list (if (map? col) :map :vector)))
#'user/collection-type
user=> (collection-type '(1 2 3))
:list
user=> (collection-type {1 2 3 4})
:map
user=> (collection-type [1 2 3])
:vector
```

## 参考

* [OCIWEB #Sequence](http://java.ociweb.com/mark/clojure/article.html#Sequences)
* [Clojure functional programming](http://clojure.org/functional_programming)

[Clojure Day 2](Clojure_day_2.md) ->