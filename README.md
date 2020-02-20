# oracle-notes-for-data-engineers
Good to start with https://blog.pythian.com/mining-the-awr-to-identify-performance-trends/

## Monitoring
**V$SQLAREA**: The V$SQLAREA view provides information about the SQL that is being currently executed or has been executed for the database. The V$SQLAREA view shows all SQL, even if it has been embedded into PL/SQL code or is sent to the database via an external program.  

Notice that the V$SQLAREA only shows the first 1000 bytes of any SQL. If the SQL is longer than 1000 bytes the V$SQLTEXT view should be used to see the entire text. The critical statistics in this view for tuning code are 
  
## Elapsed time
elapsed time = cpu time + user i/o wait time + application_wait_time + concurrency_wait_time + cluster_wait_time + plsql_exec_time + java_exec_time 

## Buffer get
Oracle storage is arranged into blocks of a given size (e.g. 8k). Tables and indexes are made up of a series of blocks on the disk. When these blocks are in memory they occupy a buffer.  

When Oracle requires a block it does a buffer get. First it checks to see if it already has the block it needs in memory. If so, the in-memory version is used. If it does not have the block in memory then it will read it from disk into memory.  

So a buffer get represents the number of times Oracle had to access a block. The reads could have been satisfied either from memory (the buffers) or have resulted in a physical IO.  

Since physical IO is so expensive (compared to memory or CPU) one approach to tuning is to concentrate on reduction in buffer gets which is assumed will flow on to reduce physical IO.

## dba_hist_sqlstat 
 * High number of buffer gets.
* Large execution count.
* High number of physical reads.
* High shared memory usage.
* High version count.
* High parse count.
* High elapsed time.
* High execution CPU time.
* High number of rows processed.
* High number of sorts, etc

# Basic queries
* Check the column name and its datatype:
```sql
etlpc_process_group	VARCHAR2(120)
etlpc_wf_process_name	VARCHAR2(120)
etlpc_business_name	VARCHAR2(120)
etlproc_effective_date_char	VARCHAR2(40)
etlproc_effective_date	DATE(7)
etlproc_result	VARCHAR2(96)
etlproc_start_datetime	VARCHAR2(64)
etlproc_end_datetime	DATE(7)
duration_hours_num	NUMBER(22)
duration_hours	VARCHAR2(172)
avg_duration_hours_num	NUMBER(22)
etlpc_business_deadline	DATE(8)
etlpc_business_deadline_char	VARCHAR2(64)
avg_duration_hours	VARCHAR2(172)
etlpc_day_window	VARCHAR2(620)
etlpc_time_window	VARCHAR2(4000)
etlpc_process_desc	VARCHAR2(480)
```

## Resources:
* http://www.dba-oracle.com/t_sql_response_time.htm
* http://www.dba-oracle.com/m_sql_execute_elapsed_time.htm
* http://www.dba-oracle.com/m_buffer_gets.htm
* http://db.geeksinsight.com/2013/06/20/sql_id-missing-in-dba_hist_sqlstat-why-2/
* http://www.dba-oracle.com/oracle10g_tuning/t_dba_hist_sqlstat.htm
* http://db.geeksinsight.com/2013/06/20/sql_id-missing-in-dba_hist_sqlstat-why-2/
