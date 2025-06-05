<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/flairNLP/fundus/blob/master/resources/logo/svg/logo_darkmode_with_font_and_clear_space.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/flairNLP/fundus/blob/master/resources/logo/svg/logo_lightmode_with_font_and_clear_space.svg">
    <img src="https://github.com/flairNLP/fundus/blob/master/resources/logo/svg/logo_lightmode_with_font_and_clear_space.svg" alt="Logo" width="50%" height="50%">
  </picture>
</p>

<p align="center">A very simple <b>news crawler</b> in Python.
Developed at <a href="https://www.informatik.hu-berlin.de/en/forschung-en/gebiete/ml-en/">Humboldt University of Berlin</a>.
</p>
<p align="center">
<a href="https://pypi.org/project/fundus/"><img alt="PyPi version" src="https://badge.fury.io/py/fundus.svg"></a>
<img alt="python" src="https://img.shields.io/badge/python-3.8-blue">
<img alt="Static Badge" src="https://img.shields.io/badge/license-MIT-green">
<img alt="Publisher Coverage" src="https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/dobbersc/ca0ae056b05cbfeaf30fa42f84ddf458/raw/fundus_publisher_coverage.json">
</p>
<div align="center">
<hr>

[Goal](#goal) | [Requirements](#requirements) | [Docker](#docker) | [News Sources](/docs/supported_publishers.md)

</div>


---

## Goal
Fundus는 정적 뉴스 크롤러로, 단 몇 줄의 Python 코드만으로 온라인 뉴스 기사를 손쉽게 크롤링할 수 있습니다.
저의 목표는 Fundus에 KR 퍼블리셔를 추가하여, 대표 언론사 중 하나인 매일경제신문사의 기사 내용을 자동으로 스크랩해오는 기능을 구현하는 것입니다.
이를 통해 한국어 뉴스 데이터 수집 범위를 확장하고, 사용자가 매일경제신문사의 최신 기사에 빠르게 접근할 수 있도록 지원합니다.

<hr>

## Requirements - pyproject.toml에 명시

```
    python >=3.8
    python-dateutil>=2.8, <3,
    lxml>=4.9, <6,
    more-itertools>=9.1, <10,
    cssselect>=1.1, <2,
    feedparser>=6.0, <7,
    colorama>=0.4, <1,
    typing-extensions>=4.6, <5,
    langdetect>=1.0, <2,
    validators>=0.24, <1,
    requests>=2.28, <3,
    tqdm>=4.66, <5,
    fastwarc>=0.14, <1,
    chardet>=5.2, <6,
    dill>=0.3, <1,
    dict2xml>=1.7.6, <2,
    xmltodict>=0.13.0, <1
```

To install from pip, simply do:

```
pip install fundus
```

Fundus requires Python 3.8+.


## Example 1: Crawl a bunch of English-language news articles

Let's use Fundus to crawl 2 articles from publishers based in the US.

```python
from fundus import PublisherCollection, Crawler

# initialize the crawler for news publishers based in the US
crawler = Crawler(PublisherCollection.us)

# crawl 2 articles and print
for article in crawler.crawl(max_articles=2):
    print(article)
```

That's already it!

If you run this code, it should print out something like this:

```console
Fundus-Article including 1 image(s):
- Title: "Feinstein's Return Not Enough for Confirmation of Controversial New [...]"
- Text:  "89-year-old California senator arrived hour late to Judiciary Committee hearing
          to advance President Biden's stalled nominations  Democrats [...]"
- URL:    https://freebeacon.com/politics/feinsteins-return-not-enough-for-confirmation-of-controversial-new-hampshire-judicial-nominee/
- From:   The Washington Free Beacon (2023-05-11 18:41)

Fundus-Article including 3 image(s):
- Title: "Northwestern student government freezes College Republicans funding over [...]"
- Text:  "Student government at Northwestern University in Illinois "indefinitely" froze
          the funds of the university's chapter of College Republicans [...]"
- URL:    https://www.foxnews.com/us/northwestern-student-government-freezes-college-republicans-funding-poster-critical-lgbtq-community
- From:   Fox News (2023-05-09 14:37)
```

This printout tells you that you successfully crawled two articles!

For each article, the printout details:
- the number of images included in the article
- the "Title" of the article, i.e. its headline 
- the "Text", i.e. the main article body text
- the "URL" from which it was crawled
- the news source it is "From"


## Example 2: Crawl a specific news source

Maybe you want to crawl a specific news source instead. Let's crawl news articles from Washington Times only:

```python
from fundus import PublisherCollection, Crawler

# initialize the crawler for The New Yorker
crawler = Crawler(PublisherCollection.us.TheNewYorker)

# crawl 2 articles and print
for article in crawler.crawl(max_articles=2):
    print(article)
```

## Example 3: Crawl 1 Million articles

To crawl such a vast amount of data, Fundus relies on the `CommonCrawl` web archive, in particular the news crawl `CC-NEWS`.
If you're not familiar with [`CommonCrawl`](https://commoncrawl.org/) or [`CC-NEWS`](https://commoncrawl.org/blog/news-dataset-available) check out their websites.
Simply import our `CCNewsCrawler` and make sure to check out our [tutorial](docs/2_crawl_from_cc_news.md) beforehand.

````python
from fundus import PublisherCollection, CCNewsCrawler

# initialize the crawler using all publishers supported by fundus
crawler = CCNewsCrawler(*PublisherCollection)

# crawl 1 million articles and print
for article in crawler.crawl(max_articles=1000000):
  print(article)
````

**_Note_**: By default, the crawler utilizes all available CPU cores on your system. 
For optimal performance, we recommend manually setting the number of processes using the `processes` parameter. 
A good rule of thumb is to allocate `one process per 200 Mbps of bandwidth`.
This can vary depending on core speed.

**_Note_**: The crawl above took ~7 hours using the entire `PublisherCollection` on a machine with 1000 Mbps connection, Core i9-13905H, 64GB Ram, Windows 11 and without printing the articles.
The estimated time can vary substantially depending on the publisher used and the available bandwidth.
Additionally, not all publishers are included in the `CC-NEWS` crawl (especially US based publishers).
For large corpus creation, one can also use the regular crawler by utilizing only sitemaps, which requires significantly less bandwidth.

````python
from fundus import PublisherCollection, Crawler, Sitemap

# initialize a crawler for us/uk based publishers and restrict to Sitemaps only
crawler = Crawler(PublisherCollection.us, PublisherCollection.uk, restrict_sources_to=[Sitemap])

# crawl 1 million articles and print
for article in crawler.crawl(max_articles=1000000):
  print(article)
````


## Example 4: Crawl some images

By default, Fundus tries to parse the images included in every crawled article.
Let's crawl an article and print out the images for some more details.

```python
from fundus import PublisherCollection, Crawler

# initialize the crawler for The LA Times
crawler = Crawler(PublisherCollection.us.LATimes)

# crawl 1 article and print the images
for article in crawler.crawl(max_articles=1):
    for image in article.images:
        print(image)
```

For [this article](https://www.latimes.com/sports/lakers/story/2024-12-13/lakers-lebron-james-away-from-team-timberwolves) you will get the following output:

```console
Fundus-Article Cover-Image:
-URL:			 'https://ca-times.brightspotcdn.com/dims4/default/41c9bc4/2147483647/strip/true/crop/4598x3065+0+0/resize/1200x800!/format/webp/quality/75/?url=https%3A%2F%2Fcalifornia-times-brightspot.s3.amazonaws.com%2F77%2Feb%2F7fed2d3942fd97b0f7325e7060cf%2Flakers-timberwolves-basketball-33765.jpg'
-Description:	         'Minnesota Timberwolves forward Julius Randle (30) works toward the basket.'
-Caption:		 'Minnesota Timberwolves forward Julius Randle, left, controls the ball in front of Lakers forward Anthony Davis during the first half of the Lakers’ 97-87 loss Friday.'
-Authors:		 ['Abbie Parr / Associated Press']
-Versions:		 [320x213, 568x379, 768x512, 1024x683, 1200x800]

Fundus-Article Image:
-URL:			 'https://ca-times.brightspotcdn.com/dims4/default/9a22715/2147483647/strip/true/crop/4706x3137+0+0/resize/1200x800!/format/webp/quality/75/?url=https%3A%2F%2Fcalifornia-times-brightspot.s3.amazonaws.com%2Ff7%2F52%2Fdcd6b263480ab579ac583a4fdbbf%2Flakers-timberwolves-basketball-48004.jpg'
-Description:	         'Lakers coach JJ Redick talks with forward Anthony Davis during a loss to the Timberwolves.'
-Caption:		 'Lakers coach JJ Redick, right, talks with forward Anthony Davis during the first half of a 97-87 loss to the Timberwolves on Friday night.'
-Authors:		 ['Abbie Parr / Associated Press']
-Versions:		 [320x213, 568x379, 768x512, 1024x683, 1200x800]

Fundus-Article Image:
-URL:			 'https://ca-times.brightspotcdn.com/dims4/default/580bae4/2147483647/strip/true/crop/5093x3470+0+0/resize/1200x818!/format/webp/quality/75/?url=https%3A%2F%2Fcalifornia-times-brightspot.s3.amazonaws.com%2F3b%2Fdf%2F64c0198b4c2fb2b5824aaccb64b7%2F1486148-sp-nba-lakers-trailblazers-25-gmf.jpg'
-Description:	         'Lakers star LeBron James sits in street clothes on the bench next to his son, Bronny James.'
-Caption:		 'Lakers star LeBron James sits in street clothes on the bench next to his son, Bronny James, during a win over Portland at Crypto.com Arena on Dec. 8.'
-Authors:		 ['Gina Ferazzi / Los Angeles Times']
-Versions:		 [320x218, 568x387, 768x524, 1024x698, 1200x818]
```

For each image, the printout details:
- The cover image designation (if applicable).
- The URL for the highest-resolution version of the image.
- A description of the image.
- The image's caption.
- The name of the copyright holder.
- A list of all available versions of the image.

## Currently Supported News Sources

You can find the publishers currently supported [**here**](/docs/supported_publishers.md).

## License

[MIT](LICENSE)
