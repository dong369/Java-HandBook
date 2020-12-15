





```python
from pymysql import *

# 1、打开数据库，连接对象
con = connect(host='localhost', port=3306, user='root', password='passw0rd', database='test', charset='utf8')
# 2、操作数据库，游标对象
cs = con.cursor()
info = cs.execute('select * from goods')
for x in range(info):
    # 获取查询结果
    result = cs.fetchone()
    print(result)
# 3、关闭数据库
cs.close()
con.close()
```

