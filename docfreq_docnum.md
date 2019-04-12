### docFreq和docNum为何有时候不对?

#### ES的shard机制

docFreq表示某一个词在多少个文档中出现，docNum表示一共多少个文档。但我们进行少量数据实验的时候，会发现这两个数字不对，目力所及，数一下就发现不对。原因在这里:

    number_of_shards

这一项是创建索引的设置，为了索引文档分布更均匀，从而提供更高的性能，一般在初始化集群的时候，就会设置更多的shards。

docFreq和docNum是针对shard的，考虑的是每一个shard内的情况。这也直接影响了打分，如果仅仅考虑每一个shard内的情况，不做多shard之间的聚合，得到的similarity是不对的。

所以，如果这一点比较关键，就需要考虑修改search__type

    http://localhost:9200/index/_search?search_type=dfs_query_then_fetch
