# Clojure Day 3

第三天的内容从STM(Software Transactional Memory)谈起，途径ref（引用）、atom（原子）、agent（代理）与future。最近的几门语言到最后都是concurrency，内容没有第二天的多。

* 一个队列实现，队列为空时阻塞并等待新的元素加入

也许我们可以参考一下：[Clojure Reference, fill-queue](http://richhickey.github.com/clojure-contrib/seq-utils-api.html#clojure.contrib.seq-utils/fill-queue)

使用引用在内存中创建一组帐户的向量，实现用于修改帐户余额的借贷函数debit和credit。

这里实现的是对一个帐户进行debit和credit的各个函数

```
;; Define the original balance
(def balance (ref 0))

(defn check []
  (if (< 0 @balance) true nil))

(defn credit [amount]
  (dosync (alter balance + amount)))

(defn debit [amount]
  (dosync (alter balance - amount))

(defn query []
  (println (str "Balance: " @balance)))

(defn bank []
  (print "----------\n---BANK---\n1.Credit:(credit amount)\n2.Debit:(debit amount)\n3.Query:(query)\n----------\n"))

;;
;; output
user=> (load-file "balance.clj")
#'user/bank
user=> (bank)
----------
---BANK---
1.Credit:(credit amount)
2.Debit:(debit amount)
3.Query:(query)
----------
nil
user=> (credit 10)
10
user=> (debit 3)
7
user=> (query)
Balance: 7
nil
```

如果是建立一个多account的“银行”的话则需要创建一组向量用来保存accounts

参见[http://blog.wakatta.jp/blog/2011/11/13/seven-languages-in-seven-weeks-clojure-day-3/](http://blog.wakatta.jp/blog/2011/11/13/seven-languages-in-seven-weeks-clojure-day-3/)

```
(defn check-balance [b]
  "Check that the balance of account is not negative"
  (<= 0 b))

(defn make-account []
  "Create a new account"
  (let [r (ref 0)]
      (set-validator! r check-balance)
      r))  

(defn credit [account, amount]
  "Add amount to account's balance"
  (alter account + amount))

(defn debit [account, amount]
  "Debit amount from account's balance"
  (alter account - amount))
  
(defn balance [account]
  "Return balance of account"
  @account)

(defn make-bank [n]
  "Create a bank of n accounts"
  (vec (repeatedly n make-account)))
  
(defn bank-credit [bank, acc_num, amount]
  "Add amount to acc_num's balance"
  (credit (nth bank acc_num) amount))

(defn bank-debit [bank, acc_num, amount]
  "Debit amount from acc_num's balance"
  (debit (nth bank acc_num) amount))

(defn bank-balance [bank, acc_num]
  "Return the balance of acc_num"
  (balance (nth bank acc_num)))

(defn bank-transfer [bank, acc_num1, acc_num2, amount]
  "Transfer amount from acc_num1 to acc_num2 in bank"
  (dosync 
      (bank-credit bank acc_num2 amount)
      (bank-debit bank acc_num1 amount)))

(defn bank-balances [bank]
  "Show the balance of all accounts"
  (dotimes [i (count bank)]
      (println (str "Account " i ": " (bank-balance bank i)))))
```

* Sleeping barber.

<- [Clojure Day 2](Clojure_day_2.md)
