# -현장실습-
현장실습 때 작성했던 SQL문들 입니다.<br>
Oracle_Architecture 에는 오라클 아키텍쳐에 관한 설명을 들을때
작성했던 txt파일들 입니다.


개발인원 : 201744076 김지용<br>

사용환경 <br>
1.vmware <br>
2.xshell <br>
3.orange <br>
4.centos <br>

vmware에 centos7을 깔아 가상환경을 구축 <br>
가상환경이 구축된 상태에서 xshell 직접 os를 컨트롤하는게 아닌<br>
xshell로 간접적인 컨트롤를 하여 오류가 뜨더라도 os는 안전한 환경을 구축<br>
xshell에 orange를 실행해 편하게 SQL문을 짤수 있는 환경구축<br>



#오라클 아키택쳐<br>
오라클 프로그램이 내부에서 어떻게 동작하는지 <br>
오라클 아키택쳐를 배움으로서 이해하게됨<br>
오라클 프로그램이 돌아가는 환경은 크게 3가지로 분류됨<br>

프로세스, 메모리 ,파일 로 구분됨<br>

프로세스<br>
프로세스는 말그대로 오라클의 프로세스들을 뜻함<br>
프로세스도 크게 분류하면 3가지 부분으로 분류되는데,<br>
유저 , 서버, 백그라운드로 분류됨 <br>

유저는 말그대로 유저 클라이언트를 뜻하여 <br>
유저가 다루는 부분이라 아키택쳐에선 그냥 있다고만 묘사됨<br>
(개개인이 다른 환경을 구축하는 상황에서 오라클은 오는 데이터만 잘 리턴해주면 된다.)<br>

서버는 이제 유저가 보낸 데이터들을 처리하는 실질적인 단계로<br> 
유저가 SQL문을 보내면
