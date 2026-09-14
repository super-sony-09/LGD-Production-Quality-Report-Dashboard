# LGD-Production-Quality-Report-Dashboard
# LG Display 생산·품질 보고서 및 3D 설비 모니터링 웹 서비스

## 📅 날짜

**2026년 09월 14일**

---

## 📌 프로젝트 소개

LG Display 생산·품질 업무를 이해하기 위해 제작한 **교육용 생산·품질 데이터 조회 및 보고서 작성 웹 서비스**입니다.

생산라인별 LOT 데이터를 조회하여 생산량, 목표 달성률, 검사 수량, 불량률 등의 주요 지표를 확인할 수 있으며, 생산 과정에서 발생한 설비 사건 이력도 함께 확인할 수 있도록 구현했습니다.

또한 **Three.js 기반의 3D 생산·검사 설비 모델**을 구현하여 패널 이송 과정과 설비 상태를 시각적으로 확인할 수 있도록 구성했습니다.

조회한 생산·품질 데이터를 바탕으로 업무 보고서 초안을 자동 생성하고, 사용자가 내용을 검토·수정하여 저장하거나 TXT 및 JSON 형태로 다운로드할 수 있습니다.

---

## 🛠 사용 기술 및 라이브러리

### Backend

* Python
* Flask
* Werkzeug
* Threading
* JSON
* pathlib
* urllib
* asyncio

### Frontend

* HTML5
* CSS3
* JavaScript

### 3D Visualization

* Three.js
* OrbitControls

### External Access

* Cloudflared Tunnel

### Development Environment

* Google Colab

---

## ⚙️ 주요 기능

### 1. 생산·품질 데이터 조회

사용자가 다음 조건을 지정하여 생산 데이터를 조회할 수 있습니다.

* 시작일
* 종료일
* 생산라인

  * 전체
  * Line A
  * Line B

조회 조건에 따라 해당 기간과 생산라인의 LOT 데이터가 화면에 표시됩니다.

---

### 2. 생산 KPI 분석

조회한 데이터를 기반으로 다음 주요 지표를 자동 계산합니다.

* 생산량
* 목표 생산량
* 생산 달성률
* 검사 수량
* 불량 수량
* 불량률

생산 달성률은 다음과 같이 계산합니다.

```text
생산 달성률 = 실제 생산량 ÷ 목표 생산량 × 100
```

불량률은 다음과 같이 계산합니다.

```text
불량률 = 불량 수량 ÷ 검사 수량 × 100
```

검사 수량이 0인 경우에는 불량률을 계산하지 않고 판단 불가 상태로 처리합니다.

---

## 📊 LOT 생산·품질 데이터

조회된 LOT별로 다음 정보를 확인할 수 있습니다.

| 항목        | 설명      |
| --------- | ------- |
| Date      | 생산 일자   |
| Line      | 생산라인    |
| LOT       | LOT 번호  |
| Target    | 목표 생산수량 |
| Produced  | 실제 생산수량 |
| Inspected | 검사수량    |
| Defects   | 불량수량    |

조회 기간 동안의 데이터를 합산하여 전체 생산 실적과 품질 수준을 분석합니다.

---

## 📈 불량률 시각화

조회 기간의 **일자별 불량률**을 막대그래프로 표현합니다.

이를 통해 날짜별 품질 상태를 비교하고 특정 날짜에 불량률이 증가했는지 쉽게 확인할 수 있습니다.

---

## 🚨 설비 사건 이력

생산 과정에서 발생한 설비 사건을 함께 조회할 수 있습니다.

예시 사건

* 설비 정지
* 온도 주의
* 정상 복귀
* 미해제 설비 이상

각 사건에는 다음 정보가 포함됩니다.

```text
사건 ID
발생일
생산라인
사건 내용
해제 여부
확인 내용
```

생산 데이터와 설비 사건을 함께 확인하여 품질 문제와 설비 상태 사이의 관계를 검토할 수 있도록 구성했습니다.

---

## 🏭 Three.js 3D 검사 설비

Three.js를 이용하여 **디스플레이 패널 생산·검사 설비를 3D로 구현**했습니다.

3D 모델에는 다음 요소가 포함되어 있습니다.

* 생산설비 프레임
* 패널 이송 Conveyor
* Roller
* 검사 장비
* 투명 검사 Chamber
* 검사 Camera
* 작업자 Monitor
* Tower Lamp
* Display Panel

사용자는 마우스를 이용하여 설비를 확인할 수 있습니다.

```text
마우스 Drag : 설비 회전
Mouse Wheel : 확대 / 축소
시점 초기화 : 기본 Camera 위치 복귀
```

패널이 Conveyor를 따라 이동하는 애니메이션도 구현되어 있습니다.

---

## 🚦 설비 상태 표시

3D 설비의 Tower Lamp를 이용하여 생산라인의 상태를 시각적으로 표시합니다.

예시

```text
정상      → Green
온도 주의 → Amber
정지      → Red
```

전체 라인을 선택한 경우에는 Line A의 현재 모의 상태를 표시합니다.

---

## 📝 생산·품질 보고서 생성

현재 조회한 데이터를 기반으로 **생산·품질 업무 보고서 초안**을 자동으로 생성할 수 있습니다.

보고서에는 다음 내용이 포함됩니다.

* 조회 범위
* 생산수량
* 목표수량
* 생산 달성률
* 검사수량
* 불량수량
* 불량률
* 조회 LOT 수
* 설비 사건
* 현재 모의 설비 상태
* 추가 확인사항

생성된 보고서는 사용자가 직접 수정할 수 있습니다.

---

## ✅ 보고서 검토 기능

작성된 보고서를 저장하기 전에 사용자가 생산 데이터와 설비 사건 내용을 검토할 수 있습니다.

```text
조회 범위
생산 수치
품질 수치
설비 사건
보고서 표현
```

검토가 완료되면 **검토 완료 상태**를 저장할 수 있습니다.

보고서 본문을 수정하면 검토 상태가 다시 초기화되어 재검토하도록 설계했습니다.

---

## 💾 보고서 저장

생성한 보고서는 JSON 데이터로 관리합니다.

기본 저장 위치

```text
/content/04_lgd_web/04_reports.json
```

각 보고서에는 다음 정보가 저장됩니다.

```text
Report ID
생성 시간
수정 시간
조회 조건
근거 데이터
보고서 본문
검토 상태
```

---

## 📥 보고서 다운로드

저장된 보고서는 다음 형식으로 다운로드할 수 있습니다.

### TXT

```text
04_LGD_R001.txt
```

보고서 본문과 검토 상태를 확인할 수 있습니다.

### JSON

```text
04_LGD_R001.json
```

보고서 작성에 사용된 생산·품질 근거 데이터를 확인할 수 있습니다.

---

## 🌐 Flask API

프로젝트에서는 Flask 기반 REST API를 사용합니다.

주요 API는 다음과 같습니다.

```text
GET  /health
GET  /api/view
GET  /api/reports
POST /api/report
GET  /api/report/<report_id>
POST /api/report/<report_id>
GET  /api/download/<report_id>/<type>
```

이를 통해 Frontend와 Backend가 데이터를 주고받습니다.

---

## ☁️ Cloudflared 외부 접속

Google Colab 환경에서도 외부 브라우저에서 웹 서비스를 실행할 수 있도록 **Cloudflare Tunnel**을 사용했습니다.

서버 실행 후 다음과 같은 임시 URL이 자동 생성됩니다.

```text
https://xxxxxxxx.trycloudflare.com
```

생성된 URL을 통해 Colab 외부에서도 웹 화면에 접속할 수 있습니다.

Colab Runtime이 종료되면 서버와 Cloudflared Tunnel도 종료되며, 프로그램을 다시 실행할 경우 URL이 변경될 수 있습니다.

---

## 📂 프로젝트 구조

```text
04_lgd_web/
│
├── 04_dashboard.html
├── 04_reports.json
├── 04_cloudflared.log
├── cloudflared
│
└── static/
    └── vendor/
        ├── three.module.js
        └── OrbitControls.js
```

---

## 🎯 프로젝트 목적

본 프로젝트는 실제 생산 시스템을 구현하는 것이 아니라 **LG Display 생산·품질 업무 프로세스를 이해하기 위한 교육용 프로젝트**입니다.

특히 다음 과정을 웹 서비스 형태로 구현하는 것을 목표로 했습니다.

```text
생산 데이터 조회
        ↓
생산·품질 KPI 분석
        ↓
LOT별 데이터 확인
        ↓
설비 사건 확인
        ↓
3D 설비 상태 확인
        ↓
업무 보고서 생성
        ↓
사용자 검토
        ↓
보고서 저장 및 다운로드
```

생산 데이터만 확인하는 것이 아니라 **생산 실적 → 품질 → 설비 상태 → 보고서 작성**까지 하나의 업무 흐름으로 연결한 것이 프로젝트의 핵심입니다.

---

## ⚠️ 참고사항

본 프로젝트에서 사용되는 생산량, 불량률, 설비 상태 및 사건 정보는 실제 LG Display 내부 데이터가 아닌 **교육용 가상 데이터**입니다.

본 시스템은 실제 생산설비를 제어하지 않으며 Three.js로 구현된 설비 역시 교육 목적의 가상 모델입니다.

보고서 생성 기능 또한 별도의 생성형 AI API를 호출하는 방식이 아니라 사전에 정의된 규칙을 이용하여 데이터를 보고서 형식으로 변환합니다.

---

## 📚 참고 문헌 및 참고 자료

### Three.js

* Three.js 공식 Documentation
* Three.js OrbitControls Documentation
* Three.js GitHub Repository

### Flask

* Flask 공식 Documentation
* Werkzeug Documentation

### Cloudflare

* Cloudflare Tunnel Documentation
* cloudflared GitHub Repository

### Web Development

* MDN Web Docs

  * HTML
  * CSS
  * JavaScript
  * Fetch API

### Development Environment

* Google Colab Documentation

---

## 👨‍💻 Development

**Development Environment**

```text
Google Colab
Python
Flask
HTML
CSS
JavaScript
Three.js
Cloudflared
```

**Date**

```text
2026.09.14
```

---

## 📌 Summary

> 생산·품질 데이터를 조회하고, LOT별 실적과 설비 사건을 분석하며, Three.js 기반 3D 생산설비를 확인한 뒤 근거 기반 생산·품질 업무 보고서를 생성·검토·저장할 수 있도록 구현한 교육용 웹 서비스입니다.
