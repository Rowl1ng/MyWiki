# pandas

## 读入
```
df = pd.DataFrame(pd.read_csv('../val.csv'))
```
## 写csv

```python
data_lines = []
label_lines = ['pid','seriesUID','studyUID']

data_lines.append([pid,seriesUID,studyUID])
label_file = pd.DataFrame(columns=label_lines, data=data_lines)
```
设置索引
```python
indexed = label_file.set_index(['batch', 'pid', 'seriesUID', 'studyUID', 'slice_index'])
```
## 统计

```
label_file.groupby(['类型']).size()
```

## 逻辑运算

使用符号`|`和`&`，记得加`（）`

```
calc_label = label_file[(label_file[u'病灶分布']=='') | (label_file[u'钙化形态']=='') | (label_file[u'良恶性']=='')]
```

## 遍历csv

```
for iter_index in df.index:
    filename = df.loc[iter_index,'filepath']
    label = df.loc[iter_index,'label']
```

## 合并表格

纵向拼接：

```python
dfs = []
mass_train = pd.read_csv(csv_dir+'Mass-Training-Description.csv')
mass_test = pd.read_csv(csv_dir+'Mass-Test-Description.csv')
calc_train = pd.read_csv(csv_dir+'Calc-Training-Description.csv')
calc_test = pd.read_csv(csv_dir+'Calc-Test-Description.csv')

dfs.append(mass_train)
dfs.append(mass_test)
dfs.append(calc_train)
dfs.append(calc_test)

concate_frame = pd.concat(dfs)
concate_frame = concate_frame.set_index(['patient_id', 'left or right breast', 'image view'])
concate_frame.sort_index(inplace=True)
concate_frame
```

修改列名：

```
df.columns = ['a', 'b', 'c', 'd', 'e']
```
横向匹配+拼接：

```
pd.merge(df2,df1,how='left',on=['key1','key2'])
```

## 去重：

```
data = data.drop_duplicates()
```

## 保存

```python
data.to_csv("data.csv",index=False,header=False)
```
中文csv：
```python
df.to_csv(“df.csv”, encoding=’utf_8_sig’,index=False) 
```
导出xlsx
```python
writer = pd.ExcelWriter('/Users/lhl/Desktop/quanlitycontrol/诊断意见/500_double.xlsx')
result.to_excel(writer,'500ran')
```

## Reference

* [Pandas & Seaborn - A guide to handle & visualize data in Python\][2]

[2]: [https://tryolabs.com/blog/2017/03/16/pandas-seaborn-a-guide-to-handle-visualize-data-elegantly/](https://tryolabs.com/blog/2017/03/16/pandas-seaborn-a-guide-to-handle-visualize-data-elegantly/)

