# 아두이노 코드

[Uploading s#include <SoftwareSerial.h>

SoftwareSerial bluetooth(2, 3); // RX, TX (2번 핀은 아두이노의 TX, 3번 핀은 아두이노의 RX)

void setup() {
  bluetooth.begin(9600); // Bluetooth 모듈을 9600bps 속도로 초기화
  pinMode(4, INPUT); // 스위치를 4번 핀에 연결
}

void loop() {
  if (digitalRead(4) == HIGH) {
    bluetooth.write("PINK"); // 스위치가 눌렸을 때 스마트폰에 "PINK" 전송
  } else {
    bluetooth.write("BLUE"); // 스위치가 떼졌을 때 스마트폰에 "BLUE" 전송
  }
  delay(100); // 잠시 대기
}
ketch_oct10a.ino…]()


# 프로세싱 코드

[Uploading sketch_23101import processing.serial.*;
import processing.net.*;

Serial myPort; // 시리얼 통신을 위한 객체
Server server; // 서버 객체
Client client; // 클라이언트 객체

void setup() {
  size(200, 200);
  // 아두이노와 시리얼 통신 설정 (아두이노와 연결된 포트 이름을 사용)
  myPort = new Serial(this, "COM3", 9600);
  
  // 서버를 12345 포트로 시작
  server = new Server(this, 12345);
}

void draw() {
  // 클라이언트 연결 확인
  if (server != null && server.available() > 0) {
    client = server.available();
    
    // 클라이언트에서 데이터를 받음
    String data = client.readStringUntil('\n');
    
    // 받은 데이터를 아두이노로 전송
    if (data != null) {
      data = data.trim(); // 데이터 앞뒤의 공백 제거
      myPort.write(data); // 아두이노로 데이터 전송
    }
    
    // 클라이언트 연결 종료
    client.stop();
  }
  
  // 아두이노로 [On], [Off], [Song] 명령 보내기
  if (mousePressed && mouseX < width / 3) {
    sendDataToArduino('1');
  } else if (mousePressed && mouseX >= width / 3 && mouseX < 2 * width / 3) {
    sendDataToArduino('0');
  } else if (mousePressed && mouseX >= 2 * width / 3) {
    sendDataToArduino('2');
  }
}

void sendDataToArduino(char data) {
  if (myPort != null) {
    myPort.write(data); // 아두이노로 데이터 전송
  }
}
0a.pde…]()


# 설명
위와같은 아두이노 코드와 프로세싱 코드를 작성하고 App inventor에서 디자인한후 실행시키면 스마트폰앱에서 ON버튼을 눌렀을시 아두이노의 13번 LED가 켜지고 OFF 버튼을 누르면 13번 LED가 꺼지는 작동이 나타난다. 그리고 SONG 버튼을 누르면 "도도솔솔라라솔"이 연주되는 프로그램이다.

# 아두이노 코드

[Uploading s#include <SoftwareSerial.h>

SoftwareSerial bluetooth(2, 3); // RX, TX (2번 핀은 아두이노의 TX, 3번 핀은 아두이노의 RX)

void setup() {
  bluetooth.begin(9600); // Bluetooth 모듈을 9600bps 속도로 초기화
  pinMode(4, INPUT); // 스위치를 4번 핀에 연결
}

void loop() {
  if (digitalRead(4) == HIGH) {
    bluetooth.write("PINK"); // 스위치가 눌렸을 때 스마트폰에 "PINK" 전송
  } else {
    bluetooth.write("BLUE"); // 스위치가 떼졌을 때 스마트폰에 "BLUE" 전송
  }
  delay(100); // 잠시 대기
}
ketch_oct20a.ino…]()


# 프로세싱 코드

[Uploading sketch_import processing.net.Server;
import processing.net.Client;

Server server; // 서버 객체
Client client; // 클라이언트 객체

void setup() {
  size(400, 400);
  background(255); // 초기 배경색 설정 (하얀색)
  
  server = new Server(this, 12345); // 12345 포트로 서버 시작
}

void draw() {
  if (client != null && client.active()) {
    while (client.available() > 0) {
      String data = client.readStringUntil('\n'); // 클라이언트로부터 데이터 읽기
      if (data != null) {
        data = data.trim(); // 앞뒤 공백 제거
        if (data.equals("PINK")) {
          background(255, 182, 193); // 분홍색
        } else if (data.equals("BLUE")) {
          background(135, 206, 235); // 하늘색
        }
      }
    }
  }
}

void serverEvent(Server someServer, Client someClient) {
  if (client == null || !client.active()) {
    client = someClient; // 클라이언트 연결 수락
  } else {
    someClient.stop(); // 이미 연결된 클라이언트가 있을 경우, 새로운 연결 거부
  }
}

void keyPressed() {
  if (key == 'c' || key == 'C') {
    background(255); // 'c' 키를 누르면 배경색을 다시 하얀색으로 변경
  }
}
231010b.pde…]()

# 설명
위와같은 아두이노코드와 프로세싱 코드를 실행시키면 아두이노에서 스위치를 붙이면 스마트폰에서 0을받으면서 분홍색화면이 나타나고, 스위치를 떼면 스마트폰에서 1을 받으면서 화면이 하늘색으로 변하는 프로그램이다. c키를 누르게되면 스마트폰화면이 원상복구되는 코드도 집어넣었다.

# 소감
분명 수업시간에 다 배웠던 매커니즘인데 하나둘씩 섞여서 한번에 해결하려니깐 쉽지않은 느낌이들었다. 개인적으로는 굉장히 복잡한 느낌이였지만 성공했을때는 뿌듯함이 있었고 여러가지 기능들이 복합적으로 수행되는것을 보고 아주 흥미롭고 신기하였다.


