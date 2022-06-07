# Quick Start Guide

## Outline

### 웹/앱

사이트 : http://15.164.52.133:8100
(사용설명서 페이지 참고)

1. login (필수 : 로그인을 해야 사진과 정보를 불러옴)
   1. 만약 사진이 제대로 안 불러와진다면 새로고침/종료 후 다시 실행해서 로그인해보기
2. 드론 설정
   1. 드론 목록 확인
   2. 비행 시작 시간 설정
   3. 드론 추가
   4. 드론 비행 정보 설정
      1. 지도로 장거리 측정
      2. 유의사항
      - !! 밭 거리, 밭 세로, 재식 간격 설정 주의 !!
      - (텔로는 cm 단위로 비행. 오차범위 20cm 고려하기)
      - 추천 비행 정보 :
        `밭 거리 : 30, 밭 세로 : 30, 재식 간격 : 10`
3. 품질 확인
   1. 날짜 선택
   2. 포도 선택 -> segmentation 결과 사진 보임

### 드론 데스크탑 앱

상단에서 2번 후, 3번 전에 비행 시작 및 영상 서버 전송 필요

1. `.env` 파일은 root 경로에 저장 (앞에 1234 제거하기)
   - `source env`
2. Wifi 연결하고 로그인 하기
   - 로그인 후 flight_info 폴더에 새로 추가된 드론의 비행 정보가 제대로 입력되었는지 확인 가능
3. Wifi 연결 끊고 텔로 Wifi와 연결한 뒤 비행 시작 버튼 클릭

   - !! 처음에 오른쪽으로 날기 시작함
   - !! 뜨거워지면 정상적으로 비행 불가. 식혀야 함.
   - !! 바람 없는 실내에서 진행하기
   - 정상 착륙해야만 영상이 정상적으로 저장됨
   - 텔로 Wifi 이름 : TELLO-9FF9C8

4. Wifi 다시 연결하고 영상 전송하기

   1. 영상을 서버에 전송할 때 드론 영상에는 포도가 없기에 아래 영상 이용
      https://drive.google.com/file/d/1t08glYIRR7alYILkFLlJugU_jIL43Xla/view?usp=sharing
   2. drone 폴더의 원하는 드론 이름의 폴더 아래 %Y-%m-%d-%H-%M-%S.mp4 형식의 이름으로 영상 저장 후 전송하기
      - 영상이 목록에 안 보이면 다른 드론 선택하고 다시 원래 드론 선택하기

!!! docker container로 람다를 생성하면 오랜만에 구동할 때 다시 build하느라 에러를 일으킵니다.
처음 시작할 때 아래 코드를 돌리면 람다가 켜지게 됩니다. 이 코드를 실행하고 10-15분 이후에 시도해주세요. (Internal Server Error는 무시하세요)

```python
import requests
yolo = "https://6spqgvscd4.execute-api.ap-northeast-2.amazonaws.com/Prod/hello"
delete_result = "https://rhaxg1dkp6.execute-api.ap-northeast-2.amazonaws.com/default/lambda_function"
boxinst = "https://knulfj7cz8.execute-api.ap-northeast-2.amazonaws.com/Prod/hello"
rfr = "https://2f3ml044ti.execute-api.ap-northeast-2.amazonaws.com/Prod/hello"

url = [yolo, delete_result, boxinst, rfr]
for val in url:
    result = requests.get(val)
    print(result, result.text)
```

## 구동 방법

### 드론 데스크탑 앱 - application/afarm_drone

```bash
# Install
git clone https://github.com/lazybuttrying/afarm_drone.git
cd afarm_drone
pip3 install -r requirements.txt

# Run
python3 main.py
```

!! 텔로 유의 사항

- 배터리 방전이 굉장히 심한 편. 비행 전 충전 필수 !

### 모델 로컬 테스트 - inference/afarm_stepfunction

(모델 서버 이상이라면 아래 방법으로 각각 시도)

move to "hello_world" folder of each model

```bash
# Build
docker build -t {model_name} .

# Run
docker run --name {model_name} -it {model_name}

# execute
cd /var/task && python3 app.py
# quaility_id가 91번으로 배정된 영상을 바탕으로 진행됨
```
