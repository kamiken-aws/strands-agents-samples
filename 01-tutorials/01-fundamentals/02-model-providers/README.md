## Strands Agents のモデルプロバイダー

Strands Agents は複数のモデルプロバイダーをサポートしており、特定のユースケースに最適な基盤モデルを選択できます。この柔軟性により、さまざまな機能、コスト構造、デプロイメントオプションを活用できるエージェントを構築できます。
サポートされているモデルプロバイダー

Strands Agents は、すぐに使えるいくつかのモデルプロバイダーをサポートしています：

1. [**AWS Bedrock**](https://strandsagents.com/0.1.x/user-guide/concepts/model-providers/amazon-bedrock/): Amazon のマネージドサービスを使用して、Anthropic、Meta、Amazon などの主要プロバイダーの高性能基盤モデルにアクセス
2. [**Anthropic**](https://strandsagents.com/0.1.x/user-guide/concepts/model-providers/anthropic/): Claude モデルへの直接 API アクセス
3. [**Ollama**](https://strandsagents.com/0.1.x/user-guide/concepts/model-providers/ollama/): プライバシーやオフライン使用のためにモデルをローカルで実行
4. [**LiteLLM**](https://strandsagents.com/0.1.x/user-guide/concepts/model-providers/litellm/): OpenAI、Mistral、その他のプロバイダーのための統一インターフェース
    - **Azure OpenAI**: Microsoft Azure でホストされている OpenAI モデルを活用
    - **OpenAI**: GPT-4 / gpt-4o などのモデルにアクセス
5. [**カスタムプロバイダー**](https://strandsagents.com/0.1.x/user-guide/concepts/model-providers/custom_model_provider/): 専門的なニーズに対応する独自のプロバイダーを構築

## モデルチュートリアル
このフォルダには 2 つのサンプルがあります：

1. [Ollama](./01-ollama-model/):
シンプルな操作（`file_read`、`file_write`、`list_directory`）を実行できるローカルファイル操作エージェントの例を紹介します。より多くのツールを追加して機能を拡張できます。

アーキテクチャは以下の通りです：
![Architecture](./01-ollama-model/images/architecture.png)

2. [LiteLLM 経由の OpenAI アクセス](./02-openai-model/):
Azure 経由で OpenAI モデルにアクセスできるシンプルなエージェントの例を示します。`current_weather` や `current_time` などの基本的なツールにアクセスできます。

アーキテクチャは以下の通りです：
![Architecture](./02-openai-model/images/architecture.png)


注：すべてのモデルに対して、エージェント実行中にイベントをインターセプトして処理できる [Callback Handlers](https://strandsagents.com/0.1.x/user-guide/concepts/streaming/callback-handlers/) を追加できます。
```
