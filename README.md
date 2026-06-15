# Auto-Ops Platform

## 개요
4대의 클라우드 서버에 Docker 기반 인프라 자동화 플랫폼을 구축한 프로젝트입니다.
GitHub Actions + Ansible을 통한 CI/CD 파이프라인과 Prometheus/Grafana 모니터링, Loki 로그 수집, n8n 워크플로우 자동화를 구현했습니다.

## 아키텍처
GitHub → GitHub Actions → node-04 (Ansible) → 각 노드 자동 배포

↓

node-01 (Prometheus + Grafana + Alertmanager)

node-02 (Loki + Promtail)

node-03 (웹앱 + Node Exporter + cAdvisor)

node-04 (n8n + Node Exporter)
## 서버 구성

| 노드 | 역할 | 주요 서비스 |
|------|------|------------|
| node-01 | 모니터링 | Prometheus, Grafana, Alertmanager |
| node-02 | 로그 수집 | Loki, Promtail |
| node-03 | 앱 타겟 | Nginx, Node Exporter, cAdvisor |
| node-04 | 자동화 허브 | n8n, Ansible, Node Exporter |

## 기술 스택

- **Cloud**: NHN Cloud (Ubuntu 22.04 LTS)
- **Container**: Docker, Docker Compose
- **CI/CD**: GitHub Actions, Ansible
- **Monitoring**: Prometheus, Grafana, Alertmanager
- **Logging**: Loki, Promtail
- **Automation**: n8n
- **Network**: VPC, Security Group

## 주요 기능

### 1. CI/CD 파이프라인
- GitHub main 브랜치 push 감지
- GitHub Actions에서 node-04로 SSH 접속
- Ansible Playbook으로 4대 노드 자동 배포

### 2. 모니터링
- Prometheus로 4대 노드 메트릭 수집 (15초 간격)
- Grafana Node Exporter Full 대시보드로 시각화
- CPU, 메모리, 디스크, 네트워크 트래픽 모니터링

### 3. 알림 자동화
- Alertmanager 장애 감지 (CPU 80%, 메모리 80%, 디스크 80%, 컨테이너 다운)
- Gmail SMTP 이메일 알림
- n8n Webhook 수신 → 이메일 자동 발송 워크플로우

### 4. 로그 수집
- Promtail로 시스템 로그 및 Docker 컨테이너 로그 수집
- Loki로 로그 저장 및 쿼리
- Grafana에서 LogQL로 로그 조회

## 보안 구성

- **sg-external**: 외부 접근 허용 (node-01, node-04)
  - SSH 22, Grafana 3000, n8n 5678 → 관리자 IP만 허용
- **sg-internal**: 노드 내부 통신 (전체 노드)
  - VPC 사설 대역 192.168.0.0/24 전체 허용

## 디렉토리 구조## 서버 구성

| 노드 | 역할 | 주요 서비스 |
|------|------|------------|
| node-01 | 모니터링 | Prometheus, Grafana, Alertmanager |
| node-02 | 로그 수집 | Loki, Promtail |
| node-03 | 앱 타겟 | Nginx, Node Exporter, cAdvisor |
| node-04 | 자동화 허브 | n8n, Ansible, Node Exporter |

## 기술 스택

- **Cloud**: NHN Cloud (Ubuntu 22.04 LTS)
- **Container**: Docker, Docker Compose
- **CI/CD**: GitHub Actions, Ansible
- **Monitoring**: Prometheus, Grafana, Alertmanager
- **Logging**: Loki, Promtail
- **Automation**: n8n
- **Network**: VPC, Security Group

## 주요 기능

### 1. CI/CD 파이프라인
- GitHub main 브랜치 push 감지
- GitHub Actions에서 node-04로 SSH 접속
- Ansible Playbook으로 4대 노드 자동 배포

### 2. 모니터링
- Prometheus로 4대 노드 메트릭 수집 (15초 간격)
- Grafana Node Exporter Full 대시보드로 시각화
- CPU, 메모리, 디스크, 네트워크 트래픽 모니터링

### 3. 알림 자동화
- Alertmanager 장애 감지 (CPU 80%, 메모리 80%, 디스크 80%, 컨테이너 다운)
- Gmail SMTP 이메일 알림
- n8n Webhook 수신 → 이메일 자동 발송 워크플로우

### 4. 로그 수집
- Promtail로 시스템 로그 및 Docker 컨테이너 로그 수집
- Loki로 로그 저장 및 쿼리
- Grafana에서 LogQL로 로그 조회

## 보안 구성

- **sg-external**: 외부 접근 허용 (node-01, node-04)
  - SSH 22, Grafana 3000, n8n 5678 → 관리자 IP만 허용
- **sg-internal**: 노드 내부 통신 (전체 노드)
  - VPC 사설 대역 192.168.0.0/24 전체 허용

## 디렉토리 구조

auto-ops-platform/

├── .github/

│   └── workflows/

│       └── deploy.yml

├── ansible/

│   ├── inventory.ini

│   └── playbook.yml

└── docker/

├── node-01/

│   ├── docker-compose.yml

│   ├── prometheus.yml

│   ├── alertmanager.yml

│   └── alert_rules.yml

├── node-02/

│   ├── docker-compose.yml

│   ├── loki.yml

│   └── promtail.yml

├── node-03/

│   └── docker-compose.yml

└── node-04/

└── docker-compose.yml
