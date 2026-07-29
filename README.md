# ☁️ AI Solution Technology Stack — Lab Repository

> AWS AI 서비스, Azure 클라우드 인프라, OpenStack 프라이빗 클라우드를 활용한 통합 솔루션 실습 가이드

🌐 **언어 / Language:** [English](./i18n/README_en.md) | [日本語](./i18n/README_ja.md) | [中文](./i18n/README_zh.md)

---

## 📁 Repository 구조

```
.
├── README.md                          ← 현재 문서 (메인 인덱스)
├── labs/
│   ├── csp-ai-serving/                ← AWS·Azure·GCP AI 서빙 시작 가이드
│   │   └── README.md
│   ├── aws/                           ← AWS AI 솔루션 Labs
│   │   ├── README.md                  ← AWS Labs 개요
│   │   ├── lab01-lex-conversational-ai/
│   │   ├── lab02-rekognition-vision-ai/
│   │   ├── lab03-textract-document-ai/
│   │   ├── lab04-bedrock-rag/
│   │   ├── lab05-pdf-qrcode-automation/
│   │   ├── lab06-ai-agent-orchestration/
│   │   └── lab07-architecture-devops/
│   ├── azure/                         ← Azure 클라우드 Labs
│       ├── README.md                  ← Azure Labs 개요
│       ├── lab01-networking/
│       ├── lab02-compute-containers/
│       ├── lab03-security-governance/
│       ├── lab04-storage-cache/
│       ├── lab05-infra-devops/
│       ├── lab06-architecture-performance/
│       ├── lab07-app-development/
│       ├── lab08-ai-agent/            ← AI Agent 개발 (신규)
│       ├── lab09-ml-data-analysis/    ← ML / 데이터 분석 (신규)
│       └── lab10-azure-platform/      ← Azure 플랫폼 환경 (신규)
│   └── openstack/                     ← OpenStack 프라이빗 클라우드 Labs
│       └── README.md                  ← OpenStack 리소스 개요
├── docs/
│   ├── intro.md                       ← 전문가 이력/프로필
│   └── assets/                        ← PDF 등 첨부 자료
├── i18n/                              ← 다국어 번역 (EN, JA, ZH)
```

---

## 🚀 멀티 CSP AI 시스템 서빙 시작하기

AWS, Azure, Google Cloud에서 AI 시스템을 시작하기 위한 계정 생성, 결제·예산 통제, AI 리소스 선택, CLI 설치, 보안·운영 체크리스트를 제공합니다.

> 📘 [AWS · Azure · Google Cloud AI 시스템 서빙 시작 가이드](./labs/csp-ai-serving/README.md)

---

## ☁️ AWS AI Solution Labs

AWS 관리형 AI 서비스를 결합한 **통합 AI 솔루션** 실습 가이드입니다.

| Lab | 주제 | 핵심 서비스 |
|-----|------|------------|
| [Lab 01](./labs/aws/lab01-lex-conversational-ai/README.md) | 대화형 AI (챗봇/음성봇) | Amazon Lex, Lambda, Transcribe, Polly |
| [Lab 02](./labs/aws/lab02-rekognition-vision-ai/README.md) | 비전 AI (얼굴/객체 분석) | Amazon Rekognition |
| [Lab 03](./labs/aws/lab03-textract-document-ai/README.md) | 문서 AI (OCR/데이터 추출) | Amazon Textract |
| [Lab 04](./labs/aws/lab04-bedrock-rag/README.md) | 생성형 AI / RAG | Amazon Bedrock, Kendra |
| [Lab 05](./labs/aws/lab05-pdf-qrcode-automation/README.md) | PDF/QRCode 문서 자동화 | S3, Lambda |
| [Lab 06](./labs/aws/lab06-ai-agent-orchestration/README.md) | AI Agent 오케스트레이션 | Bedrock Agents, Step Functions |
| [Lab 07](./labs/aws/lab07-architecture-devops/README.md) | 아키텍처 / DevOps | CloudFormation, CDK, CodePipeline |

> 📄 [AWS AI 솔루션 제안서 (PDF)](./docs/assets/AWS_AI_Solution_Company_Brochure_ko.pdf)

---

## ☁️ Azure Cloud Labs

Azure 클라우드 인프라 구성요소별 실습 가이드입니다.

| Lab | 주제 | 핵심 서비스 |
|-----|------|------------|
| [Lab 01](./labs/azure/lab01-networking/README.md) | 네트워킹 | VNet, Subnet, Private Endpoint, Gateway, ExpressRoute |
| [Lab 02](./labs/azure/lab02-compute-containers/README.md) | 컴퓨팅/컨테이너 | App Service, AKS, Container Apps, ACR, VM |
| [Lab 03](./labs/azure/lab03-security-governance/README.md) | 보안/거버넌스 | Azure AD, RBAC, Policy, Compliance |
| [Lab 04](./labs/azure/lab04-storage-cache/README.md) | 스토리지/캐시 | Storage Account, Redis Cache |
| [Lab 05](./labs/azure/lab05-infra-devops/README.md) | 인프라/DevOps | Key Vault, ARM/Bicep, Azure Arc, GitHub Actions |
| [Lab 06](./labs/azure/lab06-architecture-performance/README.md) | 아키텍처/성능 | Well-Architected Framework, Front Door, CDN |
| [Lab 07](./labs/azure/lab07-app-development/README.md) | 앱 개발 | Flask, Django, Dapr, Application Insights |
| [Lab 08](./labs/azure/lab08-ai-agent/README.md) | **AI Agent 개발** | Azure OpenAI, AI Search, Durable Functions, Logic Apps |
| [Lab 09](./labs/azure/lab09-ml-data-analysis/README.md) | **ML / 데이터 분석** | Azure ML, Synapse, Data Factory, Cosmos DB (Gremlin) |
| [Lab 10](./labs/azure/lab10-azure-platform/README.md) | **Azure 플랫폼 환경** | Microsoft Fabric, ADLS Gen2, Synapse, Power BI |

---

## ☁️ OpenStack Private Cloud Labs

OpenStack 기반 프라이빗 클라우드 핵심 리소스 실습/설계 가이드입니다.

| 리소스 영역 | 대표 서비스 | 설명 |
|------------|------------|------|
| 컴퓨트 | Nova | 가상머신 인스턴스 생성/스케줄링/수명주기 관리 |
| 네트워크 | Neutron | L2/L3 네트워크, 라우터, 보안그룹, Floating IP 관리 |
| 블록 스토리지 | Cinder | VM 연결형 영구 블록 스토리지 볼륨 제공 |
| 오브젝트 스토리지 | Swift | 대용량 비정형 데이터 저장/복제/아카이빙 |
| 이미지 관리 | Glance | VM 이미지 저장소 및 버전 관리 |
| 오케스트레이션 | Heat | 템플릿 기반 인프라 자동 배포(IaC) |
| 아이덴티티 | Keystone | 사용자/프로젝트/역할 기반 인증·인가 |
| 대시보드 | Horizon | 웹 기반 운영 콘솔 |

> 📄 [OpenStack 리소스 상세](./labs/openstack/README.md)

---

## 📊 AWS · Azure · OpenStack 비교 분석

| 비교 항목 | AWS | Azure | OpenStack |
|-----------|-----|-------|-----------|
| 서비스 형태 | 퍼블릭 클라우드(완전관리형 중심) | 퍼블릭 + 하이브리드 통합 | 오픈소스 기반 프라이빗/하이브리드 구축형 |
| 핵심 강점 | AI/ML 관리형 서비스 다양성, 글로벌 리전 | Microsoft 생태계·엔터프라이즈 통합 | 커스터마이징 자유도, 벤더 종속성 완화 |
| 운영 책임 | 클라우드 제공자 중심 (사용자는 구성/운영) | 클라우드 제공자 + 하이브리드 운영 | 조직이 인프라 설계·배포·운영을 직접 수행 |
| 비용 구조 | 사용량 기반 과금(OPEX 중심) | 사용량 기반 + 엔터프라이즈 계약 최적화 | 초기 구축비(CAPEX)+운영비(OPEX) 혼합 |
| 확장/탄력성 | 매우 높음(글로벌 오토스케일) | 높음(리전/하이브리드 확장) | 설계 역량에 따라 다름(내부 자원 기반) |
| 주요 활용 시나리오 | 빠른 AI 서비스 출시, 글로벌 SaaS | 기업 업무시스템, M365/AD 연계 | 규제 환경, 온프레미스 현대화, 데이터 주권 |

---

## 📋 문서 구조 변경 요약

기존의 루트 레벨 단일 파일들을 **Lab 스타일**로 재구성하였습니다:

| 변경 유형 | 설명 |
|-----------|------|
| **AWS (분할)** | 단일 `Readme.md`의 AWS AI 제안서를 7개 개별 Lab으로 분할 |
| **Azure (병합)** | 19개 개별 Azure .md 파일을 7개 주제별 Lab으로 병합 |
| **다국어** | `README_en/ja/zh.md`를 `i18n/` 디렉토리로 이동 |
| **지원 문서** | `Intro.md` → `docs/intro.md`, PDF → `docs/assets/` |

---

# 온프레미스(On-premises)와 클라우드(Cloud)의 단어 탄생 배경

우리가 일상적으로 사용하는 IT 용어인 **온프레미스(On-premises)**와 **클라우드(Cloud)**는 기술의 패러다임 변화를 시각적이고 직관적인 공간 개념으로 표현한 흥미로운 탄생 배경을 가지고 있습니다. 각 단어의 어원과 유래를 정리해 드립니다.

---

## 1. 온프레미스 (On-premises)
> 🏠 **어원:** 'Premises'는 법률 및 부동산 용어로 **"건물, 토지, 사내(社內) 부지"**를 뜻합니다. 즉, 직역하면 **"사내 부지 내에 직접"**이라는 의미입니다.

### 💡 탄생 배경: 구기술과 신기술을 구분하기 위한 '역성어'
* **원래는 이름이 없었다:** 클라우드 컴퓨팅이 등장하기 전에는 모든 기업이 당연히 자기 회사 건물(지하 전산실 등)에 서버를 직접 구매하고 설치하여 운영했습니다. 당시에는 이것이 유일하고 당연한 방식이었기 때문에 특별히 구별하는 단어가 필요하지 않았고, 그저 '서버실'이나 '시스템 구축'체계로 불렸습니다.
* **클라우드의 등장과 차별화:** 2000년대 중반 인터넷 너머의 자원을 빌려 쓰는 '클라우드'가 등장하면서 상황이 바뀌었습니다. 기존의 전통적인 구축 방식과 새로운 방식을 구별해야 할 필요성이 생겼고, 이에 따라 **"하늘에 떠 있는 구름(클라우드) 방식과 달리, 우리 회사 건물(Premises) 안에 직접 두고 쓰는 방식"**이라는 의미로 '온프레미스'라는 단어가 뒤늦게 명명되었습니다.
* 이를 기존에 있던 개념에 새로운 기술이 나오면서 사후적으로 이름을 붙이는 **역성어(Backronym)**라고 부릅니다.

---

## 2. 클라우드 (Cloud)
> ☁️ **어원:** 우리가 매일 하늘에서 보는 **"구름(Cloud)"**입니다. 내 컴퓨터 내부가 아닌, 인터넷망 너머 어딘가에 존재하는 거대한 컴퓨팅 인프라를 상징합니다.

### 💡 탄생 배경: 엔지니어들의 칠판 낙서와 시각적 약속
* **네트워크 도면의 관행:** 과거 네트워크 엔지니어들이 화이트보드나 종이에 시스템 구조도를 그릴 때, 사내에 있는 PC나 라우터, 방화벽 등은 상세하게 그렸습니다. 하지만 회사 외부로 나가는 방대하고 복잡한 인터넷 망(ISP 영역)은 일일이 그리기 불가능하고 굳이 표현할 필요도 없었습니다.
* **구름으로 뭉뚱그리기:** 엔지니어들은 이 복잡한 인터넷 영역을 **하나의 큰 '구름 모양'**으로 그려 넣기 시작했습니다. *"이 구름 내부가 어떻게 얽혀있는지 우리가 알 필요는 없고, 데이터를 구름에 던지면 반대편 목적지로 통과해 간다"*는 일종의 시각적 약속이었습니다.
* **서비스의 상용화:** 2006년 아마존웹서비스(AWS)와 구글 등이 하드웨어를 직접 사지 않고 인터넷 너머의 거대 인프라를 빌려 쓰는 서비스를 본격화하면서, 이 구름 마크에서 이름을 딴 **'클라우드 컴퓨팅(Cloud Computing)'**이 전 세계 표준 용어로 정착되었습니다. "내부의 복잡함은 우리가 해결할 테니, 당신은 구름 너머의 자원을 편하게 쓰기만 하라"는 의미가 담겨 있습니다.

---

## 📌 한눈에 보는 요약

| 용어 | 어원적 의미 | 진짜 유래 |
| :--- | :--- | :--- |
| **온프레미스 (On-premises)** | 사내 건물/부지 내에 | 클라우드가 생기자, 기존의 **"회사 건물(Premises) 내에 서버를 두던 전통 방식"**을 구별해 부르기 위해 탄생 |
| **클라우드 (Cloud)** | 하늘에 떠 있는 구름 | 과거 엔지니어들이 도면을 그릴 때 **복잡한 인터넷 세상을 '구름' 모양으로 뭉뚱그려 그리던 습관**에서 유래 |

---
*본 가이드는 IT 인프라 패러다임의 역사적 변화와 용어의 변천사를 바탕으로 작성되었습니다.*
