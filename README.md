# ollama-mac-dashboard

# 🎯 Mac Ollama Core Inference Dashboard

Mac 로컬 환경에서 구동되는 Ollama 서버의 `server.log`(GIN 프레임워크 규격)를 동적으로 파싱하여 실제 LLM 추론 통계 및 문장 생성 속도(Tokens/s)를 모니터링하는 웹 대시보드 애플리케이션입니다.

단순 정보 조회 API(`/api/show`, `/api/tags`, `/api/version` 등)로 인한 통계 왜곡을 원천 차단하고, `💬 Chat` 및 `📝 Generate`와 같은 핵심 추론 트래픽의 지연 시간(Latency)과 정확한 물리적 문장 생성 속도를 독립적으로 계측합니다.

---

## 📦 의존성 패키지 안내 (Dependencies)

이 프로젝트는 무겁고 복잡한 모니터링 인프라(Prometheus, Grafana 등) 없이 오직 **파이썬 데이터 과학 생태계의 표준 라이브러리 3개**만을 사용하여 가볍고 강력하게 작동합니다.

*   **`streamlit` (웹 프레임워크)**
    *   **역할**: 파이썬 코드를 웹 GUI 대시보드로 즉시 변환해 주는 UI 엔진입니다.
    *   **이유**: HTML/CSS/JS 없이 백엔드 로직만으로 실시간 데이터를 갱신(Rerun)하고 인터랙티브한 반응형 모니터링 화면을 구축하기 위해 사용합니다.
*   **`pandas` (데이터 수집 및 정형화)**
    *   **역할**: 텍스트 로그 파일에서 추출한 비구조화 데이터를 테이블(DataFrame) 형태로 가공하는 코어 엔진입니다.
    *   **이유**: 시계열 로그의 시간대별 리샘플링(1분/10분 단위 묶기), 통계 수치 계산(평균, 누적 합산), API 엔드포인트 필터링 작업을 빠르게 처리하기 위해 필수적입니다.
*   **`plotly` (데이터 시각화 그래프)**
    *   **역할**: 웹 브라우저에서 마우스 오버, 확대, 다운로드가 가능한 반응형 차트를 그려주는 그래픽 라이브러리입니다.
    *   **이유**: 점유율을 보여주는 파이 차트와 시간 흐름에 따른 추론 속도 변화를 보여주는 산점도(Scatter Chart)를 세련된 UI로 시각화하기 위해 채택했습니다.

---

## 🛠️ 설치 및 상시 서비스 자동 구동 방법 (Mac 전용)

설치되는 폴더 경로 및 Mac 컴퓨터의 계정명에 구애받지 않고, 아래 스크립트를 통해 경로가 자동으로 정규화되어 시스템 백그라운드 구동 에이전트로 등록됩니다.

```bash
# 1. 저장소 클론 및 프로젝트 이동
git clone https://github.com/jasonkim-77/ollama-dashboard-mac
cd ollama-mac-dashboard

# 2. 핵심 의존성 패키지 일괄 설치
pip install streamlit pandas plotly

# 3. 원클릭 동적 세팅 자동 실행 스크립트 가동
chmod +x install.sh
./install.sh
```

설치가 완료되면 현재 터미널 앱을 완전히 종료하거나 Mac을 껐다 켜도 **`http://localhost:9999`** 포트 환경에서 상시적으로 대시보드가 가동됩니다.

---

## 🗑️ 백그라운드 서비스 안전 삭제 방법

만약 대시보드 구동을 중지하고 포트를 반환하거나 시스템에서 완전히 제거하고 싶다면 아래 삭제 스크립트를 실행하세요.

```bash
chmod +x uninstall.sh
./uninstall.sh
```

---

## 🚨 오픈소스 업로드 시 주의사항 (.gitignore 추가)
실제 데이터가 포함되는 로그 파일이나 가동 오류 로그(`.log`)가 Public 저장소에 업로드되는 것을 사전 차단하기 위해 프로젝트 루트에 `.gitignore` 파일이 기본적으로 포함되어 있습니다.
```text
*.log
.DS_Store
__pycache__/
.streamlit/
```
