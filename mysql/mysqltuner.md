---

### 安裝 {#installing}

```
shell> apt-get install mysqltuner
shell> mysqltuner
```

```
 >>  MySQLTuner 1.1.1 - Major Hayden <major@mhtx.net>
 >>  Bug reports, feature requests, and downloads at http://mysqltuner.com/
 >>  Run with '--help' for additional options and output filtering
Please enter your MySQL administrative login: root
Please enter your MySQL administrative password: 
Warning: Using a password on the command line interface can be insecure.
Warning: Using a password on the command line interface can be insecure.
Warning: Using a password on the command line interface can be insecure.

-------- General Statistics -------------------------
[--] Skipped version check for MySQLTuner script
[OK] Currently running supported MySQL version 5.6.19-0ubuntu0.14.04.1
[OK] Operating on 64-bit architecture

-------- Storage Engine Statistics ------------------
[--] Status: +Archive -BDB -Federated +InnoDB -ISAM -NDBCluster 
Warning: Using a password on the command line interface can be insecure.
Warning: Using a password on the command line interface can be insecure.
[--] Data in InnoDB tables: 670M (Tables: 12)
[--] Data in PERFORMANCE_SCHEMA tables: 0B (Tables: 52)
[!!] Total fragmented tables: 3

-------- Security Recommendations  ------------------
Warning: Using a password on the command line interface can be insecure.
[OK] All database users have passwords assigned
Warning: Using a password on the command line interface can be insecure.

-------- Performance Metrics ------------------------
[--] Up for: 26d 0h 8m 17s (30K q [0.014 qps], 2K conn, TX: 472M, RX: 13B)
[--] Reads / Writes: 99% / 1%
[--] Total buffers: 192.0M global + 1.1M per thread (151 max threads)
[OK] Maximum possible memory usage: 352.4M (4% of installed RAM)
[OK] Slow queries: 0% (30/30K)
[OK] Highest usage of available connections: 27% (41/151)
[OK] Key buffer size / total MyISAM indexes: 16.0M/97.0K
[OK] Key buffer hit rate: 100.0% (6M cached / 2 reads)
[!!] Query cache efficiency: 0.0% (0 cached / 39K selects)
[OK] Query cache prunes per day: 0
[OK] Sorts requiring temporary tables: 0% (35 temp sorts / 14K sorts)
[OK] Temporary tables created on disk: 1% (859 on disk / 63K total)
[OK] Thread cache hit rate: 63% (853 created / 2K connections)
[!!] Table cache hit rate: 2% (122 open / 5K opened)
[OK] Open file limit used: 0% (48/5K)
[OK] Table locks acquired immediately: 100% (69K immediate / 69K locks)
[!!] InnoDB data size / buffer pool: 670.2M/128.0M

-------- Recommendations ---
General recommendations:
    Run OPTIMIZE TABLE to defragment tables for better performance
    Increase table_cache gradually to avoid file descriptor limits
Variables to adjust:
    query_cache_limit (> 1M, or use smaller result sets)
    table_cache (> 2000)
    innodb_buffer_pool_size (>= 670M)
```

```sql
SHOW TABLE STATUS;
SELECT TABLE_SCHEMA, TABLE_NAME, DATA_FREE / 1024 / 1024 FROM information_schema.TABLES WHERE ENGINE LIKE 'InnoDB' AND DATA_FREE > 0;

SELECT TABLE_NAME, (DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024 / 1024 FROM information_schema.TABLES WHERE TABLE_SCHEMA='';

OPTIMIZE TABLE foo;
ALTER TABLE test.t1 ENGINE='InnoDB'; 
```

#### :books: 參考網站：
- [mysqltuner](http://mysqltuner.com/)
