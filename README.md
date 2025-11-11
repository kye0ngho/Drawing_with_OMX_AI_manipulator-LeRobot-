# Drawing_with_OMX_AI_manipulator-LeRobot-

### OMX AI-Manipulator 를 이용한 그림그리기 . (ver --Lerobot)

### Step 1 : 각 device 의 포트번호 찾기.

```bash

#가상환경을 이미 만들었다고 가정 . 만들지 않았다면

conda create -n lerobot
#가상환경 활성화
conda activate lerobot

cd lerobot
#포트 번호 찾기

lerobot-find-port
```
### 결과 예시.
## Leader Arm
<img width="927" height="224" alt="Screenshot from 2025-11-12 08-53-41" src="https://github.com/user-attachments/assets/eb47d23e-8d47-41c5-b93e-9c91987af08f" />

## Follower Arm
<img width="927" height="224" alt="Screenshot from 2025-11-12 08-53-48" src="https://github.com/user-attachments/assets/6b96595b-7ecf-42fe-ba57-437dc2ca8c10" />

#### 사소한 주의사항! 중간에 OSError 발생 (detect 에러 ) 케이블을 바꿔서 해보는 것을 추천함. 
