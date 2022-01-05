# Example of MQTT communication between Raspberry Pi4 and aws-iot.

## 1. raspberry pi remote environment setting ( 라즈베리파이 원격 환경 세팅 )
- 라즈베리파이 설치 및 기본 세팅은 다음을 따른다.
- https://webnautes.tistory.com/899

- Remote control of raspberry pie 4 from the window.
- 윈도우컴퓨터를 이용하여 라즈베리파이4를 원격 조정하는 법 ( 하나의 컴퓨터로 라즈베리파이 및 기존 작업환경을 동시에 작업하기 위해서 )
 
### 1) ssh, vnc enable ( ssh, vnc를 enable해준다. )
   - sudo raspi-config
   - interface Options
 
### 2) ifconfig & check ip adress ( ip주소 확인 )
  - The WLAN or LAN connection of Raspberry Pie must be made up of a connection in the same environment as the Windows computer.
  - 무선랜 또는 유선랜을 컴퓨터와 동일한 연결로 구축해야 원격 조정 가능.
 
### 3) Window 컴퓨터에서 연결을 위한 툴 설치 ( Putty, MobaXterm 등 )
  - MobaXterm을 추천하는데, 이유는 아래처럼, 폴더내에 파일을 복사하기도 용이하고 작업하기 편하기 때문이다.

### <img src="https://user-images.githubusercontent.com/68573143/148022419-03993fc6-d31a-4b5e-93ca-08935ec62323.png"  width="400" height="400"/>
 
 ### 4) ifconfig에서 확인한 ip주소를 입력하여 연결 및 id, pw 입력
 
 ### 5) 코드 작성 및 컴파일
### <img src="https://user-images.githubusercontent.com/68573143/148022497-7e78a2ca-d5fc-4d61-a60a-a5515d90649a.png"  width="400" height="300"/>

--------

## 2. AWS IoT 세팅 & 인증서 다운

###  1) AWS IoT -> 보안 -> 정책
	a. 정책이름 정하기(MQTT_Test_Policy)
	b. 작업 : iot:* ( 모든 IoT )
	c. 리소스 : * ( 모든 리소스 접근 )
	d. 허용 체크
	e. 생성

###  2) AWS IoT -> 관리 -> 사물 -> 사물 생성
	a. 단일 사물 생성
	b. 사물 이름 정하기 ( MQTT_Test )
	c. 이름 없는 섀도우(클래식) -> 다음

###  3) 디바이스 인증서
	a. 새 인증서 자동 생성
	b. 다음 -> 정책 -> 아까 만들어 둔 것

###  4) 디바이스 인증서
	a. 다운로드
	b. 디바이스 인증서, 퍼블릭 키 파일, 프라이빗 키파일, Amazon 신뢰 서비스 엔드 포인트 다
	c. 확인 후 사물 생성 확인 및 다운받은 인증서 위치 인지하기
   
###  5) MQTT 엔드포인트 주소확인
	a. 관리 -> 사물 -> Mtest -> 클릭
	b. 상호작용
	c. 설정보기 -> 엔드포인트 주소 확인
	d. 이 주소를 서버주소로 넣음

---------

## 3. aws 서버가 제대로 동작하는지 테스트를 위한 Node Red설치 및 실행 ( 4.으로 바로 넘어가도 됨 )
 - IoT 장치들간 연결을 쉽게 지원하는 오픈소스인 Node-red를 사용하여 코딩없이 연결이 제대로 이루어지는지 확인한다. ( 인증서 유효성 검증 )
 - 참고
### Node red 설치 및 설정 : http://daddynkidsmakers.blogspot.com/2019/05/iot-connection-noderedorg.html
### Node red & aws 연결 : https://www.youtube.com/watch?v=0PnrpBVUjJQ&ab_channel=%EA%B9%80%EB%8F%99%EC%9D%BC 

### 1) Node 생성 및 연결 
 - 아래 그림과 같이, 왼쪽의 common, network에서 오른쪽 처럼 생성 후 연결만 해준다.
![image](https://user-images.githubusercontent.com/68573143/148144099-c8fac700-dc7e-4084-912f-d8366062cce5.png)

### 2) inject 셀을 더블클릭 후 보낼 메시지 명 수정

### 3) mqtt 연결 브로커 서버, QoS 설정
 - 토픽에 본인이 원하는 토픽명 입력
 - QoS는 0으로 설정(추천)
 - 서버 오른쪽 수정하기 버튼 클릭
 <img src="https://user-images.githubusercontent.com/68573143/148144310-eee7c7fa-d9c8-4767-bae5-df0745d8aead.png"  width="400" height="300"/>

### 4) mqtt 연결 브로커 서버 & 인증서 입력
 - 포트 8883(aws)로 변경
 - 서버는 2. 5)에서 찾은 엔드포인트 주소 입력
 - 사용 TLS 오른쪽 수정하기 버튼 클릭 후 인증서, 비밀키, CA 인증서 받은 파일 선택 ( CA는 CA1로 )

![image](https://user-images.githubusercontent.com/68573143/148144451-ae9795ca-fc23-406c-b4b2-50a23eae111f.png)

### 5) mqtt in 셀 더블클릭 및 토픽, QoS 설정
 - mqtt out 셀에서 나머지는 설정해뒀으므로, 서버 및 인증서는 제대로 되어있는 확인만 하기
 - msg.payload 부분 디버그창으로 출력이 되어있는지 확인

### 6) 배포하기 클릭 및 접속됨 확인
 - 접속됨이 뜨지 않는다면, 앞의 순서가 제대로 되어있는지 확인해보기.
![image](https://user-images.githubusercontent.com/68573143/148144608-2ff5b32a-ae49-45a6-ab19-bd638bf38201.png)

### 7) 접속 됨 확인 & aws로 이동
 - aws iot -> 테스트 -> MQTT 테스트 클라리언트
 - 주제 구독에서 본인이 MQTT out에서 입력한 주제 입력
 - 구독 버튼 클릭
<img src="https://user-images.githubusercontent.com/68573143/148144823-3903b2a1-586c-42b6-8db3-c42607caa4e4.png"  width="500" height="380"/>

### 8) Node-RED에서 Test MQTT 왼쪽의 파란색 버튼을 클릭하고 메시지가 제대로 받아지는지 확인
![image](https://user-images.githubusercontent.com/68573143/148144912-8144e653-6655-4cbd-bc1e-d8aa6622b26d.png)


### 9) aws에서 Node-Red로 메시지 전송
 - 주제 게시로 이동 mqtt in 셀에서 설정한 topic으로 이름을 입력 후 게시 클릭
 - Node-RED debug창에서 제대로 메시지 받아지는지 확인

![image](https://user-images.githubusercontent.com/68573143/148145004-0aa49159-d58c-4f61-aed6-51967fed6b1e.png)

* 이부분이 잘 수행이 되었다면, aws의 서버는 제대로 돌아감을 확인할 수 있다.
* 이제 라즈베리 파이에서 직접 코드를 작성하여 연결을 확인하도록 한다.
----------

## 4. Raspberry pi와 aws 연결 
 ### 참고 : https://docs.aws.amazon.com/ko_kr/iot/latest/developerguide/connecting-to-existing-device.html
 
 ### 1) 운영 체제 업데이트 및 필수 라이브러리 설치
 	sudo apt-get update
	sudo apt-get upgrade
	sudo apt-get install cmake
	sudo apt-get install libssl-dev
	
### 2) Git 설치 & Python3 설치 ( 있다면 skip )
 	git --version
	// 없다면, sudo apt-get install git
	python3 --version // Python 3.5 이상만 가능.
	// 없다면, sudo apt install python3
	
### 3) pip3에 대한 테스트
	pip3 --version
	// 없다면, sudo apt install python3-pip
	cd ~
	python3 -m pip install awsiotsdk
	
	git clone https://github.com/aws/aws-iot-device-sdk-python-v2.git
	
### 4) 샘플 앱을 설치 및 실행
 - 샘플 앱에 대한 디바이스 인증서 파일을 설치
	cd ~
	mkdir certs
	
 - 인증서 파일 이름 수정 및 해당 위치에 파일 이동
 	* 루트 CA 인증서
		~/certs/Amazon-root-CA-1.pem
 	* 디바이스 인증서
		~/certs/device.pem.crt
 	* 프라이빗 키
		~/certs/private.pem.key

 - End point 주소 값 기억 및 저장

### 5) 샘플 앱을 설치하고 실행
 - 샘플 앱 디렉토리로 이동
 	cd ~/aws-iot-device-sdk-python-v2/samples
	
 - 아래 명령 줄을 입력하는데, your-iot-endpoint를 위에서 기억한 End point 주소로 변경
 	python3 pubsub.py --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint

 - 실행 결과 확인
 ![image](https://user-images.githubusercontent.com/68573143/148150714-8883157b-d761-4ef5-99ed-a29ed6ec65d6.png)

 - aws에서도 topic 구독 후 제대로 메시지 들어온 것 확인 가능
 ![image](https://user-images.githubusercontent.com/68573143/148150747-7af16fd5-7a29-4c64-a69b-d5df224d53f4.png)
