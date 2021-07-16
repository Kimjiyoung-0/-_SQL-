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



# 오라클 아키택쳐<br>
오라클 프로그램이 내부에서 어떻게 동작하는지 <br>
오라클 아키택쳐를 배움으로서 이해하게됨<br>
오라클 프로그램이 돌아가는 환경은 크게 3가지로 분류됨<br>

프로세스, 메모리 ,파일 로 구분됨<br>

## 프로세스<br>
프로세스는 말그대로 오라클의 프로세스들을 뜻함<br>
프로세스도 크게 분류하면 3가지 부분으로 분류되는데,<br>
유저 , 서버, 백그라운드로 분류됨 <br>

### 유저
유저는 말그대로 유저 클라이언트를 뜻하여 <br>
유저가 다루는 부분이라 아키택쳐에선 그냥 있다고만 묘사됨<br>
(개개인이 다른 환경을 구축하는 상황에서 오라클은 오는 데이터만 잘 리턴해주면 된다.)<br>

### 서버
서버는 이제 유저가 보낸 데이터들을 처리하는 실질적인 단계로<br> 
유저가 SQL문을 보내면 메모리에 엑세스하여 처리하는 프로세스이다.<br>
서버 프로세스에는 PGA라는 메모리영역이 있는데 이것은 후술한다.<br>

### 백그라운드 프로세스
백그라운드 프로세스는 오라클 인스턴스내부에 존재하는 <br>
사용자에게 노출되지않는 프로세스들을 뜻함 <br>
PMON, SMON, CKPT, DBWR, LGWR, ARCH 등이 있음<br>

#### PMON
PMON은 프로세스들을 관리하는 역할이다.<br>
프로세스들을 모니터링 하다가 프로세스의 비정상적인 종료가<br>
감지되면 그 프로세스를 재가동, 또는 종료 시키고,<br>
그 프로세스로 인한 lock(Database으 무결성을 위한 Database 제한 방법)을 해제 한다.<br>

#### SMON
SMON은 복구 담당으로 <br>
오라클의 데이터베이스가 일치하지 않을때<br>
그것을 복구 시키는 역할<br>

#### CKPT
CKPT 체크포인트 이벤트를 발생<br>
체크포인트는 오라클의 빠른커밋 매커니즘과 연관이있다.<br>
오라클은 빠른커밋 매커니즘으로 DML명령이 들어오고 커밋되면(데이터가 수정되면)<br>
일단 로그만 Redo log file에 반영한다. 그리고 Data는 나중에 반영하게 되는데, <br>
일정시간이 지나거나, 메모리의 공간이 부족하여 Datafile에 변경된 데이터를 반영하는게<br>
바로 체크포인트 이벤트이다. <br>
이때 체크포인트 이벤트를 발생 시킬때 <br>
SCN 를 제대로 체크하여 LOG파일과 DATA파일의 싱크를 맞춰준다.<br>

#### DBWR
DBWR는 datafile에 data를 작성한다.<br>
실질적으로 테이블을 수정, 조작하는 역할이다.<br>

#### LGWR
LGWR는 Redolog file에 로그를 기록한다 <br>
테이블에 데이터는 반영이 안되도 로그가 남아있으면,<br>
로그를 복기하여 데이터를 복구 해낼수 있다.<br>
(로그에는 그동안 입력되있던 SQL문의 기록이 있기 때문에 <br>
SQL문의 기록을 복기하면 결국 최종적으로 데이터를 복구해 낼수 있다.)<br>

## 메모리
메모리는 크게 PGA ,SGA로 나뉜다.<br>

### PGA
PGA는 서버 프로세스에 속해 각 유저별로 따로 생성되는 영역이다.<br>
PGA는 유저 한명의 SORT,JOIN 데이터를 담고있다.<br>
(SORT,JOIN 데이터는 각각 다르게 쓰기때문)<br>

### SGA
SGA는 오라클 인스턴스에 있는 영역으로 <br>
이 영역은 모든 유저들이 공유해서 쓰는 공간이다.<br>
SGA는 Shared pool, Buffer Cache, Redo log Buffer Cache로 나뉜다.<br>
#### Shared pool
Shared pool은 또 Library Cache, Dictionary Cache로 나뉜다.<br>
##### Library Cache
Library Cache는 SQL의 과정 Plan을 저장한다.<br>
plan을 저장했다가 유저가 SQL을 입력하고,<br>
SQL의 오류가 없다고 판단되면 <br>
라이브러리 캐쉬의 plan을 뒤져본다.<br>
이때 plan에 똑같은 SQL문의 과정이 있으면<br>
그 Plan을 사용해 메모리를 아낀다.<br>
(이과정을 소프트 파싱이라 한다.)<br>

##### Dictionary Cache
Dictionary Cache는 유저한테 노출되지 않는<br>
테이블의 아이디 , 유저들의 권한 등을 가지고 있다.<br>
유저가 SQL문을 입력하면 딕셔너리 캐시에 있는 ,<br>
테이블 아이디, 유저 권한등을 프로세스가 참고해<br>
이 유저가 SQL문을 사용할 수 있는 지 확인한다.<br>
(유저의 권한이 적합하지 않으면 캔슬)<br>

#### Buffer Cache
Buffer Cache는 Datafile에 저장되있는 실질적인 테이블 데이터들을<br>
보관해 사용하는 메모리 영역이다. 이 영역의 최소단위는 <br>
Data block으로 유저가 SQL문을 입력하고 소프트 파싱이 안될때<br>
서버프로세스가 이 영역에 Data들을 올려 처리한다. <br>
이 영역은 세가지 상태가 있는데 Dirty, Pined, Clean 이다.<br>
Clean은 비어있는 데이터 블록을, <br>
Pined는 사용중인 데이터 블록을, <br>
Dirty는 사용하고 데이터가 남아있는 블록을 의미한다.<br>
만약 메모리 영역이 모자르거나, 일정시간이 지나면 <br>
DBWR가 데이터들을 Datafile에 반영하여 <br>
Dirty 블록들을 Clean으로 처리한다. <br>




