### ES支持字符串编辑距离

#### 6.4.0

#### 使用fuzzy查询

```json
{
    "query": {
        "fuzzy" : {
            "user" : {
                "value": "ki",
                "boost": 1.0,
                "fuzziness": 2,
                "prefix_length": 0,
                "max_expansions": 100
            }
        }
    }
}
```

#### 解读

- fuzziness 最大的编辑距离默认是`AUTO`，
- prefix_length 限制前缀匹配的长度，这一点比较符合实际的相似	度要求，前面的往往不会出错。
- max_expansions The maximum number of terms that the fuzzy query will expand to. Defaults to `50`。
- transpositions Whether fuzzy transpositions (ab → ba) are supported. Default is false.
