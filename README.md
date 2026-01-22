# RUD (Rebalancing & User-friendly Dashboard)

> **🏆 Award Winner**: IoT 빅데이터 응용 교육과정 성과물 경진대회 **우수상** 수상작

**RUD**는 주식 종목 추천, 포트폴리오 리밸런싱, 그리고 커뮤니티 기능을 결합한 통합 웹 서비스입니다. 사용자가 자신의 투자 계좌를 손쉽게 등록하고 관리할 수 있도록 **OCR 기반 자동 입력** 기능을 제공하며, 초보 투자자들에게 직관적인 포트폴리오 재조정 제안을 목표로 합니다.

> [!NOTE]
> 🚀 **이 프로젝트는 이후 [SMS (Stock Management Service)](https://github.com/sehyun00/SMS_Project)로 고도화되었습니다.**
> SMS 프로젝트에서는 그래프 신경망(TGNN)과 강화학습(DDPG)을 도입하여 더욱 강력한 포트폴리오 최적화 엔진을 탑재했습니다.

---

## 📖 프로젝트 회고 (Project Story)

### 1. Situation (문제 인식)
주식 투자를 하는 많은 개인 투자자들은 자신의 포트폴리오 현황을 파악하기 위해 **증권사 앱의 계좌 정보를 수동으로 엑셀이나 기록장에 옮겨 적는 번거로움**을 겪고 있었습니다. 또한, 목표 비중을 유지하기 위한 **리밸런싱 계산이 복잡**하여 실제로 실천하기 어려운 문제가 있었습니다.

### 2. Task (과제)
이러한 문제를 해결하기 위해 우리 팀은 두 가지 핵심 과제를 정의했습니다.
1.  **계좌 정보 자동화**: 캡처된 계좌 이미지를 AI로 분석하여 종목명, 수량, 수익률 등을 자동으로 DB에 저장하는 기능 개발.
2.  **직관적 리밸런싱**: 복잡한 수식 없이 클릭 한 번으로 최적의 매수/매도 수량을 제안하는 Full-Stack 웹 서비스 구축.

### 3. Action (해결 과정)
-   **AI/OCR 도입**: `Pororo`와 `EasyOCR` 라이브러리를 활용하여 증권사별 상이한 UI에서도 텍스트를 정확히 추출할 수 있도록 `OpenCV`로 이미지를 전처리(이진화, 노이즈 제거)했습니다.
-   **하이브리드 아키텍처**: 데이터 처리와 AI 모델 서빙은 **Python(Flask)** 으로 구축하여 유연성을 확보하고, 안정적인 서비스 운영을 위해 메인 백엔드는 **Java(Spring Boot)** 로 구성하였습니다.
-   **사용자 중심 UX**: 기술적인 데이터 나열보다는 "현재 상태 → 변경 제안 → 예상 결과"의 흐름으로 UI를 설계하여 사용자 경험을 개선했습니다.

### 4. Result (결과)
-   **우수상 수상**: IoT 빅데이터 응용 교육과정에서 그 기술력과 기획력을 인정받았습니다.
-   **기술적 성장**: 복수의 언어(Java, Python)와 프레임워크를 연동하는 마이크로서비스 형태의 아키텍처를 경험했습니다.

---

## 🛠 시스템 아키텍처 & 기술 스택

이 프로젝트는 Spring Boot를 메인 서버로 사용하며, 특수 기능(OCR, 주식 데이터, AI)을 수행하는 Python 마이크로서비스들과 통신합니다.

### 🔹 Backend (Main)
-   **Framework**: Spring Boot 3.3.4 (Java 17)
-   **Role**: 사용자 관리, 게시판, API 게이트웨이, 데이터 지속성(JPA)
-   **Database**: MySQL / H2

### 🔹 Frontend
-   **Framework**: React 18
-   **Style**: Reactstrap, Sass
-   **Integrated**: Spring Boot 내장 (`rud_boot/src/main/rud_react`)

### 🔹 AI & Data Microservices (Python)
-   **OCR Server (`rud_easyocr`)**:
    -   Python 3.7
    -   Pororo, EasyOCR, OpenCV, Torch
    -   기능: 이미지에서 계좌 텍스트 추출
-   **Stock Logic Server (`StockRebalncing...`)**:
    -   Python 3.9
    -   TensorFlow, Keras, Scikit-learn
    -   기능: PCA 및 딥러닝 기반 포트폴리오 분석
-   **Data API (`rud_stockapi`)**:
    -   Python 3.9
    -   Mojito2, Flask
    -   기능: 한국투자증권 API 연동 실시간 주식 데이터 수집

---

## 🚀 설치 및 실행 (Getting Started)

### 1. 메인 서버 (Spring Boot + React)
Spring Boot 실행 시 React 프론트엔드가 자동으로 빌드되어 정적 리소스로 서빙됩니다.

```bash
# Windows
cd rud_boot
gradlew bootRun

# Mac/Linux
cd rud_boot
./gradlew bootRun
```

### 2. 서브 서비스 (Python)

AI 및 데이터 처리를 위해 각 폴더에서 별도의 Python 서비스를 실행해야 합니다.

#### A. OCR 서비스 실행
```bash
cd rud_easyocr
# Conda 가상환경 (Python 3.7)
conda create -n rud_ocr python=3.7 && conda activate rud_ocr
pip install pororo opencv-python~=4.8.0.74 matplotlib wget flask flask_cors
# PyTorch 설치 (CPU 버전 예시)
pip install torch torchvision torchaudio

python app.py
```

#### B. 주식 데이터 API 실행
*한국투자증권 API 키가 필요합니다 (`rud_stockapi/koreainvestment.key`)*

```bash
cd rud_stockapi
# Conda 가상환경 (Python 3.9)
conda create -n rud_stock python=3.9 && conda activate rud_stock
pip install mojito2 flask pandas flask_cors googletrans

python main.py
```
