# Erlang Day 2

虽然在前一天就已经“提前”用过了一些控制结构，但毕竟还是半生不熟的。

控制结构、匿名函数、迭代、映射、过滤、列表解析等等，这一天的内容真够丰富的。(PS:并发又在最后一天。。)

* 考虑包含键－值元组的列表，如[{erlang, "a functional language"}, {ruby, "an OO language"}] 

写一个函数，接受列表和键为参数，返回该键对应的值。

这很简单，只要读入一个Hash表并获取需要查询的Key就可以查得其对应的Value了。

```
-module(day2_hw1).
-export([k_v/2]).

k_v(List, Name) -> 
    [Value || {Key, Value} <- List, Key == Name].

%Compile && Outputs:
1> c(day2_hw1).
{ok,day2_hw1}
2> day2_hw1:k_v([{ruby, "OO"}, {erlang, "FP"}], ruby).
["OO"]
```

* 考虑形如[{item quantity price}, ...]的购物列表。

写一个列表解析，构建形如[{item total_price, ...}]的商品列表，其中total_price是quantity乘以price。

```
-module(day2_hw2).
-export([total/1]).

total(List) ->
   [{Item, Quantity * Price} || {Item, Quantity, Price} <- List].

%Compile && Outputs:
1> day2_hw2:total([{noodle, 4, 5}, {cola, 3, 3}]).
[{noodle,20},{cola,9}]
```

<- [Erlang Day 1](Erlang_day_1.md) | [Erlang Day 3](Erlang_day_3.md) ->
