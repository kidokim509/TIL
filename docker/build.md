## Docker Build시 네트워크 설정
Dockerfile에서 ENV로 http_proxy, no_proxy 설정하면 proxy 사용가능
기본적으로 Build시 Run 명령 호출에는 bridge 네트워크를 사용.
이 때문에 오늘 젠킨스 서버에서 Docker Build를 하면 Yum repository 접근이 되지 않음.
아마도 Docker Image의 bridged network에서는 Proxy나 Yum repository에서 접근을 막은 모양.
이럴 때는 docker build 명령의 --network host 옵션을 사용한다.
(RUN 명령 실행떄에만 host network 사용)
그렇게 하면 Docker Build를 실행하는 서버의 네트워크 환경을 그대로 적용할 수 있다.
