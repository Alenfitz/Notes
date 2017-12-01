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

事务最初是是针对数据库的操作而言的，''不成功，便成仁''，是对事务一个很好的诠释，在对数据库操作的过程中，如果因为某些原因/异常导致数据不能正常的



