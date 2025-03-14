# veth를 이용한 nic 브릿지 구성

우리가 흔히 컨테이너를 얘기하면 대부분은 커널에 연결되는 소프트웨어 레이어에 대한 가상화를 의미하는 경우가 많다.

그렇지만 이런 소프트웨어 못지 않게 중요한것이 네트워크이다.

리눅스에서는 이를 위해 `Virtual Networking`기능을 지원해준다.

이러한 네트워크는 하기와 같다.

- Bridge

- Bounded Interface

- Team Device

- VLAN

- VETH

