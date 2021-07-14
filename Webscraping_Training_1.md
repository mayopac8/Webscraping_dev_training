## Pythonでスクレイピング_Trainig_1

### 以下のページをスクレイピングします。製品名と価格情報を取得します。

https://product.rakuten.co.jp/product/-/568c382be2381ebaa925cee393f09d8d/?l-id=www_PC_category/computer_PartsRanking

* ここではBeatifulSoupは使わずに、lxmlだけを使ってスクレイピングします。

```py
pip install lxml
```

* 必要なXpathはChromeの「デベロッパーツール」を使って簡単に取得できます。※F12押せば出る

```py
import urllib.request
import lxml.html

url = "https://product.rakuten.co.jp/product/-/568c382be2381ebaa925cee393f09d8d/?l-id=www_PC_category/computer_PartsRanking"
html = urllib.request.urlopen(url).read()
tree = lxml.html.fromstring(html)
result = tree.xpath('//*[@id="contents"]/div[1]/div/div/div[3]/div[1]/div[1]/span')

for elem in result:
    print(elem.text)


result = tree.xpath('//*[@id="contents"]/div[1]/div/div/div[3]/div[3]/div[1]/p/span[1]')
for elem in result:
    print(elem.text)
```
