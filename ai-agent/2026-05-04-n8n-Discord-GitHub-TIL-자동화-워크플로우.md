### Context
디스코드(Discord)로 작성한 간단한 메모를 n8n 워크플로우를 통해 체계적인 기술 문서(TIL)로 변환하고, 이를 깃허브(GitHub) 저장소에 자동으로 게시하는 자동화 시스템을 구축했습니다. 수동으로 파일을 생성하고 인덱싱하는 번거로움을 해결하기 위해 AI 에이전트(AI Agent)와 깃허브 API 연동을 활용했습니다.

### Core
이 워크플로우는 데이터의 수집, 가공, 저장, 피드백의 4단계로 구성됩니다.

* **Trigger (Discord Node)**: 디스코드 채널에 메시지가 생성되면 워크플로우가 시작됩니다. 특정 채널 ID를 필터링하여 불필요한 실행을 방지합니다.
* **Processing (AI Agent Node)**: 전송된 텍스트를 LLM(Claude 또는 GPT)에게 전달합니다. 미리 정의된 프롬프트에 따라 시스템은 메시지를 'Context, Core, Insight' 구조의 마크다운 형식으로 변환합니다.
* **Storage (GitHub Node)**:
    * **Create File**: 변환된 내용을 `YYYY-MM-DD-제목.md` 형식의 새 파일로 지정된 폴더에 생성합니다.
    * **Update README**: 기존 `README.md` 파일을 불러온 후, 표현식(Expression)을 사용하여 새로운 TIL 파일의 링크를 목록 상단에 추가하고 다시 업데이트합니다.
* **Feedback (Discord Node)**: 모든 작업이 완료되면 처리 결과와 깃허브 커밋 링크를 사용자에게 전송하여 확인을 돕습니다.

```javascript
// README.md 업데이트를 위한 로직 예시 (n8n Expression)
const currentContent = $node["GitHub: Get README"].json["content"];
const newLink = `* [${$node["AI Agent"].json["title"]}](${$node["GitHub: Create File"].json["url"]})`;
return currentContent.replace("## TIL List", "## TIL List\n" + newLink);
```

### Insight
n8n을 처음 사용하며 얻은 핵심적인 깨달음은 '데이터 흐름의 시각화'입니다. 각 노드(Node)가 받아들이는 입력값(Input)과 출력하는 데이터(Output)의 구조를 정확히 파악하는 것만으로도 복잡한 로직을 쉽게 설계할 수 있습니다. 특히 깃허브 노드와 같이 이전 노드의 실행 결과를 참조해야 하는 경우, JSON 데이터 구조를 미리 확인하는 과정이 필수적입니다. 또한, AI 에이전트 노드를 활용할 때 명확한 출력 스키마를 정의하면 후속 노드에서 데이터를 핸들링하기가 훨씬 수월해집니다.

**출처:** [n8n GitHub Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.github/)
**출처:** [n8n Discord Trigger Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.discordtrigger/)
**출처:** [GitHub API Reference - Create or Update File Contents](https://docs.github.com/en/rest/repos/contents?apiVersion=2022-11-28#create-or-update-file-contents)