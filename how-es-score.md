### ES是如何打分的

#### 6.4.0, 7.0.0 

#### 公式说明

```json
score(q,d)  =  #1
            queryNorm(q)  #2
          · coord(q,d)    #3
          · ∑ (           #4
                tf(t in d)   #5
              · idf(t)²      #6
              · t.getBoost() #7
              · norm(t,d)    #8
            ) (t in q)    #9
1 score(q, d) 是文档 d 与 查询 q 的相关度分数
2 queryNorm(q) 是查询正则因子（query normalization factor）
3 coord(q, d) 是协调因子（coordination factor）
4 #9 查询 q 中每个术语 t 对于文档 d 的权重和
5 tf(t in d) 是术语 t 在文档 d 中的词频
6 idf(t) 是术语 t 的逆向文档频次
7 t.getBoost() 是查询中使用的 boost
8 norm(t,d) 是字段长度正则值，与索引时字段级的boost的和（如果存在）
```

### 调试中，设置打开打分详情

```json
{
	"explain":true,
	"query": {
		"match_phrase":{
			"name": {
				"query": "产品"
			}
		}
	}
}
```

#### 结果如下：

```json
{
    "took": 1052,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 4,
            "relation": "eq"
        },
        "max_score": 0.11859184,
        "hits": [
            {
                "_shard": "[index-4-slop][0]",
                "_node": "8q8F_bhtRZG1-T1tsRWpWA",
                "_index": "index-4-slop",
                "_type": "_doc",
                "_id": "1",
                "_score": 0.11859184,
                "_source": {
                    "name": "产品总监",
                    "content": "这个职位招聘一个产品总监，负责某某产品，是一个总监岗位。"
                },
                "_explanation": {
                    "value": 0.11859184,
                    "description": "weight(name:产品 in 0) [PerFieldSimilarity], result of:",
                    "details": [
                        {
                            "value": 0.11859184,
                            "description": "score(freq=1.0), product of:",
                            "details": [
                                {
                                    "value": 2.2,
                                    "description": "boost",
                                    "details": []
                                },
                                {
                                    "value": 0.105360515,
                                    "description": "idf, computed as log(1 + (N - n + 0.5) / (n + 0.5)) from:",
                                    "details": [
                                        {
                                            "value": 4,
                                            "description": "n, number of documents containing term",
                                            "details": []
                                        },
                                        {
                                            "value": 4,
                                            "description": "N, total number of documents with field",
                                            "details": []
                                        }
                                    ]
                                },
                                {
                                    "value": 0.5116279,
                                    "description": "tf, computed as freq / (freq + k1 * (1 - b + b * dl / avgdl)) from:",
                                    "details": [
                                        {
                                            "value": 1,
                                            "description": "freq, occurrences of term within document",
                                            "details": []
                                        },
                                        {
                                            "value": 1.2,
                                            "description": "k1, term saturation parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 0.75,
                                            "description": "b, length normalization parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 2,
                                            "description": "dl, length of field",
                                            "details": []
                                        },
                                        {
                                            "value": 2.75,
                                            "description": "avgdl, average length of field",
                                            "details": []
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            },
            {
                "_shard": "[index-4-slop][0]",
                "_node": "8q8F_bhtRZG1-T1tsRWpWA",
                "_index": "index-4-slop",
                "_type": "_doc",
                "_id": "2",
                "_score": 0.101582654,
                "_source": {
                    "name": "产品高级总监",
                    "content": "这个职位招聘一个产品高级总监，负责某某产品，是一个高级总监岗位。"
                },
                "_explanation": {
                    "value": 0.101582654,
                    "description": "weight(name:产品 in 1) [PerFieldSimilarity], result of:",
                    "details": [
                        {
                            "value": 0.101582654,
                            "description": "score(freq=1.0), product of:",
                            "details": [
                                {
                                    "value": 2.2,
                                    "description": "boost",
                                    "details": []
                                },
                                {
                                    "value": 0.105360515,
                                    "description": "idf, computed as log(1 + (N - n + 0.5) / (n + 0.5)) from:",
                                    "details": [
                                        {
                                            "value": 4,
                                            "description": "n, number of documents containing term",
                                            "details": []
                                        },
                                        {
                                            "value": 4,
                                            "description": "N, total number of documents with field",
                                            "details": []
                                        }
                                    ]
                                },
                                {
                                    "value": 0.43824703,
                                    "description": "tf, computed as freq / (freq + k1 * (1 - b + b * dl / avgdl)) from:",
                                    "details": [
                                        {
                                            "value": 1,
                                            "description": "freq, occurrences of term within document",
                                            "details": []
                                        },
                                        {
                                            "value": 1.2,
                                            "description": "k1, term saturation parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 0.75,
                                            "description": "b, length normalization parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 3,
                                            "description": "dl, length of field",
                                            "details": []
                                        },
                                        {
                                            "value": 2.75,
                                            "description": "avgdl, average length of field",
                                            "details": []
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            },
            {
                "_shard": "[index-4-slop][0]",
                "_node": "8q8F_bhtRZG1-T1tsRWpWA",
                "_index": "index-4-slop",
                "_type": "_doc",
                "_id": "3",
                "_score": 0.101582654,
                "_source": {
                    "name": "高级产品总监",
                    "content": "这个职位招聘一个高级产品总监，负责某某产品，是一个高级总监岗位。"
                },
                "_explanation": {
                    "value": 0.101582654,
                    "description": "weight(name:产品 in 2) [PerFieldSimilarity], result of:",
                    "details": [
                        {
                            "value": 0.101582654,
                            "description": "score(freq=1.0), product of:",
                            "details": [
                                {
                                    "value": 2.2,
                                    "description": "boost",
                                    "details": []
                                },
                                {
                                    "value": 0.105360515,
                                    "description": "idf, computed as log(1 + (N - n + 0.5) / (n + 0.5)) from:",
                                    "details": [
                                        {
                                            "value": 4,
                                            "description": "n, number of documents containing term",
                                            "details": []
                                        },
                                        {
                                            "value": 4,
                                            "description": "N, total number of documents with field",
                                            "details": []
                                        }
                                    ]
                                },
                                {
                                    "value": 0.43824703,
                                    "description": "tf, computed as freq / (freq + k1 * (1 - b + b * dl / avgdl)) from:",
                                    "details": [
                                        {
                                            "value": 1,
                                            "description": "freq, occurrences of term within document",
                                            "details": []
                                        },
                                        {
                                            "value": 1.2,
                                            "description": "k1, term saturation parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 0.75,
                                            "description": "b, length normalization parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 3,
                                            "description": "dl, length of field",
                                            "details": []
                                        },
                                        {
                                            "value": 2.75,
                                            "description": "avgdl, average length of field",
                                            "details": []
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            },
            {
                "_shard": "[index-4-slop][0]",
                "_node": "8q8F_bhtRZG1-T1tsRWpWA",
                "_index": "index-4-slop",
                "_type": "_doc",
                "_id": "5",
                "_score": 0.101582654,
                "_source": {
                    "name": "总监负责产品",
                    "content": "现在 高级产品经理\n2003。4-2003。11 产品副经理\n向产品群经理汇报工作\n负责产品为：得普利麻\n2002。5-2003。3 产品副经理\n向产品群经理汇报工作\n负责推广产品为：精分（思瑞康），麻醉（得普利麻）"
                },
                "_explanation": {
                    "value": 0.101582654,
                    "description": "weight(name:产品 in 3) [PerFieldSimilarity], result of:",
                    "details": [
                        {
                            "value": 0.101582654,
                            "description": "score(freq=1.0), product of:",
                            "details": [
                                {
                                    "value": 2.2,
                                    "description": "boost",
                                    "details": []
                                },
                                {
                                    "value": 0.105360515,
                                    "description": "idf, computed as log(1 + (N - n + 0.5) / (n + 0.5)) from:",
                                    "details": [
                                        {
                                            "value": 4,
                                            "description": "n, number of documents containing term",
                                            "details": []
                                        },
                                        {
                                            "value": 4,
                                            "description": "N, total number of documents with field",
                                            "details": []
                                        }
                                    ]
                                },
                                {
                                    "value": 0.43824703,
                                    "description": "tf, computed as freq / (freq + k1 * (1 - b + b * dl / avgdl)) from:",
                                    "details": [
                                        {
                                            "value": 1,
                                            "description": "freq, occurrences of term within document",
                                            "details": []
                                        },
                                        {
                                            "value": 1.2,
                                            "description": "k1, term saturation parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 0.75,
                                            "description": "b, length normalization parameter",
                                            "details": []
                                        },
                                        {
                                            "value": 3,
                                            "description": "dl, length of field",
                                            "details": []
                                        },
                                        {
                                            "value": 2.75,
                                            "description": "avgdl, average length of field",
                                            "details": []
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            }
        ]
    }
}
```
