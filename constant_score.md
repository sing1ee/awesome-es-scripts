### 如何使用ES实现短文本是否匹配的需求

#### 使用constant_score

匹配就是1分，不匹配就是0分。相当于自定义的忽略tf，以及length的TF/IDF计算。非常适合在业务中使用，例如职位的标题命中。

#### 多个字段匹配示例

```json
{
  "explain": true,
  "query": {
    "bool": {
      "should": [
        {
          "constant_score": {
            "filter": {
              "match": {
                "name": "A"
              }
            }
          }
        },
        {
          "constant_score": {
            "filter": {
              "match": {
                "content": "A"
              }
            }
          }
        }
      ]
    }
  }
}
```
名字匹配“A”，或者内容匹配"A"，都可以。如果两个都匹配，文档的得分就是2，只有一个匹配，就是1.

#### 结合boost使用

如果将boost设置为2，如果命中，得分为1\*2=2

```json
{
  "explain": true,
  "query": {
    "bool": {
      "should": [
        {
          "constant_score": {
            "filter": {
              "match": {
                "name": {
                	"query":"A"
                }
              }
            },
            "boost":2
          }
        },
        {
          "constant_score": {
            "filter": {
              "match": {
                "content": "A"
              }
            }
          }
        }
      ]
    }
  }
}
```


