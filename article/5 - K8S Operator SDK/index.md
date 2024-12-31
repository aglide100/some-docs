# Operator SDK

쿠버네테스에서 CRD을 생성하기 위해서는 Kustomize, Controller등 여러 요소등을 개발하여야 하는데 이를 한번에 해줄 수 있게 도와주는것이 바로 Operator SDK이다.


# 준비

설치는 굉장히 간단하다. macos라면 brew를 통해 설치가 가능하다.

```shell
brew install operator-sdk
```

operator-sdk를 설치한다고 해도 끝은 아니고...의존성을 설치하여야 한다.

우서 kublet과 golang을 설치한다.

```shell
brew install kublet
```

```shell
brew install golang
```

golang을 설치 후 아래와 같이 GOPATH를 설정한다. 통상적으로는 유저 디렉터리 go디렉터리를 생성 후 지정한다.

해당 디렉터리는 golaps 백엔드를 통해서 받아오는 파일등 여럿 요소들이 저장되는 디렉터리이다.

그 이외의 GOROOT, GOBIN등은 homebrew를 통해서 설치되니 큰 고민없이 사용하면 된다.

```shell
export GOPATH="/Users/user/go"
export PATH=${PATH}:$GOPATH/pkg
export PATH=${PATH}:$GOPATH/bin
```

# Init

기본 준비가 되었다면 프로젝트를 생성할 것이다.

```shell
mkdir ./sample-project

cd ./sample-project
```

생성이 되었다면 go init를 통해 golang 프로젝트임을 선언한다.

```shell
go mod init gitlab.like-a-junk.com/sample-project
```

```shell
operator-sdk init --domain gitlab.like-a-junk.com --project-name sample-project --owner "aglide"
```

