### Context
Hermes(주로 Nous Hermes 계열) LLM 설치를 완료한 후, 이를 개인 지식 관리 도구인 옵시디언(Obsidian)과 연동하여 파편화된 기술 메모를 구조화된 문서로 자동 정리하는 시스템을 구축하고자 합니다. Hermes 모델은 특히 지시 이행(Instruction Following) 능력이 뛰어나 기술 문서의 요약 및 태그 생성에 적합합니다.

### Core
Hermes 모델(Ollama 기반)을 옵시디언의 'Smart Connections' 또는 'Text Generator' 플러그인과 연동하여 자동 문서 정리를 수행하기 위한 설정 및 로직입니다.

* **Ollama API 엔드포인트 설정**
  * URL: `http://localhost:11434/v1`
  * Model: `hermes-2-pro-llama-3` (또는 설치한 Hermes 버전)

* **프롬프트 템플릿 (System Role)**
```markdown
* 당신은 기술 전문 편집자 에이전트입니다.
* 입력된 원시 메모를 바탕으로 다음 형식을 준수하여 기술 문서를 재작성하세요.
* 출력 형식: 
  * YAML Frontmatter (tags, date 포함)
  * ### 핵심 요약
  * ### 상세 내용 (불렛 포인트 사용)
  * ### 연관 개념
```

* **n8n 연동 워크플로우 로직**
  * Obsidian REST API 노드를 사용하여 특정 폴더의 파일을 감시합니다.
  * 신규 메모 발생 시 AI Agent 노드(Hermes 모델)를 호출하여 내용을 정제합니다.
  * 정제된 텍스트를 `Obsidian Create File` 노드를 통해 지정된 경로에 저장합니다.

### Insight
* **도구 호출(Tool Calling) 성능**: Hermes-2-Pro 모델은 함수 호출 능력이 강화되어 있어, 옵시디언의 복잡한 폴더 구조나 메타데이터(Properties)를 수정하는 에이전트 작업에서 타 오픈소스 모델 대비 높은 정확도를 보입니다.
* **로컬 프라이버시**: 클라우드 LLM이 아닌 로컬 환경에서 Hermes를 구동함으로써 민감한 기술 자산을 외부 유출 없이 안전하게 자동화할 수 있습니다.
* **구조화된 출력**: Hermes 모델은 JSON 형식을 엄격하게 준수하는 특성이 있어, n8n과 같은 자동화 도구에서 데이터를 파싱(Parsing)하여 후속 작업을 처리하기에 매우 용이합니다.

**출처:** [Nous Research Hermes-2-Pro-Llama-3](https://github.com/NousResearch/Hermes-LLM), [Obsidian Smart Connections Docs](https://smartconnections.app/), [Ollama API Guide](https://github.com/ollama/ollama/blob/main/docs/api.md)