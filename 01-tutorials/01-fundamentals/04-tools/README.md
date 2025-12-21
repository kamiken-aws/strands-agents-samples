# Strands SDK でツールを作成する

このガイドでは、Strands Agents のツールを作成するさまざまな方法を説明します。

## ツールを作成する方法

### 1. `@tool` デコレーターの使用

ツールを作成する最も簡単な方法は、Python 関数に `@tool` デコレーターを使用することです：

```
from strands import tool

@tool
def my_tool(param1: str, param2: int) -> str:
    """
    このツールが何をするかの説明。
    
    Args:
        param1: 最初のパラメータの説明
        param2: 2 番目のパラメータの説明
        
    Returns:
        返されるものの説明
    """
    # ダミー実装
    return f"Result: {param1}, {param2}"
```

注：このアプローチは、Python の docstring を使用してツールを文書化し、型ヒントをパラメータ検証に使用します

### 2. TOOL_SPEC 辞書の使用

ツール定義をより細かく制御するには、TOOL_SPEC 辞書アプローチを使用できます：

```
from strands.types.tools import ToolResult, ToolUse
from typing import Any

TOOL_SPEC = {
    "name": "my_tool",
    "description": "このツールが何をするかの説明",
    "inputSchema": {
        "json": {
            "type": "object",
            "properties": {
                "param1": {
                    "type": "string",
                    "description": "最初のパラメータの説明"
                },
                "param2": {
                    "type": "integer",
                    "description": "2 番目のパラメータの説明",
                    "default": 2
                }
            },
            "required": ["param1"]
        }
    }
}

# 関数名はツール名と一致する必要があります
def my_tool(tool: ToolUse, **kwargs: Any) -> ToolResult:
    tool_use_id = tool["toolUseId"]
    param1 = tool["input"].get("param1")
    param2 = tool["input"].get("param2", 2)
    
    # ツールの実装
    result = f"Result: {param1}, {param2}"
    
    return {
        "toolUseId": tool_use_id,
        "status": "success",
        "content": [{"text": result}]
    }
```

このアプローチにより、入力スキーマ定義をより細かく制御できます。ここでは、成功状態とエラー状態の明示的な処理を定義できます。

注：これは Amazon Bedrock Converse API 形式に従います

#### 使用方法

次のように、関数を通じて、または別のファイルからツールをインポートすることもできます：

```
agent = Agent(tools=[my_tool])
# または 
agent = Agent(tools=["./my_tool.py"])
```

### 3. MCP（Model Context Protocol）ツールの使用

Model Context Protocol を使用して外部ツールを統合することもできます：

```
from mcp import StdioServerParameters, stdio_client
from strands.tools.mcp import MCPClient

# MCP サーバーに接続
mcp_client = MCPClient(
    lambda: stdio_client(
        StdioServerParameters(
            command="uvx", args=["awslabs.aws-documentation-mcp-server@latest"]
        )
    )
)

# エージェントでツールを使用
with mcp_client:
    tools = mcp_client.list_tools_sync()
    agent = Agent(tools=tools)
```

このアプローチは、MCP を通じて外部ツールプロバイダーに接続し、異なるサーバーからのツールを使用できるようにします。stdio と HTTP トランスポートの両方をサポートしています

## ベストプラクティス

1. **ツールの命名**: ツールには明確でわかりやすい名前を使用する
2. **ドキュメント**: ツールが何をするか、そのパラメータについて詳細な説明を提供する
3. **エラーハンドリング**: ツールに適切なエラーハンドリングを含める
4. **パラメータ検証**: 処理前に入力を検証する
5. **戻り値**: エージェントが理解しやすい構造化されたデータを返す

## 例

このディレクトリのサンプルノートブックを確認してください：
- [MCP ツールの使用](01-using-mcp-tools/mcp-agent.ipynb): エージェントで MCP ツールを統合する方法を学ぶ
- [カスタムツール](02-custom-tools/custom-tools-with-strands-agents.ipynb): カスタムツールを作成して使用する方法を学ぶ

詳細については、[Strands ツールドキュメント](https://strandsagents.com/0.1.x/user-guide/concepts/tools/python-tools/)を参照してください。
```
