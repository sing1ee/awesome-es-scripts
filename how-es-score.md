### ES是如何打分的

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
				"query": "产品总监"
			}
		}
	}
}
```
