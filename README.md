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

## Leader Arm (/dev/ttyACM1)

<img width="927" height="224" alt="Screenshot from 2025-11-12 13-15-09" src="https://github.com/user-attachments/assets/576c0579-f0f0-46cf-ab68-8cea430f3f8c" />

## Follower Arm (/dev/ttyACM0)

<img width="927" height="224" alt="Screenshot from 2025-11-12 13-15-13" src="https://github.com/user-attachments/assets/7defd4ff-2806-4e0d-916c-8d0dc738b089" />



#### 사소한 주의사항! 중간에 OSError 발생 (detect 에러 ) 케이블을 바꿔서 해보는 것을 추천함. 

<img width="927" height="224" alt="Screenshot from 2025-11-12 13-12-16" src="https://github.com/user-attachments/assets/c38234b3-c6ce-4027-b94f-a34317416944" />

## PermissionError

<img width="630" height="34" alt="Screenshot from 2025-11-12 13-23-08" src="https://github.com/user-attachments/assets/100f5b36-2166-47f2-80bc-89a873dd35eb" />


```bash
#와일드 카드 이용해서 모든 포트에 대해 권한 부여
sudo chmod 666 /dev/tty/ACM* 
```


### Teleopoeration Code

```bash

python -m lerobot.teleoperate \
    --robot.type=omx_follower \
    --robot.port=/dev/ttyACM0 \ # 현재 find-port 의 결과가 리더는 0 
    --robot.id=omx_follower_arm \
    --teleop.type=omx_leader \
    --teleop.port=/dev/ttyACM1 \ # 현재 find-port 의 결과가 팔로워는 1
    --teleop.id=omx_leader_arm

# 자신이 찾은 port 번호에 맞게 사용 (S0100 Arms 을 이용했을 때는 1,3 번 나오기도 하였음.)
```

