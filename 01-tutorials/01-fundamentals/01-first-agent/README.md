# Strands Agents を始めよう

このガイドでは、Strands Agents の基本概念を理解し、最初のエージェントを実行できるようサポートします。

## 前提条件

- Python 3.10 以降
- 適切な権限で設定された AWS アカウント
- Python プログラミングの基礎知識

## インストール

pip を使用して Strands Agents とツールパッケージをインストールします：

```
pip install strands-agents strands-agents-tools
```

## 基本概念

Strands Agents は、AWS サービスと連携し、複雑なタスクを実行できる AI エージェントを構築するためのフレームワークです。主要なコンポーネントは以下の通りです：

1. **Agent（エージェント）**: 会話を管理し、ツールを調整するコアコンポーネント
2. **Model（モデル）**: エージェントを動かす基盤となる LLM（大規模言語モデル）
3. **Tools（ツール）**: エージェントが特定のタスクを実行するために使用できる関数
4. **Sessions and State（セッションと状態）**: インタラクション全体で会話履歴とエージェントの状態を維持する仕組み
5. **Agent Loop（エージェントループ）**: エージェントが入力を受け取り、処理し、応答を生成するプロセスフロー
6. **Context Management（コンテキスト管理）**: エージェントがメモリと検索を含む会話コンテキストを維持・管理する方法

## クイックスタートガイド

このディレクトリの `01-first-agent.ipynb` ノートブックは、以下のコード例を含む包括的なガイドを提供します：

1. **シンプルなエージェントの作成**: システムプロンプトを使用して基本的なエージェントを初期化する方法を学ぶ
2. **ツールの追加**: 組み込みツールとカスタムツールでエージェントを強化する方法を発見する
3. **ロギングの設定**: デバッグと監視のための適切なロギングをセットアップする
4. **エージェントのカスタマイズ**: 異なるモデルを選択し、そのパラメータを設定する

## サンプルの実行

このフォルダには、始めるのに役立つ入門ノートブックとシンプルなユースケースが含まれています：

1. **01-first-agent.ipynb**: 包括的なクイックスタートガイドとユースケースを含む Jupyter ノートブック。
ここでは以下を構築します：
![Architecture](./images/agent_with_tools.png)

そしてレシピエージェント：

![Architecture](./images/interactive_recipe_agent.png)


2. **02-simple-interactive-usecase/**: CLI 経由で実行するシンプルなインタラクティブな料理/レシピエージェントを含むディレクトリ。


インタラクティブエージェントを実行するには：

1. ディレクトリに移動：`cd 02-simple-interactive-usecase`
2. 必要なパッケージをインストール：`pip install -r requirements.txt`
3. スクリプトを実行：`python recipe_bot.py`

## リソース

- より詳細なガイドについては [Strands ドキュメント](https://strandsagents.com/latest/user-guide/quickstart/) を参照してください
- [Sessions and State](https://strandsagents.com/latest/user-guide/concepts/agents/sessions-state) について詳しく学ぶ
- [Agent Loop](https://strandsagents.com/latest/user-guide/concepts/agents/agent-loop/) を理解する
- [Context Management](https://strandsagents.com/latest/user-guide/concepts/agents/context-management/) を深く掘り下げる
- 事前実装されたツールについては [strands-agents-tools](https://github.com/strands-agents/tools) リポジトリをチェックしてください
- システムプロンプトをカスタマイズし、関連するツールを追加して、独自のタスク特化型エージェントを構築してみてください

Strands Agents で楽しく開発しましょう！🚀
```
