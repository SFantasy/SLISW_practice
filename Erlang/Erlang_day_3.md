# Erlang Day 3

Erlang的重头戏来了－－并发。还记得那句让人记忆深刻的话么：“就让他崩溃吧”。

* 找可以在进程终止时重启它的OTP服务。

在[Erlang Doc](http://www.erlang.org/doc/design_principles/sup_princ.html#id71306)中可以看到OTP监督行为(Supervisor Behaviour)中的3种重启策略(Restart Strategy)，翻译如下:

- one_for_one

如果一个子进程终止了，那么只有那个终止的进程会被重启。

- one_for_all

如果一个子进程终止了，那么所有同一级别的子进程都会被终止并且重启（包含那个终止的子进程）。

- rest_for_one

如果一个子进程终止了，那么子进程集合中的“rest”部分－－如终止的子进程之后的进程都会都会被终止并且重启。

当然，关于重启服务的还有包括：最大重启频率、子进程规范等。该Doc中也给出了一个完整的[例子](http://www.erlang.org/doc/design_principles/sup_princ.html#id71235):

```
-module(ch_sup).
-behaviour(supervisor).

-export([start_link/0]).
-export([init/1]).

start_link() ->
    supervisor:start_link(ch_sup, []).

init(_Args) ->
    {ok, {{one_for_one, 1, 60},
          [{ch3, {ch3, start_link, []},definitively
            permanent, brutal_kill, worker, [ch3]}]}}.
```

* 找构建简单的OTP服务器的文档

[rlang/OTP gen_server](http://www.erlang.org/doc/design_principles/gen_server_concepts.html)

* 监视translate_service，并在它终止时重启它

translate_service是在之前的时候写的一个并发程序，使用的是同步消息的机制。

这里用到的就是OTP中的supervisor，用来监督子进程，而init(_)即是supervisor的回调函数.

```
-module(translate_service).
-behaviour(supervisor).
-export([loop/0, translate/1]).
-export([start/0]).
-export([init/1]).
-export([start_service/0]).

loop() ->
   receive
      {From, "casa"} ->
         From ! "house",
         loop();
      {From, "blanca"} ->
         From ! "white",
         loop();
      % If the Word is not recognized, shutdown the process.
      {From, M} ->
         From ! "I don't understand.",
         exit({M, " Not Understand!"})
end.

% Add an atom(translator) to do the translation
translate(Word) ->
   translator ! {self(), Word},
   receive
      Translation -> Translation
   end.

start() ->
   io:format("Start translating program~n"),
   register(translator, spawn_link(translate_service, loop, [])),
   {ok, whereis(translator)}.

% CALL BACK function
init(_) ->
   {ok, {{one_for_one, 1, 60 },
              [{translate_service, {translate_service, start, []},
                  permanent, brutal_kill, worker, [translate_service]}]}}.

% Use the supervisor to monitor the service and keep it running.
start_service() ->
   io:format("Start service~n"),
   supervisor:start_link(translate_service, []).
```

这一天的内容只完成了一个translate_service的supervisor，感觉还是对OTP里的内容的不了解。<br />Erlang的并发的强大之处恐怕也不止只是在可以很快的通过重启服务来做到“Let is crash”；其实Erlang的并发原语只有三个：spawn(create process), receive, !(send message, same as Scala)，但是通过OTP这个“企业级”的库，却有着可以构建各式各样应用的函数；有点让人不快的是，习惯了一些语言之后，再来看Erlang那行末的", . ;"就会感觉很不适，其实感觉可以将这些不必要的终结符移除的；如果以后有机会的话应该还是会接触到Erlang这门为并发而生的语言。

## 参考

* [Erlang Doc](http://www.erlang.org/doc)
* [Wakatta's Blog](http://blog.wakatta.jp/blog/2011/11/06/seven-languages-in-seven-weeks-erlang-day-3/)
* [Learn you som Erlang](http://learnyousomeerlang.com/what-is-otp#the-common-process-abstracted)

<- [Erlang Day 2](Erlang_day_2.md)
