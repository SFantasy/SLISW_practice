# Erlang Day 1

这一天其实没有介绍很多，很多内容（HW中的）都需要从doc或者Google找到；当然，还没有接触到Erlang的核心内容Concurrency呢。

* Erlang官网: [erlang.org](http://www.erlang.org)

* Erlang/OTP函数库的官方文档：[erlang.org/doc](http://www.erlang.org/doc/)

* 写一个函数，用递归返回字符串中的单词数

查询Erlang的Doc可以发现Erlang的stdlib的string部分就包含了

    words(String) -> Count

这样的方法。当然，题目主要还是为了要理解Erlang中的递归。

与Prolog中的相似(如书中所言，Erlang受Prolog影响很大)，Erlang也有[Head|Tail]这种分隔字符串的机制。所以我们可以通过逐个字符进行分析，遇到space就将之前读取到的字符归为一个列表，最后统计总的数量即可。

    -module(word_number).
    -export([number/1]).
    -export([words/3]).

    number(Text) -> length(words(Text, [], [])).

    words(Text, Words, Word) -> 
       if length(Text) == 0 ->
             if length(Word) /= 0 ->
                 lists:append(Words, [Word]);
             true -> 
                 Words
             end;
       true -> 
          [First | Rest] = Text,
          if First == ($ ) ->
              if length(Word) == 0 ->
                  words(Rest, Words, Word);
              true ->
                  words(Rest, lists:append(Words, [Word]),[])
              end;
          true ->
              words(Rest, Words, lists:append(Word, [First]))
          end
       end.

* 写一个递归计数到10的函数

比前一道简单，只要递归输出计数的数字即可，不过Erlang的格式化输出还是有点奇怪的，出了很多错.

```
-module(count_ten).
-export([number/1]).

number(10) -> io:format("Count:10~n");

number(N) -> 
   io:format("Count:" ++ integer_to_list(N) ++ "~n"),
   number(N + 1).
```

* 写一个函数，在给定输入为{errror, Message}或success的条件下，利用匹配相应地打出"success"或"error: message"

```
-module(printMessage).
-export([message/1]).

message(Content) ->
    case Content of success -> io:format("success~n");
                    {error, Message} -> io:format("error: "　++　Message ++ "~n");

    end.
```

[Erlang day 2](Erlang_day_2.md) ->
