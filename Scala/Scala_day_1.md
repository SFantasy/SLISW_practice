# Scala Day 1


* Scala的API

[http://www.scala-lang.org/node/216](http://www.scala-lang.org/node/216)

* 对比Java和Scala

联系：Scala有着和Java相同的编译模型并且可以调用Java的库；Scala工作特征和Java几乎一致；Scala编译产生的字节码与Java编译产生的几
乎一致（事实上，Scala的代码可以编译成可读的Java代码）；对于JVM而言，两者是没有区别的（唯一的区别就是一个附加的库:scala-
library.jar），etc.

区别：Scala拥有更小的代码体积与更小的内存占用；Scala的语法相比Java更为灵活（如无分号等）；Scala在Java的面向对象之上提供了“函数式编程
”的实现；一切都是表达式，即在Scala语言中不再有表达式和赋值语句之分，etc.

更多有关Scala与Java之间的关系和区别可以参考：[wikipedia.](http://en.wikipedia.org/wiki/Scala_\(p
rogramming_language\))以及Google对于集中语言的对比：[addr](http://www.theregister.co.uk/20
11/06/03/google_paper_on_cplusplus_java_scala_go/)

* val 与 var

"val means immutable and var means mutable."--赋值给val的对象不能被其他值替代而var的则可以。  
关于这个问题，stackoverflow上有一个hot的讨论：[addr](http://stackoverflow.com/questions/17914
08/what-is-the-difference-between-a-var-and-val-definition-in-scala)

* Scala Tic-Tac-Toe

看完一天的内容，在做这个小游戏的时候还是感觉对这门语言很生疏。比如经常遇到一些之前没有预料到的问题，比如List和Array的问题，包含没有实现的函数需要抽
象化类的问题，函数定义时的“＝”问题等等ORZ。

    
class Game {
    //initialize the Game Table
    var table = Array(
        "_", "_", "_",
        "_", "_", "_",
        "_", "_", "_"
    )
    //0 for going and 1 for winning
    var gameState = 0
    //define two kinds of pieces in the game
    val piece = List("X", "O")
    //0 for "X" to play and 1 for "O" to play
    var player = 0
    var currentPiece = -1

    //print information for users and read the choose
    def choose() {
        if ( this.player == 0) {
            println("X's move(0~8):")
        } else {
            println("O's move(0~8):")
        }
        currentPiece = Console.readInt();
    }

    def validMove ():Boolean = {
    	  if(this.table(currentPiece) == "_") {
          return true
      } else {
          return false
      }
    }

    def setPiece() {
    	  if ( this.player == 0) {
      	  this.table(currentPiece) = "X"
      } else {
          this.table(currentPiece) = "O"
      }
    }

    //judge the game
    def judger() {
    	  if( (table(0) != "_" &&
      	  table(0) == table(1) &&
      	  table(0) == table(2)) ||
    	  (table(3) != "_" &&
    	  table(3) == table(4) &&
    	  table(3) == table(5)) ||
    	  (table(6) != "_" &&
    	  table(6) == table(7) &&
    	  table(6) == table(8)) ||
    	  (table(0) != "_" &&
    	  table(0) == table(3) &&
    	  table(0) == table(6)) ||
    	  (table(1) != "_" &&
    	  table(1) == table(4) &&
    	  table(1) == table(7)) ||
    	  (table(2) != "_" &&
    	  table(2) == table(5) &&
    	  table(2) == table(8)) ||
    	  (table(0) != "_" &&
    	  table(0) == table(4) &&
    	  table(0) == table(8)) ||
    	  (table(2) != "_" &&
    	  table(2) == table(4) &&
    	  table(2) == table(6))) {
    	      gameState = 1
    	  }			  
    }
    def hasFinished():Boolean = {
    	  this.judger()
      if ( this.gameState == 1 ) {
      	 if (this.player == 1) {
    	 	 println("O win!")
    	 } else {
    	     println("X win!")
    	 }
         return true
      } else {
         return false
      }
    }
    //print game table
    def printTable() {
    	  println(table(0) + "|" + table(1) + "|" + table(2))
      println(table(3) + "|" + table(4) + "|" + table(5))
      println(table(6) + "|" + table(7) + "|" + table(8))
    }
    //main function for play
    def play() {
    	  this.printTable()
    	  while(!this.hasFinished) {
          this.choose()
    	  while( !this.validMove()){
    	      println("INVALID MOVE!")
    	      this.choose()
    	  }
    	  this.setPiece()
    	  this.printTable()
    	  if( player == 0) {
    	      player = 1
    	  } else {
    		  player = 0
    	  }
      }
    }
}

var game = new Game()
game.play()

[ Scala day 2](Scala_day_2.md) ->

