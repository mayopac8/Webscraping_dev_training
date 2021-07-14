# PythonでWebスクレイピングをしてみよう

### 目次
  
  * クローリング、スクレイピングとは
  * Requestsを使ってWebページを取得
  * Beautiful Soupを使ってHTMLを抽出
  * RequestsとBeautiful Soupを合わせた実用的なスクレイピング

### 前提知識

## クローリングとは

クローリングとは、複数のウェブサイトのリンクをなぞってウェブページを巡回することを言います。また、クローリングをするプログラムをクローラーと呼びます。
クローリングはスクレイピングとセットでよく出てくる言葉です。
今回はスクレイピングの基礎になるので深くは掘り下げませんが、知っておく必要がある言葉なのでぜひ覚えておいてください。

## Webスクレイピングとは

Webスクレイピングとは、ウェブサイトのHTMLから必要なデータを取得する事を言い、それを行うプログラムをスクレイパとも呼びます。
ウェブ上の情報を収集するうえで欠かせない技術となっており、冒頭でも述べましたがこのスキルだけで仕事を取れてしまうぐらい需要のある技術です。

先ほどのクローリングとの関連性はと言うと、
クローラによって必要な情報のあるWebページにアクセスし
スクレイパによって実際に情報を取得する
という風にしてスクレイピングが行われる事が多いので、セットで出てくる事が多くあります。

### 注意点

ウェブサイトによっては、無許可のクローリング・スクレイピングを拒否している場合があります。例を挙げるとTwitterなどはスクレイピングが禁止されているサービスです。
そのようなサイトでスクレイピングを行うと、違反行為にあたりそのサービスで罰則を受けたり、アクセスの度合いによっては違法な攻撃として法的に処罰されてしまうなんて可能性も考えられます。
そういうサイトは基本的にクローラが入れないようになっているのですが、クローラを入れないようには設定されてないけどルール上禁止と記述しているサイトもあります。
そのため、スクレイピングをする際には各サイトのルールをよく確認し、アクセス頻度などを考えて迷惑をかけないように気をつけましょう！

## Pythonでスクレイピングする方法

今回はPythonを用いてスクレイピングをするということですが、スクレイピングをする方法はいくつかあります。

大きく分けると、

  * 標準ライブラリを用いて生書きでスクレイピング  
  * 強力なライブラリを用いてスクレイピング
  * フレームワークを用いてスクレイピング
  * 標準ライブラリを用いる方法は、Webスクレイピングの基礎となる考え方や方法を学べますが、文字コード問題の対応がめんどくさかったり、大規模なスクレイピングをするときには大変です。

強力なライブラリを用いる方法は、ライブラリによってはかなり楽に実行可能で、サクッとスクレイピングできます。
フレームワークを使う方法は、PythonではScrapyというフレームワークが有名です。しかし、そのものの学習コストもありますし、大型のフレームワークですので、小規模なスクレイピングには向いていません。
それぞれの特徴を踏まえた上で今回は一番実用的な、強力なライブラリを用いたスクレイピングのやり方に絞って説明していこうと思います。

## 使用するライブラリ

PythonのWebスクレイピングの代表的なライブラリにはRequestsやSelenium、Beautifl Soupがあります。

それぞれ簡単に説明すると、

  * RequestsはHTTPライブラリでWebページを取得するためのもの
  * Seleniumはブラウザ操作をするためのもの
  * Beautiful Soupは取得したHTMLから情報を抽出するためのもの

です。今回はWebスクレイピング初心者の方向けなので、RequestとBeautiful Soupだけを使った基礎的なスクレイピングの方法を解説します！

## Requestsを使って、Webページを取得しよう！

Requestsは「人間のためのHTTP」と呼ばれるほど、使いやすいライブラリです。
標準ライブラリでスクレイピングをするときは「urllib」というものを使うのですが、その上位互換と言っても良いでしょう。

## Requestsをインストールしよう

まずは、インストールしましょう。

```py
pip3 install requests
```

pipを使ってinstallしてください。Python3系であれば、pip3、２系であればpipで大丈夫ですね。

## Requestsの基本的な使い方をみてみよう

それでは、実際につかってみましょう。
今回は、Yahoo!ニュースのページを取得してみようと思います。

```py
import requests
 
r = requests.get('https://news.yahoo.co.jp')
```

基本的な使用方法はこんな感じで、requestsをインポートしてから、get()関数でurlを指定してやるとそのページの情報を格納することが出来ます。

```py
import requests

r = requests.get('https://news.yahoo.co.jp')

  print(r.headers)
  print("--------")
  print(r.encoding)
  print(r.content)
```

オプションは色々有りますが、headersでheader情報が、encodingでWebページの指定encoding、contentでBody以下のコンテンツが分かるのでこれくらいは覚えておくとよいでしょう。

```py
{'Date': 'Sat, 05 May 2018 13:44:30 GMT', 'P3P': 'policyref="http://privacy.yahoo.co.jp/w3c/p3p_jp.xml", CP="CAO DSP COR CUR ADM DEV TAI PSA PSD IVAi IVDi CONi TELo OTPi OUR DELi SAMi OTRi UNRi PUBi IND PHY ONL UNI PUR FIN COM NAV INT DEM CNT STA POL HEA PRE GOV"', 'X-Content-Type-Options': 'nosniff', 'X-XSS-Protection': '1; mode=block', 'X-Frame-Options': 'SAMEORIGIN', 'Vary': 'Accept-Encoding', 'Content-Encoding': 'gzip', 'Content-Length': '9869', 'Content-Type': 'text/html; charset=UTF-8', 'Age': '0', 'Connection': 'keep-alive', 'Via': 'http/1.1 edge2201.img.umd.yahoo.co.jp (ApacheTrafficServer [c sSf ])', 'Server': 'ATS', 'Set-Cookie': 'TLS=v=1.2&r=1; path=/; domain=.yahoo.co.jp; Secure'}
--------
UTF-8
```
