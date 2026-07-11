*(Comma-Separated Values,逗号分隔值)，一种简单,通用的文本文件格式，用于存储表格数据，可以直接使用Excel打开*

# 操作方式
## 一，文本文件操作
with open("xxxx.csv","w",encoding="utf-8") as f:
\_      f.write("姓名，年龄，姓别，爱好\n")
\_     f.write("xxxx,xxxxx,xxxxx,xxxxx\n")

with read("xxxx.csv","w",encoding="utf-8") as f:
\_     for line in f:
\_          print(line.strip())

## 二，CSV（）
with open("xxxx.csv","w",encoding="utf-8",newline="") as f:
\_     writer = csv.DictWriter(f,fieldnames=\["xxx","xxx","xxx","xxx"\])
\_     writer.wtiteheater
\_    writer.writerow({"y":"xxx","y":"xxx","y":"xxx","y":"xxx"})

with open("xxx.CSV","r",encoding="utf-8") as f 
\_       reader = csv.DictReader(f)
\_       for row in reader:
\_             print(row)
