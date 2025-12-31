# Webスクレイピング向けのトップPython HTMLパーサー

[![Bright Data Promo](https://github.com/bright-jp/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.jp/)

Webサイトからデータを抽出する際には、適切なHTMLパーサーを用意することが不可欠です。ここでは、Webスクレイピングプロジェクトを強力に加速させる、最もパワフルなPython HTMLパーサー5つを見ていきましょう。

- [Beautiful Soup](#beautiful-soup)
- [HTMLParser](#htmlparser)
- [lxml](#lxml)
- [PyQuery](#pyquery)
- [Scrapy](#scrapy)
- [Choosing the Right Parser](#choosing-the-right-parser)

## Beautiful Soup

Beautiful Soupは、HTMLおよびXMLドキュメントのパースに優れたPythonライブラリです。ドキュメント構造を反映したナビゲーション可能なパースツリーを作成するため、データ抽出が簡単になります。

Beautiful Soupをインストールするには、シェルまたはターミナルから次のコマンドを実行します。

```sh
pip3 install beautifulsoup4
```

### Key Strengths

- 複数のパーサー（`html.parser`、`lxml`、`html5lib`）に対応しています
- 整形式・不整形式のHTMLの両方を扱えます
- `find()`、`find_all()`、`select()`など直感的な検索メソッドがあります
- 初心者に最適で、シンプル〜中程度のスクレイピングタスクに向いています

最速の選択肢ではありませんが、Beautiful Soupは速度面の制限を補う柔軟性を提供します。最新のHTML標準に準拠しており、ドキュメントも充実し、ユーザーコミュニティも大きいため、Webスクレイピングを始めたばかりの方に最適です。

### Code Example

次のコードスニペットは、Beautiful Soupを使用して[Books to Scrape website](https://books.toscrape.com/)からデータをパースします。

```python
import requests
from bs4 import BeautifulSoup

# URL of the webpage to scrape
books_page_url = "https://books.toscrape.com/"

# Fetch the webpage content
response = requests.get(books_page_url)

# Check if the request was successful
if response.status_code == 200:
    # Parse the HTML content of the page
    soup_parser = BeautifulSoup(response.text, 'html.parser')

    # Find all articles that contain book information
    book_articles = soup_parser.find_all('article', class_='product_pod')

    # Loop through each book article and extract its title and price
    for book_article in book_articles:
        # Extract the title of the book
        book_name = book_article.h3.a['title']
        
        # Extract the price of the book
        book_cost = book_article.find('p', class_='price_color').text
        
        # Print the title and price of the book
        print(f"Title: {book_name}, Price: {book_cost}")
else:
    # Print an error message if the page could not be retrieved
    print("Failed to retrieve the webpage")
```

スクリプトを実行すると、1ページ目に掲載されているすべての書籍タイトルと価格が、ターミナルまたはシェルに出力されます。

```
…output omitted…
Title: Soumission, Price:  £50.10
Title: Sharp Objects, Price:  £47.82
Title: Sapiens: A Brief History of Humankind, Price: £54.23
Title: The Requiem Red, Price: £22.65
Title: The Dirty Little Secrets of Getting Your Dream Job, Price: £33.34
…output omitted…
```

## HTMLParser

HTMLParserはPythonの標準ライブラリに組み込まれているため、追加のインストールなしで直ちに利用できます。

### Key Strengths

- 外部依存関係が不要です
- シンプルで整形式のHTMLのパースに適しています
- 軽量でPythonに統合されています

このパーサーは、単純なHTML処理にはうまく機能しますが、不整形式のコンテンツでは苦戦し、HTML5も完全にはサポートしていません。速度は小〜中規模のドキュメントには十分ですが、複雑なパース要件には最適ではありません。

### Code Example

以下は、`html.parser`を使用してHTMLデータをパースするコード例です。

```python
from html.parser import HTMLParser

class MyHTMLParser(HTMLParser):
    def handle_starttag(self, tag, attrs):
        print("Encountered a start tag:", tag)
        
    def handle_endtag(self, tag):
        print("Encountered an end tag :", tag)
        
    def handle_data(self, data):
        print("Encountered some data  :", data)

parser = MyHTMLParser()

html_data = """
<html>
  <head><title>Example</title></head>
  <body><h1>Heading</h1><p>Paragraph.</p></body>
</html>
"""

parser.feed(html_data)
```

出力には各タグとデータが表示されます。

```
…output omitted…
Encountered a start tag: html
Encountered some data  : 
  
Encountered a start tag: head
Encountered a start tag: title
Encountered some data  : Example
Encountered an end tag : title
Encountered an end tag : head
…output omitted…
```

## lxml

lxmlは、PythonのシンプルさとCベースのXML処理ライブラリの強力さを組み合わせており、非常に高速かつ汎用性が高いのが特徴です。

`lxml`をインストールするには、次を実行します。

```sh
pip3 install lxml
```

### Key Strengths

- Cライブラリ（`libxml2`および`libxslt`）により優れたパフォーマンスを発揮します
- XPath、XSLT、XPointerなどの高度な機能があります
- 整形式・構造が不十分なHTMLの両方を処理できます
- 大規模ドキュメントの処理や複雑なデータ抽出に最適です

速度が重要な場合や大規模データセットを扱う場合、lxmlが最良の選択となることが多いです。最新のHTML標準をサポートし、ドキュメントも包括的です。

### Code Example

次の例は、`lxml`でHTMLデータをパースする方法を示します。

```python
from lxml import html

html_content = """
<html>
  <body>
    <h1>Hello, world!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
"""

tree = html.fromstring(html_content)

h1_text = tree.xpath('//h1/text()')[0]
print("H1 text:", h1_text)

p_text = tree.xpath('//p/text()')[0]
print("Paragraph text:", p_text)
```

`<h1>`および`<p>`要素のテキストが、次のように出力されます。

```
H1 text: Hello, world!
Paragraph text: This is a paragraph.
```

## PyQuery

PyQueryは、Pythonに[jQuery-like](https://www.w3schools.com/jquery/jquery_ref_selectors.asp)な構文をもたらし、JavaScriptやDOM操作に慣れている開発者にとって魅力的です。

### Key Strengths

- 親しみやすいjQuery-likeなAPIです
- CSSセレクターをサポートしています
- HTMLパースにlxmlを利用しています
- フロントエンド開発者にとって直感的です

lxmlを直接使用する場合ほど高速ではありませんが、Web開発のバックグラウンドを持つ開発者にとってはより取っつきやすいです。最新のHTML標準をサポートし、ドキュメントも明確です。

### Code Example

以下は、`pyquery`を使用してHTMLデータをパースするコードスニペットです。

```python
from pyquery import PyQuery as pq

html_content = """
<html>
  <body>
    <h1>Hello, from PyQuery!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
"""

doc = pq(html_content)

h1_text = doc('h1').text()
print("H1 text:", h1_text)

p_text = doc('p').text()
print("Paragraph text:", p_text)
```

出力は次のようになります。

```
H1 text: Hello, from PyQuery!
Paragraph text: This is a paragraph.
```

## Scrapy

Scrapyは単なるパーサーではなく、リクエストの送信から抽出データの処理・保存まで、すべてを扱う完全なWebスクレイピングフレームワークです。

Scrapyをインストールするには、次を実行します。

```sh
pip3 install scrapy
```

### Key Strengths

- エンドツーエンドのスクレイピングソリューションです
- 同時接続が組み込みで、より高速にスクレイピングできます
- リクエストのスロットリングやユーザーエージェントのローテーションなど高度な機能があります
- Seleniumなどのツールを含む複雑なスクレイピングワークフローに対応できるモジュラーアーキテクチャです

Scrapyは、パフォーマンスと堅牢性が重要となる大規模スクレイピングプロジェクトで真価を発揮します。他の選択肢より学習コストは高いものの、包括的な機能と充実した[documentation](https://docs.scrapy.org/en/latest/)により、複雑なプロジェクトでは投資に見合う価値があります。

### Example

以下は、Scrapyスパイダーを使用してデータを抽出する例です。

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"
    start_urls = [
        'http://quotes.toscrape.com/page/1/',
    ]

    def parse(self, response):
        for quote in response.css('div.quote'):
            yield {
                'text': quote.css('span.text::text').get(),
                'author': quote.css('small.author::text').get(),
                'tags': quote.css('div.tags a.tag::text').getall(),
            }
```

Scrapyはスクレイピングしたデータを`quotes.json`ファイルに保存します。内容は次のようになります。

```json
[
{"text": "\u201cThe world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.\u201d", "author": "Albert Einstein", "tags": ["change", "deep-thoughts", "thinking", "world"]},
{"text": "\u201cIt is our choices, Harry, that show what we truly are, far more than our abilities.\u201d", "author": "J.K. Rowling", "tags": ["abilities", "choices"]}
…output omitted...
]
```

## Choosing the Right Parser

- **Beautiful Soup**: 初心者や、分かりやすいパースタスクに最適です
- **HTMLParser**: 外部依存関係が不要なシンプルなプロジェクトに適しています
- **lxml**: パフォーマンスが重要なアプリケーションや複雑なパースに理想的です
- **PyQuery**: jQueryに慣れている開発者に最適です
- **Scrapy**: 大規模で本番運用レベルのスクレイピングプロジェクトに最適です

各パーサーにはそれぞれ強みがあり、最適な選択はニーズによって異なります。判断する際は、対象Webサイトの複雑さ、パフォーマンス要件、さまざまなAPIへの習熟度といった要素を考慮してください。

スクレイピングを省略してすぐにデータを取得したい場合は、サインアップして[our datasets](https://brightdata.jp/products/datasets)を確認し、今すぐ無料サンプルをダウンロードしてください。