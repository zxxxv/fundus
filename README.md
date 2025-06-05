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

## 🔥Goal

Fundus는

* **A static news crawler.**
단 몇 줄의 Python 코드만으로 온라인 뉴스 기사를 손쉽게 크롤링할 수 있습니다.

* 저의 목표는 Fundus에 🇰🇷 퍼블리셔를 추가하여, 대표 언론사 중 하나인 매일경제신문사의 기사 내용을 자동으로 스크랩해오는 기능을 구현하는 것입니다.
이를 통해 한국어 뉴스 데이터 수집 범위를 확장하고, 사용자가 매일경제신문사의 최신 기사에 빠르게 접근할 수 있도록 지원합니다.

## 🔥Requirements

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

## 🔥Docker
How to install & Run with Docker

 📢 `Dockerfile` , 이미지, 컨테이너 만들기
```
FROM ubuntu:22.04

RUN apt-get update
RUN apt-get install -y python3 python3-pip vim

ENTRYPOINT ["/bin/bash"]
```

```
# 도커 이미지 빌드
docker build -t [이름:태그] ./

도커 이미지 확인하기
docker images
```

```
# 도커 컨테이너 만들고 실행
docker run -dit [이름:태그]

도커 컨테이너 확인하기
docker ps -a

#도커 컨테이너 접속
docker exec -it <CONTAINER_ID> /bin/bash
```

### 📢로컬로 프로젝트 클론 해오기

```
git clone https://github.com/zxxxv/fundus.git
```

### 📢필수 패키지 설치

```
Requirements에 명시된 패키지 자동 설치
pip install .

#개발자용 추가 패키지 설치
pip install .[dev]
```

## ⭐Example 1: 한국 뉴스 기사 크롤링 하기

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

## ⭐Example 2: 특정 news source 크롤링해보기

특정 뉴스 출처만 크롤링하고 싶을 수도 있습니다. 매일경재신문(Maeil Business Newspaper)에서만 뉴스 기사를 크롤링해보겠습니다:

```python
from fundus import PublisherCollection, Crawler

# initialize the crawler for 매일경제신문사(MBN)
crawler = Crawler(PublisherCollection.kr.MBN)

# crawl 2 articles and print
for article in crawler.crawl(max_articles=2):
    print(article)
```

## 🔥도커 실행 후 종료하기(+저장)

```python
#컨테이너 밖으로 나오기
exit
Ctrl + P 그리고 Ctrl + Q

#도커 커밋 (컨테이너 이미지로 저장하기)
docker commit [OPTIONS] <컨테이너_ID_or_이름> <새_이미지이름>:<태그>

#컨테이너 멈추기
docker stop <CONTAINER_ID>

#컨테이너 삭제
docker rm <CONTAINER_ID>

#이미지 삭제
docker rmi <Image_ID 또는 Image_Name:태그>
```
## 🔥디렉토리 구조

```console
fundus/
├── .github/                       # GitHub 설정 및 워크플로우
│   └── workflows/                 # CI/CD 워크플로우 정의 (예: 테스트, 린트)
├── docs/                          # 프로젝트 관련 문서 및 튜토리얼
│   ├── 1_getting_started.md       # 기본 사용법 안내
│   ├── how_to_add_a_publisher.md  # 퍼블리셔 추가 가이드
│   ├── supported_publishers.md    # 지원 중인 퍼블리셔 목록
│   └── …                          # 기타 튜토리얼 및 고급 주제 문서
├── resources/                     # 정적 리소스  
│   └── logo/                      # 로고 이미지 등
│       ├── fundus-logo.png        # 프로젝트 로고
│       └── …                      # 추가 로고 파일
├── scripts/                       # 유틸리티 스크립트
│   ├── some_setup_script.sh       # 예: 환경 설정을 위한 스크립트
│   └── …                          # 기타 자동화 스크립트
├── src/                           # 소스 코드
│   └── fundus/                    # Python 패키지 루트
│       ├── __init__.py            # 패키지 초기화 파일
│       ├── article.py             # 크롤링된 기사를 나타내는 클래스 정의
│       ├── crawler.py             # 일반 웹 크롤러 메인 로직
│       ├── ccnews_crawler.py      # CC-NEWS 아카이브를 위한 대규모 크롤러
│       ├── exceptions.py          # 커스텀 예외 정의
│       ├── helpers.py             # 내부적으로 쓰이는 유틸리티 함수 모음
│       ├── publishers/            # 퍼블리셔별 스펙 및 파서 정의
│       │   ├── __init__.py        # 퍼블리셔 패키지 초기화
│       │   ├── us.py              # 미국 기반 퍼블리셔(예: TheNewYorker 등)
│       │   ├── uk.py              # 영국 기반 퍼블리셔(예: TheGuardian 등)
│       │   ├── kr/                # 한국 퍼블리셔 관련 모듈
│       │   │   ├── __init__.py    # KR 퍼블리셔 패키지 초기화
│       │   │   ├── mbn.py         # 매일경제신문사(MBN) 파서 구현
│       │   │   └── …              # 추가된 다른 한국 언론사 파서
│       │   └── …                  # 기타 국가별 퍼블리셔 정의
│       └── utils/                 # 내부 도우미 함수 및 공용 유틸리티
│           ├── __init__.py        # 유틸리티 패키지 초기화
│           ├── fetch_utils.py     # HTTP 요청 및 콘텐츠 다운로드 도구
│           └── parse_utils.py     # HTML 파싱 보조 함수
├── tests/                         # 테스트 코드
│   ├── test_crawler.py            # 크롤러 동작 검증 테스트
│   ├── test_publishers.py         # 퍼블리셔 파서 테스트
│   └── …                          # 기타 유닛/통합 테스트
├── .gitignore                     # Git에서 무시할 파일/폴더 목록
├── CODE_OF_CONDUCT.md             # 오픈소스 기여를 위한 행동 강령
├── LICENSE                        # MIT 라이선스
├── MANIFEST.in                    # 패키징 시 포함할 파일 지정
├── README.md                      # 프로젝트 개요 및 설치/사용법 설명
└── pyproject.toml                 # 빌드 및 프로젝트 설정 파일

```


## 🔥현재 지원되는 News Source

여기에서 현재 지원되는 퍼블리셔를 확인할 수 있습니다. [**here**](/docs/supported_publishers.md).

## 🔥License

[MIT](LICENSE)
