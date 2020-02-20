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
select lower(column_name),  data_type || '(' || data_length || ')' as datatype
from all_tab_columns 
where TABLE_NAME = upper('table_name')
and owner = upper('schema_name')
;
```

## Resources:
* http://www.dba-oracle.com/t_sql_response_time.htm
* http://www.dba-oracle.com/m_sql_execute_elapsed_time.htm
* http://www.dba-oracle.com/m_buffer_gets.htm
* http://db.geeksinsight.com/2013/06/20/sql_id-missing-in-dba_hist_sqlstat-why-2/
* http://www.dba-oracle.com/oracle10g_tuning/t_dba_hist_sqlstat.htm
* http://db.geeksinsight.com/2013/06/20/sql_id-missing-in-dba_hist_sqlstat-why-2/
