# Kubernets PSA와 PSS

Kubernets v1.22[stable] 버전부터 PSA라는 새로운 컨트롤러가 내장된다.

이와 비슷한 역할을 수행하던 Pod Security Policy (PSP)는 1.21부터 deperecated가 된다.


# PSA(Pod Security Admission)

Kubernets의 offers는 빌트인된 Pod Security Controller에게서 Pod Security Standards를 강제된다.

Pod의 보안 제한의 경우는 namesapce레벨부터 적용이 된다.


각 레벨은 아래와 같이 분리가 됨

Mode|Descriptioon|
|--|--|
enforce|Policy violations will cause the pod to be rejected|
audit|	Policy violations will trigger the addition of an audit annotation to the event recorded in the audit log, but are otherwise allowed.|
warn|Policy violations will trigger a user-facing warning, but are otherwise allowed.|

이와 같은 레벨을 통해 PSS가 정의가 되며 일를 통해 Pod에 대해서 일돤된 보안 수준을 유지하고 일반적인 보안 문제를 방지한다고 한다.

PSS는 Kubernets 클러스터 관리자에 의해 정책이 구성되며 강제 될 수 있으며, Pod의 보안 구성을 검사하여 이에 적합하지 않는다면 이를 거부할 수 있다.



# PSS (Pod Security Standards)

PSS의 경우 이름 그대로 Standards로서 보안 표준을 정의한다.

이러한 PSS는 Kubernetes v.1.23부터 기본적으로 활성화가 되며 PSA를 통해 PSS를 구현한다고 한다.


# Pod Security Admission labels


PSA의 사용방법은 Label을 통해 namespace에 지정하게 된다.

아래는 간단한 적용에 대한 예시이다.

```
pod-security.kuvernets.io/<MODE>: <LEVEL>

or

pod-security.kubernets.io<MODE>-version: <VERSION>
```


# Reference 

> https://kubernetes.io/docs/concepts/security/pod-security-admission/