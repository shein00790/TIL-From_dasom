### Context
* 옵시디언(Obsidian)의 핵심 기능인 위키 링크(Wikilinks)를 활용하여 파편화된 정보를 연결하고 지식 그래프를 구축하는 방법론을 학습합니다.
* 단순한 메모 기록을 넘어 문서 간의 상호 참조를 통해 '제2의 뇌(Second Brain)'를 구성하는 과정에서의 기술적 특징을 분석합니다.

### Core
* 위키 링크(Wikilinks) 기본 문법: `[[문서명]]`을 사용하여 새 문서를 생성하거나 기존 문서로 즉시 연결할 수 있습니다.
* 특정 섹션 및 블록 링크: `[[문서명#섹션명]]`을 통해 특정 헤더(Header)로 이동하거나, `[[문서명^블록ID]]`를 사용하여 특정 단락으로 정밀하게 연결(Block Reference)할 수 있습니다.
* 표시 텍스트 변경(Alias): `[[실제문서명|보여질이름]]` 형식을 사용하여 문맥에 맞는 유연한 링크 텍스트를 설정합니다.
* 자동 업데이트: 옵시디언 설정에서 위키 링크 형식을 사용하면 파일 이름 변경 시 이를 참조하는 모든 링크가 실시간으로 자동 수정되어 링크의 무결성을 유지합니다.

### Insight
* 옵시디언의 위키 링크는 백링크(Backlinks) 기능을 통해 해당 문서를 언급하는 다른 노드들을 역방향으로 추적할 수 있게 함으로써 지식의 발견 가능성(Discoverability)을 극대화합니다.
* 지식 그래프(Graph View)와 결합할 때 데이터 간의 클러스터링 현상을 시각적으로 파악할 수 있어, 향후 AI 에이전트의 검색 증강 생성(RAG)을 위한 구조화된 지식 베이스 구축에 매우 유리한 형태입니다.
* 데이터의 범용성과 호환성이 중요한 환경에서는 표준 마크다운 링크(Markdown Links)를 사용하되, 옵시디언 내부의 관리 효율성을 우선한다면 위키 링크 방식을 채택하는 것이 기술적으로 우위에 있습니다.

**출처:** [Internal links - Obsidian Help](https://help.obsidian.md/Linking+notes+and+files/Internal+links)
**출처:** [Wikilinks vs Markdown links - Obsidian Forum](https://forum.obsidian.md/t/wikilinks-vs-markdown-links/1154)
