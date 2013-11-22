# Scala Day 3

* 对于sizer程序，如果你没有为每一个要跟踪的链接创建一个新的actor，这段程序的性能会发生怎样的变化？

感觉会很慢，因为每个actor都包含了react和receive方法，如果没有对于每个链接都创建actor的话，那和sequentially的方法就没有什么
区别了。

* 修改sizer程序，增加一个计算页面上链接总和的消息。

```  
import scala.io._
import scala.actors._
import Actor._

object PageLoader {
    def getPageSize(url: String) = Source.fromURL(url).mkString.length
    // get the contents of the the URL
    def getPageContent(url: String) = Source.fromURL(url).mkString
    // regular expression of a link
    def getUrlNumber(url: String) = {
        val reg = """^(?i)<a[^>]+?href""".r
   
        reg.findAllIn(getPageContent(url)).size
    }
}
// the List of urls
val urls = List("http://www.baidu.com/",
                "http://www.seu.edu.cn/",
                "http://www.fantasyshao.tk/"
               )
// Cauculate the time of running
def timeMethod(method: ()=> Unit) = {
    val start = System.nanoTime
    method()
    val end = System.nanoTime
    println("Method took " + (end - start)/1000000000.0 + " seconds.")
}
// not using actors
def getPageSizeSequentially() = {
    for(url <- urls) {
        println("Size for " + url + ": " + PageLoader.getPageSize(url))
    }
    for(url <- urls) {
        println("Links in " + url + ": " + PageLoader.getUrlNumber(url))
    }
}
// concurrency method
def getPageSizeConcurrently() = {
    val caller = self
    
    for(url <- urls) {
        actor { caller ! ("Size", url, PageLoader.getPageSize(url))}
        actor { caller ! ("Number", url, PageLoader.getUrlNumber(url))}
    }
    // two actor for each url, so the size became lager
    for(i <- 1 to (urls.size * 2)) {
        receive {
            case ("Size", url, size) =>
                println("Size for " + url + ": " + size)
            case ("Number", url, number) =>
                println("Number of " + url + ": " + number)
        }
    }
}

println("Sequential run:")
timeMethod { getPageSizeSequentially }

println("Concurrent run:")
timeMethod { getPageSizeConcurrently }
```

<- [ Scala day 2](Scala_day_2.md)

