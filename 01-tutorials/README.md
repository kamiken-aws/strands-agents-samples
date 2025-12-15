# Strands SDK 入門

このフォルダでは、Strands Agents の様々な機能の始め方を解説する Jupyter Notebook の例を提供します。

## 基礎
| 例 | 説明 | 紹介する機能 |
|---|---|---|
| F1 | [最初の Strands エージェントの作成](01-fundamentals/01-first-agent) | エージェントの初期化、デフォルトツールの使用、カスタムツールの作成 |
| F2 | [モデルプロバイダー - OpenAI](01-fundamentals/02-model-providers/02-openai-model) | GPT 4.0 をモデルとしてエージェントを作成 |
| F2 | [モデルプロバイダー - Ollama](01-fundamentals/02-model-providers/01-ollama-model) | Ollama モデルでエージェントを作成 |
| F3 | [AWS サービスとの接続](01-fundamentals/03-connecting-with-aws-services) | Amazon Bedrock Knowledge Base および Amazon DynamoDB への接続 |
| F4 | [ツール - MCP ツールの使用](01-fundamentals/04-tools/01-using-mcp-tools) | エージェントへの MCP ツール呼び出しの統合 |
| F4 | [ツール - カスタムツール](01-fundamentals/04-tools/02-custom-tools) | エージェントでのカスタムツールの作成と使用 |
| F5 | [エージェントからのストリーミング応答](01-fundamentals/05-streaming-agent-response) | 非同期イテレータまたはコールバック (ストリームハンドラ) を使用したエージェント応答のストリーミング |
| F6 | [Bedrock Guardrail の統合](01-fundamentals/06-guardrail-integration) | Amazon Bedrock Guardrail をエージェントに統合 |
| F7 | [エージェントへのメモリの追加](01-fundamentals/07-memory-persistent-agents) | メモリと Web 検索ツールを使用したパーソナルアシスタント |
| F8 | [オブザーバビリティ (可観測性) と評価](01-fundamentals/08-observability-and-evaluation) | エージェントへのオブザーバビリティと評価の追加 |

## マルチエージェントシステム
| 例 | 説明 | 紹介する機能 |
|---|---|---|
| M1 | [ツールとしてのエージェントの使用](02-multi-agent-systems/01-agent-as-tool) | エージェントをツールとして使用したマルチエージェント連携の例を作成 |
| M2 | [Swarm (群知能) エージェントの作成](02-multi-agent-systems/02-swarm-agent) | 連携して動作する複数の AI エージェントで構成されるマルチエージェントシステムを作成 |
| M3 | [Graph エージェントの作成](02-multi-agent-systems/03-graph-agent) | 定義された通信パターンを持つ、専門化された AI エージェントの構造化ネットワークを作成 |

## デプロイメント
| 例 | 説明 | 紹介する機能 |
|---|---|---|
| D1 | [AWS Lambda デプロイメント](03-deployment/01-lambda-deployment) | エージェントを AWS Lambda 関数にデプロイ |
| D2 | [AWS Fargate デプロイメント](03-deployment/02-fargate-deployment) | エージェントを AWS Fargate にデプロイ |
```
