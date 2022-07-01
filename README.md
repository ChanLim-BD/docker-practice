## Docker?
**<span style="color:red">컨테이너</span>를 사용하여 응용프로그램을 더 쉽게 만들고 배포하고 실행할 수 있도록 설계된 도구이며, <span style="color:red">컨테이너</span> 기반의 오픈소스 가상화 플랫폼이며 생태계입니다.
Go언어로 작성되며 리눅스 컨테이너 기반입니다.
**

---
## 가상화? 그거 왜 사용하는가?
서버 관리자 입장에서 CPU 사용률이 현저히 낮은 서버들을 그대로 두면 리소스 낭비, 돈 낭비입니다. 
그렇다고 해서 모든 서비스들을 한꺼번에 한 서버에 올린다면 그 서버가 박살날 가능성은 매우 높습니다.

그래서 **안정성을 높이며** **리소스도 최대한 활용할 수 있는 방안**으로 등장한 기술이 **서버 가상화**입니다.

**VM**이 대표적인 가상화 오픈소스입니다.

하이퍼 바이저 기반의 등장으로 가상화가 출현하였고, 
**하이퍼 바이저**는 호스트 시스템에서 다수의 게스트 OS를 구동할 수 있게 해주는 소프트웨어입니다.
하이퍼 바이저가 하드웨어를 직접 제어하기에 자원 효율적으로 사용 간으하며, 별도의 호스트 OS가 없으므로 오버헤드가 적습니다.

---

## Container?
![](https://velog.velcdn.com/images/chan9708/post/3fcf7b1d-eaf0-4705-964f-bfc8ae767a9a/image.png)

일반적으로 **컨테이너**를 떠올리면 위의 사진을 떠올립니다.

위의 '컨테이너'의 역할은 무엇일까요?
: **'물건을 넣고 다양한 운송수단으로 쉽게 옮길 수 있는 도구'**입니다.

: 컨테이너라 하면 배에 실는 네모난 화물 수송용 박스를 생각할 수 있는데 각각의 컨테이너 안에는 옷, 신발, 전자제품, 술, 과일등 다양한 화물을 넣을 수 있고 규격화되어 컨테이너선이나 트레일러등 다양한 운송수단으로 쉽게 옮길 수 있습니다.


![](https://velog.velcdn.com/images/chan9708/post/ce2bafc6-2b63-47c0-99d5-94141d4502f5/image.png)

그렇다면, **서버(도커)에서의 컨테이너의 개념?**
: **하나의 운영 체제** 안에서 **커널을 공유**하며 **개별적인 실행 환경을 제공**하는 **격리된 공간**입니다.
: **다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 합니다.**
: 일반 컨테이너의 개념에서 물건을 손쉽게 운송해주는 것처럼 프로그램을 손쉽게 이동, 배포, 관리할 수 있게 합니다.

---

## VM과 차이
>
![](https://velog.velcdn.com/images/chan9708/post/8465eadc-df45-463e-9c05-ba893b929f67/image.png)
>
우리에게 익숙한 VMware나 VirtualBox같은 **가상머신은 호스트 OS위에 게스트 OS 전체를 가상화하여 사용하는 방식**입니다. 
이 방식은 여러가지 _**OS를 가상화(리눅스에서 윈도우를 돌린다던가) 할 수 있고 비교적 사용법이 간단하지만 무겁고 느려서 운영환경에선 사용할 수 없었습니다.**_
>
전가상화든 반가상화든 추가적인 OS를 설치하여 가상화하는 방법은 어쨋든 성능문제가 있었고 이를 개선하기 위해 **프로세스**를 격리 하는 방식이 등장합니다.
>
리눅스에서는 이 방식을 **리눅스 컨테이너**라고 하고 **단순히 프로세스를 격리시키기 때문에 가볍고 빠르게 동작합니다.**
CPU나 메모리는 딱 프로세스가 필요한 만큼만 추가로 사용하고 성능적으로도 거의 손실이 없습니다.

즉,

하나의 서버에 여러개의 컨테이너를 실행하면 **서로 영향을 미치지 않고 독립적으로 실행되어 마치 가벼운 VM(Virtual Machine)을 사용하는 느낌**을 줍니다. 

**CPU나 메모리 사용량을 제한할 수 있고 호스트의 특정 포트와 연결하거나 호스트의 특정 디렉토리를 내부 디렉토리인 것처럼 사용할 수도 있습니다.**

>**공통점**
: 도커 컨테이너와 가상 머신은 기본 하드웨어에서 격리된 환경 내에 애플리케이션을 배치하는 방법입니다.
>**차이점**
: 격리된 환경을 얼마나 격리를 시키는지의 차이입니다.

---
## 컨테이너 격리?
: **Cgroup(Control groups)과 Namespace(네임스페이스)가 기능을 합니다.**
:: **컨테이너와 호스트에서 실행되는 다른 프로세스 사이에 벽을 만드는 리눅스 커널 기능들입니다.**

* **C Group**
: CPU, 메모리, Network Bandwidth, HD I/O 등 프로세스 그룹의 시스템 리소스 사용량을 관리합니다.

* **Namespace**
: 하나의 시스템에서 프로세스를 격리시킬 수 있는 가상화 기술입니다.
: 별개의 독립된 공간을 사용하는 것처럼 격리된 환경을 제공하는 경량 프로세스 가상화 기술입니다.

**하지만 얘네들은 Linux 환경에서 실행되는데, 왜? 어떻게?**
### 사실은,
![](https://velog.velcdn.com/images/chan9708/post/a6e20d48-9492-41ab-9b2d-725cba82933c/image.png)

: 우리의 OS 위에 Linux VM이 실행된다.


---

## Docker를 사용하는 이유?
**어떤 프로그램을 다운로드하는 과정을 굉장히 간단하게 만들기 위해서입니다.**
: 도커를 이용해여 프로그램을 설치하면 예상하지 못한 에러도 덜 발생하고, 설치하는 과정도 매우 간단해지는 것을 볼 수 있습니다.

---

## Image
![](https://velog.velcdn.com/images/chan9708/post/31d97897-a801-461d-977f-d138a863c995/image.png)

**이미지**는 코드, 런타임, 시스템 도구, 시스템 라이브러리 및 설정과 같은 **응용 프로그램을 실행하는 데 필요한 모든 것을 포함하는 가볍고 독립적이며 실행 가능한 소프트웨어 패키지**입니다.

이미지는 **컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것**으로 **상태값을 가지지 않고 변하지 않습니다.**
컨테이너는 이미지를 실행한 상태라고 볼 수 있고 추가되거나 변하는 값은 컨테이너에 저장됩니다.

같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 이미지는 변하지 않고 그대로 남아있습니다.

말그대로 이미지는 컨테이너를 실행하기 위한 <span style="color:red">**모든 정보**</span>를 가지고 있기 때문에 **더 이상 의존성 파일을 컴파일하고 이것저것 설치할 필요가 없습니다.** 
**새로운 서버가 추가되면** 미리 만들어 놓은 **이미지를 다운받고 컨테이너를 생성**만 하면 됩니다. 
**한 서버에 여러개의 컨테이너를 실행할 수 있고, 수십, 수백, 수천대의 서버도 문제없습니다.**

---
## 인기가 넘치는 이유?
* **레이어 저장방식**
: 효율적으로 이미지 관리 가능
* **이미지 경로**
: url방식으로 관리하며 태그를 붙일 수 있다.
* **Dockfile**
* **Docker Hub**
* **Command와 API**
* **유용한 새로운 기능들**
* **훌륭한 생태계**
* **커뮤니티 지원**
* **귀여운 케릭터**

---

## Reference
>
[초보를 위한 도커 안내서](https://www.inflearn.com/course/%EB%8F%84%EC%BB%A4-%EC%9E%85%EB%AC%B8?inst=446961aa)
>
[따라하며 배우는 도커와 CI환경](https://www.inflearn.com/course/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%8F%84%EC%BB%A4-ci/dashboard)
