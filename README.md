# Example of MQTT communication between Raspberry Pi4 and aws-iot.

1. raspberry pi remote environment setting ( 라즈베리파이 원격 환경 세팅 )

- Remote control of raspberry pie 4 from the window.
- 윈도우컴퓨터를 이용하여 라즈베리파이4를 원격 조정하는 법 ( 하나의 컴퓨터로 라즈베리파이 및 기존 작업환경을 동시에 작업하기 위해서 )
 1) ssh, vnc enable ( ssh, vnc를 enable해준다. )
   a. sudo raspi-config
   b. interface Options
 
 2) ifconfig & check ip adress ( ip주소 확인 )
  - The WLAN or LAN connection of Raspberry Pie must be made up of a connection in the same environment as the Windows computer.
  - 무선랜 또는 유선랜을 컴퓨터와 동일한 연결로 구축해야 원격 조정 가능.
 
 3) Window 컴퓨터에서 연결을 위한 툴 설치 ( Putty, MobaXterm 등 )
  - MobaXterm을 추천하는데, 이유는 아래처럼, 폴더내에 파일을 복사하기도 용이하고 작업하기 편하기 때문이다.

![image](https://user-images.githubusercontent.com/68573143/148022419-03993fc6-d31a-4b5e-93ca-08935ec62323.png)
 
 4) ifconfig에서 확인한 ip주소를 입력하여 연결 및 id, pw 입력
 
 5) 코드 작성 및 컴파일

![image](https://user-images.githubusercontent.com/68573143/148022497-7e78a2ca-d5fc-4d61-a60a-a5515d90649a.png)


2. AWS IoT 세팅
