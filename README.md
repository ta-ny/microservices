# Microservice Deployment Configurations

이 저장소는 마이크로서비스 애플리케이션을 세 가지 방식으로 배포하는 예제입니다.

## 구조

```
microservice/
├── git-repo/          # Plain Kubernetes manifest
├── helm-chart/        # Helm Chart
└── kustomize/         # Kustomize
```

## 1. Git-Repo 방식

가장 기본적인 Kubernetes manifest 파일

### 사용법

```bash
kubectl apply -f git-repo/microservice.yaml
#kubectl apply -f ../microservice/git-repo/microservice.yaml
kubectl get pods
kubectl port-forward svc/apache 8080:80
```

## 2. Helm Chart 방식

템플릿화된 Chart로 재사용 가능

### 사용법

```bash
# 설치 (release 이름은 자유롭게 지정 가능)
helm install microservice-demo helm-chart/

# 확인
helm list
kubectl get pods

# 업그레이드 (values.yaml 수정 후)
helm upgrade microservice-demo helm-chart/

# 삭제
helm uninstall microservice-demo

# Package 생성 (.tgz 파일 생성됨)
helm package helm-chart/
# 결과: microservice-demo-1.0.0.tgz
```

## 3. Kustomize 방식

환경별로 다른 설정 적용

### 사용법

```bash
# Base 적용
kubectl apply -k kustomize/base/

# Dev 환경
kubectl apply -k kustomize/overlays/dev/

# Prod 환경
kubectl apply -k kustomize/overlays/prod/
```

## 접근 방법

### 로컬 Port Forward

```bash
kubectl port-forward svc/apache 8080:80
```

브라우저에서 `http://localhost:8080` 접속

## 서비스 구성

- **apache**: 프론트엔드 (포트 80)
- **catalog**: 카탈로그 서비스 (포트 8080)
- **customer**: 고객 서비스 (포트 8080)
- **order**: 주문 서비스 (포트 8080)
