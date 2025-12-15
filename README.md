<div align="center">
  <div>
    <a href="https://strandsagents.com">
      <img src="https://strandsagents.com/latest/assets/logo-github.svg" alt="Strands Agents" width="55px" height="105px">
    </a>
  </div>

  <h1>
    Strands Agents サンプル
  </h1>

  <h2>
    わずか数行のコードでAIエージェントを構築する、モデル駆動型のアプローチ
  </h2>

  <div align="center">
    <a href="https://github.com/strands-agents/samples/graphs/commit-activity"><img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/m/strands-agents/samples"/></a>
    <a href="https://github.com/strands-agents/samples/issues"><img alt="GitHub open issues" src="https://img.shields.io/github/issues/strands-agents/samples"/></a>
    <a href="https://github.com/strands-agents/samples/pulls"><img alt="GitHub open pull requests" src="https://img.shields.io/github/issues-pr/strands-agents/samples"/></a>
    <a href="https://github.com/strands-agents/samples/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/github/license/strands-agents/samples"/></a>
  </div>
  
  <p>
    <a href="https://strandsagents.com/">ドキュメント</a>
    ◆ <a href="https://github.com/strands-agents/samples">サンプル</a>
    ◆ <a href="https://github.com/strands-agents/sdk-python">Python SDK</a>
    ◆ <a href="https://github.com/strands-agents/sdk-typescript">TypeScript SDK</a> <img src="https://img.shields.io/badge/NEW-brightgreen" alt="New"/>
    ◆ <a href="https://github.com/strands-agents/tools">ツール</a>
    ◆ <a href="https://github.com/strands-agents/agent-builder">Agent Builder</a>
    ◆ <a href="https://github.com/strands-agents/mcp-server">MCP サーバー</a>
  </p>
</div>

Strands Agents サンプルリポジトリへようこそ！

<a href="https://strandsagents.com">Strands Agents</a> を始めるための、使いやすいサンプル集をご覧ください。

このリポジトリに含まれる例は、**デモンストレーションおよび教育目的**のみを意図しています。これらは概念や技術を示すものであり、**本番環境での直接的な使用を意図したものではありません**。本番環境で使用する前に、必ず適切な**セキュリティ**および**テスト**手順を適用してください。

## クイックスタート

<table>
<tr>
<td width="40%" valign="top">

### <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="24" height="24" alt="Python"/> Python

**前提条件:**
- Python 3.10 以上
- pip パッケージマネージャー
  - 確認コマンド: `pip --version` または `pip3 --version`
  - 通常、python.org の Python 3.4+ インストーラーに同梱されています
  - pip がない場合、以下のいずれかの方法でインストールしてください:
    ```
    # 方法 1 - Python の組み込みモジュールを使用
    python -m ensurepip --upgrade

    # 方法 2 - 公式インストーラーをダウンロードして実行
    curl [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py) -o get-pip.py
    python get-pip.py
    ```

**ステップ 1: 仮想環境の作成**
```
# 仮想環境を作成
python -m venv venv

# 仮想環境をアクティベート
# macOS/Linux の場合:
source venv/bin/activate
# Windows の場合:
venv\Scripts\activate
```

**ステップ 2: インストール**
```
pip install strands-agents strands-agents-tools
```

**最初のエージェント:**
```
from strands import Agent

agent = Agent()
response = agent("Hello! Tell me a joke.")
print(response)
```

[Python チュートリアルを見る →](./01-tutorials/)

</td>
<td width="60%" valign="top">

### <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" width="24" height="24" alt="TypeScript"/> TypeScript

**前提条件:**
- Node.js 18 以上
- npm または yarn パッケージマネージャー

**インストール:**
```
npm install @strands-agents/sdk
```

**最初のエージェント:**
```
import { Agent } from "@strands-agents/sdk";

async function main() {
    const agent = new Agent({
        systemPrompt: "You are a helpful assistant."
    });

    const response = await agent.invoke("Hello! Tell me a joke.");
    console.log(response.toString());
}

main();
```

[TypeScript チュートリアルを見る →](./typescript/01-tutorials/)

</td>
</tr>
</table>

### モデルプロバイダーの設定

モデルプロバイダーとモデルアクセスの設定については、[こちら](https://strandsagents.com/latest/user-guide/quickstart/#model-providers) の手順に従ってください。

## リポジトリの探索

### <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="20" height="20"/> Python サンプル

- **[01-tutorials](./01-tutorials/)** - 基礎、マルチエージェントシステム、デプロイメントをカバーする Jupyter ノートブック形式のチュートリアル
- **[02-samples](./02-samples/)** - 実世界のユースケースと業界別の例
- **[03-integrations](./03-integrations/)** - AWS サービスやサードパーティツールとの統合例
- **[04-UX-demos](./04-UX-demos/)** - ユーザーインターフェースを備えたフルスタックアプリケーション
- **[05-agentic-rag](./05-agentic-rag/)** - 高度な Agentic RAG パターン
- **[06-edge](./06-edge/)** - 物理 AI やロボティクスを含むエッジデバイス統合

### <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" width="20" height="20"/> TypeScript サンプル

- **[typescript/01-tutorials](./typescript/01-tutorials/)** - TypeScript SDK のステップバイステップチュートリアル

## コントリビューション ❤️

コントリビューションを歓迎します！以下の詳細については、[Contributing Guide (貢献ガイド)](CONTRIBUTING.md) をご覧ください:
- バグ報告と機能要望
- 開発環境のセットアップ
- プルリクエストによる貢献
- 行動規範 (Code of Conduct)
- セキュリティ問題の報告

## ライセンス

このプロジェクトは Apache License 2.0 の下でライセンスされています - 詳細は [LICENSE](LICENSE) ファイルをご覧ください。

## セキュリティ

詳細については [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) をご覧ください。
```
