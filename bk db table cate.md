bk db table cate


三种虚拟表：临时表、内存表、视图。



我们创建的内存表和MySQL内部临时表有所不同：

      内存表的数据存放在内存中，而内部临时表（我们的query语句产生的）在恰当的时候存放在内存中，当内部临时表变得很大时，MySQL会自动地把它转化为在磁盘上存储的表，而我们创建的内存表，却不会自动转换。


      使用内存表
   内存表是通过哈希散列索引把数据保存在内存中，因此具有极快的速度，适合缓存中小型数据库，但是使用上受到一些限制。

   
 
对于重启造成的数据丢失，有以下的解决办法：
在任何查询之前，执行一次简单的查询，判断heap表是否存在数据，如果不存在，则把数据重新写入，或者DROP表重新复制某张表。这需要多做一次查询。不过可以写成include文件，在需要用该heap表的页面随时调用，比较方便。
对于需要该heap表的页面，在该页面第一次且仅在第一次查询该表时，对数据集结果进行判断，如果结果为空，则需要重新写入数据。这样可以节省一次查询。
更好的办法是在mysql每次重新启动时自动写入数据到heap，但是需要配置服务器，过程比较复杂，通用性受到限制。
————————————————
 

 临时表与内存表

内存表分为2种，但共同点是，重起数据库以后，内存中的数据全部丢失,内存表的功能有部分的限制，有些属性不能像正常表一样使用，所以请大家使用的时候谨慎参照官方文档.下面只是抛砖引玉. 
1.临时表:表建在内存里，数据在内存里 
2.内存表:表建在磁盘里，数据在内存里 




mysql创建内存表 
如何创建内存表？
    创建内存表非常的简单，只需注明 ENGINE= MEMORY 即可:
    CREATE TABLE  `tablename` ( `columnName` varchar(256) NOT NUL) ENGINE=MEMORY DEFAULT CHARSET=latin1 MAX_ROWS=100000000; 


注意：
    当内存表中的数据大于max_heap_table_size设定的容量大小时，mysql会转换超出的数据存储到磁盘上，因此这是性能就大打折扣了，所以我们还需要根据我们的实际情况调整max_heap_table_size，例如在.cnf文件中[mysqld]的下面加入：
-----------------------------------
 