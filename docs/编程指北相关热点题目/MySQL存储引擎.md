### 1.MySQL ⽀持哪些存储引擎？默认使⽤哪个？

> **MEMORY**
> MRG_MYISAM
> CSV
> FEDERATED
> PERFORMANCE_SCHEMA
> **MyISAM**
> **InnoDB**(默认)
> ndbinfo
> BLACKHOLE
> ARCHIVE
> ndbcluster

### 2.MyISAM 和 InnoDB 有什么区别？

> - InnoDB 支持行级别的锁粒度，MyISAM 不支持，只支持表级别的锁粒度。
> - MyISAM 不提供事务支持。InnoDB 提供事务支持，实现了 SQL 标准定义了四个隔离级别。
> - MyISAM 不支持外键，而 InnoDB 支持。
> - MyISAM 不支持 MVCC，而 InnoDB 支持。
> - 虽然 MyISAM 引擎和 InnoDB 引擎都是使用 B+Tree 作为索引结构，但是两者的实现方式不太一样。
> - MyISAM 不支持数据库异常崩溃后的安全恢复，而 InnoDB 支持。
> - InnoDB 的性能比 MyISAM 更强大。