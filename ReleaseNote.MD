# Quick Start Guide

## Outline

### 웹/앱

사이트 : http://15.164.52.133:8100

- 앱의 경우 아이오닉 특성 상 웹뷰로 변환되어 나오기에 위와 동일함
- 앱에서 사이트를 실행할 경우 Chrome 이용하기

(사용설명서 페이지 참고)

1. login (필수 : 로그인을 해야 사진과 정보를 불러옴)
   1. 만약 사진이 제대로 안 불러와진다면 새로고침/종료 후 다시 실행해서 로그인해보기
2. 드론 설정
   1. 드론 목록 확인
   2. 비행 시작 시간 설정
   3. 드론 추가
   4. 드론 비행 정보 설정
      1. 지도로 장거리 측정
3. 영상 전송

   - home에서 영상 전송 페이지 접속
     - skip_frame : 1 (영상에서 얼마나 프레임을 스킵할 지 지정)
     - 드론 선택 : 내 드론 목록이 나옴. 원하는 드론 선택
     - 영상 : https://drive.google.com/file/d/1t08glYIRR7alYILkFLlJugU_jIL43Xla/view?usp=sharing

4. 품질 확인
   1. 날짜 선택
   2. 포도 선택 -> segmentation 결과 사진 보임

+) 영상 업로드 및 딥러닝 서버 작동 확인
!!! docker container로 람다를 생성하면 오랜만에 구동할 때 다시 build하느라 에러를 일으킵니다.
처음 시작할 때 아래 코드를 돌리면 람다가 켜지게 됩니다. 30분마다 실행하도록 설정했습니다. 이 시간 간격이 너무 길게 느껴진다면 아래 코드를 실행하고 10-15분 이후에 시도해주세요. (Internal Server Error는 무시하세요)

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

### 모델 로컬 테스트 - inference/afarm_stepfunction

(모델 서버에 이슈가 발생한다면 아래 방법으로 각각 시도)

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
