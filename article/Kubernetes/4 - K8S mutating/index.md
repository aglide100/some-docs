---

Otel operator에서 제공하는 Auto Instrunment는 Mutating Webhook을 이용한 Pod injecting임

이는 kubernetes에서 리소스 관리는 Reconsile을 통해서 수행하는데 해당 행동은 대부분 API 요청을 통해서 작업이 처리됨

이중 Muatating admission부분이 존재하는데 해당 event의 webhook을 통해서 pod injecting을 구현할 수 있다고 한다.

구조도는 아래와 같음

간략하게 flow를 설명하자면

1. request가 etcd에 등록되기 전 MutatingWEbhookConfiguration의 rule과 일치할경우 intercept를 한다.
2. MutatingWebhook은 webhook server에 reuqest를 review하는 과정을 가지고 mutating을 수행한다.

약 3년정도 되었으나 해당 요소에 대한 예제:

<https://github.com/morvencao/kube-sidecar-injector>

---

## opentelemetry-operator내에서 podmutator 코드 분석

webhook의 경우 podMutationWebhook의 struct의 임베딩된 Handle함수에서 처리됨

해당 함수는 controller의 reconsile함수와 마찬가지로 이벤트에 따라 발생되며 Handle의 경우 아직 살펴보지는 않았으나 위의 그림처럼 mutating하는 과정에서 발생할 것으로 추측됨.

아무튼  podMutationWebhook struct는 아래와 같음

여기서 client는 sigs.k8s.io/controller-runtime/pkg/client이며 이는 operator-sdk에서 사용하는 client와 동일한 라이브러리이다.

컨트롤러 구조상 루프시에 pull방식으로 동작하기에 비동기는 아니라고 추측됨(mutax관련 코드가 없음)

따라서 mutating되는 과정에서 Handle을 호출하게 되고 임베딩된 p의 podMutator함수에 따라 아래 비즈니스 로직이 동작한다는 것을 알 수 있음

따라서 93라인의 pod, err = m.Mutate(ctx, ns, pod)에서 관련 정보를 넘기고 pod정보에서 metadata의 annotation을 파싱후의 Instrument코드를 실행하게 됨

아래는 Mutate코드임,  임베딩된 pm에서 Injecting을 할 것인지 체크함

만약 return되지 않고 왔다면 pm.sdkInjector.inject를 통해서 pod의 내용을 수정함.

수정되는 내용은 initContainer추가, java라면 환경변수 주입정도가 있음

아래는 inject함수임. 그렇게 복잡한 내용은 없고 특이사항이라면 pod 내용을 수정하고 해당 pod객체를 리턴한다는 것을 알 수 있음

여기까지가 opentelemetry-operator에 작성된 Instrument에 관한 내용을 살펴보았습니다.