# MT_TiDB
DBA Operation And Maintenance Tool For TiDB Database


# 1. 工具使用简介
## 1.1 配置别名 - [别名及路径基于实际情况进行修改]

alias vi='vim'  
alias viti='vi /nas/vm_workdir/Ti_TiDB'  
alias ti='/nas/vm_workdir/Ti_TiDB'  

## 1.2 设置数据库连接
  Ti_TiDB为一个shell脚本，直接编辑修改，修改开始部分的数据库连接信息
![image](https://github.com/mfhd04/MT_TiDB/assets/68178811/3dad52f0-36c6-4bdc-ab41-31e25cf0b23c)


## 1.3 验证配置
 修改相关必要变量后，通过如下命令验证工具和数据库集群连通性。
 $ ti test
```
[root@tidb-server vm_workdir]# ti test
+---------------------+--------------------+
| SYSDATE()           | version()          |
+---------------------+--------------------+
| 2024-05-07 15:29:56 | 5.7.25-TiDB-v6.5.9 |
+---------------------+--------------------+
[root@tidb-server vm_workdir]# 
```
     
## 1.4 特别说明：
  - 部分菜单功能需要基于tiup环境才可以正常使用，建议该工具部署在可以直接调用tiup命令的用户下，如tidb用户


# 2. 功能明细
## 2.1 工具菜单
```
[tidb@tidb-server tidb]$ ti

  Usage: mt <command> [<arguments>]
  e.g. : mt test
  
    General
      [] is the default ;  <> is a must
                                          
    Commands list:                          

      - basic                                                                 Display the TiDB Cluster Basic information
      - user                                                                  Display useres information in the server       
      - var | par <variables>                                                 Show the values of TiDB system variables. Support like query
      - config | conf <variables>                                             Show the values of TiDB component configuration. Support like query
      - char_coll | character and collation                                   Show all available character sets and collations,Support like query
      - autoinc                                                               Display the auto-incrementing information of the table in the TiDB database
      - d    | database                                                       Show all databases in mysql server. Support like query
      - dt                                                                    Show all tables in the database. Support like query
      - source <exec.sql>                                                     Execute an SQL script file. Takes a file name as an argument
      - desc <database_name> <table_name>                                     Display columns information about the table
      - col  | column  <database_name> <table_name>                           Display columns information about the table
      - find_obj                                                              Find table or index information containing specific keywords
      - host_cpu                                                              Display the host CPU information
      - time_to_tso <tso>                                                     Convert the time to tso
      - tso_to_time <time>                                                    Convert the tso to time

                                                                                 
      - tab_nopk                                                              Display tables in TiDB database without primary keys
      - tab_idx_col_dup                                                       Display the index information with duplicate columns
      - tab_idx  | index <database_name> <table_name>                         Display table indexes information
      - tab_info <database_name> <table_name>                                 Display table information, including attributes、indexes、columns、and create statements     
      - tab_size                                                              Display the table or index size
      - db_size                                                               Display the spescial db or all db size
      - tab_region_dist <database_name> <table_name>                          Display the table and index region distribute
      - tab_region_leader <database_name> <table_name>                        Display the table and index region leader distribute
      - index_region_dist <database_name> <table_name>  <index_name>          Display the index region distribute

      - stats_unhealthy_missing                                               Display Table statistics is unhealthy and missing
      - stats_table | stats_table <database_name> <table_name>                Display the table statistics information detail
      - stats_analyze_table  | sat <database_name> <table_name>               Analyze the table statistics
      - stats_analyze_table2 | sat2 <database_name> <table_name>              Analyze the table statistics with 1 samplerate
      - stats_drop_table | sdt <database_name> <table_name>                   Drop the table statistics     
                                                                
      - top_tikv_cpu_sql                                                      Display the Top consume tikv cpu SQL
      - top_avg_tikv_cpu_sql                                                  Display the Top avg consume tikv cpu SQL
      - top_tikv_keys_sql                                                     Display the Top scan keys SQL
      - top_fre_sql                                                           Display the High frequency SQL
      - top_mem_sql                                                           Display the top memory consume SQL
      - top_time_noidx_sql                                                    Display Query which SQL statements are not indexed and have the highest overall time consumption
      - top_time_high_fre_sql                                                 Display A single SQL statement is fast but has too many executions, resulting in the most time-consuming SQL statement in total
      - top_slow_query_sql                                                    Display SQL with the highest number of occurrences in slow query
                                                                               
      - hot_read_write                                                        Display the Current Hot table through information_schema.tidb_hot_regions
      - hot_tikv_store                                                        Display the Current Hot tikv store through information_schema.tidb_hot_regions
      
      - sql_diag_analysis                                                     SQL diagnosis and output related information
      - sql_plan_change                                                       Display the SQL List which Plan change at during special time;
      - sql_slow_exec_hist                                                    Display the slow query SQL execute history changes
      - sql_full_scan                                                         Display the full table scan SQL
      - sql_slow_long                                                         Display the slow query SQL long query time
      - sql_exec_detail                                                       Obtain the execution status of the specified SQL digest in the statement summary
      - sql_search_text                                                       Find SQL statements containing specific text from a slow query and INFORMATION_SCHEMA.CLUSTER_STATEMENTS_SUMMARY
      - sql_search_digest                                                     Find SQL statements containing specific sql digest from a slow query and INFORMATION_SCHEMA.CLUSTER_STATEMENTS_SUMMARY
      - sql_binding                                                           List All sql binding information from mysql.bind_info
      - sql_binding_filter | sql_binding_filter[ or sbw] <key_word>           List All sql binding information from mysql.bind_info,filter with the original sql text key words
      - sql_binding_enable_stmt                                               Enable the binding by the SQL Statement
      - sql_binding_disable_stmt                                              Disable the binding by the SQL Statement
      - sql_binding_enable_digest    | above v7.1.x                           Enable the binding by the SQL Digest
      - sql_binding_disable_digest   | above v7.1.x                           Disable the binding by the SQL Digest
      - sql_text                                                              Display the SQL Text through the SQL digest

      - dbstatus                                                              Obtain the current status of the database, including active sessions, token limit configuration, transaction blocking, and top slow queries in the past 30 minutes
      - active_sess_simple                                                    Display the active sessions Parts
      - active_sess                                                           Display the active sessions Parts and Group by instance/user/client ip
      - connection_summary                                                    TiDB database current connection information, excluding background processes ,Display in columns
      - connection_xplan                                                      Obtain the execution plan of the SQL corresponding to the specified connection ID
      - connection_detail <connection_id>                                     Display the special connection details
      - lock                                                                  Display session locking information  
      - kill_conneciton_id |mt kci connection_id                              Kill the specified connection ID
      - kill_session_all_select                                               Script to generate kill all select session's sql statements and execution
      - kill_session                                                          Script to generate kill session's sql statements and execution
      - kill_session_sql_digest                                               Script to generate kill session's sql statements via digest and execution

      - gc_blocker                                                            Query GC for session block through CLUSTER-LOG
      - ddl_blocker                                                           Query DDL for session blocker
      - pd_leader_change                                                      Change the PD Leader to other PD Member
      - tikv_leader_evict                                                     Evict the region leader on the special tikv host or tikv instance
      - tikv_leader_evict_recovery                                            Recovery the region leader evict policy on the special tikv

      - drcheck | dr <cluster_name>                                           Display the DR Auto-Sync Configuration and Status
      

  NOTE 
  ================
    - Set environment variable DBUSER to change connect string which  is "/bin/mysql -uroot -h10.0.0.89 -P4000"
    - Set environment variable tit_TMP to the default temp directory (default if /tmp when not set)
    - Common abbreviations, beginning with g(global), e(exactly), ms(master slave), ending with g(\G Display in columns)

```


## 2.2 功能演示
### 2.2.1 基本信息
```
[tidb@tidb-server tidb]$ ti basic
+---------------------+--------------------+
| SYSDATE()           | version()          |
+---------------------+--------------------+
| 2024-05-05 16:59:44 | 5.7.25-TiDB-v6.5.9 |
+---------------------+--------------------+


========================================= Current Session Status =========================================
--------------
/bin/mysql  Ver 15.1 Distrib 5.5.68-MariaDB, for Linux (x86_64) using readline 5.1

Connection id:          485
Current database:
Current user:           root@10.0.0.89
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server:                 MySQL
Server version:         5.7.25-TiDB-v6.5.9 TiDB Server (Apache License 2.0) Community Edition, MySQL 5.7 compatible
Protocol version:       10
Connection:             10.0.0.89 via TCP/IP
Server characterset:    utf8mb4
Db     characterset:    utf8mb4
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               4000
Uptime:                 46 min 53 sec

Threads: 0  Questions: 0  Slow queries: 0  Opens: 0  Flush tables: 0  Open tables: 0  Queries per second avg: 0.000
--------------



========================================= information_schema.cluster_info =========================================
+---------+-----------------+-----------------+---------+------------------------------------------+---------------------------+------------------+-----------+
| TYPE    | INSTANCE        | STATUS_ADDRESS  | VERSION | GIT_HASH                                 | START_TIME                | UPTIME           | SERVER_ID |
+---------+-----------------+-----------------+---------+------------------------------------------+---------------------------+------------------+-----------+
| pd      | 10.0.0.89:2379  | 10.0.0.89:2379  | 6.5.9   | efb03ec46d1d76940f13d31e643ca263cc2d5d2d | 2024-05-05T04:12:02-04:00 | 47m42.033876847s |         0 |
| pd      | 10.0.0.89:2479  | 10.0.0.89:2479  | 6.5.9   | efb03ec46d1d76940f13d31e643ca263cc2d5d2d | 2024-05-05T04:11:57-04:00 | 47m47.033877639s |         0 |
| pd      | 10.0.0.90:2479  | 10.0.0.90:2479  | 6.5.9   | efb03ec46d1d76940f13d31e643ca263cc2d5d2d | 2024-05-05T04:12:00-04:00 | 47m44.03387817s  |         0 |
| tidb    | 10.0.0.89:4000  | 10.0.0.89:10080 | 6.5.9   | 9815b4534e22d5db87ad38347546071d27c58431 | 2024-05-05T04:12:51-04:00 | 46m53.033874282s |   3889696 |
| tiflash | 10.0.0.89:3930  | 10.0.0.89:20292 | 6.5.9   | 0be447c1167454e092fadf228aa2ab11cef3f6aa | 2024-05-05T04:11:53-04:00 | 47m51.033882097s |         0 |
| tikv    | 10.0.0.90:20160 | 10.0.0.90:20180 | 6.5.9   | fcbd1624b4445574af328fe78fbb683de76057c8 | 2024-05-05T04:12:38-04:00 | 47m6.033878751s  |         0 |
| tikv    | 10.0.0.90:20161 | 10.0.0.90:20181 | 6.5.9   | fcbd1624b4445574af328fe78fbb683de76057c8 | 2024-05-05T04:12:40-04:00 | 47m4.033879242s  |         0 |
| tikv    | 10.0.0.90:20162 | 10.0.0.90:20182 | 6.5.9   | fcbd1624b4445574af328fe78fbb683de76057c8 | 2024-05-05T04:12:47-04:00 | 46m57.033881065s |         0 |
| tikv    | 10.0.0.89:20160 | 10.0.0.89:20180 | 6.5.9   | fcbd1624b4445574af328fe78fbb683de76057c8 | 2024-05-05T04:12:05-04:00 | 47m39.033881637s |         0 |
| tikv    | 10.0.0.89:20162 | 10.0.0.89:20182 | 6.5.9   | fcbd1624b4445574af328fe78fbb683de76057c8 | 2024-05-05T04:12:32-04:00 | 47m12.033882558s |         0 |
| tikv    | 10.0.0.89:20161 | 10.0.0.89:20181 | 6.5.9   | fcbd1624b4445574af328fe78fbb683de76057c8 | 2024-05-05T04:12:19-04:00 | 47m25.033882999s |         0 |
+---------+-----------------+-----------------+---------+------------------------------------------+---------------------------+------------------+-----------+


========================================= TiKV Store Status =========================================
+----------+-----------------+------------------+------+---------+-----------+---------+----------+-----------+--------------+--------------+------------------+
| store_id | address         | STORE_STATE_NAME | Dc   | Logic   | Host      | VERSION | CAPACITY | AVAILABLE | LEADER_COUNT | region_count | uptime           |
+----------+-----------------+------------------+------+---------+-----------+---------+----------+-----------+--------------+--------------+------------------+
|       96 | 10.0.0.89:3930  | Up               | NULL | tiflash | sh        | v6.5.9  | 45.1GiB  | 12.23GiB  |            0 |            0 | 47m41.59306799s  |
|        1 | 10.0.0.89:20160 | Up               | dc1  | logic1  | host-89-1 | 6.5.9   | 20GiB    | 12.23GiB  |           11 |           14 | 47m35.532933495s |
|    18002 | 10.0.0.89:20161 | Up               | dc1  | logic2  | host-89-2 | 6.5.9   | 20GiB    | 12.23GiB  |            1 |           14 | 47m23.398313846s |
|    18001 | 10.0.0.89:20162 | Up               | dc1  | logic3  | host-89-3 | 6.5.9   | 20GiB    | 12.23GiB  |            2 |           14 | 47m11.516631843s |
|    40562 | 10.0.0.90:20160 | Up               | dc2  | logic4  | host-90-1 | 6.5.9   | 20GiB    | 19.69GiB  |            0 |           14 | 47m5.873848721s  |
|    40563 | 10.0.0.90:20161 | Up               | dc2  | logic5  | host-90-2 | 6.5.9   | 20GiB    | 19.69GiB  |            0 |           14 | 47m1.928760027s  |
|    40578 | 10.0.0.90:20162 | Up               | dc2  | logic6  | host-90-3 | 6.5.9   | 20GiB    | 19.69GiB  |            0 |           14 | 46m53.681793432s |
+----------+-----------------+------------------+------+---------+-----------+---------+----------+-----------+--------------+--------------+------------------+


========================================= mysql.tidb =========================================
+--------------------------+----------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
| VARIABLE_NAME            | VARIABLE_VALUE                                                                               | COMMENT                                                                                     |
+--------------------------+----------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
| bootstrapped             | True                                                                                         | Bootstrap flag. Do not delete.                                                              |
| tidb_server_version      | 110                                                                                          | Bootstrap version. Do not delete.                                                           |
| system_tz                | Asia/Shanghai                                                                                | TiDB Global System Timezone.                                                                |
| new_collation_enabled    | True                                                                                         | If the new collations are enabled. Do not edit it.                                          |
| tikv_gc_leader_uuid      | 63d1f4070440009                                                                              | Current GC worker leader UUID. (DO NOT EDIT)                                                |
| tikv_gc_leader_desc      | host:tidb-server, pid:25353, start at 2024-05-05 04:12:52.139339183 -0400 EDT m=+0.377560706 | Host name and pid of current GC leader. (DO NOT EDIT)                                       |
| tikv_gc_leader_lease     | 20240505-05:00:52.150 -0400                                                                  | Current GC worker leader lease. (DO NOT EDIT)                                               |
| tikv_gc_auto_concurrency | true                                                                                         | Let TiDB pick the concurrency automatically. If set false, tikv_gc_concurrency will be used |
| tikv_gc_enable           | true                                                                                         | Current GC enable status                                                                    |
| tikv_gc_run_interval     | 10m0s                                                                                        | GC run interval, at least 10m, in Go format.                                                |
| tikv_gc_life_time        | 10m0s                                                                                        | All versions within life time will not be collected by GC, at least 10m, in Go format.      |
| tikv_gc_last_run_time    | 20240505-04:56:52.162 -0400                                                                  | The time when last GC starts. (DO NOT EDIT)                                                 |
| tikv_gc_safe_point       | 20240505-04:46:52.162 -0400                                                                  | All versions after safe point can be accessed. (DO NOT EDIT)                                |
| tikv_gc_scan_lock_mode   | legacy                                                                                       | Mode of scanning locks, "physical" or "legacy"                                              |
| tikv_gc_mode             | distributed                                                                                  | Mode of GC, "central" or "distributed"                                                      |
| tikv_gc_concurrency      | 2                                                                                            | How many goroutines used to do GC parallel, [1, 128], default 2                             |
+--------------------------+----------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+


========================================= Placement Labels Information =========================================
+--------+--------------------------------------------------------------------------------+
| Key    | Values                                                                         |
+--------+--------------------------------------------------------------------------------+
| dc     | ["dc1", "dc2"]                                                                 |
| engine | ["tiflash"]                                                                    |
| host   | ["host-89-1", "host-89-2", "host-89-3", "host-90-1", "host-90-2", "host-90-3"] |
| logic  | ["logic1", "logic2", "logic3", "logic4", "logic5", "logic6"]                   |
| zone   | ["sh"]                                                                         |
+--------+--------------------------------------------------------------------------------+
[tidb@tidb-server tidb]$ 
```
### 2.2.2 用户信息
![image](https://github.com/mfhd04/MT_TiDB/assets/68178811/811298da-96c9-4363-aac6-f034a66f08b6)


### 2.2.3 Tikv top cpu
```
[tidb@tidb-server tidb]$ ti top_tikv_cpu_sql
请输入开始时间（留空则默认为半小时前:2024-05-05 17:05:21）: 
开始时间：2024-05-05 17:05:22
请输入结束时间（留空则默认为当前时间:2024-05-05 17:35:22）: 
结束时间：2024-05-05 17:35:22
****************************** Check through the STATEMENTS_SUMMARY ******************************
****************************** Check through the CLUSTER_SLOW_QUERY ******************************
*************************** 1. row ***************************
            digest: 1a96197a635ab3d1c0fc027ce178d8aa751e4bd033c39db6163d3ba75b49fe3f
             query: select job_meta, processing from mysql.tidb_ddl_job where job_id in   (select min(job_id) from mysql.tidb_ddl_job group by schema_ids, table_ids, processing)   and not reorg  order by processing desc, job_id;
sum(Request_count): 8
            avg_rq: 2.0000
     count(digest): 4
*************************** 2. row ***************************
            digest: ea4709893ffb8edc8d58191ccbd93c4c4fdfc1d20ebbcc7f48707df328d6dbb2
             query: SELECT version, table_id, modify_count, count from mysql.stats_meta where version > 449550306866102277 order by version;
sum(Request_count): 4
            avg_rq: 2.0000
     count(digest): 2
*************************** 3. row ***************************
            digest: 1419dc3f429d38aa633c567e9a7382c40500243679dfa77e81a8e3ecb9436ee2
             query: select job_meta, processing from mysql.tidb_ddl_job where job_id in   (select min(job_id) from mysql.tidb_ddl_job group by schema_ids, table_ids, processing)   and  reorg  order by processing desc, job_id;
sum(Request_count): 4
            avg_rq: 2.0000
     count(digest): 2
*************************** 4. row ***************************
            digest: 9686cfbfb4a7fc958612fb976fd5a83eb6e860034faa5b9ccc6c7e87ce5fa67c
             query: SELECT original_sql, bind_sql, default_db, status, create_time, update_time, charset, collation, source, sql_digest, plan_digest  FROM mysql.bind_info WHERE update_time > '2024-04-25 23:40:33.403' ORDER BY update_time, create_time;
sum(Request_count): 4
            avg_rq: 1.0000
     count(digest): 4
*************************** 5. row ***************************
            digest: 0ed49e5e439bb421cb38216101128ac3c2aa171d31dfb32bbd4c655767a2a697
             query: SELECT message  FROM INFORMATION_SCHEMA.CLUSTER_LOG     WHERE message like '%gc safepoint blocked by a running session%'      AND time between '2024-05-05 17:01:30' and '2024-05-05 17:31:30'     AND type='tidb'     ORDER BY time DESC     LIMIT 1;
sum(Request_count): 0
            avg_rq: 0.0000
     count(digest): 1
[tidb@tidb-server tidb]$ 
[tidb@tidb-server tidb]$ 
```
### 2.2.4 DR Auto-Sync检查
```
[tidb@tidb-server tidb]$ ti dr tidb-v6
Need to execute under the tidb user and on tiup host!

==================================== The PD Member Status ====================================
Cluster type:       tidb
Cluster name:       tidb-v6
Cluster version:    v6.5.9
Deploy user:        tidb
SSH type:           builtin
Dashboard URL:      http://10.0.0.89:2379/dashboard
ID              Role  Host       Ports      OS/Arch       Status  Data Dir                 Deploy Dir
--              ----  ----       -----      -------       ------  --------                 ----------
10.0.0.89:2379  pd    10.0.0.89  2379/2380  linux/x86_64  Up|UI   /tidb/tidb-data/pd-2379  /tidb/tidb-deploy/pd-2379
10.0.0.89:2479  pd    10.0.0.89  2479/2480  linux/x86_64  Up      /tidb/tidb-data/pd-2479  /tidb/tidb-deploy/pd-2479
10.0.0.90:2479  pd    10.0.0.90  2479/2480  linux/x86_64  Up|L    /tidb/tidb-data/pd-2479  /tidb/tidb-deploy/pd-2479
Total nodes: 3

==================================== The Region and Region Leader Distribute ====================================
+-----------------+------+---------+-----------+--------------+--------------+---------------+---------------+
| address         | dc   | logic   | host      | leader_count | region_count | leader_weight | region_weight |
+-----------------+------+---------+-----------+--------------+--------------+---------------+---------------+
| 10.0.0.89:3930  | NULL | tiflash | sh        |            0 |            0 |             1 |             1 |
| 10.0.0.89:20160 | dc1  | logic1  | host-89-1 |           11 |           14 |             1 |             1 |
| 10.0.0.89:20161 | dc1  | logic2  | host-89-2 |            1 |           14 |             1 |             1 |
| 10.0.0.89:20162 | dc1  | logic3  | host-89-3 |            2 |           14 |             1 |             1 |
| 10.0.0.90:20160 | dc2  | logic4  | host-90-1 |            0 |           14 |             1 |             1 |
| 10.0.0.90:20161 | dc2  | logic5  | host-90-2 |            0 |           14 |             1 |             1 |
| 10.0.0.90:20162 | dc2  | logic6  | host-90-3 |            0 |           14 |             1 |             1 |
+-----------------+------+---------+-----------+--------------+--------------+---------------+---------------+

==================================== The Placement-Rules Configuration ====================================
[dc1-logic1].[voter].["logic1"]
[dc1-logic2].[voter].["logic2"]
[dc1-logic3].[voter].["logic3"]
[dc2-logic4].[follower].["logic4"]
[dc2-logic5].[follower].["logic5"]
[dc2-logic6].[learner].["logic6"]

==================================== The DR Auto-Sync Configuration ====================================
{
  "replication-mode": "dr-auto-sync",
  "dr-auto-sync": {
    "label-key": "dc",
    "primary": "dc1",
    "dr": "dc2",
    "primary-replicas": 3,
    "dr-replicas": 2,
    "wait-store-timeout": "15s",
    "wait-recover-timeout": "0s",
    "pause-region-split": "false"
  }
}
```
