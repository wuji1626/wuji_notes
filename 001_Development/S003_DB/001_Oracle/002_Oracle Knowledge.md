#1 V$ACTIVE_SESSION_HISTORY
Oracle 11g相对10g增加了一些可用列：  
1）标示这条ASH记录是否被刷入了磁盘  
IS_AWR_SAMPLE：dba_hist_active_sess_history中就没有这一列  


2）SQL语句信息  
SQL_OPNAME：SQL_OPCODE的翻译名，SQL语句类型  

3）对于递归SQL，捕获其父SQL的信息  
&nbsp;&nbsp;&nbsp;&nbsp;TOP_LEVEL_SQL_ID      
&nbsp;&nbsp;&nbsp;&nbsp;TOP_LEVEL_SQL_OPCODE  
  
可以通过这个列，找到存过中最消耗资源的SQL，或者DDL递归调用中，最慢得SQL语句
`SELECT sql_id,count(*) FROM v$active_session_history` 
`WHERE TOP_LEVEL_SQL_ID='5w6mc35fa18tk'`  
`GROUP BY sql_id`  
`ORDER BY 2 DESC;`    
	
4）在ASH中捕获执行计划信息，包括这个语句正在执行哪一步操作  
　SQL_PLAN_LINE_ID                                   
　SQL_PLAN_OPERATION                               
　SQL_PLAN_OPTIONS     
 
可以通过这些列，找到SQL语句最慢得地方，就需要优化这个  

~~~sql  
    SELECT A.SQL_PLAN_HASH_VALUE,
           A.SQL_PLAN_LINE_ID,
           A.SQL_PLAN_OPERATION,
           A.SQL_PLAN_OPTIONS,
           B.OWNER || '.' || B.OBJECT_NAME OBJECT_NAME,
           COUNT(*)
      FROM V$ACTIVE_SESSION_HISTORY A, DBA_OBJECTS B
     WHERE A.SQL_ID = '11jpuymjh9vsc'
       AND A.CURRENT_OBJ# = B.OBJECT_ID(+)
     GROUP BY A.SQL_PLAN_HASH_VALUE,
          A.SQL_PLAN_LINE_ID,
          A.SQL_PLAN_OPERATION,
          A.SQL_PLAN_OPTIONS,
          B.OWNER || '.' || B.OBJECT_NAME
     ORDER BY COUNT(*) DESC;  ====
~~~  
5）SQL一次执行的唯一标示符，  SQL_ID, SQL_EXEC_START, SQL_EXEC_ID 三列来标示一次SQL的执行  
可以找到这次SQL的开始执行时间，以及计算出其这次已经执行了多少时间  
　SQL_EXEC_ID                                        
　SQL_EXEC_START
判断一下一个SQL，有没有出现执行的很慢的时候，比如平时1s，有段时间，执行超过12s  
~~~sql  
	SELECT SQL_ID, SQL_EXEC_START, SQL_EXEC_ID, COUNT(*)
      FROM V$ACTIVE_SESSION_HISTORY A
     WHERE A.SQL_ID = '11jpuymjh9vsc'
     GROUP BY SQL_ID, SQL_EXEC_START, SQL_EXEC_ID
     ORDER BY COUNT(*) DESC  
~~~  

6）并行增强，增加了QC_SESSION_SERIAL# 列，并且增加了PX_FLAGS状态列  
　QC_SESSION_SERIAL#                                 
　PX_FLAGS  
QC_SESSION_ID <> SESSION_ID 的，都是并行子进程。增加了QC_SESSION_SERIAL#可以定义到唯一的一个协调者  

7）Blocking增强，11g通过Blocking解决问题已经很容易了  
BLOCKING_INST_ID：11g新增，怪的很，10g的v$session有该列，但是ASH没有  
  BLOCKING_HANGCHAIN_INFO：指出BLOCKING_SESSION是否在hang chain上  
  REMOTE_INSTANCE#：用于集群等待，标明需要请求的数据块应该由那个实例提供。只有cluster类等待才有这个  
  
8）8.当前处理的对象，新增了一个row# ,以前已经有CURRENT_OBJ#，CURRENT_FILE#，CURRENT_BLOCK#了  
CURRENT_ROW#  
可以检查TX等待柱塞的行，通过拼装ROWID可以找到柱塞的行。  
~~~sql  
 SELECT A.SQL_ID,
           A.CURRENT_OBJ#,
           A.CURRENT_FILE#,
           A.CURRENT_BLOCK#,
           A.CURRENT_ROW#,
           COUNT(*)
      FROM dba_hist_active_sess_history A
     WHERE A.EVENT = 'enq: TX - row lock contention'
     GROUP BY A.SQL_ID,
          A.CURRENT_OBJ#,
          A.CURRENT_FILE#,
          A.CURRENT_BLOCK#,
          A.CURRENT_ROW#
     ORDER BY COUNT(*) DESC
~~~
9）CONSUMER_GROUP  
CONSUMER_GROUP_ID，DBA_RSRC_CONSUMER_GROUPS对应  

10）Time Mobel  
TIME_MODEL：后面的IN,按照二进制组合起来的值，在这次采样间隔内，会话做了那些操作  
   IN_CONNECTION_MGMT：connection management call elapsed time  
   IN_PARSE：parse time elapsed  
   IN_HARD_PARSE：hard parse elapsed time  
   IN_SQL_EXECUTION：sql execute elapsed time  
   IN_PLSQL_EXECUTION：PL/SQL execution elapsed time  
   IN_PLSQL_RPC：inbound PL/SQL rpc elapsed time  
   IN_PLSQL_COMPILATION：PL/SQL compilation elapsed time  
   IN_JAVA_EXECUTION：Java execution elapsed time  
   IN_BIND：repeated bind elapsed time  
   IN_CURSOR_CLOSE        
   IN_SEQUENCE_LOAD：sequence load elapsed time  
当AWR中显示某个TM存在问题时，通过这些列，找到TOP 进程或者SQL存在硬解析的SQL，结果应该和v$sql去比较下  
~~~sql  
 SELECT SQL_PLAN_HASH_VALUE, COUNT(*)
          FROM V$ACTIVE_SESSION_HISTORY
         WHERE IN_HARD_PARSE = 'Y'
         GROUP BY SQL_PLAN_HASH_VALUE
         ORDER BY 2 DESC
~~~  
11）REPLAY特性的会话标示  
REPLAY_OVERHEAD        
   IS_REPLAYED            
   DBREPLAY_FILE_ID       
   DBREPLAY_CALL_COUNTER  

12）时间统计  
TM_DELTA_TIME：一次统计间隔  
   TM_DELTA_CPU_TIME：在这个间隔内，CPU时间  
   TM_DELTA_DB_TIME：在这个间隔内，DB时间  
 因为ASH采样的粒度是1秒，但是进程并不是在1s内都ACTIVE的。该统计的粒度是微秒(百万分之一秒)  
   TM_DELTA_TIME - TM_DELTA_DB_TIME = INACTIVE TIME  
   TM_DELTA_DB_ TIME - TM_DELTA_CPU_TIME = WAIT TIME  
   
13）IO网络统计  
 DELTA_TIME                           
   DELTA_READ_IO_REQUESTS               
   DELTA_WRITE_IO_REQUESTS              
   DELTA_READ_IO_BYTES                  
   DELTA_WRITE_IO_BYTES                 
   DELTA_INTERCONNECT_IO_BYTES  
统计时间内，物理读/写/心跳流量高的SQL  
~~~sql  
     SELECT SQL_ID,
               SUM(DELTA_READ_IO_REQUESTS),
               SUM(DELTA_WRITE_IO_REQUESTS),
               SUM(DELTA_READ_IO_BYTES),
               SUM(DELTA_WRITE_IO_BYTES),
               SUM(DELTA_INTERCONNECT_IO_BYTES)
          FROM V$ACTIVE_SESSION_HISTORY
         GROUP BY SQL_ID
         ORDER BY 2 DESC
~~~  

14）PGA/TMP当前使用统计   
　PGA_ALLOCATED        
　TEMP_SPACE_ALLOCATED 
~~~sql  
select * from (
   select sample_time,session_id,sql_id,PGA_ALLOCATED,TEMP_SPACE_ALLOCATED from v$active_session_history 
   where TEMP_SPACE_ALLOCATED is not null
    order by TEMP_SPACE_ALLOCATED desc
   ) where rownum<=20 
~~~  

15）其他  
IS_SQLID_CURRENT：指出该SQL_ID是否是正在执行的，该列具体意义不明
   TOP_LEVEL_CALL#                               
   TOP_LEVEL_CALL_NAME：v$toplevelcall中有对应，具体怎么用不明  


