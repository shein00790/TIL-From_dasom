### Context
* 사용자는 로컬 LLM인 Hermes(주로 OpenHermes 시리즈) 모델의 설치와 기초 학습을 완료한 상태임.
* 이를 지식 관리 도구인 옵시디언(Obsidian)과 결합하여 파편화된 기술 메모를 체계적으로 구조화하고 자동화된 워크플로우를 구축하려는 기술적 수요가 발생함.
* Hermes 모델의 우수한 지시 이행(Instruction Following) 능력을 활용하여 옵시디언 내 마크다운 데이터를 정제하는 것이 핵심 목표임.

### Core
* 로컬 환경에서 Hermes 모델을 구동하고 옵시디언과 통신하기 위한 기술적 설정 예시임.
* Ollama를 통한 모델 실행 및 옵시디언 'Text Generator' 또는 'Smart Connections' 플러그인 연결 로직을 포함함.

```bash
# 1. Ollama를 이용한 Hermes 모델 실행 (CLI)
ollama run openhermes

# 2. Obsidian Local REST API 또는 커뮤니티 플러그인 설정 예시
# 플러그인 설정 내 LLM Provider를 'Ollama'로 지정
# API Endpoint: http://localhost:11434/api/generate
# Model: openhermes
```

* 기술 노트 자동 요약 프롬프트 구조 (Hermes 최적화):
```markdown
* 시스템 역할: 너는 기술 문서 편집 전문가이다.
* 작업: 입력된 원시 메모를 옵시디언 TIL 형식으로 재구성하라.
* 제약 사항: 모든 출력은 마크다운 형식을 따르며 핵심 개념은 불렛 포인트로 정리한다.
```

### Insight
* Hermes 모델은 Mistral 또는 Llama 3 기반으로 튜닝되어 단순 요약을 넘어 논리적인 구조 설계에 강점이 있음.
* 옵시디언의 로컬 우선(Local-first) 철학과 로컬 LLM인 Hermes의 결합은 데이터 보안을 유지하면서도 고도화된 AI 비서를 구축할 수 있는 최적의 조합임.
* 특히 태그 자동 생성 및 관련 문서 링크(Backlink) 추천 기능 구현 시 Hermes의 문맥 이해도가 타 모델 대비 우수함을 검증함.

**출처:** [OpenHermes 2.5 - Nous Research](https://huggingface.co/NousResearch/OpenHermes-2.5-Mistral-7B)
**출처:** [Obsidian Smart Connections Plugin Documentation](https://github.com/brianpetro/obsidian-smart-connections)
**출처:** [Ollama Library - Hermes Models](https://ollama.com/library/openhermes)
