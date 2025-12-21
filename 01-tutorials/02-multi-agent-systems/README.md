# マルチエージェントシステム

このチュートリアルセクションでは、Strands Agents SDK を使用してマルチエージェントシステムを構築するさまざまなアプローチを探ります。

## マルチエージェントシステムへのアプローチ

### 1. Agents as Tools（ツールとしてのエージェント）
[ドキュメントへのリンク](https://strandsagents.com/latest/user-guide/concepts/multi-agent/agents-as-tools/)

「Agents as Tools」パターンは、専門的な AI エージェントを他のエージェントが使用できる呼び出し可能な関数（ツール）としてラップする階層構造を作成します：

- **オーケストレーターエージェント**: ユーザーとのやり取りを処理し、専門エージェントにタスクを委譲
- **専門ツールエージェント**: オーケストレーターから呼び出されたときにドメイン固有のタスクを実行
- **主な利点**: 関心事の分離、階層的な委譲、モジュール型アーキテクチャ

実装では、`@tool` デコレーターを使用して専門エージェントを呼び出し可能な関数に変換します：

```
@tool
def research_assistant(query: str) -> str:
    """研究関連のクエリを処理して応答します。"""
    research_agent = Agent(system_prompt=RESEARCH_ASSISTANT_PROMPT)
    return str(research_agent(query))
```


### 2. Agent Swarms（エージェントスウォーム）
[ドキュメントへのリンク](https://strandsagents.com/latest/user-guide/concepts/multi-agent/swarm/)

エージェントスウォームは、協力して働く自律的な AI エージェントのコレクションを通じて集合知を活用します：

- **分散制御**: システム全体を指揮する単一のエージェントは存在しない
- **共有メモリ**: エージェントが洞察を交換して集合的知識を構築
- **調整メカニズム**: 協調的、競争的、またはハイブリッドアプローチ
- **通信パターン**: エージェントが互いに通信できるメッシュネットワーク

組み込みの `swarm` ツールにより実装が簡素化されます：

```
from strands import Agent
from strands_tools import swarm

agent = Agent(tools=[swarm])
result = agent.tool.swarm(
    task="Analyze this dataset and identify market trends",
    swarm_size=4,
    coordination_pattern="collaborative"
)
```

### 3. Agent Graphs（エージェントグラフ）
[ドキュメントへのリンク](https://strandsagents.com/latest/user-guide/concepts/multi-agent/graph/)

エージェントグラフは、明示的な通信経路を持つ相互接続された AI エージェントの構造化ネットワークを提供します：

- **ノード（エージェント）**: 専門的な役割を持つ個別の AI エージェント
- **エッジ（接続）**: エージェント間の通信経路を定義
- **トポロジーパターン**: スター型、メッシュ型、または階層構造

`agent_graph` ツールにより、洗練されたエージェントネットワークの作成が可能になります：

```
from strands import Agent
from strands_tools import agent_graph

agent = Agent(tools=[agent_graph])
agent.tool.agent_graph(
    action="create",
    graph_id="research_team",
    topology={
        "type": "star",
        "nodes": [
            {"id": "coordinator", "role": "team_lead"},
            {"id": "data_analyst", "role": "analyst"},
            {"id": "domain_expert", "role": "expert"}
        ],
        "edges": [
            {"from": "coordinator", "to": "data_analyst"},
            {"from": "coordinator", "to": "domain_expert"}
        ]
    }
)
```

### 4. Agent Workflows（エージェントワークフロー）
[ドキュメントへのリンク](https://strandsagents.com/latest/user-guide/concepts/multi-agent/workflow/)

エージェントワークフローは、明確な依存関係を持つ定義されたシーケンスで、複数の AI エージェント間でタスクを調整します：

- **タスク定義**: 各エージェントが達成する必要があることの明確な説明
- **依存関係管理**: 順次依存関係、並列実行、結合ポイント
- **情報フロー**: あるエージェントの出力を別のエージェントの入力に接続

`workflow` ツールは、タスクの作成、依存関係の解決、実行を処理します：

```
from strands import Agent
from strands_tools import workflow

agent = Agent(tools=[workflow])
agent.tool.workflow(
    action="create",
    workflow_id="data_analysis",
    tasks=[
        {
            "task_id": "data_extraction",
            "description": "Extract key data from the report"
        },
        {
            "task_id": "analysis",
            "description": "Analyze the extracted data",
            "dependencies": ["data_extraction"]
        }
    ]
)
```

## 適切なアプローチの選択

- **Agents as Tools**: 専門知識を持つ明確な階層構造に最適
- **Agent Swarms**: 創発的な知性を伴う協調的な問題解決に理想的
- **Agent Graphs**: 通信パターンの正確な制御に最適
- **Agent Workflows**: 明確な依存関係を持つ順次プロセスに最適

各アプローチは、複雑さ、制御、協調パターンの面で異なるトレードオフを提供します。適切な選択は、特定のユースケースと要件によって異なります。
```
