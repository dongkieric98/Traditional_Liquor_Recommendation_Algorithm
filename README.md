# 전통주 추천 알고리즘
## 1. 프로젝트 개요

#### [배경]
- 전통주 시장 규모의 확대: 2020년 627억에서 2022년 1629억까지 약 260% 상승과 2030 세대의 관심도 상승을 기반으로 지속적인 전통주 시장 활성화의 필요성
- 전통주 정보 불균형 문제: 전통주 온라인 통합 플랫폼의 부재와 생산자와 소비자 간의 소통 부족으로 인해 다양한 전통주 판매처가 있지만 소비자들은 전통주에 대한 전문 지식이 부족하여 선택이 제한되며, 제품 구매에 대한 불확실성이 증가, 따라서 전통주 정보 불균형 완화를 위해 소비자의 정보 접근성 향상이 필요한 상황

#### [프로젝트 목표]
- 개인별 선호하는 술 속성을 파악하여 개인 맞춤화 서비스 제공
- 전통주 관련 실용적인 정보를 제공함으로써 전통주 산업 발전에 기여

#### [팀원 및 담당업무]
- 나의 역할: 사용자의 설문 내역을 바탕으로 전통주 추천 알고리즘 개발
- 팀원A: 서비스 화면 설계, 앱 서비스 기획 및 개발
- 팀원B: 프로젝트 기획 및 시장 분석, 비즈니스 모델 설계
- 팀원C: 데이터 수집 및 전처리, 데이터 시각화
- 팀원D: 데이터 수집 및 전처리, 데이터 분석

#### [README 주요 내용]
- 개인 맞춤형 전통주 추천 알고리즘을 개발을 담당하여 프로젝트의 다른 내용은 제외하고 해당 내용에 대해서만 기술

<br/></br>
<br/></br>

## 2. 개발 환경 (Environment)
- 운영체제: Window 11 Home
- 개발언어: Python 3.9.5
- 개발도구: Visual Studio Code (VSCode)

<br/></br>
<br/></br>

## 3. 라이브러리 (Library)
- numpy == 1.26.4
- pandas == 2.2.2

<br/></br>
<br/></br>

## 4. 파일 구조
- 'data/' : 전통주 추천에 필요한 데이터 (실제 데이터는 SQL에 저장되어 실시간으로 누적됨, 알고리즘 개발을 위해 실제 데이터에서 일부의 데이터만 추출)
  - 'INPUT.csv' : 고객이 평소 선호하는 주류들에 관한 데이터
  - 'OUTPUT.csv' : 고객에게 최종 추천할 전통주들이 모여 있는 데이터
  - 'user.csv' : 서비스를 이용하여 설문을 마친 유저의 정보가 모여있는 데이터
- 'TLRAlgorithm.ipynb' : 전통주 추천 알고리즘 CODE
- 'TLRAlgorithm.pdf' : 전통주 추천 알고리즘 PDF

<br/></br>
<br/></br>

## 5. 전통주 추천 알고리즘 제작 시 고민 & 어려웠던 점
#### [전통주 관련 데이터 수집]
- 어려움: 전통주의 속성과 관련된 다양한 데이터의 부재
- 해결방안: 전통주 추천 알고리즘을 만들기 위해서는 전통주를 수치화한 데이터가 있어야 했다.하지만 전통주를 수치화해둔 데이터는 네이버 지식백과에 있는 것 뿐이었고, 그 마저도 일부 데이터의 경우 결측치가 있었다. 따라서 어쩔 수 없이 해당 데이터를 활용하기로 하였고, OPENCV를 활용하여 전통주의 속성과 관련된 이미지 데이터를 추출하고, 결측치의 경우 일부는 전통주 축제 박람회에 참여하여 사람들의 설문을 통해 채워 넣었고, 나머지 채우지 못한 결측치의 경우 KNN Imputation을 활용하여 채워넣었다.

<br/></br>

#### [어떤 방식으로 추천할 것인가?]
- 어려움: 노베이스에서 시작하는 프로젝트였기에 고객에게 추천해줄 전통주에 있어서 '추천'이라는 단어를 정의하는데 많은 고민을 함
- 해결방안: 여러가지 아이디어들이 나왔지만 가지고 있는 데이터의 한계로 인해 보유하고 있는 데이터를 기반으로 추천하기로 결정하였고, 보유한 데이터들이 전통주의 속성과 관련된 데이터였기에 사용자로부터 선호하는 술의 속성과 선호하지 않는 술의 속성을 입력받아 해당 내용을 바탕으로 추천하기로 결정

<br/></br>
<br/></br>

## 6. 알고리즘 사용 근거 및 설명
#### [대표 술 좌표 생성 알고리즘]
-	사용자로부터 평소 선호하는 술의 종류를 입력 받고 이를 가장 잘 표현할 수 있는 하나의 가상의 술을 만드는 알고리즘이다. 각 술들은 모두 속성값들을 가지고 있고 이를 이용해 각 술 별로 고유한 좌표를 나타낼 수 있다.
-	사용자가 입력한 술들의 좌표가 있다면, 이제 이를 이용해 입력된 술을 대표하는 하나의 좌표를 만들어야 한다. 하지만 여기에는 문제가 있다. 바로 어떤 방식으로 대표하는 좌표를 만드냐는 것이다.
-	이 문제는 양조장에서 실제 새로운 술을 만드는 방식을 참고하여 해결할 수 있다. 양조장에서는 새로운 술을 만들 때, 기존에 존재하는 술들을 바탕으로 만든다. 간단한 예를 들면 단맛의 정도가 3인 새로운 술을 만들고자 할 때, 단맛의 정도가 2인 술과 단맛의 정도가 4인 술을 조합하여 단맛의 정도가 3인 술은 만든다. 
-	실제로 만드는 새로운 술을 만드는 방식은 이 방법과 더불어 추가적인 방법이 있겠지만, 결국의 본질은 두 술을 조합하고 평균을 내어 새로운 술을 만든다는 것이다.
-	따라서 우리 역시 이러한 방식을 사용하여 3가지 술을 대표하는 하나의 술을 만들었다. 즉 사용자가 선택한 3가지 술의 특성을 기반으로 가상의 맥주를 만들 때, 각 술의 특성값을 평균하여 최적의 특성을 도출하였다.

<br/></br>

#### [가성비 필터링 알고리즘]
-	가성비는 전통주만이 아니라 모든 상품을 구매하는데 있어 중요한 요소이다. 그리고 이것은 모두가 아는 자명한 사실이다. 하지만 중요한 요소임에도 불구하고 실제로 가성비를 중요하게 여기는 사람도 있고, 중요하게 여기지 않는 사람도 있다. 따라서 가성비 측면에 있어서 우리는 전통주를 구매하는 고객을 두 가지 집단으로 나누고 다음과 같은 추천 방식을 적용했다. 
-	(1) 가성비가 중요하지 않은 집단: 가성비에 상관없이 모든 술을 추천해준다.
  (2) 가성비를 중요하게 생각하는 집단: 일정 수준의 가성비를 제시하고 그 이하의 가성비는 추천하지 않는다.
- 여기서 바로 우리가 해결해야 할 문제점이 생긴다. 그것은 바로 어떤 술을 기준으로 가성비를 정해야 할까? 그리고 그에 대한 조건은 다음과 같다.
-	(조건1) 가성비의 기준이 되는 술은 모두가 알아야 하는 술이어야 한다. 그래야 사람들이 가성비의 기준을 이해하고 해당 질문에 답변을 할 수 있기 때문이다. 즉 대중적인 술이여야 한다.
  (조건2) 선택된 술을 기준으로 Output에서 필터링 되기 때문에 해당 술의 가성비 정도가 Output에 있는 술의 가성비 분포에서 너무 편향되게 나눠서는 안된다. 예를 들어 기준이 되는 술의 가성비가 0.1이라고 가정하자. 이때, Output에서 가성비가 0.1 이하의 술은 1%밖에 없고, 가성비가 0.1 초과의 술이 99%라면 이는 필터링을 하나마나가 된다.
- 따라서 이를 위해 우리가 정한 기준은 필터링을 하는 정도가 30% ~ 70% 정도가 되어야 한다고 잡았다. 왜냐하면 너무 적으면 필터링을 하는 의미가 없어지고 너무 많을 경우 최종 추천되는 술이 없을 수도 있기 때문이다. 따라서 이러한 조건에 만족하는 술로 나온 것이 바로 편의점 맥주이다. 왜냐하면 편의점 맥주의 경우 대부분의 사람들이 시도해본 경험이 있으며 Output 데이터의 분포에서 30%정도의 전통주를 필터링할 수 있기 때문이다.

<br/></br>

#### [도수 설정 알고리즘(절대도수 필터링 & 상대도수 좌표편입)]
-	술에 있어서 도수는 매우 중요한 요소이다. 따라서 술을 추천해줄 때도 도수와 관련된 질문이 있어야한다. 이때 우리는 두 가지로 나눠서 도수를 추천해주기로 했다. 첫번째, 평소에 마시던 정도의 도수를 선호하는 사람, 두번째, 도수에 상관없이 모든 술을 좋아하는 사람. 이렇게 두 부류로 분류했다. 
-	두번째 사람들의 경우, 도수에 상관없이 모든 술을 추천해주면 되기 때문에 따로 필터링할 작업은 없다. 하지만 문제는 첫번째 사람들이다. 첫번째, 사람들의 경우 평소에 마시던 술에 대한 데이터가 있어서 이 사람이 평소에 마시던 술의 도수에 대해서 알 수는 있다.
-	하지만 평소 마시던 술의 도수에 대한 기준을 잡는 것에 어려움을 느꼈다. 예를 들어 이 사람이 평소에 마시던 술의 도수가 13도라면 몇 도까지 평소에 마시던 술이라고 할 수 있을까? 즉 +? -? 까지 평소에 마시던 술이라고 할 수 있을까? 
-	이에 대한 문제의 해결법을 찾기 위해서 선행 연구를 참고했다. 여기서 말하는 선행 연구의 경우 기존에 존재하던 술 추천을 참고하는 것이다. 이에 따르면 일반적으로 술 추천에 있어서 도수가 3도 정도의 차이가 날 경우 평소 마시던 술이라고 정의하였다. 따라서 우리 역시 기존에 하던 방식을 따라하여 이 공식을 적용하였다.
-	따라서 어떤 사용자가 평소에 마시던 정도의 도수가 좋다고 할 경우 우리가 추천해주는 도수의 범위는 평소 마시던 도수 -3 < 추천 도수 < 평소 마시던 도수 +3 가 될 것이다.

<br/></br>

#### [중요 요소 알고리즘]
- 중요요소에는 크게 6가지가 있다. (1)향의 종류 (2)향의 강도 (3)단맛 (4)신맛 (5)청량감 (6)바디감 이 중에서 우리는 사용자가 가장 중요하게 생각하는 요소 중 한 개를 고르라고 설문을 한 후에 그 값의 정도에 따라 모든 전통주에 대해 가중치를 부여한다. 
- 가중치를 부여하기 전에 6가지의 요소들이 어떤 형태로 데이터프레임에 존재하는지 알아야 한다. 이는 크게 두가지로 나눌 수 있는데 바로 향의 종류와 나머지이다. 향의 종류의 경우 해당 전통주가 어떤 향을 가지고 있는지 우리가 분류한 종류에 따라 텍스트 형식으로 나타나있다. 반면에 향의 강도, 단맛, 신맛, 청량감, 바디감의 경우 모두 1이상 5이하의 숫자로 된 값이 들어가있다. 그렇기 때문에 우리는 데이터타입이 숫자일 경우와 문자일 경우 이 두가지로 구분해서 가중치를 부여해야 한다.
- 다음으로 가중치에 관해서이다. 초기에 가중치를 부여하는데 있어서 주관적인 판단이 필연적으로 들어갈 수 밖에 없다. 왜냐하면 최적의 가중치를 처음부터 바로 알 수 없기 때문이다. 그렇기 때문에 초기에는 주관적인 판단에 따른 가중치를 설정한 후 이후 사용자의 피드백을 통한 (우리에게 있어서는 평점) 지속적인 가중치의 조정이 필요하다.
- 가중치의 적절성을 판단하는데 있어서는 크게 두 가지 요소가 있다. 첫번째로 사용자의 경험과 피드백이 반영되었는가? 두 번째로 추천 결과의 성능이 좋은가? 이다. 따라서 우리는 적절한 가중치를 찾기 위해 사용자에게 추천해준 술을 사용자가 직접 마셔본 후 해당 추천 상품에 대한 평가점수를 입력 받는다. 이는 추천 결과의 성능을 사용자의 경험해본 피드백이기 때문에 이러한 방식이 지속된다면 적절한 가중치를 찾을 수 있을 것이다.
- 이에 따르면 우리가 초기에 설정한 가중치를 구하는 공식은 다음과 같다.
Case1) 향의 종류 가중치 구하는 공식: max(0.2,1−공통향의개수/전통주향의개수)
Case2) 향의 종류 이외의 가중치 구하는 공식: weight=(6−중요요소 값)/5
- Case1의 경우 모두 가중치의 범위가 (0.2 ~ 1)의 값이 나온다. Case1의 경우 0.2를 최소값으로 따로 설정한 이유는 공통 향의 개수와 전통주 향의 개수가 같을 경우 값이 1이 되어 가중치가 0이 되는 것을 방지하기 위함이다. 마찬가지로 Case2의 경우 6에서 중요요소 값을 뺀 이유는 중요요소 값이 1~5까지이고 만약에 6이 아닌 5로 할 경우 값이 0이 되는 경우가 있기 때문이다.
- 즉 가중치가 0이 되는 경우를 방지하기 위해 최솟값을 설정하여 가중치를 부여하였다. 가중치가 0이 될 경우 기존에 있는 거리와 곱했을 경우 가중된 거리 값이 0이 되어 항상 추천되기 때문이다. 이 경우 값이 0인 전통주가 항상 추천되기 때문에 적절한 추천이 이루어질 수 없다고 생각했다.

<br/></br>
<br/></br>

## 7. 참고사항
- 해당 코드는 알고리즘 제작을 위해 만들어진 독립적인 코드로, 해당 알고리즘을 바탕으로 Django 코드에 적용하여 실제 웹 서비스에 사용됨
- 실제 서비스는 웹 기반 서비스이며, 데이터 역시 SQL에서 불러오기에 해당 repository에 있는 데이터의 경우 알고리즘 제작을 위해 일부의 전체 데이터 중 컬럼만 동일하게 유지한 채 일부의 데이터를 추출해온 것
