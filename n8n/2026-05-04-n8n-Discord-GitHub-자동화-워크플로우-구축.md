### Context
학습한 내용을 디스코드 메시지로 전송하면 이를 자동으로 구조화된 TIL(Today I Learned) 문서로 변환하고 GitHub 저장소에 관리하기 위한 자동화 시스템을 구축함. 수동으로 파일을 생성하고 인덱스를 업데이트하는 번거로움을 해결하기 위해 n8n의 AI 에이전트와 GitHub 연동 기능을 활용함.

### Core
n8n 워크플로우는 다음과 같은 논리적 흐름으로 구성됨.

*   **Discord Trigger**: 특정 채널에서 메시지가 발생하면 워크플로우가 시작됨. `Message Created` 이벤트를 감지하여 입력 텍스트를 캡처함.
*   **AI Agent (LLM)**: 수신된 단편화된 메시지를 사전 정의된 TIL 템플릿(Context, Core, Insight 구조)에 맞춰 정교하게 재구성함. OpenAI 또는 Anthropic 모델을 에이전트 노드에 연결하여 활용함.
*   **GitHub (Push File)**: 에이전트가 생성한 마크다운 콘텐츠를 `YYYY-MM-DD-제목.md` 형식의 파일명으로 지정된 Repository의 폴더에 업로드함.
*   **GitHub (README Update)**: 기존 `README.md` 파일을 가져와(Get) 새로 생성된 파일의 링크를 목록 최상단에 추가한 뒤 다시 업데이트(Update)하는 로직을 수행함.
*   **Discord (Send Message)**: 모든 프로세스가 성공적으로 완료되었음을 사용자에게 알림.

### Insight
n8n 워크플로우 설계 시 가장 핵심적인 역량은 각 노드의 데이터 구조(JSON)를 파악하는 것임.
*   **Input/Output 데이터 흐름**: 노드 사이의 연결에서 앞선 노드의 `Output`이 다음 노드의 `Input`으로 어떻게 매핑되는지 정확히 이해해야 함. 특히 GitHub 노드에서 파일을 업데이트할 때 기존 파일의 `sha` 값이나 인코딩된 콘텐츠를 다루는 방식이 중요함.
*   **에이전트의 유연성**: 단순한 텍스트 변환을 넘어 AI 에이전트 노드를 사용함으로써 비정형 데이터를 정형화된 기술 문서로 품질 높게 변환할 수 있음을 확인함.

**출처:**
*   [n8n Discord Trigger Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.discordtrigger/)
*   [n8n GitHub Node Guide](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.github/)
*   [n8n AI Agent Node Overview](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.aigent/)