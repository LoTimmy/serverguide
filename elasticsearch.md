---

```
shell> apt-get install default-jre
shell> java -version

shell> curl -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.1.2.tar.gz
shell> tar -zxvf elasticsearch-5.1.2.tar.gz
shell> cd elasticsearch-5.1.2
shell> ./bin/elasticsearch
shell> curl http://localhost:9200/
```

```
{
  "name" : "r6ur642",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "vxH4NSBVS_qDdENZUw34mA",
  "version" : {
    "number" : "5.1.2",
    "build_hash" : "c8c4c16",
    "build_date" : "2017-01-11T20:18:39.146Z",
    "build_snapshot" : false,
    "lucene_version" : "6.3.0"
  },
  "tagline" : "You Know, for Search"
}
```


---

```
ERROR: bootstrap checks failed
max number of threads [1810] for user [ubuntu] is too low, increase to at least [2048]
max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

`/etc/sysctl.conf`
```
vm.max_map_count = 262144
```

`/etc/security/limits.conf`
```
*            -    nproc     2048
# End of file
```
---

```
OpenJDK 64-Bit Server VM warning: INFO: os::commit_memory(0x0000000085330000, 2060255232, 0) failed; error='Cannot allocate memory' (errno=12)
```
---

- [elasticsearch](https://www.elastic.co/products/elasticsearch)
- [kibana](https://www.elastic.co/products/kibana)
- [How to set or change the default soft or hard limit for the number of user's processes?](https://access.redhat.com/solutions/406663)