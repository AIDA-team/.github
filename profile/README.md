# AIDA - AI-Driven Intrusion Detection & Analysis
<img width="1656" height="868" alt="image" src="https://github.com/user-attachments/assets/a11515b0-ac19-4659-9804-fab1b0738cd2" />

> XGBoost 기반 네트워크 침입 탐지 시스템 (NIDS)
> AI 기반 공격 분석 및 자동 대응 방안 제시

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0.0-green.svg)](https://flask.palletsprojects.com/)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0.0-orange.svg)](https://xgboost.readthedocs.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

네트워크 패킷을 실시간으로 분석하여 **9가지 공격 유형**을 탐지하고, Feature 기반 판단 근거와 함께 AI 기반 위협 평가 및 대응 방안을 자동으로 제공하는 **지능형 네트워크 침입 탐지 시스템(Intelligent Network Intrusion Detection System)** 입니다.
</br></br>

## 주요 기능

### 1. 다중 공격 유형 탐지
UNSW-NB15 데이터셋 기반으로 9가지 공격 유형을 실시간 분류:

- **DoS** - 서비스 거부 공격 (심각도: 8/10)
- **Exploits** - 소프트웨어 취약점 악용 (심각도: 10/10)
- **Backdoors** - 시스템 비인가 접근 (심각도: 9/10)
- **Shellcode** - 원격 코드 실행 (심각도: 10/10)
- **Worms** - 자가 복제형 네트워크 전파 (심각도: 9/10)
- **Generic** - 암호화 취약점 공격 (심각도: 7/10)
- **Fuzzers** - 프로토콜 교란 공격 (심각도: 6/10)
- **Reconnaissance** - 정보 수집 및 스캐닝 (심각도: 5/10)
- **Analysis** - 포트 분석 및 정보 수집 (심각도: 3/10)

</br>

### 2. Feature 기반 지능형 분석
42개의 네트워크 특성을 종합 분석하여 **구체적인 탐지 근거**를 자동 생성:

```
판단 근거 예시 (DoS 공격)
- 패킷 분석: 전송 패킷 수 1,234개, 총 2,456,789 bytes 전송됨
- 비정상적으로 높은 패킷 전송률 감지 (초당 1,234 packets)
- 소스 로드 52,340 bytes/sec로 대역폭 소진 공격 판단
- 단일 소스에서 집중적 Flooding 패턴 확인
```

주요 분석 Feature:
- 패킷 전송률 (`packet_rate`)
- 네트워크 부하 (`src_load`, `dst_load`)
- 연결 지속 시간 (`duration`)
- 패킷 지터 (`jitter`)
- TCP RTT (`tcp_rtt`)
- 서비스별 연결 수 (`service_connections`)

</br>

### 3. 자동 모델 선택 시스템
10일간의 점진적 학습을 통해 **최적의 모델을 자동으로 선택**:

```python
# 자동 선택 기준: FPR + FNR 최소화
best_score = FPR + FNR
if new_model_score < best_score:
    replace_best_model(new_model)
```

**실제 성능 지표** (`logs/model_history.json`):
- **Best Model: Day9**
  - FPR (오탐률): 4.97%
  - FNR (미탐률): 4.83%
  - Accuracy: 83.57%
  - Total Error: 9.80% (가장 낮음)

</br>

### 4. 실시간 성능 추적
일별 탐지 성능을 실시간으로 모니터링:

- **FPR** (False Positive Rate): 오탐률 추적
- **FNR** (False Negative Rate): 미탐률 추적
- **Detection Rate**: 실제 공격 탐지율
- **Accuracy**: 전체 분류 정확도

</br>

### 5. 시각화 대시보드
- **10일 캘린더**: 일별 공격 수 및 평균 심각도 시각화
- **실시간 공격 분석**: 공격 유형, 패킷 정보, AI 분석 결과
- **통계 차트**: 공격 유형 분포 (Doughnut), 심각도 분포 (Bar)
- **성능 메트릭**: FPR/FNR 실시간 추이 그래프

</br>

### 6. AI 기반 대응 방안
공격 유형별로 즉시 실행 가능한 구체적인 대응 방법 제시:

```
DoS 공격 대응 방안
1. DDoS 방어 솔루션 즉시 가동, 클라우드 트래픽 분산 활용
2. Rate Limiting 적용 (단일 IP당 요청 제한)
3. 공격 출발지 IP 자동 차단 규칙 적용
4. ISP 협조 요청으로 upstream 차단
```
</br></br>


## UX/UI
<img width="1276" height="813" alt="image" src="https://github.com/user-attachments/assets/bdf84bb7-85ba-4098-91bd-e0f9e535d61b" />
</br></br>


## Architecture Diagram
<img width="1220" height="350" alt="image" src="https://github.com/user-attachments/assets/548b1153-4eec-412d-97dd-5dbfbaa72a1f" />

- **NIDS는 보조 시스템(Subsystem)으로** 네트워크 침입을 감지하고 알림하는 서비스
- 이를 위해 사용자의 요청(Network Traffic)을 **복사(Mirroring)하여 NIDS로 전송**
    - **L3 스위치** : Packet 레벨의 이상 탐지가 필요한 경우 사용, ex) `DDoS` 등
    - **L7 로드밸런서** : HTTP 레벨의 이상 탐지가 필요한 경우 사용, ex) `XSS`, `CSRF` 등
</br></br>

## API Sequence Diagram
<img width="1533" height="952" alt="image" src="https://github.com/user-attachments/assets/eb030d58-e406-430f-913d-bfeb2175393e" />

- **1️⃣ 모델 학습 프로세스**
    - 복사된 네트워크 패킷 데이터를 바탕으로 **전처리를 진행,** 학습을 위한 데이터로 변환
    - 전처리된 데이터를 **DB에 저장**
    - DB에 저장된 데이터를 통해 **모델을 학습**
- **2️⃣ 침입 탐지 프로세스**
    - 사용자의 실시간 요청 데이터를 복사하여 NIDS로 전달 받음
    - 분류를 위한 전처리 과정을 수행
    - 침입 탐지 과정을 수행 (분류, Detection)
    - 공격으로 분류되었다면, LLM(Chat GPT)으로 전달하여 대처 방안에 대한 응답을 생성
    - 관리자 페이지로 전달하여 알림(Alert)
</br></br>

## 프로젝트 구조

```
nids/
├── backend/
│   ├── api.py                    # Flask REST API 서버
│   ├── attack_analyzer.py        # Feature 기반 공격 분석 엔진
│   └── packet_provider.py        # 실제 패킷 샘플링 제공자
├── frontend/
│   ├── index.html                # 사용자 대시보드
│   ├── login.html                # 로그인 페이지
│   ├── app.js                    # 대시보드 로직 (Chart.js)
│   └── style.css                 # UI 스타일
├── admin/
│   └── index.html                # 관리자 패널 (공격 시뮬레이션)
├── models/
│   ├── xgboost_model.pkl         # XGBoost 모델
│   ├── label_encoder.pkl         # 레이블 인코더
│   └── best_model.pkl            # 자동 선택된 최적 모델
├── logs/
│   └── model_history.json        # 모델 학습 히스토리
├── train_day_models.py           # Day1~Day10 모델 학습 스크립트
├── train_optimized_model.py      # 최적화된 모델 학습
├── requirements.txt              # Python 의존성
└── README.md
```

## 빠른 시작

### 1. 필수 요구사항

- Python 3.11+
- pip

### 2. 설치

```bash
# 저장소 클론
git clone https://github.com/your-username/nids.git
cd nids

# 가상환경 생성 (권장)
python -m venv nids
source nids/bin/activate  # Mac/Linux
# nids\Scripts\activate   # Windows

# 의존성 설치
pip install -r requirements.txt
```

### 3. 모델 학습 (선택사항)

```bash
# 최적화된 단일 모델 학습
python train_optimized_model.py

# 또는 Day1~Day10 점진적 학습
python train_day_models.py
```

**Note**: 사전 학습된 모델이 `models/` 디렉토리에 포함되어 있으므로 즉시 실행 가능합니다.

### 4. 서버 실행

**방법 1: 통합 실행 스크립트** (권장)
```bash
./run.sh  # Mac/Linux
```

**방법 2: 수동 실행**
```bash
# Backend API 서버 (터미널 1)
cd backend
python api.py
# → http://localhost:5001

# Frontend 서버 (터미널 2)
cd frontend
python -m http.server 8000
# → http://localhost:8000
```

### 5. 접속

- **사용자 대시보드**: http://localhost:8000
- **관리자 패널**: http://localhost:8000/admin
- **API 서버**: http://localhost:5001

## API 사용법

### 공격 시뮬레이션

```bash
POST http://localhost:5001/api/simulate-attack
Content-Type: application/json

{
  "attack_type": "DoS",
  "date": "2025-10-20",
  "use_openai": false  # OpenAI API 사용 여부
}
```

**응답 예시:**
```json
{
  "success": true,
  "attack": {
    "attack_type": "DoS",
    "severity": 8,
    "severity_level": "High",
    "packet_info": {
      "src_ip": "192.168.1.100",
      "dst_ip": "192.168.1.50",
      "protocol": "TCP",
      "packet_rate": 1234.5
    },
    "llm_response": {
      "summary": "DoS 공격이 탐지되었습니다...",
      "detection_reason": "패킷 분석: 전송 패킷 수 1,234개...",
      "risk_assessment": "위험도: High (심각도 8/10)",
      "recommended_actions": [...]
    },
    "feature_analysis": {
      "duration": 0.12,
      "src_packets": 1234,
      "packet_rate": 1234.5,
      "src_load": 52340.0
    }
  }
}
```

### 일별 공격 데이터 조회

```bash
GET http://localhost:5001/api/daily-attacks
```

### 성능 메트릭 조회

```bash
GET http://localhost:5001/api/daily-metrics
```

### 모델 학습 히스토리

```bash
GET http://localhost:5001/api/model-history
```

## 테스트

### 백엔드 테스트

```bash
# 공격 분석 엔진 테스트
cd backend
python attack_analyzer.py

# 패킷 제공자 테스트
python packet_provider.py
```

### 프론트엔드 테스트

1. 관리자 패널 접속: http://localhost:8000/admin
2. "공격 시뮬레이션" 버튼 클릭
3. 공격 유형 선택 (또는 랜덤)
4. 사용자 대시보드에서 실시간 탐지 결과 확인

### 성능 검증

```bash
# 모델 성능 히스토리 확인
cat logs/model_history.json | python -m json.tool
```

## 주요 화면

### 사용자 대시보드
- 실시간 공격 탐지 결과
- 10일간 공격 히스토리 캘린더
- 공격 유형별 분포 차트
- 심각도별 통계 그래프
- Feature 기반 판단 근거 표시

### 관리자 패널
- 공격 시뮬레이션 (9가지 유형)
- 일별 성능 메트릭 (FPR/FNR)
- 모델 학습 히스토리
- OpenAI API 연동 설정

## 기술 스택

### Backend
- **Flask 3.0.0** - REST API 서버
- **XGBoost 2.0.0** - 공격 분류 모델
- **Pandas 2.0.3** - 데이터 전처리
- **Scikit-learn 1.3.0** - ML 파이프라인
- **Joblib 1.3.2** - 모델 직렬화

### Frontend
- **Vanilla JavaScript** - 프론트엔드 로직
- **Chart.js** - 통계 시각화
- **HTML5/CSS3** - UI/UX

### 데이터
- **UNSW-NB15 Dataset** - 257,673건의 네트워크 패킷

## 성능 지표

### 모델 성능 (Best Model - Day9)
- **Accuracy**: 83.57%
- **FPR** (오탐률): 4.97%
- **FNR** (미탐률): 4.83%
- **Detection Rate**: 95.17%
- **Total Error**: 9.80%

### 탐지 가능한 공격 유형
- 9가지 공격 유형 분류
- 42개 네트워크 Feature 분석
- 실시간 Feature 기반 판단 근거 생성

## 보안 정책

이 시스템은 **방어적 보안 도구**로 설계되었습니다:

**허용**:
- 공격 탐지 및 분류
- 보안 위협 평가
- 방어 대응 방안 제시
- 성능 분석 및 모니터링

**금지**:
- 실제 공격 수행
- 악성 코드 생성/배포
- 비인가 시스템 접근

## 향후 개선 계획

- [ ] **실시간 패킷 캡처** - Scapy 통합으로 라이브 트래픽 분석
- [ ] **데이터베이스 연동** - MongoDB/PostgreSQL로 영구 저장
- [ ] **알림 시스템** - Email/SMS/Slack 통합
- [ ] **딥러닝 모델** - LSTM/CNN 기반 고급 분석
- [ ] **멀티모달 분석** - 로그 + 네트워크 + 시스템 이벤트 통합
- [ ] **자동 대응** - IDS/IPS 연동으로 자동 차단






