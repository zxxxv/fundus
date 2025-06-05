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

Fundus
* **A static news crawler.**
단 몇 줄의 Python 코드만으로 온라인 뉴스 기사를 손쉽게 크롤링할 수 있습니다.

* 저의 목표는 Fundus에 KR 퍼블리셔를 추가하여, 대표 언론사 중 하나인 매일경제신문사의 기사 내용을 자동으로 스크랩해오는 기능을 구현하는 것입니다.
이를 통해 한국어 뉴스 데이터 수집 범위를 확장하고, 사용자가 매일경제신문사의 최신 기사에 빠르게 접근할 수 있도록 지원합니다.

<hr>

## Requirements

```console

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

## Docker
How to start with Docker


`Dockerfile` 만들기
```
FROM ubuntu:22.04

RUN apt-get update
RUN apt-get install -y python3 python3-pip vim

ENTRYPOINT ["/bin/bash"]
```

### 도커 이미지 빌드

```
docker build -t [이름:버전] ./
```

### 도커 컨테이너 만들고 실행

```
docker run -dit [이름:버전]
```

### 도커 컨테이너 접속

```
docker exec -it <CONTAINER_ID> /bin/bash
```

### 로컬로 프로젝트 클론 떠오기

```
git clone https://github.com/zxxxv/fundus.git
```

### 필수 패키지 설치

```
pip install .
```

개발자용 추가 패키지 설치

```
pip install .[dev]
```

## Example 1: 한국 뉴스 기사 크롤링 하기

Fundus를 이용하여 한국 기반 publishers에서 기사 2개 크롤링해온다.

```python
from fundus import PublisherCollection, Crawler

# initialize the crawler for news publishers based in the KR
crawler = Crawler(PublisherCollection.kr)

# crawl 2 articles and print
for article in crawler.crawl(max_articles=2):
    print(article)
```

That's already it!

이제 코드를 실행해보면 아래와 같은 결과가 나올것이다:

```console
Fundus-Article including 1 image(s):
- Title: "러 푸틴, 교황 레오 14세와 첫 통화 … ‘근본 원인’ 해결 필요 재차 강조"
- Text:  "지난 4일(현지시각) 러시아 크렘린궁에 따르면, 블라디미르 푸틴 러시아 대통령과 레오 14세 교황이 통화로 러시아 - 우크라이나 전쟁에 대해
          논의했다.  크렘린궁 측은 성명을 발표해 푸틴 대통령이 통화에서 정치적·외교적 수단으로 평화를 이룩하는 것에 관심이 [...]"
- URL:    https://www.mk.co.kr/news/world/11335094
- From:   Maeil Business Newspaper (2025-06-05 10:56)

Fundus-Article including 1 image(s):
- Title: "두산에너빌, 380MW급 가스터빈 성능시험 성공"
- Text:  "데이터센터 확대에 가스터빈 시장 공략 박차  두산에너빌리티가 자체 기술로 개발한 초대형 가스터빈 성능시험에 성공했다. 친환경 발전 수요와
          데이터센터 확대로 인해 가스터빈 수요가 늘어나는 상황인 만큼 관련 시장 공략 준비를 마친 셈이다.  두산에너빌리티는 [...]"
- URL:    https://www.mk.co.kr/news/business/11335088
- From:   Maeil Business Newspaper (2025-06-05 10:50)

```

이 출력은 두 개의 기사를 성공적으로 크롤링했음을 알려줍니다!

각 기사에 대해 출력물은 다음을 상세히 보여줍니다:

- 기사에 포함된 이미지 수
- “제목”(Title), 즉 헤드라인
- “본문”(Text), 즉 주요 기사 본문 텍스트
- 기사가 크롤링된 “URL”
- 뉴스 출처(From)

## Example 2: Crawl a specific news source

Maybe you want to crawl a specific news source instead. Let's crawl news articles from Washington Times only:

```python
from fundus import PublisherCollection, Crawler

# initialize the crawler for 매일경제신문사
crawler = Crawler(PublisherCollection.kr.MBN)

# crawl 2 articles and print
for article in crawler.crawl(max_articles=2):
    print(article)
```


## 현재 지원되는 News Source

여기에서 현재 지원되는 퍼블리셔를 확인할 수 있습니다. [**here**](/docs/supported_publishers.md).

## License

[MIT](LICENSE)
