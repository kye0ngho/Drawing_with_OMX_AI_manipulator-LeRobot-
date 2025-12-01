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

### ROS2 진행.


```bash
# 참고자료
https://ai.robotis.com/omx/setup_guide_physical_ai_tools.html
```

```bash
#Docker Engine 과 nvidia Toolkit 를 설치하세요. (연구실 컴퓨터는 이미 설치되어 있어 생략함 자료참고.)

#공식 Docker Engine 설치 문서
https://docs.docker.com/engine/install/

#공식 nvidia Toolkit 설치 문서
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#with-apt-ubuntu-debian
```

### Docker Volume 구성
```bash

cd /kyeongho/omx # 대충 본인의 맞는 디렉토리 찾아가기
sudo install git
nano -l docker-compose.yml

#docker-compose.yml 구성

volumes:
  # Hardware and system access
  - /dev:/dev
  - /tmp/.X11-unix:/tmp/.X11-unix:rw
  - /tmp/.docker.xauth:/tmp/.docker.xauth:rw

  # Development and data directories
  - ./workspace:/workspace
  - ../:/root/ros2_ws/src/open_manipulator/ 
```

```bash

# container 시작

git clone https://github.com/ROBOTIS-GIT/open_manipulator

cd open_manipulator/docker && ./container.sh start
```

```bash

# 컴퓨터가 docker compose -f 에서 compose 를 인식하지 못해 오류 발생.
sudo apt-get update
sudo apt-get install docker-compose-plugin

# docker-compose -f 에서 - 가 없기에 인식 불가

cd open_manipulator/docker

# 일단 백업
cp container.sh container.sh.bak

# 'docker compose' → 'docker-compose' 로 전부 치환
sed -i 's/docker compose/docker-compose/g' container.sh
# sed -i 's/바꾸고 싶은/바꿔서 결과/g' 파일이름

#현재 실행중인 컨테이너 확인 가능.
docker ps 
```

<img width="1369" height="95" alt="image" src="https://github.com/user-attachments/assets/c3f7ba85-c0ef-4e91-a24a-45245fb1fd38" />

### container.sh 내부에 들어가기

```bash
./container.sh enter
```

### 실행 파일 포트 설정

```bash
ls -al /dev/serial/by-id/ #leader에만 케이블 연결.
```
### 결과 
<img width="1369" height="95" alt="image" src="https://github.com/user-attachments/assets/701a7a9b-2d75-4fec-8358-662325e9512c" />

### 파일 수정 (리더 , 팔로워도 동일.) 
```bash
sudo nano ~/ros2_ws/src/open_manipulator/open_manipulator_bringup/launch/omx_l_leader_ai.launch.py
```
<img width="1360" height="186" alt="image" src="https://github.com/user-attachments/assets/e1461420-e799-42d5-8852-e0e4972b559e" />


### Phyiscal AI Tools Docker 컨테이너 설정

```bash
git clone --recurse-submodules https://github.com/ROBOTIS-GIT/physical_ai_tools.git

# 역시나 여기도 docker compose 를 인식하지 못하기에 백업으로 만들고 docker-compose 로 다 바꿔줘야 함.
cd physical_ai_tools/docker && ./container.sh start

# 컨테이너 들어가기.
./container.sh enter

# 이후부터는 카메라 설정인데 우선 카메라는 스킵. (나중에 카메라를 통한 피드백 제어를 할 시에는 추가 해주기.)
```

### Teleoperation 해보기! 

arm controller 가 active 하지 못하는 오류 발생. 
그래서 teleoperation 작동하지 않음.



```bash
# 처음부터 오류.

ros2 launch open_manipulator_bringup omx_ai.launch.py

# port 번호 넣는 곳에 저 node ID 를 넣음으로 해결
```

https://github.com/user-attachments/assets/eb193879-3078-4c8e-aa53-bbd5d836cc49


<img width="1340" height="285" alt="image" src="https://github.com/user-attachments/assets/7aaac09a-201d-40a1-a079-fa42d870c0e4" />


### Mujoco ( 공식 Document 에 MJCF 파일 존재)
```bash
ros2 launch open_manipulator_bringup omx_f_gazebo.launch.py
# 가제보 launch
```

### 시뮬레이션으로 그림그리기. 

```bash

# 교수님 github 참고.

# 클론
git clone https://github.com/bareblackfoot/Drawing-with-OMX

# 필요 패키지들 설치
pip install -r requirements.txt

# notebook 이 없다면 (선택사항 있으면 안 해도 괜찮음)
pip install notebook

# notebook 실행
jupyter notebook notebook/drawing/drawing_omx.ipynb

```

stl 파일이 형식이 깨져서 다운받아지는 오류 발생 . 39byte밖에 안됨 그래서 mujoco 에서 못 띄움.

```bash

sudo apt-get update
sudo apt-get install git-lfs


```
