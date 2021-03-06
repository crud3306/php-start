

https://www.cnblogs.com/xzwblog/p/6956817.html



什么是事务
-------------
事务是一条或多条数据库操作语句的组合，具备ACID，4个特点。

事务的特性
--------------
原子性：要不全部成功，要不全部撤销

一致性：数据库正确地改变状态后，数据库的一致性约束没有被破坏

隔离性：事务之间相互独立，互不干扰

持久性：事务的提交结果，将持久保存在数据库中



事务问题
--------------
脏读（Dirty Reads）：一个事务正在对一条记录做修改，在这个事务完成并提交前，这条记录的数据就处于不一致状态；这时，另一个事务也来读取同一条记录，如果不加控制，第二个事务读取了这些“脏”数据，并据此做进一步的处理，就会产生未提交的数据依赖关系。这种现象被形象地叫做"脏读"。

不可重复读（Non-Repeatable Reads）：一个事务读取某些数据,在它结束读取之前，另一个事务可能完成了对数据行的更改。当第一个事务试图再次执行同一个查询，服务器就会返回不同的结果。

幻读（Phantom Reads）：一个事务按相同的查询条件重新读取以前检索过的数据，却发现其他事务插入了满足其查询条件的新数据，这种现象就称为“幻读”。

　　“脏读”、“不可重复读”和“幻读”，其实都是数据库读一致性问题，必须由数据库提供一定的事务隔离机制来解决。


数据库实现事务隔离的方式，基本上可分为以下两种。
---------------
1) 一种是在读取数据前，对其加锁，阻止其他事务对数据进行修改。

2) 另一种是不用加任何锁，通过一定机制生成一个数据请求时间点的一致性数据快照（Snapshot)，并用这个快照来提供一定级别（语句级或事务级）的一致性读取。从用户的角度来看，好像是数据库可以提供同一数据的多个版本，因此，这种技术叫做数据多版本并发控制（MultiVersion Concurrency Control，简称MVCC或MCC），也经常称为多版本数据库。


隔离级别
--------------
1) read uncommitted不提交的读：  
即脏读，一个事务修改了一行，另一个事务也可以读到该行。  
如果第一个事务执行了回滚，那么第二个事务读取的就是从来没有正式出现过的值。  

2) read committed提交的读
即不可重复读，试图通过只读取提交的值的方式来解决脏读的问题，但是这又引起了不可重复读取的问题。
一个事务执行一个查询，读取了大量的数据行。在它结束读取之前，另一个事务可能完成了对数据行的更改。当第一个事务试图再次执行同一个查询，服务器就会返回不同的结果。(未加锁)

3) repeatable read 可重复读（默认）
即可重复读，在一个事务对数据行执行读取或写入操作时锁定了这些数据行。
但是这种方式又引发了幻读的问题。

因为只能锁定读取或写入的行，不能阻止另一个事务插入数据，后期执行同样的查询会产生更多的结果。

4) serializable可串行化
事务被强制为依次执行。这是 SQL 标准建议的默认行为。



间隙(gap)锁（Next-Key锁）
--------------
当我们用范围条件而不是相等条件检索数据，并请求共享或排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”，InnoDB也会对这个“间隙”加锁，这种锁机制就是所谓的间隙锁（Next-Key锁）。
　　举例来说，假如emp表中只有101条记录，其empid的值分别是 1,2,...,100,101，下面的SQL：
Select * from emp where empid > 100 for update;
是一个范围条件的检索，InnoDB不仅会对符合条件的empid值为101的记录加锁，也会对empid大于101（这些记录并不存在）的“间隙”加锁。
　　InnoDB使用间隙锁的目的，一方面是为了防止幻读，以满足相关隔离级别的要求，对于上面的例子，要是不使用间隙锁，如果其他事务插入了empid大于100的任何记录，那么本事务如果再次执行上述语句，就会发生幻读；另外一方面，是为了满足其恢复和复制的需要（这一点涉及事物的回滚）。

　　很显然，在使用范围条件检索并锁定记录时，InnoDB这种加锁机制会阻塞符合条件范围内键值的并发插入，这往往会造成严重的锁等待。因此，在实际应用开发中，尤其是并发插入比较多的应用，我们要尽量优化业务逻辑，尽量使用相等条件来访问更新数据，避免使用范围条件。
　　还要特别说明的是，InnoDB除了通过范围条件加锁时使用间隙锁外，如果使用相等条件请求给一个不存在的记录加锁，InnoDB也会使用间隙锁！




多版本并发控制 MVCC
--------------
MVCC (Multiversion Concurrency Control)，即多版本并发控制技术,它使得大部分支持行锁的事务引擎，不再单纯的使用行锁来进行数据库的并发控制，取而代之的是，把数据库的行锁与行的多个版本结合起来，只需要很小的开销,就可以实现一致性非锁定读，从而大大提高数据库系统的并发性能。
　　为了实现MVCC，InnoDB对每一行都加上了两个隐藏的列，其中一列存储行被创建的”时间”，另外一列存储行被删除的”时间”。当然InnoDB存储的并不是绝对的时间，而是系统版本号，即记录创建版本号和删除版本号。每当一个事务开始的时候，InnoDB都会给这个事务分配一个递增的版本号，事务开始时的系统版本号会作为事务的版本号。

注意：只有read-committed和 repeatable-read 两种事务隔离级别才能使用MVCC。

下面在repeatable read隔离级别下，说明MVCC的具体操作：

1) SELECT
对于select语句，只有同时满足了下面两个条件的行，才能被返回:

创建版本号小于或者等于当前事务版本号 ，就是说记录创建是在事务中（等于的情况）或者事务启动之前。
行的删除版本号要么没有被定义，要么大于当前事务的版本号：行的删除版本号如果没有被定义，说明该行没有被删除过；如果删除版本号大于当前事务的版本号，说明该行是被该事务后面启动的事务删除的，由于是repeatable read隔离级别，后开始的事务对数据的影响不应该被先开始的事务看见，所以该行可能被返回。

2) INSERT
在插入操作时，记录的创建版本号改为当前事务版本号。

3) DELETE
在删除操作时，记录的删除版本号改为当前事务版本号，相当于标记为删除，而不是实际删除。

4) UPDATE
在更新操作的时候，采用的是先标记旧的那行记录为已删除，并且删除版本号改为当前事务版本号，然后插入一行新的记录。

上述策略的结果就是，在读取数据的时候，InnoDB几乎不用获得任何锁，每个查询都通过版本检查，只获得自己需要的数据版本，从而大大提高了系统的并发度。  

这种策略的缺点是，每行记录都需要额外的存储空间，更多的行检查工作和一些额外的维护工作。
另外，只有read-committed和 repeatable-read 两种事务隔离级别才能使用MVCC，read-uncommited由于是读到未提交的，所以不存在版本的问题。而serializable 则会对所有读取的行加锁。



死锁
--------------
MyISAM表锁不会出现死锁，这是因为MyISAM总是一次获得所需的全部锁，要么全部满足，要么等待，因此不会出现死锁。但在InnoDB中，除单个SQL组成的事务外，锁是逐步获得的，这就决定了在InnoDB中发生死锁是可能的。
　　
两个事务都需要获得对方持有的排他锁才能继续完成事务，这种循环锁等待就是典型的死锁。

　　InnoDB目前处理死锁的方法是：将持有最少行级排它锁的事务回滚。如果是因为死锁引起的回滚，可以考虑在应用程序中重新执行。但在涉及外部锁，或涉及表锁的情况下，InnoDB并不能完全自动检测到死锁，这需要通过设置锁等待超时参数 innodb_lock_wait_timeout来解决。需要说明的是，这个参数并不是只用来解决死锁问题，在并发访问比较高的情况下，如果大量事务因无法立即获得所需的锁而挂起，会占用大量计算机资源，造成严重性能问题，甚至拖跨数据库。我们通过设置合适的锁等待超时阈值，可以避免这种情况发生。


　　通常来说，死锁都是应用设计的问题，通过调整业务流程、数据库对象设计、事务大小，以及访问数据库的SQL语句，绝大部分死锁都可以避免。介绍几种避免死锁的常用方法。
（1）在应用中，如果不同的程序会并发存取多个表，应尽量约定以相同的顺序来访问表，这样可以大大降低产生死锁的机会。  

（2）在程序以批量方式处理数据的时候，如果事先对数据排序，保证每个线程按固定的顺序来处理记录，也可以大大降低出现死锁的可能。比如两个会话读取前十个用户的信息，每次读取一个，那么我们可以规定他们从第一个用户开始读，而不是倒序，这样不会死锁。  

（3）在事务中，如果要更新记录，应该直接申请足够级别的锁，即排他锁，而不应先申请共享锁，更新时再申请排他锁，因为当用户申请排他锁时，其他事务可能又已经获得了相同记录的共享锁，从而造成锁冲突，甚至死锁。

（4） 选择合理的事务大小，小事务发生锁冲突的几率也更小；  
　　如果出现死锁，可以用SHOW INNODB STATUS命令来确定最后一个死锁产生的原因。返回结果中包括死锁相关事务的详细信息，如引发死锁的SQL语句，事务已经获得的锁，正在等待什么锁，以及被回滚的事务等。  







