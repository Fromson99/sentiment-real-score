# MLM(Masked Language Model)을 활용한 금융 감성 점수 계산 (IN PROGRESS)
---

## 목차 (Table of Contents)

1. [프로젝트 소개 (Introduction)](#프로젝트-소개-introduction)  
2. [프로젝트 포인트 (Project Point)](#프로젝트-포인트-project-point)  
3. [프로젝트 구조 (Project-Structure)](#프로젝트-구조-project-structure)  

---

## 프로젝트 소개 (Introduction)

- **형식**: 대학 지원 과제 (300만 원 규모)  
- **주관**: Kyung Hee University  
- **역할**: 팀 리더, 아이디어 제시, 코딩 담당  

---

## 프로젝트 포인트 (Project Point)

- **목적**  
  - 실제 금융 투자에 적합한 감성분석을 구현하는 것

- **선행 연구의 한계**  
  - 현재 금융 문서 감성분석은 투자자들이 원하는 긍·부정 지표를 정확히 반영하지 못함  
  - 예시: “삼성전자의 주가가 10% 상승했다”라는 문장은 감성 모델에서 높은 긍정도로 분류되지만,  
    실제 투자에는 직접적인 도움이 되지 않는 경우가 많음

- **가설**  
  - 특정 기업(A기업) 관련 문서에서 A기업명을 전부 마스킹(MASK) 처리  
  - 사전 학습된 MLM이 MASK를 정확히 예측한다면, 해당 문서는 A기업과 밀접한 연관이 있다고 볼 수 있음  
  - 이를 통해 실제 투자에 활용할 수 있는 감성 점수를 산출할 수 있을 것으로 기대

---

## 프로젝트 구조 (Project-Structure)

1. **텍스트 데이터 추출**  
   - 제공된 데이터셋에서 텍스트 형태만 선별/정제
   
2. **기업 이름 제거**  
   - 추출된 텍스트에서 해당 기업명을 전부 제거(MASK)

3. **사전 학습 모델에 기업 이름 추가**  
   - 사전학습된 모델(MLM)의 단어 집합(Vocabulary)에 기업 이름을 추가해 모델이 해당 토큰을 학습할 수 있도록 준비

4. **MLM 예측(Logit)값 저장**  
   - 마스킹된 문장을 MLM에 입력해, 모델이 MASK를 어떻게 예측하는지에 대한 **logit**(출력 값)을 저장

5. **Factor Model 구축**  
   - 저장된 logit 값을 활용해 **Factor Model**(예: Fama-French 확장 모델 등)을 구성하고,  
     투자에 직접 활용 가능한 감성 점수를 산출

### 데이터 (Data Classification)

| 데이터 유형       | 내용 및 특징                                                                                         | 예시                                                                                           |
|-------------------|-----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| **company**       | - **company id**로 유니크하게 식별 가능<br>- 기업 고유 정보 (예: 회사명, 설립일, 본사 위치 등)           | - `company_id = AAPL` <br>- `name = Apple Inc.`                                                 |
| **financials**    | - **분기별(point-in-time)** 재무 데이터<br>- 매출, 영업이익, 순이익 등 분기마다 변동하는 값              | - `fiscalDateEnding = 2023-03-31` <br>- `netIncome = 50,000,000 USD`                             |
| **estimate**      | - 재무 정보의 **추정치(Estimate)**<br>- 애널리스트, 리서치 기관 등이 제공하는 전망치                 | - 향후 분기 매출, EPS(Earnings Per Share) 추정치                                                 |
| **key develop**   | - **주요 이벤트**가 뉴스 형태로 제공<br>- 기업의 핵심 이슈나 제품 출시 등                            | - 예: "Apple이 새로운 iPhone 출시를 발표"                                                        |
| **ownership data**| - 회사의 **지분율** 정보<br>- **owner**는 기업으로 제한<br>- `company` 데이터와 결합하여 어떤 기업이 보유 중인지 식별 가능 | - `company_id = MSFT` <br>  `owner_company_id = GS` (소유주가 Goldman Sachs)                    |
| **professional**  | - 임원진 정보<br>- **금액(보수 등) 표기는 없음**                                                     | - CEO, CFO 등 임원 목록 <br>  단, 임원 보수 등 수치는 없음                                       |
| **transcript**    | - 특정 이벤트에 대한 **코멘트**<br>- LNP(Live News/Press) 형태의 전문(全文) 또는 요약본               | - 어닝 콜(Earnings Call) <br>  기자 간담회 전문 등                                               |
| **exchange Rate** | - **환율 변환**에 관한 데이터<br>- 여러 통화를 서로 변환할 때 사용                                     | - USD/EUR 환율, KRW/JPY 환율 등                                                                 |
| **market**        | - **주가 데이터**<br>- 시가, 종가, 거래량 등                                                         | - `open`, `high`, `low`, `close`, `volume` 등 주식 시세 정보                                     |



