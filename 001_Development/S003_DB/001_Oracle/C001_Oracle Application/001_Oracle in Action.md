[TOC]
##1 查询当天执行慢的SQL
查询每天执行慢的SQL：  
> select *  
> from (select sa.SQL_TEXT,  
> sa.SQL_FULLTEXT,  
> sa.EXECUTIONS "exec times",  
> round(sa.ELAPSED_TIME / 1000000, 2) "total time",  
> round(sa.ELAPSED_TIME / 1000000 / sa.EXECUTIONS, 2) "avg time",  
> sa.COMMAND_TYPE,  
> sa.PARSING_USER_ID "user ID",  
> u.username "user name",  
> sa.HASH_VALUE  
> from v$sqlarea sa  
> left join all_users u  
> on sa.PARSING_USER_ID = u.user_id  
> where sa.EXECUTIONS > 0  
> order by (sa.ELAPSED_TIME / sa.EXECUTIONS) desc)  
> where rownum <= 50;

/*  SQL中 COMMAND_TYPE意义:   
2：INSERT  
3：SELECT  
6：UPDATE  
7：DELETE  
189：MERGE  
详情可通过查找V$SQLCOMMAND视图  */

查看历史SQL中的慢SQL  
> select t.sql_text,  
>    u.username,  
>  t.command_type,  
>  h.sample_time,  
>  h.TIME_WAITED/1000000,  
>  h.tm_delta_time/1000000,  
>  h.tm_delta_cpu_time/1000000,  
>  h.tm_delta_db_time/1000000  
>   from dba_hist_active_sess_history h  
>   join dba_hist_sqltext t on h.sql_id =   t.sql_id  
>   JOIN dba_users u on h.user_id = u.user_id
>  where sample_time >  
>    to_date('2017-02-14 14:00:00', 'yyyy-mm-dd hh24:mi:ss')  
>    and sample_time <  
>    to_date('2017-02-14 16:00:00', 'yyyy-mm-dd hh24:mi:ss')  
>    and t.command_type in (2,3,6,7)  
>    and h.tm_delta_cpu_time is not null  
>    order by h.TIME_WAITED/1000000 desc  

##2 各种AWR工具的使用
1.生成**单实例 AWR 报告**：  
@$ORACLE_HOME/rdbms/admin/awrrpt.sql


2.生成**Oracle RAC AWR 报告**：  
@$ORACLE_HOME/rdbms/admin/awrgrpt.sql


3.生成**RAC环境中特定数据库实例的AWR报告**:  
@$ORACLE_HOME/rdbms/admin/awrrpti.sql


4.生成**Oracle RAC环境中多个数据库实例的 AWR 报告**的方法：
@$ORACLE_HOME/rdbms/admin/awrgrpti.sql


5.生成 **SQL 语句的 AWR 报告**：
@$ORACLE_HOME/rdbms/admin/awrsqrpt.sql


6.生成**特定数据库实例上某个 SQL 语句的 AWR 报告**：
@$ORACLE_HOME/rdbms/admin/awrsqrpi.sql
--生成 AWR 时段对比报告

7.生成**单实例 AWR 时段对比报告**：
@$ORACLE_HOME/rdbms/admin/awrddrpt.sql


8.生成**Oracle RAC AWR 时段对比报告**:
@$ORACLE_HOME/rdbms/admin/awrgdrpt.sql


9.生成**特定数据库实例的 AWR 时段对比报告**:
@$ORACLE_HOME/rdbms/admin/awrddrpi.sql


10.生成**Oracle RAC 环境下特定（多个）数据库实例的 AWR 时段对比报告**：
@$ORACLE_HOME/rdbms/admin/awrgdrpi.sql

##3 DBMS_STATS.GATHER_SCHEMA_STATS介绍使用
dbms_stats能良好地估量统计数据（尤其是针对较大的分区表），并能取得更好的统计后果，最终制订出速度更快的SQL施行计划。
~~~
　　exec dbms_stats.gather_schema_stats(
　　ownname          => 'SCOTT',
　　options          => 'GATHER AUTO',
　　estimate_percent => dbms_stats.auto_sample_size,
　　method_opt       => 'for all columns size repeat',
　　degree           => 15
　　)       
~~~
为了充沛认识dbms_stats的益处，需要仔细领会每一条次要的预编译指令（directive）。上面让咱们钻研每一条指令，并领会如何用它为基于代价的SQL优化器搜罗最高品质的统计数据。  
■options参数：4个预设值，这个选项能把握Oracle统计的刷新方法：  
- gather——重新剖析整个架构（Schema）。
- gather empty——只剖析目前还没有统计的表。
- gather stale——只重新剖析修改量超过10%的表（这些修改包含拔出、更新和删除）。
- gather auto——重新剖析以后没有统计的对象，以及统计数据过期（变脏）的对象。注意，使用gather auto相似于组合使用gather stale和gather empty。

>注意，不论gather stale仍是gather auto，都请求进行监视。假如你施行一个alter table xxx monitoring命令，Oracle会用dba_tab_modifications视图来跟踪发生发火变动的表。这样一来，你就确实地知道，自从上 一次剖析统计数据以来，发生发火了多少次拔出、更新和删除操作。

■estimate_percent选项  
estimate_percent参数是一种比照新的设计，它答应Oracle的dbms_stats在搜罗统计数据时，自动估量要采样的一个segment的最佳百分比：  
estimate_percent => dbms_stats.auto_sample_size  
要考证自动统计采样的准确性，你可检视dba_tables sample_size列。一个有趣的地方是，在使用自动采样时，Oracle会为一个样本尺寸挑选5到20的百分比。记住，统计数据品质越好，CBO做出的抉择越好。  

■method_opt选项  
- for table --只统计表
- for all indexed columns --只统计有索引的表列
- for all indexes --只剖析统计相干索引
- for all columns

dbms_stats的method_opt参数尤其合适在表和索引数据发生发火变动时刷新统计数据。method_opt参数也合适用于判断哪些列需要直方图（histograms）。  
某些情形下，索引内的各个值的散播会影响CBO是使用一个索引仍是施行一次全表扫描的决议计划。例如，假如在where子句中指定的值的数量不合错误称，全表扫描就显得比索引走访更经济。  
假如你有一个高度歪斜的索引（某些值的行数不合错误称），就可创建Oracle直方图统计。但在现实世界中，出现这种情形的机率相称小。使用 CBO时，最罕见的过失之一就是在CBO统计中不用要地引入直方图。根据经验，只需在列值请求必需修改施行计划时，才应使用直方图。  
为了智能地生成直方图，Oracle为dbms_stats准备了method_opt参数。在method_opt子句中，还有一些首要的新选项，包含skewonly，repeat和auto：  
- method_opt=>'for all columns size skewonly'
- method_opt=>'for all columns size repeat'
- method_opt=>'for all columns size auto'

skewonly选项会耗损大量处置时间，因为它要检查每个索引中的每个列的值的散播情形。  
假如dbms_stat觉察一个索引的各个列散播得不均匀，就会为那个索引创建直方图，辅助基于代价的SQL优化器抉择是进行索引走访，仍是进行全表 扫描走访。例如，在一个索引中，假设有一个列在50%的行中，如清单B所示，那么为了检索这些行，全表扫描的速度会快于索引扫描。  
　　--*************************************************************
　　-- SKEWONLY option—Detailed analysis
　　--
　　-- Use this method for a first-time analysis for skewed indexes
　　-- This runs a long time because all indexes are examined
　　--*************************************************************
　　begin
　　dbms_stats.gather_schema_stats(
　　ownname          => 'SCOTT',
　　estimate_percent => dbms_stats.auto_sample_size,
　　method_opt       => 'for all columns size skewonly',
　　degree           => 7
　　);
　　end;

　　重新剖析统计数据时，使用repeat选项，重新剖析义务所耗费的资源就会少一些。使用repeat选项（清单C）时，只会为现有的直方图重新剖析索引，不再搜寻其余直方图机会。活期重新剖析统计数据时，你应当采用这种方法。
~~~
　　--*************************************************************
　　-- REPEAT OPTION - Only reanalyze histograms for indexes
　　-- that have histograms
　　--
　　-- Following the initial analysis, the weekly analysis
　　-- job will use the “repeat” option. The repeat option
　　-- tells dbms_stats that no indexes have changed, and
　　-- it will only reanalyze histograms for
　　-- indexes that have histograms.
　　--**************************************************************
　　begin
　　dbms_stats.gather_schema_stats(
　　ownname          => 'SCOTT',
　　estimate_percent => dbms_stats.auto_sample_size,
　　method_opt       => 'for all columns size repeat',
　　degree           => 7
　　);
　　end;
~~~
----------------------------------------------------------------------------------------------------------------------------
Exec dbms_stats.gather_schema_stats(ownname=>'用户名称',estimate_percent=>100,cascade=> TRUE, degree =>12);
含义解释 ownname:填写需要分析的用户（该用户下所有表都将被分析） 
              estimate_percent:分析抽样的力度 
              cascade:是否对索引进行分析 
              degree:并行处理的cpu数量