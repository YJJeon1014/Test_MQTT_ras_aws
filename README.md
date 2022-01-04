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
### <img src="https://user-images.githubusercontent.com/68573143/148022497-7e78a2ca-d5fc-4d61-a60a-a5515d90649a.png"  width="400" height="400"/>

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

## 3. Raspberry pi code 작성 및 실행
