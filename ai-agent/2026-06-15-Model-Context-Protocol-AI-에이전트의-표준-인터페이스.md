### Context
기존의 AI 에이전트 개발 방식은 각 데이터 소스나 도구(SaaS, 데이터베이스 등)마다 개별적인 통합 로직을 수동으로 구축해야 하는 번거로움이 있었습니다. 이러한 파편화된 인터페이스 문제를 해결하기 위해 Anthropic에서 제안한 모델 컨텍스트 프로토콜(Model Context Protocol, 이하 MCP)은 AI 모델이 외부 데이터 및 도구와 통신하는 방식을 표준화합니다. 마치 다양한 전자기기가 USB-C 포트 하나로 연결되듯, 에이전트가 단일한 규격을 통해 수많은 도구와 즉시 상호작용할 수 있도록 돕습니다.

### Core
MCP는 서버-클라이언트 아키텍처를 기반으로 하며, JSON-RPC 통신을 통해 클라이언트(LLM 애플리케이션)가 서버(데이터/도구 제공자)의 기능을 호출합니다. 주요 구성 요소는 다음과 같습니다.

* **MCP Server**: 데이터베이스, 로컬 파일 시스템, 또는 외부 API를 리소스(Resources)나 도구(Tools)의 형태로 노출하는 역할을 합니다.
* **MCP Client**: 사용자와 상호작용하는 LLM 에이전트로, 서버가 제공하는 리스트를 탐색하고 필요한 도구를 실행합니다.
* **주요 기능 단위**:
    * **Resources**: 문서나 로그 등 읽기 전용 데이터 제공
    * **Tools**: 검색이나 코드 실행 등 실행 가능한 함수 제공
    * **Prompts**: 특정 작업을 위해 사전에 정의된 프롬프트 템플릿 제공

아래는 MCP 서버에서 도구를 정의하는 구조적 예시입니다.

```json
{
  "name": "fetch_data",
  "description": "외부 API로부터 특정 정보를 조회합니다.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "endpoint": { "type": "string" },
      "params": { "type": "object" }
    },
    "required": ["endpoint"]
  }
}
```

### Insight
MCP의 도입은 단순한 편의성을 넘어 AI 생태계의 '상호운용성(Interoperability)'을 극대화합니다. 개발자는 더 이상 각 모델마다 커스텀 글루 코드(Glue Code)를 작성할 필요가 없으며, 한 번 구축한 MCP 서버는 Claude나 IDE 등 MCP를 지원하는 모든 클라이언트에서 즉시 재사용 가능합니다. 이는 에이전트 구축 비용을 획기적으로 줄이고, LLM이 실제 세계의 데이터와 결합되는 속도를 가속화할 것입니다.

**출처:** [Model Context Protocol 공식 문서](https://modelcontextprotocol.io/introduction)
**출처:** [Anthropic: Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol)
**출처:** [GitHub: MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)