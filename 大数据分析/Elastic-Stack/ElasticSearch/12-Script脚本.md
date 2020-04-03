# 1 认识

```properties
POST prod_search_info/_search
{
  "query": {
    "bool": {
      "filter": {
        "script": {
          "script": {
            "lang": "painless",
            "source": "doc['teacherName'].value.startsWith('教授')"
          }
        }
      }
    }
  }
}
```



