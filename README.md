# MLM(Masked Language Model)을 활용한 금융 감성 점수 계산(Data Analysis Project) (IN PROGRESS)
---

## 목차 (Table of Contents)

1. [프로젝트 소개 (Introduction)](#프로젝트-소개-introduction)  
2. [분석 개요 (Overview)](#분석-개요-overview)  
3. [프로젝트 구조 (Project-Structure)](#프로젝트-구조-project-structure)  


---

## 프로젝트 소개 (Introduction)

- **목적**: 실제 금융 투자에 적합한 감성분석 
- **선행 연구의 한계**:
  - 현재 금융 문서의 감성분석은 투자자들이 원하는 긍,부정이라고 보기 어렵다.
    
  예를 들어 " '삼성전자'의 주가가 10% 상승했다" 라는 문장은 높은 강도로 긍정이 나온다. 하지만 이러한 정보는 투자에 도움이 되지 않는다. 이와 같은 문제를 해결 할 수 있을까?
- **가설**:
  - A기업과 관련된 문서에서 A기업명을 전부 MASK 씌우고 해당 훈련된 모델이 해당 MASK를 잘 맞춘다면 문서는 A기업과 밀접하게 연관되어 있을 것이다. 

이 저장소를 활용하면 공통적으로 발생하는 데이터 분석 단계를 보다 효율적으로 수행할 수 있습니다.

---

## 분석 개요 (Overview)
###  1. 데이터 분류 (Data Classification)

아래 표는 다양한 유형의 금융 도메인 데이터를 한눈에 파악할 수 있도록 정리한 것입니다.  
각 데이터 유형은 고유한 목적과 특성을 가지며, 상호 연계하여 종합적인 금융 분석에 활용할 수 있습니다.

| 데이터 유형       | 내용 및 특징                                                                                         | 예시                                                                                           |
|-------------------|-----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| **company**       | - **company id**로 유니크하게 식별 가능<br>- 기업 고유 정보 (예: 회사명, 설립일, 본사 위치 등)           | - `company_id = AAPL` <br>- `name = Apple Inc.`                                                 |
| **financials**    | - **분기별(point-in-time)** 재무 데이터<br>- 매출, 영업이익, 순이익 등 분기마다 변동하는 값              | - `fiscalDateEnding = 2023-03-31` <br>- `netIncome = 50,000,000 USD`                             |
| **estimate**      | - 재무 정보의 **추정치(Estimate)**<br>- 애널리스트, 리서치 기관 등이 제공하는 전망치                 | - 향후 분기 매출, EPS(Earnings Per Share) 추정치                                                 |
| **advanceData**   | - **주요 이벤트**가 뉴스 형태로 제공<br>- 기업의 핵심 이슈나 제품 출시 등                            | - 예: "Apple이 새로운 iPhone 출시를 발표"                                                        |
| **ownership data**| - 회사의 **지분율** 정보<br>- **owner**는 기업으로 제한<br>- `company` 데이터와 결합하여 어떤 기업이 보유 중인지 식별 가능 | - `company_id = MSFT` <br>  `owner_company_id = GS` (소유주가 Goldman Sachs)                    |
| **professional**  | - 임원진 정보<br>- **금액(보수 등) 표기는 없음**                                                     | - CEO, CFO 등 임원 목록 <br>  단, 임원 보수 등 수치는 없음                                       |
| **transcript**    | - 특정 이벤트에 대한 **코멘트**<br>- LNP(Live News/Press) 형태의 전문(全文) 또는 요약본               | - 어닝 콜(Earnings Call) <br>  기자 간담회 전문 등                                               |
| **exchange Rate** | - **환율 변환**에 관한 데이터<br>- 여러 통화를 서로 변환할 때 사용                                     | - USD/EUR 환율, KRW/JPY 환율 등                                                                 |
| **market**        | - **주가 데이터**<br>- 시가, 종가, 거래량 등                                                         | - `open`, `high`, `low`, `close`, `volume` 등 주식 시세 정보                                     |

- **데이터 전처리**: 결측치 처리, 이상치 탐지, 스케일링 등  
- **데이터 탐색(EDA)**: 통계 요약, 분포 분석, 상관분석, 시각화  
- **모델 학습 및 예측**: 머신러닝/딥러닝 기법을 사용한 분류, 회귀 등  
- **결과 정리**: 분석 결과를 Notebook 형태로 체계적으로 정리  

---

## 프로젝트 구조 (Project structure)

1. **리포지토리 클론**  
   ```bash

