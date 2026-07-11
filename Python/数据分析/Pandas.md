[[lxml库与Xpath语法]]
*Pandas是一个功能强大的结构化数据分析的工具集，底层是基于Numpy构建的，无论是在数据分析领域、还是大数据开发场景中都有显著的优势 *
核心：DataFrame(类似表格）、Series(类似表格中的一列)*

![[Pandas.png]]
# DataFrame 的创建方式
![[DataFrame 的创建方式.png|697]]


# Series创建方式
- s = pd.Series(\[10,20,30,40,50\])
- s = pd.Series((10,20,30,40,50),index=\['a','b','c','d','e'\])
- s = pd.Series({'a':10, 'b':20, 'c':30, 'd':40, 'e':50})
- s = df\['xxx'\]

# 常用方法
- df.index.tolist()   获取索引并转为列表
- df.columns.tolist()    获取列名并转为列表
- df.values.tolist()       获取值并转为列表
- fd.size                       获取元素个数
- df.dtypes                 获取数据类型
- df.shape                    获取维度
- pd.set_option('display.max_rows',None)       配置：展示最大行数
- pd.option.display.min_rows = 30                   配置：展示前15行与后15行
## 数据的读取与写入
```
df = pd.read_csv('data/sales.csv',usecols=['订单号','产品类别','产品名称','销售数量',''单价])
df['销售金额'] = df['销售数量'] * df['单价']
df = pd.to_csv('data/sales_01.csv',index=False) #index:索引列是否写入
```
![[Pandas_数据的读取与写入.png]]

## 数据的查看
```
df.head(n) #s查看前n行数据
df.tail(n) #查看结尾n行数据
df.describe() #数值列的统计描述
df.info() #杳看数据信息（列名，非空计数，数据类型）
df.shape #属性，杳看数据维度（行数，列数）
df.columns #属性，查看列名

df['列名'] #df.列名:操作单列
df[['','']] #操作多列
df.iloc[start:stop:step] #基于行号完成切片(不含stop)
df.loc[start:stop:step] #基于索引标签守成切片(含stop)
```

## 数据的判断与筛选
```
df[x].isin([y,z]) 判断y,z是否在x列内
df[x].between(y,z) 判断x列内的数字是否在y到z内(包首尾)
df[x].isnull       判断x列内是否为空
```
#### 例
```
# 数据过滤  
# 1. 获取 销售数量 >= 10 的订单数据  
df[df['销售数量'] >= 10]  
  
# 2. 获取产品类别为 食品 或 图书 的订单数据  
df[df['产品类别'].isin(['食品','图书'])]  
  
# 3. 获取 单价在100-200之间 的订单数据  
df[(df['单价']<=200) & (df['单价']>=100)]  
df[df['单价'].between(100,200)]  
  
# 4. 获取 销售数量 >= 8 ，并且 单价 >= 100 的订单数据  
df[(df['销售数量']>=8) & (df['单价']>=100)]
```

## 数据清洗
```
#缺失值处理
df = df.dropna()  删除缺失值所在行并返回删除后的dataframe  ，axis = 1 删除缺失值所在列
df.fillna(x)      以x填充缺失值
df.bfill()        以下一行数据填充缺失值 
df.ffill()        以上一行数据填充缺失值

#重复值处理
df.duplicated()   查看重复值（所有列重复时返回True） 第一次不算重复，第二开始起算
df.duplicated(subset=['订单号'])   查看重复值（指定列的数据重复）
df.drop_duplicats(subset=['订单号'],keep='first') 删除重复值,keep='first','last',（保留）

#异常值处理
筛选->删除或修改

#数据格式处理
df['订单日期'] = df['订单日期'].str.replace('/'，'-')     str这里为访问器

```
## 数据排序
```
df.sort_values('销售数量',ascending=False)  asvending为False时为降序
多列排序:df.sort_values(['销售数量','单价'],ascending[True,True])  先看第一个，再看第二个
```

## 数据分组
```
df.groupby('产品类别')['销售额'].sum() 分组求和
df.groupby('')[''].count()           分组统计数量
df.groupby('')[''].max()             分组求最大值
df.groupby('')[''].min()             分组求最小值
df.groupby('')[''].mean()            分组求平均值
df.groupby('')[''].agg(['sum','count','max','min','mean'])   一次执行多个统计操作
```
### 例
```
# 3.1 根据 产品类别 分组，统计各个类别的 订单数量  
df.groupby('产品类别')['产品类别'].count()  
# 3.2 根据 产品类别 分组，统计各个类别的 销售数量 之和  
df.groupby('产品类别')['销售数量'].sum()  
# 3.3 根据 产品类别 分组，统计各个类别的 销售金额 之和  
df.groupby('产品类别')['销售金额'].sum()  
# 3.4 根据 产品类别 分组，统计各个类别的最低商品 单价  
df.groupby('产品类别')['单价'].min()  
# 3.5 根据 产品类别 分组，统计各个类别的最高商品 单价  
df.groupby('产品类别')['单价'].max()  
# 3.6 根据 产品类别 分组，统计各个类别的平均商品 单价  
df.groupby('产品类别')['单价'].mean()  
# 3.7 根据 产品类别 分组，统计各个类别的商品的 平均单价、最高单价、最低单价  
df.groupby('产品类别')['单价'].agg(['mean','max','min'])  
# 3.8 根据 产品类别 分组，统计各个类别的商品的 销售数量 之和，销售金额 之和，平均 单价
```
