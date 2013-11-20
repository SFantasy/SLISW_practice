# Clojure Day 2

* 找Clojure中一些常用宏的实现

可以在GitHub中找到[一些常用宏的实现](https://github.com/clojure/clojure/blob/d0c380d9809fd242bec688c7134e900f0bbedcac/src/clj/clojure/core.clj#L455)

* 一个自定义延迟序列的例子

看到这么个例子：

```
(defn recur-fibo [n]
    (letfn [(fib [current next n] (if (zero? n) current (recur next (+ current next) (dec n))))]
    (fib 0 1 n)))
```

使用LazySeq后：

```
(defn lazy-seq-fibo
    ([] (concat [0 1] (lazy-seq-fibo 0 1)))
    ([a b] (let [n (+ a b)] (lazy-seq (cons n (lazy-seq-fibo b n))))))
```

* 用宏实现一个包含else条件的unless

在使用普通的函数定义unless时，如：

```
user=> (defn unless [test body] (if (not test) body))
#'user/unless
#会输出如下:
user=> (unless true (println "True!"))
True
nil
```

也就是说，先对代码块进行了求值，所以返回了True。因此，需要用到宏，使得扩展编程语言的时候能够避免执行一些函数。

而要实现else只需要将原来的nil替换为另外一个form。

```
user=> (defmacro unless [expr form1 form2] (list 'if expr form2 form1))
#'user/unless
user=> (unless true (println "1") (println "2"))
2
nil
```

如果要兼容原来的unless，就要考虑到两种不同的参数情况：

```
user=> (defmacro unless 
         ([expr form1 form2] (list 'if expr form2 form1))
         ([expr form] (list 'if expr nil form)))
#'user/unless
user=> (unless true (println "hello"))
nil
user=> (unless true (println "1") (println "2"))
2
nil
```

* 编写一个类型用defrecord实现的协议

```
;; File Name: compass.clj
;; Open it in REPL with: (loaf-file "compass.clj")
;; 书中实现的Compass protocol与SimpleCompass record
;; 如最后一行toString是调用了java.lang.Object中的方法
;; 这样的好处就是可以与JVM上的其他类型相交互，即可以使用Java的类型和接口

(defprotocol Compass
  (direction [c])
  (left [c])
  (right [c]))

(def directions [:north :east :south :west])

(defn turn
  [base amount]
  (rem (+ base amount) (count directions)))

(defrecord SimpleCompass [bearing]
  Compass
  (direction [_] (directions bearing))
  (left [_] (SimpleCompass. (turn bearing 3)))
  (right [_] (SimpleCompass. (turn bearing 1)))
  Object
  (toString [this] (str "[" (direction this) "]")))
```

## 参考

* [Programming Clojure 学习笔记](http://blog.csdn.net/zh2qiang/article/details/7234487)

<- [Clojure Day 1](Clojure_day_1.md) | [Clojure Day 3](Clojure_day_3.md) ->