/\*

\* JAVA类首次装入时，会对静态成员变量或方法进行一次初始化,

\* 但方法不被调用是不会执行的，

\* 静态成员变量和静态初始化块级别相同，

\* 非静态成员变量和非静态初始化块级别相同。

\* 先初始化父类的静态代码---&gt;初始化子类的静态代码--&gt;

\* 初始化父类的非静态代码---&gt;初始化父类构造函数---&gt;

\* 初始化子类非静态代码---&gt;初始化子类构造函数

\* \*/

**Java 事务\(Transaction\)**

定义:A database transaction is a larger unit that frames multiple SQL statements. A transaction ensures that the action of the framed statements is atomic  with respect to recovery.

事务最初是是针对数据库的操作而言的，\(一起\)''不成功，便成仁''，是对事务一个很好的诠释，在对数据库操作的过程中,从事务开始到事务提交的过程中，对数据库的操作若成功则同时成功\(commit,持久化在数据库中\)，若有一个失败，则全部失败，回滚\(rollback\)到事务开始的状态。

现有的事务范畴已超出最初的定义范围，现在也可用来指一个程序或程序段，在一个或者多个资源 数据库 文件上为完成某些功能的执行过程的集合。

**事务的基本属性**

ACID   分别是指原子性（atomicity）、一致性（consistency）、隔离性（isolation）和持久性（durability）

A：actomicity  原子性  一个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做。

C:  consistency 一致性 事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。

I:    isolation     隔离性   一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰

D:  durability     持久性  持续性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。



