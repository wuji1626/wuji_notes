#1 查询当天执行慢的SQL
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

#2 各种AWR工具的使用
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
