# veth를 이용한 nic 브릿지 구성

리눅스의 경우 대부분의 하드웨어 자원은 커널을 통해서 접근 및 제어를 한다.

이는 api를 이용하여 커널을 제어하며 이러한 점을 이용하여 운영체제를 가상화하는 컨테이너 기술이 널리 이용된다.


네트워크 또한 이와 비슷하게 동작할 수 있다.

이는 Socket API를 통해 표준화된다고 한다.

<br>

Socket API에 해당하는 syscall의 경우 하기 링크에서 확인할 수 있으며

https://elixir.bootlin.com/linux/v6.13.7/source/include/linux/syscalls.h#L837

해당 함수는 아래에서 정의된다.

https://elixir.bootlin.com/linux/v5.13.9/source/net/socket.c


```
+-----------------------------------------------------+
|                    User Space                       |
|                                                     |
|  +----------------+                                 |
|  |   Socket API   |                                 |
|  +-------+--------+                                 |
|          |                                          |
+-----------------------------------------------------+
           |
           v
+-----------------------------------------------------+
|          |         Kernel Space                     |
|          |                                          |
|          |                                          |
|  +-------+--------+                                 |
|  | Network        |                                 |
|  | Stack          |                                 |
|  +-------+--------+                                 |
|          |                                          |
|  +-------+--------+                                 |
|  |   NIC          |                                 |
|  +----------------+                                 |
+-----------------------------------------------------+

```


이러한 네트워킹 기술을 리눅스에서는 `Virtual Networking`라고 하며 지원되는 네트워크는 하기와 같다.

- Bridge

- Bounded Interface

- Team Device

- VLAN

- VETH

# Reference 

> https://gist.github.com/mtds/4c4925c2aa022130e4b7c538fdd5a89f