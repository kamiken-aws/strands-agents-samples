# 基礎

このフォルダには、AWS Strands を使用して AI エージェントを構築するための基本概念をカバーする一連のチュートリアルが含まれています。各チュートリアルは前のチュートリアルの上に構築され、洗練された AI エージェントを作成するのに役立つ主要な概念と機能を紹介します。

## 構成

| 例 | 説明                                                                  | 紹介される機能                                                                                   |
|---------|------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| F1      | [最初の Strands Agent を作成](./01-first-agent)                             | エージェントの初期化、デフォルトツールの使用、カスタムツールの作成                              |
| F2      | [モデルプロバイダー - Ollama](./02-model-providers/01-ollama-model)             | Ollama モデルでエージェントを作成                                                                       |
| F2      | [モデルプロバイダー - OpenAI](./02-model-providers/02-openai-model)             | GPT 4.0 をモデルとしてエージェントを作成                                                                   |
| F3      | [AWS サービスとの接続](./03-connecting-with-aws-services)            | Amazon Bedrock Knowledge Base と Amazon DynamoDB への接続                                      |
| F4      | [ツール - MCP ツールの使用](./04-tools/01-using-mcp-tools)                     | エージェントへの MCP ツール呼び出しの統合                                                           |
| F4      | [ツール - カスタムツール](./04-tools/02-custom-tools)                           | エージェントでカスタムツールを作成して使用                                                      |
| F5      | [エージェントからのストリーミングレスポンス](./05-streaming-agent-response)               | Async Iterators または Callbacks（Stream Handlers）を使用したエージェントのレスポンスのストリーミング                 |
| F6      | [Bedrock Guardrail の統合](./06-guardrail-integration)                  | エージェントに Amazon Bedrock Guardrail を統合                                                  |
| F7      | [エージェントへのメモリの追加](./07-memory-persistent-agents)                 | メモリと Web 検索ツールを使用したパーソナルアシスタント                                                  |
| F8      | [可観測性と評価](./08-observability-and-evaluation)            | エージェントに可観測性と評価を追加                                                    |
| F9      | [双方向ストリーミングエージェント](./09-bidirectional-streaming)                | リアルタイム音声チャットを可能にする双方向ストリーミングエージェント
```
