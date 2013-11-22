# Prolog Day 1

看到‘地图着色’那个例子的时候觉得prolog这下牛逼了……的确，只要通过描述的事实，就能将其填色，这在高中做数学作业的时候要有这工具，‘妈妈在也不用担心我的学习了’。:)

BTW,这次的作业蛮少的……

## 做

* 建立一个简单的知识库。描述一些你喜欢的书籍和作者。

感觉这题都不用写‘规则’……事实也的确是，就像SQL一样进行Select

    
    author('hanhan').
    author('wujun').
    author('lutz').
    author('Qianneng').
    
    book('1988').
    book('C++ programming').
    book('learning python').
    book('qingchun').
    book('Art of Math').
    book('lang chao zhi dian').
    
    wrote('hanhan', '1988').
    wrote('hanhan', 'qingchun').
    wrote('wujun', 'Art of Math').
    wrote('wujun', 'lang chao zhi dian').
    wrote('lutz', 'learning python').
    wrote('Qianneng', 'C++ programming').
    		

* 找出知识库中某位作者编写的所有书籍。

```
| ?-wrote('hanhan', What).
What = '1988' ? ;
What = qingchun ? 
yes
```    		

* 建立一个描述音乐家和乐器的知识库，同时也描述出音乐家以及他们的音乐风格。

```    
musician('Jay').
musician('Eason').
musician('Langlang').
musician('Lee hom').
musician('Xin').

instrument('Guitar').
instrument('Piano').

style('Jay', 'R&B;').
style('Eason', 'Pop').
style('Langlang', 'classic').
style('Lee hom', 'Pop').
style('Xin', 'Pop').

musician_use('Jay', 'Guitar').
musician_use('Jay', 'Piano').
musician_use('Eason', 'Piano').
musician_use('Langlang', 'Piano').
musician_use('Lee hom','Piano').
musician_use('Xin', 'Guitar').
```    		

* 找出所有使用吉他的音乐家。

```   
| -?musician_use(Who, 'Guitar').
Who = 'Jay' ? ;
Who = 'Xin' ?
yes
```    		

[Prolog Day 2](Prolog_day_2.md) ->

