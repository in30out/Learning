[[lxml库与Xpath语法]]
[[网页结构]]

url = "xxxx"
response = requests.get(url)

document = html.fromstring(response.text)

movie_list = document.xpath("//\*\[@id='page_1'/div\[@class='card_style_1'\]\]")
