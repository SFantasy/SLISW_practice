# Prolog Day 2

发现了一个有趣的事，在用我的vim编辑Prolog程序的时候, 是默认将其用Perl的高亮进行处理的。

## 做

* 翻转一个列表中的元素次序

由于Head表示的就是列表中的第一个元素，而Tail表示的列表中剩余的几个元素，那么要翻转列表中的元素次序，就先将Head取出并将其放入翻转的列表之中，并对此进行递归操作。

    
    %-- 给定几个翻转列表的事实
    reserve([], []).
    reserve([a],[a]).
    reserve([a,b,c], [c,b,a]).
    
    reserve(List, ReserveList) :- 
       (List = [Head|Tail]), 
       reserve(Tail, ReserveTail), 
       append(ReserveTail, [Head], ReserveList).
    %-- Output
    | ?- reserve([1,2,3,4], List).
    
    List = [4,3,2,1] ? ;
    
    no
    		

* 找出列表中最小的元素

要找出列表中的最小元素，先将Head作为最小的元素，然后不断从剩下的列表中取出其Head，并将之与当前的最小元素相比较，这样就可以找出最小的元素了。

    
    min([A], A).
    min(List, MinNum) :-
       (List = [Head|Tail]),
       min(Tail, MinTailNum),
       MinNum is min(Head, MinTailNum).
    %-- Output
    | ?- min([1,2,3,4], Min).
    
    Min = 1 ? ;
    
    no
    | ?- min([1,2,3,4,5,6], Min).
    
    Min = 1 ? ;
    
    no
    		

* 对一个列表中的元素进行排序。

既然在上一道题目中已经可以找出一个列表中的最小元素，那么可以用选择排序来对列表进行排序，即每一次找出列表中的最小元素，然后放入一个List中，随着该List
的扩大，至原列表中无元素的时候排序也就完成了。不过遗憾的是自己还是没有实现出来－－

以下贴出几种排序的方法（归并、快排等）

    
    %-- 归并 import from en.literateprograms.org
    mergeSort( [], [] ).
    mergeSort( [A], [A] ).
    mergeSort( List, SortedList ) :-
        (List = [FirstItem, SecondItem | Tail]),
        listDivide( List, ListOne, ListTwo ),
    
        mergeSort( ListOne, SortedListOne ),
        mergeSort( ListTwo, SortedListTwo ),
    
        listMerge( SortedListOne, SortedListTwo, SortedList ).
    
    listDivide( [], [], [] ).
    listDivide( [A], [A], [] ).
     
    
    listDivide( List, FirstHalf, LastHalf ) :-
        (List = [FirstItem, SecondItem | Rest]),
        (FirstHalf = [FirstItem | FirstRest]),
        (LastHalf = [SecondItem | SecondRest]),
        listDivide( Rest, FirstRest, SecondRest ).
    
    listMerge( A, [], A ).
    listMerge( [], A, A ).
    
    listMerge( ListOne, ListTwo, MergedList ) :-
        (ListOne = [HeadOne | TailOne]),
        (ListTwo = [HeadTwo | TailTwo]),
        (HeadOne =< HeadTwo),
    
        (MergedList = [HeadOne | MergedTail]),
     
        listMerge( TailOne, ListTwo, MergedTail ).
    
    listMerge( ListOne, ListTwo, MergedList ) :-
        (ListOne = [HeadOne | TailOne]),
        (ListTwo = [HeadTwo | TailTwo]),
        (HeadOne > HeadTwo),
        (MergedList = [HeadTwo | MergedTail]),
        listMerge( ListOne, TailTwo, MergedTail ).		  
    
    %-- 快排 import from wikipedia.org
    q:- L=[33,18,2,77,66,18,9,25], last(P,_), (quicksort(L,P,_), write(P), nl).
     
    partition([], _, [], []). 
    partition([X|Xs], Pivot, Smalls, Bigs) :- 
        (   X @< Pivot ->
            Smalls = [X|Rest],
            partition(Xs, Pivot, Rest, Bigs)
        ;   Bigs = [X|Rest],
            partition(Xs, Pivot, Smalls, Rest)
        ).
     
    quicksort([])     --> [].
    quicksort([X|Xs]) --> 
        { partition(Xs, X, Smaller, Bigger) },
        quicksort(Smaller), [X], quicksort(Bigger).
    :- initialization(q).
    
    %-- Wikipedia上还有一种排序的方法，相当简洁
     
    q:- L=[33,18,2,77,18,66,9,25], (sortcsj(L,P), write(P), nl). 
     
    sortcsj(L,S) :-  permutation(L,S), ordered(S).
     
    ordered([]). 
    ordered([_|[]]).
    ordered([A|[B|T]]) :- A =< B, ordered([B|T]). 
     
    :- initialization(q). 
    		

<- [ Prolog Day 1 ](Prolog_day_1.md) | [ Prolog Day 3 ](Prolog_day_3.md) ->

