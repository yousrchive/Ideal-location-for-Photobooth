# 🚀 Dart-B 2024-1st Toyproject <Ideal location Recommendation for ; Photobooth>

<img width="218" alt="스크린샷 2024-05-30 오후 3 06 52" src="https://github.com/yousrchive/Ideal-location-for-Photobooth/assets/147587058/0e30e320-32d2-4455-a9d7-23f4e4b90d08">

## 📚 목차

1. [기획배경](#기획배경)
2. [프로젝트 기간](#프로젝트-기간)
3. [팀원 소개](#팀원-소개)
4. [기능 소개](#기능-소개)
5. [모델 설계서](#모델-설계서)
6. [API 설계서](#api-설계서)
7. [ERD](#erd)
8. [시스템 아키텍처](#시스템-아키텍처)
9. [데이터 명세서](#데이터-명세서)
10. [기술 스택](#기술-스택)
11. [결과물](#결과물)

## 🎯 기획배경

" 왜 중앙대학교 후문에는 포토부스가 하나도 없을까? 내가 세우면 사람들이 오려나? "

- “이따 사진 한 장 찍을까?”
당연시되고 있는 우리 주위의 포토부스.
셀프 포토부스가 성행하는 시점에서 어떤 요소가 포토부스의 입점에 관련되어 있고,
새롭게 포토부스를 설치하고자 한다면 어디에 입점시키는 것이 가장 좋을까?
  
## ⏰ 프로젝트 기간

24년 3월 20일 (수) ~ 24년 4월 3일 (수) (2주)

## 👥 팀원 소개

| 이름 | 역할 |
|---|---|
| 이은학(팀장) | 프론트엔드, 웹 서빙 |
| 심영보 | 데이터 사이언티스트(데이터 수집, 추천 모델링 개발) |
| 이유정 | 데이터 사이언티스트(데이터 수집, 정제, 시각화) |
| 김나현 | 데이터 애널리스트(데이터 시각화) |

## 📄 모델 설계서
  ### 데이터

데이터셋은 총 5가지를 사용했다.
1) 서울시 가구 특성정보 - 소득정보
2) 소상공인시장진흥공단_상가(상권)정보
3) 한국대학및전문대학정보표준데이터 & 전국초중등학교위치표준데이터
4) 상권별 소규모 상가 임대료
5) 서울시 상권분석서비스(추정매출-상권)

+ 서울시 내 랜덤 스팟의 포토부스 개수 및 상관 피쳐 개수 (300개)

<img width="620" alt="스크린샷 2024-05-30 오후 3 07 35" src="https://github.com/yousrchive/Ideal-location-for-Photobooth/assets/147587058/c77caa7c-33d0-4fcf-8256-71a62497acf7">

<img width="656" alt="스크린샷 2024-05-30 오후 3 07 56" src="https://github.com/yousrchive/Ideal-location-for-Photobooth/assets/147587058/eb10a521-89da-4386-b4ac-48ef02cc81b3">


##### 모델 학습 및 추론 결과
<img width="220" alt="스크린샷 2024-05-30 오후 3 08 13" src="https://github.com/yousrchive/Ideal-location-for-Photobooth/assets/147587058/6045e4d0-7a9a-4a4f-b0ef-44211bb23560">

input을 정확한 주소, 혹은 키워드로 입력해도 kakao api 기준 피쳐(현재 술집, 노래방)를 통해 주요 상권을 분석, 예상 포토부스 개수와 실제 포토부스 개수가 나옵니다.

if ( 예상 포토부스 개수 < 실제 포토부스 개수) = 포화상태
if ( 예상 포토부스 개수 = 실제 포토부스 개수) = 정석 개수 수준
if ( 예상 포토부스 개수 > 실제 포토부스 개수) = 추가 포토부스 설치 고려 가능 지역


##### 모델 서빙 플로우 설계

##### 추천서비스 로직 구성

핵심 기능: 
1) 키워드를 입력하면 좌표값을 반환할 수 있다
2) 좌표, 범위 지정 후 키워드를 쿼리해 반경 내 요소를 검색할 수 있다

<img width="599" alt="스크린샷 2024-05-30 오후 3 11 42" src="https://github.com/yousrchive/Ideal-location-for-Photobooth/assets/147587058/973cb5e5-5a87-424d-9392-edf6919a0a27">

<img width="599" alt="스크린샷 2024-05-30 오후 3 11 55" src="https://github.com/yousrchive/Ideal-location-for-Photobooth/assets/147587058/02bb0236-f9d5-4902-bb63-b6c7117a6c75">


## 🌐 API 설계서

### 카카오 API
- **기능**: 키워드 검색을 통한 입력 위치 설정 
- **요청 URL**: `https://dapi.kakao.com/v2/maps/sdk.js?appkey=5b475d258fb5e345e3944cb9418a3f5b&libraries=services`
- **요청 방식**: GET
- **요청 파라미터**: `query` (검색어)
- **응답 형식**: JSON
- **에러 코드**: 401 (인증 실패), 404 (찾을 수 없음)

## 🏗️ 시스템 아키텍처

![웹아키택처]

## 📄 데이터 명세서

### 1. 가구_특성정보

- **항목**: 가구_특성정보_(+소득정보)_211203.csv
- **칼럼**: adstrd_nm, adstrd_cd, legaldong_nm, legaldong_cd, tot_po, tot_hshld_co, hshld_per_po, ave_income_amt
- **갯수**: 19577
- **데이터타입**:
  - astrd_nm, legaldong_nm : object
  - adstrd_cd, legaldong_cd, tot_po, tot_hshld_co, hshld_per_po, ave_income_amt: float

### 2. 대한민국 대학 및 전문대학 데이터

- **항목**: 대한민국 대학 및 전문대학 데이터
- **칼럼**: 학교명, 학교영문명, 본분교구분명, 학교구분명, 설립형태구분명, 시도코드, 시도명, 소재지도로명주소, 소재지지번주소, 도로명우편번호, 소재지우편번호, 홈페이지주소, 대표전화번호, 대표팩스번호, 설립일자, 기준연도, 데이터기준일자, 제공기관코드, 제공기관명, 지번주소
- **갯수**: 441
- **데이터타입**:
  - 시도코드, 도로명우편번호, 소재지우편번호, 대표전화번호, 대표팩스번호, 설립일자, 기준연도, 데이터기준일자, 제공기관코드: int
  - 학교명, 학교영문명, 본분교구분명, 학교구분명, 설립형태구분명, 시도명, 소재지도로명주소, 소재지지번주소, 홈페이지주소, 제공기관명 : object

### 3. 서울시 상권분석서비스(소득소비-상권배후지)

- **항목**: 챗봇 데이터
- **칼럼**: 이름, 주소, 조회, 상세설명, 이용안내, 임베딩, 태그임베딩
- **갯수**: 1487
- **데이터타입**:
  - 이름, 주소, 이용안내, 상세설명: object
  - 조회: float
  - 임베딩, 태그임베딩: array

### 4. 이미지 추천 데이터

- **항목**: 이미지 추천 데이터
- **칼럼**: 이름, 이미지URL, 이미지임베딩, 관광지간 유사도
- **갯수**: 1472
- **데이터타입**:
  - 이름, 이미지URL: object
  - 이미지임베딩: array
  - 관광지간 유사도: object

### 5. 관광지별 추천 데이터

- **항목**: 관광지별 추천 데이터
- **칼럼**: ID, 이름, 태그, 조회, 임베딩, 태그임베딩, 유사관광지1, 유사관광지2, 유사관광지3, 유사관광지1의 유사도, 유사관광지2의 유사도, 유사관광지3의 유사도
- **갯수**: 1465
- **데이터타입**:
  - ID: object (웹페이지 구분자)
  - 조회: float
  - 임베딩(설명 핵심키워드의 임베딩 값), 태그 임베딩(태그 임베딩 값): array
  - 이름, 태그, 유사관광지1, 유사관광지2, 유사관광지3(각 관광지별 유사 관광지), 유사관광지1의 유사도, 유사관광지2의 유사도, 유사관광지3의 유사도(각 관광지별 유사 관광지 유사도): object

## 💻 기술 스택

| 분류 | 항목 |
|---|---|
| FRONT-END | HTML, CSS |
| BACK-END | Django, Python, Kakao API |
| DATA | Python |
| 협업 | Colab |

## 🎈 결과물


## 📎 이슈
1. 행정동, 법정동, 위치 정규화 -> 병합의 한계
2. 모든 구, 동에 대한 임대료 정보가 없음 -> 각 포토부스 내 거리 상 가까운 3곳을 뽑아 거리에 가중치를 두어 평균을 구함
3. 데이터 정규화: 스튜디오 구분
4. 웹의 input, output 결정 상 고민
5. 주위 {n}m 내의 {feature}, 반경 기수 설정
6. 머신러닝 학습 평가지표, 라벨의 부재 -> 3000개 샘플 데이터 수집
