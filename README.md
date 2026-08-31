# 청년 복지정책 RAG 테스트셋 파이프라인

복지정보 사이트에서 청년 대상 복지 서비스 정보를 수집하고, 이를 RAG(Retrieval-Augmented Generation) 챗봇에 활용할 수 있는 형태로 가공한 뒤, RAGAS로 테스트셋을 생성·평가하는 전체 파이프라인입니다.

> **저작권 안내**: 코드가 접근하는 실제 사이트 도메인 및 사이트명은 저작권 문제로 `target_site`라는 이름으로 치환했습니다. 실제로 실행하려면 본인이 스크래핑을 허가받은 사이트의 URL로 직접 교체해야 하며, 대상 사이트의 이용약관 및 robots.txt를 반드시 확인 후 사용하시기 바랍니다.

## 파이프라인 구조

```
1. 크롤링         01_crawling_central_gov / 02_crawling_private / 03_crawling_local_gov
   (Selenium)     중앙정부 / 민간 / 지자체 카테고리별 복지 서비스 정보 수집 → CSV/TSV/JSON

2. 데이터 전처리    04_preprocessing_markdown
   (Bronze→Silver) 원본 JSON → Markdown 변환 → Silver JSON 문서로 표준화

3. 테스트셋 생성    05_testset_multi_query / 06_testset_single_query
   (ragas)         청년 정책 필터링 → Knowledge Graph 구성 → 페르소나 기반
                    Multi-hop / Single-hop 질문·답변 테스트셋 생성

4. 평가            07_evaluation_ragas
   (RAGAS)         Faithfulness / AnswerRelevancy / ContextPrecision / ContextRecall
                    지표로 테스트셋 품질 평가
```

## 노트북 구성

| 파일 | 역할 |
|---|---|
| `01_crawling_central_gov.ipynb` | 중앙정부 카테고리 복지정보 크롤러 |
| `02_crawling_private.ipynb` | 민간 카테고리 복지정보 크롤러 |
| `03_crawling_local_gov.ipynb` | 지자체 카테고리 복지정보 크롤러 |
| `04_preprocessing_markdown.ipynb` | 크롤링 결과(JSON) → Markdown → Silver JSON 변환 |
| `05_testset_multi_query.ipynb` | Multi-hop 테스트셋 생성 및 RAGAS 평가 |
| `06_testset_single_query.ipynb` | Single-hop 테스트셋 생성 및 RAGAS 평가 |
| `07_evaluation_ragas.ipynb` | RAGAS 평가 로직만 분리한 독립 실행 노트북 |

각 노트북 맨 위에는 개요 설명이, 각 코드 셀 위에는 번호와 함께 역할이 마크다운으로 달려 있습니다.

## 실행 환경

이 코드는 Google Colab에서 실행하는 것을 전제로 작성되었습니다 (`google.colab`, `/content/...` 경로 사용). 로컬에서 참고용으로 라이브러리를 맞추려면:

```bash
pip install -r requirements.txt
```

`google.colab` 관련 코드는 Colab 환경 전용이라 로컬 실행 시 별도 대체가 필요합니다.

## API 키 설정

코드에서 사용하는 `api_key`(OpenAI API 키)는 보안을 위해 실제 값을 코드에서 제거했습니다. 실행 전 아래 코드를 해당 셀 이전에 추가해주세요.


## 데이터

크롤링·전처리·테스트셋 생성 과정에서 만들어지는 CSV/TSV/JSON 산출물은 저장소에 포함되어 있지 않습니다.
