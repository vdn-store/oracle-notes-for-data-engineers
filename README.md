# oracle-notes-for-data-engineers


## Monitoring
**V$SQLAREA**: The V$SQLAREA view provides information about the SQL that is being currently executed or has been executed for the database. The V$SQLAREA view shows all SQL, even if it has been embedded into PL/SQL code or is sent to the database via an external program.  

Notice that the V$SQLAREA only shows the first 1000 bytes of any SQL. If the SQL is longer than 1000 bytes the V$SQLTEXT view should be used to see the entire text. The critical statistics in this view for tuning code are 
  
## Elapsed time
elapsed time = cpu time + user i/o wait time + application_wait_time + concurrency_wait_time + cluster_wait_time + plsql_exec_time + java_exec_time 
  
## Resources:
* http://www.dba-oracle.com/t_sql_response_time.htm
* http://www.dba-oracle.com/m_sql_execute_elapsed_time.htm
