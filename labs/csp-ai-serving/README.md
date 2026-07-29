# 멀티 CSP AI 시스템 서빙 시작 가이드 — AWS · Azure · Google Cloud

[← 메인으로](../../README.md)

> AI 서비스를 처음 클라우드에 올릴 때 필요한 **계정 생성 → 결제/비용 통제 → AI 리소스 선택 → CLI 설정 → 안전한 서빙**의 공통 흐름을 정리한 가이드입니다. 서비스명·모델 가용 리전·가격은 수시로 바뀌므로, 실제 생성 전에는 각 항목의 공식 콘솔과 가격 페이지를 반드시 확인합니다.

---

## 1. 전체 도입 흐름

```
조직/계정 설계 → 결제 연결·예산 알림 → IAM 최소 권한 → CLI 인증
       → AI API/모델 사용 승인 → 개발 환경 검증 → 사설망·비밀 관리
       → API/컨테이너 서빙 → 관측성·비용·안전성 운영
```

처음에는 한 CSP, 한 리전, 개발용 프로젝트(또는 계정/구독)에서 작은 요청량으로 검증합니다. 운영 전환 시에는 개발·스테이징·운영 환경을 분리하고, 운영 결제 관리자와 개발자 권한을 분리합니다.

### 시작 전 체크리스트

- 조직 소유 이메일과 다중 인증(MFA)을 사용한다. 개인 루트/소유자 계정을 일상 작업에 사용하지 않는다.
- 결제 관리자, 보안 관리자, 개발자, 읽기 전용 운영자 역할을 분리한다.
- 리전은 사용자 위치, 데이터 주권, 모델/서비스 가용성, 네트워크 연결, 비용을 함께 고려한다.
- PII·기밀 문서는 암호화, 보존 기간, 접근 기록, 외부 모델 전송 여부를 검토한 뒤 반입한다.
- 예산 경보를 먼저 만들고, 태그/라벨로 `service`, `environment`, `owner`, `cost-center`를 필수화한다.

---

## 2. CSP별 계정과 결제 등록

| CSP | 기본 관리 단위 | 계정/프로젝트 생성 | 결제 등록 및 비용 통제 |
|---|---|---|---|
| AWS | Account, Organization, OU | [AWS 가입](https://signin.aws.amazon.com/signup) 후 Organization에서 워크로드별 멤버 계정 생성 | Billing and Cost Management에서 결제수단 확인 → Budgets 생성 → Cost Explorer와 Cost Anomaly Detection 활성화 |
| Azure | Microsoft Entra 테넌트, Subscription, Management Group, Resource Group | [Azure 가입](https://azure.microsoft.com/free/) 후 구독 생성; Resource Group을 서비스/환경 단위로 구성 | Cost Management + Billing에서 결제 프로필과 구독을 확인 → Budget/Alert 생성 → Cost analysis에서 태그별 분석 |
| Google Cloud | Organization, Folder, Project | [Google Cloud 시작](https://console.cloud.google.com/freetrial) 후 프로젝트를 환경/서비스 단위로 생성 | Billing 계정을 프로젝트에 연결 → Budgets & alerts 생성 → Billing export를 BigQuery로 내보내 세부 분석 |

### 권장 분리 예시

| 목적 | AWS | Azure | Google Cloud |
|---|---|---|---|
| 조직/결제 관리 | Management account | Management group + billing account | Organization + Cloud Billing account |
| 개발 | 별도 AWS account | Dev subscription | `ai-dev` project |
| 운영 | 별도 AWS account | Prod subscription | `ai-prod` project |
| 공용 관측/보안 | Log archive / security account | 중앙 Log Analytics workspace | 보안/로그 전용 project |

> 무료 체험·크레딧은 기간, 국가, 대상에 따라 다릅니다. 크레딧이 있다고 해서 사용량 초과가 자동 차단되지는 않을 수 있으므로, 예산 알림과 리소스 정리 절차를 함께 운영합니다.

---

## 3. AI 서빙에 쓰는 리소스 선택

AI 시스템은 보통 **모델 추론**, **애플리케이션 실행**, **지식/데이터**, **보안·운영** 네 계층으로 구성합니다.

| 계층 | AWS | Azure | Google Cloud | 언제 선택하는가 |
|---|---|---|---|---|
| 관리형 생성 AI API | Amazon Bedrock | Azure OpenAI Service / Azure AI Foundry | Vertex AI (Gemini, Model Garden) | 모델 운영 부담 없이 LLM·임베딩·이미지/음성 API를 사용할 때 |
| 사용자 정의 모델/ML 엔드포인트 | Amazon SageMaker AI | Azure Machine Learning Managed Online Endpoints | Vertex AI Endpoints | 학습한 모델 또는 특정 오픈 모델을 HTTP 엔드포인트로 배포할 때 |
| GPU 컨테이너/쿠버네티스 | ECS/EKS/EC2 GPU | Azure Container Apps/AKS/VM GPU | Cloud Run GPU/GKE/Compute Engine GPU | vLLM·TGI·Triton 등 자체 추론 서버를 제어해야 할 때 |
| RAG 검색/벡터 | Bedrock Knowledge Bases, OpenSearch | Azure AI Search | Vertex AI Search, Vector Search | 사내 문서 검색·인용·하이브리드 검색이 필요할 때 |
| 문서·음성·이미지 AI | Textract, Rekognition, Transcribe, Polly | Document Intelligence, Vision, Speech | Document AI, Vision AI, Speech-to-Text/Text-to-Speech | OCR·객체 인식·STT/TTS 같은 특화 AI가 필요할 때 |
| 비밀·키 관리 | Secrets Manager, KMS | Key Vault | Secret Manager, Cloud KMS | API 키·DB 비밀번호·암호화 키를 코드 밖에서 관리할 때 |
| 관측성과 비용 | CloudWatch, X-Ray, Cost Explorer | Azure Monitor, App Insights, Cost Management | Cloud Monitoring/Logging, Cloud Billing | 지연시간·오류·토큰/요청량·비용을 운영할 때 |

### 서빙 방식 결정표

| 방식 | 장점 | 주의점 | 적합한 경우 |
|---|---|---|---|
| 관리형 모델 API | 가장 빠른 출시, 서버/GPU 미관리, 자동 확장 | 모델·리전·할당량과 요청/토큰 과금 확인 필요 | 챗봇, RAG, 요약, 분류의 초기/일반 운영 |
| 관리형 ML 엔드포인트 | 모델 패키징·배포·버전/오토스케일 관리 | 엔드포인트 유휴 비용 및 배포 규격 관리 | 자체 학습 모델, 고정된 추론 API |
| 서버리스 컨테이너 | 앱과 모델 경량 추론을 함께 배포, 운영 단순 | 콜드 스타트·GPU·긴 요청 제약 확인 | API 백엔드, 전처리, 비동기 워커 |
| Kubernetes + GPU | 런타임/스케줄링/모델 캐시 세밀 제어 | GPU 용량·보안·업그레이드·SRE 역량 필요 | 고처리량 오픈 모델, 멀티모델, 엄격한 성능 제어 |

---

## 4. CLI 설치와 초기 인증 (Ubuntu/Debian 기준)

브라우저 로그인·MFA가 가능한 개발 PC에서 먼저 설정합니다. 운영 자동화에는 사람의 장기 액세스 키 대신 워크로드 ID(역할, 관리형 ID, 서비스 계정)를 사용합니다.

### 4.1 AWS CLI v2

```bash
sudo apt-get update
sudo apt-get install -y curl unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o /tmp/awscliv2.zip
unzip -q /tmp/awscliv2.zip -d /tmp
sudo /tmp/aws/install
aws --version

# IAM Identity Center(SSO)를 사용한다면
aws configure sso
aws sso login --profile dev
aws sts get-caller-identity --profile dev
```

- 콘솔에서 IAM Identity Center를 구성하고, 개발자에게 필요한 permission set만 부여합니다.
- 불가피하게 액세스 키를 쓸 경우에도 루트 키는 만들지 말고, 개인 IAM 사용자 키를 최소 권한·짧은 교체 주기로 관리합니다.
- [AWS CLI 설치 문서](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), [IAM Identity Center CLI 구성](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)을 참고합니다.

### 4.2 Azure CLI

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az version
az login
az account list --output table
az account set --subscription "<subscription-id-or-name>"
az account show --output table
```

- 로그인한 사용자는 Microsoft Entra ID에 있어야 하며, 해당 구독/리소스 그룹에 필요한 Azure RBAC 역할을 받아야 합니다.
- Azure OpenAI 등 일부 서비스는 구독·리전별 접근 권한/할당량이 별도로 필요합니다. 포털에서 서비스 생성 가능 여부와 모델 배포 가능 리전을 확인합니다.
- [Azure CLI Linux 설치](https://learn.microsoft.com/cli/azure/install-azure-cli-linux), [Azure CLI 로그인](https://learn.microsoft.com/cli/azure/authenticate-azure-cli)을 참고합니다.

### 4.3 Google Cloud CLI (gcloud)

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | \
  gpg --dearmor | sudo tee /usr/share/keyrings/cloud.google.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | \
  sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list
sudo apt-get update
sudo apt-get install -y google-cloud-cli
gcloud init
gcloud projects list
gcloud config set project "<project-id>"
gcloud auth application-default login  # 로컬 애플리케이션 개발용
```

- 프로젝트 선택 후 **APIs & Services**에서 Vertex AI API 등 필요한 API를 활성화합니다. 예: `gcloud services enable aiplatform.googleapis.com`.
- 운영 워크로드에는 서비스 계정에 최소 IAM 역할을 부여하고, 가능하면 키 파일 대신 Workload Identity/Attached Service Account를 사용합니다.
- [Google Cloud CLI 설치](https://cloud.google.com/sdk/docs/install), [gcloud 초기화](https://cloud.google.com/sdk/docs/initializing), [Vertex AI API 활성화](https://cloud.google.com/vertex-ai/docs/start/cloud-environment)을 참고합니다.

> `curl | sudo bash` 형태의 Azure 설치 명령은 공식 설치 도구를 실행합니다. 규제가 있는 환경에서는 [공식 단계별 설치 절차](https://learn.microsoft.com/cli/azure/install-azure-cli-linux)를 검토하여 저장소 키와 패키지를 개별 설치합니다.

---

## 5. CSP별 AI 리소스 생성과 사용 순서

### AWS: Bedrock 중심의 관리형 LLM/RAG

1. AI 서비스를 지원하는 리전을 선택하고, Bedrock 콘솔에서 필요한 모델의 액세스 가능 여부를 확인합니다.
2. S3에 원본 문서를 저장하고, Bedrock Knowledge Bases 또는 OpenSearch로 검색 인덱스를 구성합니다.
3. 애플리케이션 실행 계층으로 Lambda/API Gateway 또는 ECS/EKS를 선택합니다.
4. 호출 역할에 `bedrock:InvokeModel` 등 필요한 액션만 허용하고, S3·KMS·Secrets Manager 접근 범위를 리소스 단위로 제한합니다.
5. CloudWatch에 요청 수, 오류, 지연시간을 기록하고 Budgets/Cost Explorer로 모델별 비용을 점검합니다.

```bash
# 선택한 프로파일과 리전의 호출 주체 확인
aws sts get-caller-identity --profile dev
aws bedrock list-foundation-models --region <bedrock-region> --profile dev
```

Bedrock 모델 액세스와 모델별 지원 리전은 다를 수 있습니다. [Bedrock 모델 액세스](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html)와 [Bedrock API 권한](https://docs.aws.amazon.com/bedrock/latest/userguide/security_iam_id-based-policy-examples.html)을 확인합니다.

### Azure: Azure OpenAI + AI Search 중심의 엔터프라이즈 RAG

1. Resource Group을 `rg-ai-dev`처럼 환경과 용도가 드러나게 생성합니다.
2. Azure OpenAI 리소스를 생성한 뒤 사용할 모델을 **deployment name**으로 배포합니다. SDK 호출 시 모델명 대신 이 배포명을 사용합니다.
3. Blob Storage에 문서를 저장하고, Azure AI Search에서 벡터/하이브리드 검색 인덱스를 만듭니다.
4. App Service, Container Apps, Functions 또는 AKS에 API를 배포하고, Key Vault·Managed Identity로 비밀과 권한을 연결합니다.
5. Application Insights와 Azure Monitor에서 요청 실패·지연시간·의존성 호출을, Cost Management에서 구독/태그별 비용을 관찰합니다.

```bash
az group create --name rg-ai-dev --location <region>
az provider register --namespace Microsoft.CognitiveServices
az provider register --namespace Microsoft.Search
az cognitiveservices account list --resource-group rg-ai-dev --output table
```

Azure OpenAI의 모델 가용성·할당량은 리전과 구독에 따라 달라집니다. [Azure OpenAI 리소스/모델 배포](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/create-resource)와 [Azure AI Search 벡터 검색](https://learn.microsoft.com/azure/search/vector-search-overview)을 기준으로 구성합니다.

### Google Cloud: Vertex AI 중심의 모델 API/엔드포인트

1. 개발 프로젝트를 만들고 Cloud Billing 계정을 연결합니다.
2. Vertex AI API를 활성화하고, 리전을 정합니다. 생성형 모델과 엔드포인트는 지원 위치가 다를 수 있습니다.
3. Gemini 또는 Model Garden 모델을 API로 호출하거나, 사용자 정의 모델을 Vertex AI Endpoint에 배포합니다.
4. 문서 기반 서비스는 Vertex AI Search 또는 Vector Search와 Cloud Storage를 조합합니다.
5. Cloud Run/GKE에서 API를 실행하고, 서비스 계정·Secret Manager·Cloud Logging/Monitoring을 연결합니다.

```bash
gcloud services enable aiplatform.googleapis.com
gcloud config set ai/region <vertex-region>
gcloud ai models list --region=<vertex-region>
```

[Vertex AI 시작](https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform), [Vertex AI 위치](https://cloud.google.com/vertex-ai/docs/general/locations), [Vertex AI IAM](https://cloud.google.com/vertex-ai/docs/general/access-control)을 확인합니다.

---

## 6. 권장 AI 서빙 아키텍처

```
사용자/업무 시스템
        │ HTTPS, 인증·권한·WAF/Rate limit
        ▼
API Gateway / Load Balancer
        ▼
AI Application API (Container / Functions / Kubernetes)
  ├── Managed LLM API 또는 Model Endpoint
  ├── RAG Retrieval (검색·벡터 DB) ── 문서 저장소
  ├── 비동기 작업 Queue/Workflow ── OCR·인덱싱·배치
  └── Secret Manager / Key Management
        ▼
Logs · Metrics · Traces · Audit · Budget alerts
```

### 최소 운영 기준

| 영역 | 적용 기준 |
|---|---|
| 인증/인가 | 외부 API는 OAuth/OIDC 또는 서비스 간 ID를 사용하고, 사용자/테넌트별 데이터 필터를 검색 단계에도 적용 |
| 네트워크 | 가능한 경우 Private Endpoint/PrivateLink/Private Service Connect를 사용; 관리형 AI API의 사설 연결 지원 범위를 사전 확인 |
| 비밀 | 키를 소스·이미지·CI 로그에 넣지 않고 Secret Manager/Key Vault/Secrets Manager에서 런타임 주입 |
| 안전성 | 프롬프트 인젝션 방어, 입력 크기 제한, PII 마스킹, 콘텐츠 필터/가드레일, 도구 호출 허용 목록, 인용 검증 적용 |
| 신뢰성 | 타임아웃·재시도·지수 백오프·서킷 브레이커, 모델/리전 장애 시 폴백, 비동기 작업의 DLQ 구성 |
| 관측성 | 요청 ID, 모델/배포명, 토큰/요청량, 검색 문서 ID, 지연시간, 오류 유형을 기록하되 원문 프롬프트의 민감정보는 마스킹 |
| 비용 | 토큰·엔드포인트 시간·GPU·벡터 인덱스·데이터 전송을 함께 계측; 개발 엔드포인트 자동 중지/삭제 일정 운영 |

---

## 7. 운영 전 전환 체크리스트

- [ ] 개발/스테이징/운영의 계정·구독·프로젝트와 결제가 분리되어 있다.
- [ ] 루트/소유자 MFA, 비상 접근 계정, 최소 권한 RBAC/IAM, 감사 로그가 설정되어 있다.
- [ ] 예산·이상 비용 경보와 필수 태그/라벨이 설정되어 있다.
- [ ] 모델 접근 승인, 리전 가용성, 할당량, 데이터 처리 조건을 확인했다.
- [ ] API 키·DB 비밀번호·인증서가 비밀 관리 서비스에 있고 코드/저장소에 없다.
- [ ] RAG 인덱스에 ACL/테넌트 필터가 적용되고 응답에 출처가 포함된다.
- [ ] 부하·장애·프롬프트 인젝션·권한 우회·비용 급증 시나리오를 시험했다.
- [ ] 로그 보존 기간, 개인정보 마스킹, 모델/프롬프트/인덱스 버전 롤백 절차가 문서화되어 있다.

---

## 8. 기존 Lab과의 연결

| 목표 | 이어서 볼 Lab |
|---|---|
| AWS 생성형 AI/RAG | [AWS Lab 04 — Bedrock · Kendra](../aws/lab04-bedrock-rag/README.md) |
| AWS AI Agent 오케스트레이션 | [AWS Lab 06 — Bedrock Agents](../aws/lab06-ai-agent-orchestration/README.md) |
| Azure RAG/Agent | [Azure Lab 08 — AI Agent](../azure/lab08-ai-agent/README.md) |
| Azure ML·데이터 파이프라인 | [Azure Lab 09 — ML / 데이터 분석](../azure/lab09-ml-data-analysis/README.md) |
| Azure 컨테이너·컴퓨팅 | [Azure Lab 02 — 컴퓨팅/컨테이너](../azure/lab02-compute-containers/README.md) |

