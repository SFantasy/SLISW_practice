# Scala Day 2

* 关于如何使用Scala文件的讨论

Scala可以使用所有的Java的对象，所以，java.io.File就可以被用来在Scala中读写文件。  
例如将“Hello，Scala”写进文件：

    
    import java.io.*
    			
    object Test {
        def main(args: Array[String]) {
            val writer = new PrintWriter(new File("test.txt"))
            writer.write("Hello Scala")
            writer.close()
        }
    }
                

读取文件内容：

    
    import sacla.io.Source
    
    object Test {
        def main(args: Array[String]) {
            println("Following is the content read:")
            Source.fromFile("test.txt").forEach{
                print
            }
        }
    }
    		    

AN:在找Scala资料的时候发现了一个不错的网站(<http://www.tutorialspoint.com/index.htm>)

* 闭包（closure）与代码块的不同

代码块是一些语法上的－－一些逻辑性语句组成的单元：

    
    if (Condition) {
        // one Block
    } else {
        // another Block
    }
    		  

闭包是引用了自由变量的函数，这个自由变量将与其函数一同存在，即使离开了原有的环境也是一样的。如：

    
    def foo() {
        var x = 0
        return () => { x += 1; return x}
    }
    val counter = foo()
    println(counter)  //输出2,x虽然离开了原有函数foo，但实际上仍随着foo的结果被调用而存在。
    println(counter)  //输出3
    		  

* 使用foldLeft方法计算一个列表中所有字符串的总长度

```   
val test = List(
    "One",
    "Two",
    "Three",
    "Four"
)

println(test.foldLeft(0)((sum, value) => sum + value.length))
```    		

* 编写一个Censor trait，包含一个可将Pucky和Beans替换为Shoot和Darn的方法。使用映射存储脏话和它们的特点

```  
trait Censor {
    val test = Map(
        "Pucky" -> "Shoot",
        "Beans" -> "Darn"
    )
    def transform(words:String) = {
		
        test.foldLeft(words)(
            (rightWords, formerWords) =>
            rightWords.replaceAll(
                ("(?i)\\b" + formerWords._1 + "\\b"),
                formerWords._2			    
            )
        )		
    }
}
    
class Speak(val words:String) extends Censor

val tom = new Speak("Pucky is Beans")

println(tom.transform(tom.words))
```
    		

其中，Censor里的transform函数参考了一下[这里](http://www.bennadel.com/blog/2078-Seven-
Languages-In-Seven-Weeks-Scala-Day-2.htm), 当然主要是因为replaceAll这个方法。以上所给网址的实现也是很不错的，但总感觉太繁琐了，不简约。

* 从一个文件中加载脏话或它们的替代品

这个问题，是不是太简单了？就加载脏话？或，替代品？

```
for(line <- io.Source.fromFile("test.txt").getLines)
println(line)
``` 


<- [ Scala day 1](Scala_day_1.md) ｜[ Scala day 3](Scala_day_3.md) ->
