*(Regular Expression)是一种由特定语法规则组成的文本模式，用来描述、匹配字符串中符合特定规则的字符序列。就相当于一种模式匹配工具，允许用户通过简介的语法进行复杂文本的搜索、匹配、提取和替换工作*

match - 从字符串的开头开始匹配（返回Match对象）
	result = re.match(r"1\[3-9\]\d{9}",s) 
	print(result.group())

serch - 从任意位置开始，搜索第一个匹配项（返回Match对象）
	result = re.serch(r"1\[3-9\]\d{9}",s) 
	print(result.group())

findall - 从任意位置开始，搜索所有匹配项（返回list)
	result = re.findall(r"1\[3-9\]\d{9}",s)
	print(result)

result.group() 获取到匹配的结果
result.span() 获取匹配项的索引
result.start() 获取匹配殴的开始索引
result.end() 获取匹配项的结束索引

![[正则表达式-语法.png]]