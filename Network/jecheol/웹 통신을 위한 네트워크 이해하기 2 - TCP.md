
지난 시간에 OSI 7 Layer나 TCP/IP Stack에 대하여 다뤘다. 여러 레이어를 거치며 통신이 이루어진다고만 이해해도 일단은 괜찮다 더 중요한 건, 통신의 목적이 되는 "중요 데이터"를 어떻게 주고받는가 이다. 이렇게 통신의 목적이 되는 중요 데이터를 어떤 규약으로 통신하는가를 담당하는 레이어는 Transport Layer이다. 전송 레이어라고도 하며, 여기에는 `TCP`와 `UDP`라는 두 개의 프로토콜이 메인을 이루고 있다. 이에 대하여 잘 이해하면, 네트워크에서 가장 중요한 것 중 하나를 이해했다고 해도 과언이 아니다. 따라서 차근차근 transport 레이어의 전송 프로토콜인 TCP, UDP에 대하여 자바 코드로 이해해볼 예정이다. 이번 시간에는 TCP 위주로 확인해보자.

## TCP

TCP는 전송 계층 프로토콜의 가장 중요한 데이터 통신 프로토콜이다. 이는 연결형 프로토콜로, 상대 프로세스와 연결을 수립한 뒤 데이터를 주고받는 방식으로 이루어진다. 통신을 하기 전에 연결 요청과 연결 수락 과정을 거치면 통신 회선이 고정되어 데이터가 전달된다. 이를 통해 전달되는 데이터는 순서대로 전달되며 손실이 발생하지 않도록 컨트롤된다.

- 연결형 프로토콜
- 연결 수립 후 통신 회선이 고정되어 데이터 전달
- 데이터 순서를 보장, 패킷 손실 방지 컨트롤 O

간단하게 연결 수립과 끝맺음에 대하여 사진으로 갈무리하고 넘어가려고 한다. TCP는 3-way handshake라는 과정으로 연결을 수립하고 4-way handshake라는 과정으로 연결을 끝낸다. handshake란 악수라는 의미로, 서로 합의 하에 연결 관계를 수립하고 일련의 데이터를 보낸 뒤 연결을 끝냄으로써 통신을 마치는 과정을 악수로 시작해서 악수로 끝나는 사람간의 대화처럼 표현한 거라고 이해할 수 있다.

![](https://media.geeksforgeeks.org/wp-content/uploads/20220406110952/TheThreeWayConnectionEstablishment.jpg)

연결을 수립하는 3-way handshake는 이처럼 편도 3번의 `SYN`, `ACK` 패킷을 주고받음으로써 수행된다. `SYN`이란 synchronize를 떠올리며 의미를 이해할 수 있는데, 의역하자면 "데이터 보낼건데, 나랑 연결할래?"와 같은 의미로 이해하면 된다. `ACK`은 acknowledge로 이해하면 되고, 알겠다/패킷 잘 받았다는 응답의 의미를 담는 패킷이다. 그리고 서버 입장에서도 `SYN`을 보냄으로써 "데이터 받을 수 있는데, 연결할래?"와 같이 회신을 보내준다. 그러면 client에서 최종 `SYN`에 대한 `ACK`을 보냄으로써 두 노드 간 TCP 연결이 수립된다.

![](https://media.geeksforgeeks.org/wp-content/uploads/20220406111342/TheFourWayHandshakeProcess.jpg)

TCP 통신을 끝내는 건 4번의 `SYN`, `ACK` 패킷을 통신해줘야 해서 4-way handshake라고 부른다. 그 이유는, 한참 데이터가 통신되고 있는 과정에서 client는 자기가 보낼 건 다 보냈다는 의미에서 통신 종료를 의미하는 `Fin` 패킷을 보내는 것인데, 데이터를 받고 있던 입장에서는 아직 모든 패킷이 수신되지 않은 상태일 수 있기 때문이다. 따라서 최초 `FIN`에 대한 `ACK`을 일단 보냄으로써 `FIN` 패킷이 잘 수신되었음을 나타낸 뒤, 나중에 모든 패킷이 정상적으로 수신되었음이 확인된 뒤, 서버에서 `FIN` 패킷을 클라이언트에 보내어 진짜 종료를 나타내고, 이에 client가 `ACK`을 보냄으로써 통신이 종료하게 된다.

TCP는 대표적으로 웹 통신에 지배적으로 사용되는 통신 프로토콜인 HTTP/1.1과 HTTP/2 에 사용된다.(이 사실 만으로 TCP가 웹 통신에서 중요한 이유는 충분히 설명되었다 생각한다.) HTTP를 잘 모른다면, 웹 브라우저가 웹 서버와 통신할 때 보통 TCP를 사용한다고 이해하면 된다. 그 외에도 이메일 전송(SMTP), 파일 전송(FTP)에도 TCP가 사용된다. 이와 유사한 개념으로, 흔히들 우리는 TCP와 함께 네트워크 레이어의 IP 프로토콜을 같이 묶어서 TCP/IP라고 하는데, 이는 두 프로토콜이 항상 세트로 사용되기 때문이다. 각 레이어는 유기적으로 연결되어 하나의 통신 과정일 뿐이므로 공부할때에는 논리적으로 구분하더라도, 모두 분리된 개념이 아니라 하나로 통하는 것임을 명심해야 한다.

### 자바에서는 이를 어떻게 코드로 구현하나요?

자바에서는 TCP 통신을 수립하기 위해 `java.net` 패키지의 `ServerSocket`과 `Socket`클래스를 제공한다. 말 그대로, `ServerSocket` 클래스가 TCP 연결을 수락하는 서버 쪽 클래스고, `Socket`은 클라이언트 쪽에서 TCP 통신 연결을 요청하기 위해서도 쓰이고, 서버와 클라이언트 둘 다 데이터를 주고 받을 때 사용하는 클래스이다.

```java
// 방법 1
ServerSocket serverSocket = new ServerSocket(15000); // port 번호 : 15000

// 방법 2
ServerSocket serverSocket = new ServerSocket();
serverSocket.bind(new InetSocketAddress(15000));
```

기본적으로 자바 코드로 TCP 서버를 구축하기 위해서는 `ServerSocket` 객체를 생성해야 연결 요청을 수락할 수 있다. 이 객체를 사용하기 위해서는 프로세스 port 번호를 바인딩해줘야 하는데, 편한 방법은 객체를 생성할 때 해당 데이터를 통신할 서버 프로세스 port 번호를 생성자 argument로 지정해주는 것이다. 이렇게 서버 프로세스와 `ServerSocket` 객체가 바인딩 되었으며, client의 TCP 연결 요청 수락을 위한 준비가 끝난 것이다. (만약 서버 컴퓨터에 IP가 여러개로 할당되어 있다면, `InetSocketAddress`의 parameter에 특정 IP 주소를 지정해주면 된다.)

```java
// server
Socket server = serverSocket.accept(); // 클라이언트의 연결 요청을 기다림

// client
Socket client = new Socket("localhost", 15000); // 해당 IP 주소와 port로 TCP 연결
```

`.accept()` 메서드를 수행하면 client의 연결 요청이 들어올 때 까지 프로세스가 블락되며, 요청이 들어오면 해당 요청을 받으며 Socket 객체를 반환한다. 이 과정에서 두 노드 간 TCP 연결이 수립된 것이며, Socket 객체는 상대 프로세스의 Port 번호는 물론, 상대 호스트의 IP 주소까지 갖고 있다. 이는 다음과 같이 확인이 가능하다.

```java
InetSocketAddress isa = (InetSocketAddress) socket.getRemoteSocketAddress();  
String clientIp = isa.getHostName(); // 클라이언트 IP 주소  
int clientPort = isa.getPort(); // 클라이언트 Port 번호
```

위 로직 중 `socket.getRemoteSocketAddress();`를 통해 `RemoteSocketAddress`라는 추상 클래스로 형변환된 변수를 반환받을 수 있는데, 이를 상속하여 구현한 클래스가 `InetSocketAddress`이다. 따라서 구현체 타입으로 다운캐스팅을 통해 해당 구현 클래스가 갖는 메서드를 사용할 수 있도록 한 코드이다.

```java
serverSocket.close(); // 서버 소켓 종료
```

`serverSocket` 객체의 `close()` 메서드를 통해 TCP 서버를 종료할 수 있으며, 이를 통해 socket에 바인딩 해 뒀던 port 번호를 언바인딩할 수 있다. 이렇게 한 이후에야 다른 프로그램에서 다시 해당 Port 번호를 사용할 수 있게 된다. (그 전에는 바인딩되어있는 프로세스가 있는 거라서 불가능)

### TCP 통신으로 Echo 구현하기

네트워크를 배우는 과정에서 한번쯤은 해보는, TCP 기반으로 echo를 제공하는 서버와 클라이언트 코드를 작성해 보자.

아래 코드는 로컬호스트의 15000번 포트를 이용하여 TCP 통신으로 echo 서비스를 제공하는 서버와 이를 이용하는 클라이언트를 구현한 내용이다. 

**TCP 기반 Socket Server**

TCP 방식은 당연히 서버가 먼저 실행되어 기다리고 있는 상황에서 client가 실행되어 데이터를 전송하고 이를 echo로 받는 시나리오에서만 가능하다. 따라서 SocketServer를 기준으로 먼저 서술하겠다.

```java
public class TCPServer {  
    private static ServerSocket serverSocket = null;  
  
    public static void main(String[] args) throws IOException {  
		// tcp socker server start
        startServer();  

		// set quit condition
        System.out.println("서버를 종료하려면 q를 입력하시오.");          
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        while (true) {  
            String input = br.readLine();  
            if (input.equals("q")) {  
                break;  
            }  
        }  
        br.close();  

		// terminate socket server
        stopServer();  
    }  
  
    public static void startServer() {  
        // define worker thread  
        Thread thread = new Thread(() -> {  
            try {  
                // serverSocket generated & port bound  
                System.out.println("server : start");  
                serverSocket = new ServerSocket(15000);  
                System.out.println("server : bind port");  
  
                while (true) {  
                    System.out.println("================================");  
                    System.out.println("server : wait for client request"); 
                     
                    // wait for establishing connection  
                    Socket socket = serverSocket.accept();  
  
                    // get informations of tcp connection host  
                    InetSocketAddress isa = (InetSocketAddress) socket.getRemoteSocketAddress();  
                    String clientIp = isa.getHostName();  
                    System.out.println("server : clientIP " + clientIp + " connected");  
                    // get data from client  
                    byte[] buffer = new byte[1024];  
                    InputStream inputStream = socket.getInputStream();  
                    int input = inputStream.read(buffer);  
                    String data = new String(buffer, 0, input, "UTF-8");  
                    System.out.println("server : read data -> " + data);  
  
                    // send echo data to client  
                    DataOutputStream dataOutputStream = new DataOutputStream(socket.getOutputStream());  
                    dataOutputStream.writeUTF(data);  
                    dataOutputStream.flush();  
                    System.out.println("server : send echo -> " + data);  
  
                    // data transfer using tcp ended  
                    socket.close();  
                    System.out.println("server : clientIP " + clientIp + " connetion closed");  
                }  
  
            } catch (IOException e) {  
                throw new RuntimeException(e);  
            }  
        });  
        // worker thread start  
        thread.start();  
    }  
  
    public static void stopServer() {  
        try {  
            serverSocket.close();  
            System.out.printf("server : close");  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

코드가 어렵진 않으니 간단하게만 설명하겠다. 기본적으로 클라이언트에서 데이터를 보내면 바이트 단위로 전달되는 해당 데이터를 버퍼를 이용하여 받아두도록 작성되어 있다. (만약 버퍼를 사용하지 않으면 바이트 하나하나 받아야 하므로 성능이 매우 느리다. 하지만 이 데이터를 메모리에 잠시 쌓아두면서 덩어리로 한번에 많은 양씩을 처리하게 되면 훨씬 성능이 빨라지게 된다. 즉, 버퍼는 CPU의 처리량을 끌어올리기 위해 고안된 아이디어로, 바이트 하나하나 디스크에 저장하거나 입출력하는 방식은 CPU 연산 성능을 최적으로 사용할 수 없어 메모리에 잠시 데이터를 보관하여 한번에 여러 데이터를 처리할 수 있도록 하는 방법을 의미한다.) 이렇게 받은 데이터를 매개값으로 하여 OutputStream의 `write()`메서드를 호출하면 echo 서비스를 구현할 수 있다. 위 코드에서는 설명과 조금 달리, 인코딩 과정을 좀 더 간편하게 처리하기 위해 `new DataInputStream()`을 이용하고 있다.


**TCP 기반 클라이언트**

```java
public class TCPClient {  
    public static void main(String[] args) {  
        try {  
            Socket socket = new Socket("localhost", 15000);  
  
            System.out.println("client : server connected!");  
  
            // send data  
            String data = "자바로 네트워크 이해하기!!!";  
            byte[] bytes = data.getBytes("UTF-8");  
            OutputStream outputStream = socket.getOutputStream();  
            outputStream.write(bytes);  
            outputStream.flush();  
            System.out.println("client : send data -> " + bytes);  
  
            // receive echo  
            DataInputStream dataInputStream = new DataInputStream(socket.getInputStream());  
            String echo = dataInputStream.readUTF();  
            System.out.println("client : receive echo -> " + echo);  
  
            socket.close();  
            System.out.println("client : close");  
  
        } catch (UnknownHostException e) {  
            // IP 주소 잘못된 경우  
            throw new RuntimeException(e);  
        } catch (IOException e) {  
            // 해당 포트에 연결이 어려운 경우  
            throw new RuntimeException(e);  
        }  
    }  
}
```

클라이언트도 서버와 유사하게 구성하고 있으므로, 자세한 설명은 생략한다.

위 코드를 통해 알 수 있는 건 다음과 같다.
- TCP는 연결을 수립한 뒤 데이터를 통신하는 프로토콜
- IP 주소와 포트 정보를 갖는 소켓을 이용하여 입출력을 통해 (논리적) 통신을 수행
- 통신은 바이트 단위로 이루어지는 것이 기본이며, 이러한 통신의 성능 향상을 위해 수신 측에서는 버퍼를 사용

## TCP의 흐름 제어, 혼잡 제어

TCP는 연결을 수립한 신뢰성 있는 통신 방식을 위해 만들어졌다. 데이터의 손실이나 순서의 보장을 위해 상대방과 연결을 수립하고, 통신 패킷들에 대한 상대 노드의 응답을 활용하여 전송 흐름 제어와 혼잡 제어를 수행한다.

### 흐름 제어

수신측의 데이터 처리 속도에 비해 송신측의 전송 속도가 빠르다면 문제가 발생한다. 수신 측에서 제한한 저장 용량을 초과한 이후에 데이터가 수신되면 손실이 발생할 수 있고, 이는 재요청을 해야 하므로 통신 오버헤드가 발생한다. 따라서 통신 흐름을 적절하게 가져가야 한다.

1. stop and wait : 패킷 하나하나 `ACK`(응답)을 받아야만 다음 패킷을 보내주는 방식. 이를 통해 수신 노드가 준비된 경우에만 데이터를 보내줄 수 있게 된다.
		![](https://mblogthumb-phinf.pstatic.net/MjAxODEwMjRfMjg5/MDAxNTQwMzQ4MTYwOTE4.Fdc3Xcf4InjBbUFrQe1m0A7iFSOrBGZLR42KhU7i6k8g.FEB9244sXrb1qleDgM9Ct2I1EARu46HkL7wKimJKu1Yg.PNG.nieah914/102418_0230_TCP1.png?type=w800)
2. sliding window : 수신 측에서 설정한 버퍼(윈도우)의 크기만큼 송신 측에서 `ACK`을 기다리지 않고 여러 세그먼트를 전송하는 pipelined protocols 방식이다.(버퍼 사이즈는 3-way handshake과정에서 수신 호스트의 receive window size에 송신 호스트가 window size를 맞추게 됨) `ACK`데이터를 카운트하여 window를 늘려가며 전송을 하게 된다. 즉, 일정 크기만큼은 응답을 기다리지 않고 계속 전송을 하며, 윈도우는 설정된 버퍼 사이즈보다 항상 작거나 같은 크기로 유지되면서 응답을 받는 개수만큼 윈도우를 늘려가며 데이터를 전송하는 방식으로 이어진다.
	
	![](https://t1.daumcdn.net/cfile/tistory/253F7E485715ED5F27)

	sliding windoe는 flow control을 수행할 때 다음 두 가지 방식으로 처리할 수 있다.
	
	- Go Back N : 기본적으로 수신 호스트는 cumulative ack만을(=최종으로 받은 패킷이 뭐다.라는 말 만을 할 수 있다.) 보낼 수 있다. 따라서 sender는 timer를 가지고 ack 수신 timeout이 발생하는 패킷부터 다시 "전부" 패킷을 전송하는 방식을 의미한다. 따라서 n개만큼 다시 돌아가서 보낸다는 의미로 go back n 이라고 칭한다.

	- Selective Repeat : 수신 호스트에서 각 수신 패킷에 대하여 individual ack을 보낼 수 있는 상황에서, sender host에서 timout이 발생한 패킷에 대하여만 패킷을 재전송해주는 방식을 말한다. 이는 두 호스트가 SACK(selective acknowledgement)을 지원해야 하며, TCP 헤더의 옵션 필드를 활용하여 SACK을 사용한다는 정보(option kind = 5)를 지정해야 하고, 손실된 패킷의 left edge, right edge 정보를 담아 보내어 이를 sender가 체크하여 재전송할 수 있게 한다.

### 혼잡 제어

세상에 호스트 단 두대만 통신을 하는 게 아니라 정말 수없이 많은 호스트가 거의 동시에 정말 많은 데이터를 통신하고 있는 상황에서, 만약 너무 빈번하게 패킷 재전송이 일어나는 경우가 발생하면 특정 라우터에 데이터는 계속 몰리게 될 거고, 이는 더 많은 재전송을 유발하는 등 극심한 통신 오버헤드를 유발할 수 있다. 이는 결국 오버플로우, 데이터 손실을 유발하므로, 상대방 호스트의 사정만을 고려하여 통신을 진행할 수 없다. 따라서 네트워크 혼잡을 고려하여 데이터 패킷 송신 속도를 제어해야 하며, 이를 혼잡 제어(Congestion control)라고 한다.

혼잡 제어를 구성하는 방식은 초기의 극단적인 방식인 Tahoe와 그를 개선한 Reno, 그리고 그 이후의 VEGAS, CUBIC, Hybla등 오늘날의 네트워크 환경에 맞게 더욱 개선된 여러 알고리즘이 존재한다. 이 글에서의 설명은 이 중에서 오늘날 congestion control의 가장 기본적인 형태인 Reno를 기준으로 설명한다.

혼잡 제어는 다음 3단계가 있다. 
- Slow start
- Congestion Avoidance
- Fast Retransmit
- Fast Recovery

![](https://t1.daumcdn.net/cfile/tistory/256E39425715F10103)

- slow start
통신을 시작하는 과정에서 패킷을 하나씩 보내면서, 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size(MSS)를 2배씩 늘려준다. 이렇게 전송 속도를 지수 함수 꼴로 증가시켜서 빠르게(exponentially fast) window size를 확보한다.

- Congestion Avoidance
윈도우 사이즈가 일정 크기가 되면(ssthresh ; slow start threshold) 혼잡 회피 단계로 지정하여, 윈도우 크기를 선형으로 증가시킨다. 가장 초기의 ssthresh는 slow start 단계를 충분히 확보하기 위해 큰 값으로 지정되며, 이후 congestion이 발생할 때마다 window size를 절반으로 줄이면서 ssthresh를 동적으로 조절하며 통신이 수행된다. 

- fast recovery
만약 송수신 과정에서 `ACK` 패킷 수신이 정상적으로 이루어진다는 시간을 초과하여 timeout이 발생하거나, 송신 과정에서 패킷이 유실되어 수신 측에서 특정 패킷이 아직 수신되지 않았다는 의미의 duplicate ack이 3번 연속으로 발생하는 경우를 네트워크 상태가 congestion에 있다고 판단한다. 이런 이벤트가 발생했을 때 송신 window size를 절반으로 줄이고 congestion avoidance로 다시 돌아가서 다시 window size를 선형으로 증가시키며 congestion을 감지하는 과정을 반복하는데, 이를 fast recovery라고 한다.

- fast retransmit
데이터를 송수신하는 과정에서 패킷 손실이 발생한 것에 대하여, 손실 패킷만을 재전송하는 것을 의미한다. 이는 flow control과 마찬가지로 초창기에는 go back n을 사용하였지만, selective repeat을 지원하는 호스트가 많아짐에 따라 오늘날에는 이를 주로 이용한다.

### 참고 : AIMD란?

위에서 설명한 Tahoe, 그리고 Reno는 AIMD라는 네트워크 혼잡 제어 방법의 기본 원칙을 의미합니다. 여기서 AIMD는 Additive Increase와 Multiplicative Decrease를 의미합니다.
- Additive Increase : 혼잡이 발생하지 않는다면 데이터의 전송 속도를 선형적으로 증가시킨다. (= 윈도우 사이즈를 선형적으로 증가시킨다.)
- Multiplicative Decrease : 패킷 손실을 감지하면 congestion이 있다고 판단하고, 윈도우 사이즈를 감축시켜서 전송 속도를 줄이는 기법을 의미합니다.

이는 Tahoe와 Reno라는 방식을 고안하는 데 기본이 된 원칙이며, 참고만 하면 될 듯 합니다.