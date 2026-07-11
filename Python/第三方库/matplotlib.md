# 画布
```
#画布创建子图,输出元组，解包获取，figure为整张画布，axes为列表数组
figure,axes = plt.subplots(nrows=1,ncols=2,figsize=(20,6),dpi=100)
#dpi:每英寸像素个数（分辨率）
#figsize:整张画布的大小

axes1 : Axes = axes[0]
```
# 柱状图
```
axes1.bar(list[any],list[int])
```
# 饼状图
```
axes1.pie(list[int],labels=list[any],autopct=%1.1f%%)
#autopct:格式化显示比例，整数位数字指不足指定位数前面补0，如50指整数位不足50位就在前面补0，小数位指小数的分位
```
# 曲线图
![[Matplotlib图表详解.png]]
#### 常用方法及代码示例
```
import matplotlib.pyplot as plt

import random

#让图表显示中文

plt.rcParams['font.sans-serif'] = ['SimHei']

x = [i for i in range(20)]

y_bj = [random.randint(5,20) for i in range(20)]

y_xa = [random.randint(8,25) for i in range(20)]


plt.figure(figsize=[15,7]) #设置画布与图表大小

  
plt.plot(x,y_bj,label = "北京") #绘制第一条曲线并设置标签

plt.plot(x,y_xa,label = '西安') #绘制第二条曲线并设置标签


plt.title("气温变化曲线",fontsize = 15 ) #设置标题与字体大小


plt.xlabel('日期',fontsize = 15) #设置x轴标签与字体大小

plt.ylabel('温度',fontsize = 15) #设置y轴标签与字体大小


plt.xticks(x,fontsize = 12) #设置x轴刻度大小与字体大小

plt.yticks([i for i in range(5,26)],fontsize = 12) #设置y轴刻度大小与字体大小


plt.grid(True,linestyle = '--',alpha = 0.5) #添加网络线，设置样式为虚线，透明度为0.5


plt.legend(fontsize = 12,loc = 'upper left') #添加图例，设置字体大小为12，位置为左上角


plt.show() #显示图表
```


# 保存为图片
```
plt.savefig('data/08.png')
```

