# Drawing_with_OMX_AI_manipulator_LeRobot

### OMX AI-Manipulator 를 이용한 그림그리기 . (ver --Lerobot)

### 참고자료 
```bash
https://ai.robotis.com/omx/lerobot_imitation_learning_omx.html
```

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

```bash

# teleoperation code.

python -m lerobot.teleoperate \
    --robot.type=omx_follower \
    --robot.port=/dev/ttyACM0 \ # 현재 find-port 의 결과가 리더는 0 
    --robot.id=omx_follower_arm \
    --teleop.type=omx_leader \
    --teleop.port=/dev/ttyACM1 \ # 현재 find-port 의 결과가 팔로워는 1
    --teleop.id=omx_leader_arm

# 자신이 찾은 port 번호에 맞게 사용 (S0100 Arms 을 이용했을 때는 1,3 번 나오기도 하였음.)
```

