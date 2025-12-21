# Swarm マルチエージェントパターン

## 概要

Swarm は、複数のエージェントがチームとして協力して複雑なタスクを解決する、協調的なエージェントオーケストレーションシステムです。従来の順次型または階層型のマルチエージェントシステムとは異なり、Swarm は共有コンテキストと作業メモリを持つエージェント間の自律的な調整を可能にします。

Swarm の例を以下に示します：

![Architecture](./images/swarm_example.png)

## 主な機能

- 共有作業メモリを持つ**自己組織化エージェントチーム**
- エージェント間の**ツールベースの調整**
- 中央制御なしの**自律的なエージェント協調**
- エージェントの能力に基づく**動的なタスク分散**
- 共有コンテキストによる**集合知**
- テキスト、画像、その他のコンテンツタイプを処理するための**マルチモーダル入力サポート**

## Swarm の作成

Swarm を作成するには、異なる専門性を持つエージェントのコレクションを定義する必要があります：

```
import logging
from strands import Agent
from strands.multiagent import Swarm

# デバッグログを有効にして stderr に出力
logging.getLogger("strands.multiagent").setLevel(logging.DEBUG)
logging.basicConfig(
    format="%(levelname)s | %(name)s | %(message)s",
    handlers=[logging.StreamHandler()]
)

# 専門エージェントを作成
researcher = Agent(name="researcher", system_prompt="You are a research specialist...")
coder = Agent(name="coder", system_prompt="You are a coding specialist...")
reviewer = Agent(name="reviewer", system_prompt="You are a code review specialist...")
architect = Agent(name="architect", system_prompt="You are a system architecture specialist...")

# これらのエージェントで Swarm を作成
swarm = Swarm(
    [researcher, coder, reviewer, architect],
    max_handoffs=20,
    max_iterations=20,
    execution_timeout=900.0,  # 15 分
    node_timeout=300.0,       # エージェントあたり 5 分
    repetitive_handoff_detection_window=8,  # 直近 8 回のハンドオフに 3 つ以上のユニークなエージェントが必要
    repetitive_handoff_min_unique_agents=3
)

# タスクで Swarm を実行
result = swarm("Design and implement a simple REST API for a todo app")

# 最終結果にアクセス
print(f"Status: {result.status}")
print(f"Node history: {[node.node_id for node in result.node_history]}")
```

## マルチモーダル入力サポート

Swarm は ContentBlocks を使用して、テキストや画像などのマルチモーダル入力をサポートします：

```
from strands import Agent
from strands.multiagent import Swarm
from strands.types.content import ContentBlock

# 画像処理ワークフロー用のエージェントを作成
image_analyzer = Agent(name="image_analyzer", system_prompt="You are an image analysis expert...")
report_writer = Agent(name="report_writer", system_prompt="You are a report writing expert...")

# Swarm を作成
swarm = Swarm([image_analyzer, report_writer])

# テキストと画像を含むコンテンツブロックを作成
content_blocks = [
    ContentBlock(text="Analyze this image and create a report about what you see:"),
    ContentBlock(image={"format": "png", "source": {"bytes": image_bytes}}),
]

# マルチモーダル入力で Swarm を実行
result = swarm(content_blocks)
```

## ツールとしての Swarm

エージェントは `swarm` ツールを使用して、動的に Swarm を作成およびオーケストレーションできます：

```
from strands import Agent
from strands_tools import swarm

agent = Agent(tools=[swarm], system_prompt="Create a swarm of agents to solve the user's query.")

agent("Research, analyze, and summarize the latest advancements in quantum computing")
```

## Swarm の設定

Swarm コンストラクタでは、動作と安全性のパラメータを制御できます：

| パラメータ | 説明 | デフォルト |
|-----------|-------------|---------|
| `max_handoffs` | 許可されるエージェントハンドオフの最大数 | 20 |
| `max_iterations` | すべてのエージェント全体の最大反復回数 | 20 |
| `execution_timeout` | 総実行タイムアウト（秒） | 900.0（15 分） |
| `node_timeout` | 個別エージェントのタイムアウト（秒） | 300.0（5 分） |
| `repetitive_handoff_detection_window` | ピンポン動作をチェックする最近のノード数 | 0（無効） |
| `repetitive_handoff_min_unique_agents` | 最近のシーケンスで必要な最小ユニークノード数 | 0（無効） |

## Swarm 調整ツール

Swarm を作成すると、各エージェントには自動的に調整用の特殊なツールが装備されます：

### ハンドオフツール

エージェントは、専門的な支援が必要な場合に、制御を別のエージェントに転送できます：

```
handoff_to_agent(
    agent_name="coder",
    message="I need help implementing this algorithm in Python",
    context={"algorithm_details": "..."}
)
```

## 非同期実行

Swarm を非同期で実行することもできます：

```
import asyncio

async def run_swarm():
    result = await swarm.invoke_async("Design and implement a complex system...")
    return result

result = asyncio.run(run_swarm())
```

## Swarm の結果

Swarm が実行を完了すると、詳細な情報を含む SwarmResult オブジェクトが返されます：

```
result = swarm("Design a system architecture for...")

# 実行ステータスを確認
print(f"Status: {result.status}")  # COMPLETED, FAILED など

# 関与したエージェントを確認
for node in result.node_history:
    print(f"Agent: {node.node_id}")

# 特定のノードから結果を取得
analyst_result = result.results["analyst"].result
print(f"Analysis: {analyst_result}")

# パフォーマンスメトリクスを取得
print(f"Total iterations: {result.execution_count}")
print(f"Execution time: {result.execution_time}ms")
print(f"Token usage: {result.accumulated_usage}")
```

## 安全機構

Swarm には、無限ループを防ぎ、信頼性の高い実行を保証するためのいくつかの安全機構が含まれています：

1. **最大ハンドオフ数**: エージェント間で制御を転送できる回数を制限
2. **最大反復回数**: Swarm の総実行反復回数の上限を設定
3. **実行タイムアウト**: Swarm の最大総実行時間を設定
4. **ノードタイムアウト**: 単一エージェントの実行時間を制限
5. **繰り返しハンドオフ検出**: エージェントが制御を無限に往復させることを防止

## ベストプラクティス

1. **専門エージェントを作成する**: Swarm 内の各エージェントに明確な役割を定義する
2. **わかりやすいエージェント名を使用する**: 名前はエージェントの専門性を反映すべき
3. **適切なタイムアウトを設定する**: タスクの複雑さと予想実行時間に基づいて調整する
4. **繰り返しハンドオフ検出を有効にする**: ピンポン動作を防ぐために `repetitive_handoff_detection_window` と `repetitive_handoff_min_unique_agents` に適切な値を設定する
5. **多様な専門知識を含める**: Swarm に補完的なスキルを持つエージェントがいることを確認する
6. **エージェントの説明を提供する**: 他のエージェントが能力を理解できるように、エージェントに説明を追加する
7. **マルチモーダル入力を活用する**: 画像を含むリッチな入力には ContentBlocks を使用する

## 使用すべき場面

- 複数の視点を必要とする複雑な問題解決
- 創造的なアイデア創出とブレインストーミング
- 包括的な研究と分析
- 複数の基準による意思決定
- 異なる領域からの専門知識を必要とするタスク

詳細については、[ドキュメント](https://strandsagents.com/latest/documentation/docs/user-guide/concepts/multi-agent/swarm/)をお読みください。
```
