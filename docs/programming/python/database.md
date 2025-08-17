## 数据库

[python3教程][10]

- 连接MySQL：
    
    ```python
        db = pymysql.connect(host='localhost', port=3306,user='root',passwd='2005726',db='review')
        cur = db.cursor()
        self.db = db
        self.cur = cur
        self.fetch_data()
    ```
    
- 读取数据：
    
    ```python
        train = []
        self.cur.execute("SELECT * FROM Bug_Report_Data")
        for row in self.cur:
            review = str(row[16])
            review= str(review.decode('utf-8', errors='ignore'))
            train.append((review, 'bug'))
    ```
    

### 使用pymysql连接Mysql

```python
import pymysql;
def access_mysql():
     conn = pymysql.connect(host='localhost', port=3306,user='root',db='test')
    cur = conn.cursor()
    cur.execute("SELECT * FROM dictionarylimit 10")
    for r in cur:
        print("row_number:"+str(cur.rownumber))
        print("id:"+str(r[0])+"key:"+str(r[1])+" mean:"+str(r[2]))
    cur.close()
    conn.close()

if __name__== '__main__':
    access_mysql();
```