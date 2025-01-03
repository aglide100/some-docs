# K8S mutating

Otel Operator을 이용하다보면 Auto Instrument라는 계측기를 자동으로 주입해주는 기능이 있다.

문자 그대로 pod injecting을 통해서 otel agent를 넣는데 metadata의 annotation만 넣어주면 되는 신기한 기능이다.


이는 코드를 보면 알 수 있지만 Mutating webhook을 이용한 Pod injecting이라는 것을 알 수 있다.

