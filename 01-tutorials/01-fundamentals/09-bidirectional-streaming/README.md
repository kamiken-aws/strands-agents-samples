# Strands BidiAgent を始めよう

## 双方向ストリーミングエージェントとは？

双方向ストリーミングエージェントは、AI モデルとのリアルタイムな双方向の音声会話を可能にします。従来のリクエスト-レスポンスパターンとは異なり、これらのエージェントは：

- **双方向の音声ストリーミング** - 自然に話しながら、エージェントが聞いて音声で応答
- **割り込みのサポート** - 実際の会話のように、いつでも割り込むことができる
- **リアルタイムでのツール実行** - エージェントは会話の流れを維持しながら関数（計算や検索など）を呼び出すことができる
- **ライブ文字起こし** - あなたが言ったことやエージェントが言っていることをリアルタイムで確認

これらのサンプルは、AWS Nova Sonic、Google Gemini Live、OpenAI Realtime API などのモデルを使用して、Strands で音声対応 AI エージェントを構築する方法を示しています。

```
from strands.experimental.bidi.agent import BidiAgent
from strands.experimental.bidi.models.nova_sonic import BidiNovaSonicModel
from strands_tools import calculator

# ツールを持つ音声対応エージェントを作成
agent = BidiAgent(
    model=BidiNovaSonicModel(
        region="us-east-1",
        model_id="amazon.nova-sonic-v1:0",
        provider_config={
            "audio": {
                "input_sample_rate": 16000,
                "output_sample_rate": 16000,
                "voice": "matthew"
            }
        }
    ),
    tools=[calculator],
    system_prompt="You are a helpful voice assistant."
)

# ストリーミング会話を開始
await agent.run(inputs=[...], outputs=[...])
```

## アーキテクチャ

```
ブラウザ (HTML/JS) ←→ WebSocket ←→ BidiAgent ←→ AI モデル
```

- ブラウザがマイクの音声をキャプチャし、base64 PCM にエンコード
- WebSocket が双方向に音声イベントを転送
- BidiAgent が音声を処理してツールを実行
- レスポンスが音声 + 文字起こしとしてストリーミングで返される

## インストール

### 前提条件

- Python 3.12+
- pip または uv パッケージマネージャー

### セットアップ

1. **仮想環境を作成**
```
python -m venv .venv

# Mac または Linux
source .venv/bin/activate  

# Windows の場合: 
.venv\Scripts\activate
```

2. **依存関係をインストール**
```
pip install -r requirements.txt
```

または直接インストール：
```
pip install fastapi uvicorn strands-agents[bidi-all] strands-agents-tools
```

3. **認証情報を設定**（使用するモデル用）

**AWS Nova Sonic の場合：**
```
export AWS_ACCESS_KEY_ID="your-key"
export AWS_SECRET_ACCESS_KEY="your-secret"
export AWS_SESSION_TOKEN="your-token"  # 一時的な認証情報を使用する場合
```

**Google Gemini Live の場合：**
```
export GOOGLE_API_KEY="your-key"
```

**OpenAI Realtime の場合：**
```
export OPENAI_API_KEY="your-key"
```

## 使用方法

### WebSocket デモ（推奨）

WebSocket サーバーを起動して自動的にブラウザを開きます：

```
# デフォルトポート: 8000
python websocket_example.py
```

またはカスタムポートを指定：

```
python websocket_example.py 8080
```

ブラウザが自動的に `http://localhost:8000`（または指定したポート）を開きます。

**ブラウザでの操作：**
1. ドロップダウンから希望の AI モデルを選択
2. 「🚀 Start Session」をクリックして接続と録音を開始
3. 自然に話す - 「25 かける 8 は何？」と試してみる
4. エージェントが音声で応答し、文字起こしを表示
5. エージェントが話している間に話すことで割り込むことができる
6. 「🛑 End Session」をクリックして停止

### コマンドラインテスト

コマンドラインから個々のモデルを直接テスト：

> **⚠️ 重要：** コマンドラインテストは PyAudio を使用しており、エコーキャンセレーション機能は**ありません**。音声フィードバックループを防ぐために、ヘッドセットを**必ず**使用してください。スピーカーで最高の体験を得るには、ブラウザでエコーキャンセレーションが有効になっている WebSocket デモを使用してください。

**Nova Sonic：**
```
python test_simple_novasonic.py
```

**Gemini Live：**
```
python test_simple_gemini.py
```

**OpenAI Realtime：**
```
python test_simple_openai.py
```

## プロジェクト構成

```
.
├── websocket_example.py      # FastAPI WebSocket サーバー
├── websocket_client.html     # モダンな Web UI クライアント
├── test_simple_novasonic.py  # Nova Sonic CLI テスト
├── test_simple_gemini.py     # Gemini Live CLI テスト
├── test_simple_openai.py     # OpenAI Realtime CLI テスト
├── requirements.txt          # Python 依存関係
└── README.md                 # このファイル
```

## WebSocket イベント

### クライアント → サーバー

- `bidi_audio_input` - マイクからの PCM 音声チャンク
  - フォーマット: base64 エンコードされた PCM
  - サンプルレート: モデル固有（16kHz または 24kHz）
  - チャンネル: 1（モノラル）

### サーバー → クライアント

- `bidi_audio_stream` - エージェントからの PCM 音声レスポンス
- `bidi_transcript_stream` - リアルタイム文字起こし（ユーザー/アシスタント）
- `bidi_interruption` - ユーザーが割り込んだときの通知
- `tool_use_stream` - ツール実行開始
- `tool_result` - ツール実行結果

## イベントフォーマット

### クライアント → サーバーイベント

**bidi_audio_input**
```
{
  "type": "bidi_audio_input",
  "audio": "base64-encoded-pcm-data..."
}
```
マイクからの音声チャンクを base64 エンコードされた PCM として送信します。サンプルレートはモデルの要件に一致する必要があります（Nova Sonic は 16kHz、Gemini/OpenAI は 24kHz）。

### サーバー → クライアントイベント

**bidi_audio_stream**
```
{
  "type": "bidi_audio_stream",
  "audio": "base64-encoded-pcm-data..."
}
```
エージェントからの音声レスポンスを受信します。デコードしてスピーカーで再生します。

**bidi_transcript_stream**
```
{
  "type": "bidi_transcript_stream",
  "role": "user",  // または "assistant"
  "text": "What is 25 times 8?"
}
```
ユーザーの発話とアシスタントのレスポンスの両方のリアルタイム文字起こし。

**bidi_interruption**
```
{
  "type": "bidi_interruption"
}
```
ユーザーがエージェントを割り込んだときに送信されます。現在の音声の再生を停止し、バッファをクリアします。

**tool_use_stream**
```
{
  "type": "tool_use_stream",
  "tool_name": "calculator",
  "tool_input": {"operation": "multiply", "a": 25, "b": 8}
}
```
エージェントがツールを実行していることの通知。

**tool_result**
```
{
  "type": "tool_result",
  "tool_name": "calculator",
  "result": 200
}
```
ツール実行から返された結果。

## 文字起こしバッファリング

- **Nova Sonic**: 文字起こしを即座に表示（そのままで適切に動作）
- **Gemini & OpenAI**: 短い文字起こしチャンクを 1 秒間バッファリングしてから表示
  - 複数の小さな更新を一貫性のあるメッセージにグループ化
  - チャンクが到着すると同時にリアルタイムで更新
  - より綺麗で読みやすい会話フローを作成

## 開発

### 新しいツールの追加

ツールは `websocket_example.py` の `tools` パラメータに追加できます。エージェントにはすでに calculator ツールが設定されています：

```
from strands_tools import calculator

agent = BidiAgent(
    model=model,
    tools=[calculator],
    system_prompt="You are a helpful assistant with access to a calculator tool.",
)
```

`strands_tools` から追加のツールを追加したり、Strands ツール仕様に従ってカスタムツールを作成したりできます。

## イベントフォーマットリファレンス

このセクションでは、クライアントとサーバー間で交換されるすべての WebSocket イベントの詳細な仕様を提供します。

### クライアント → サーバーイベント

**bidi_audio_input**

マイクからの音声チャンクをエージェントに送信します。

```
{
  "type": "bidi_audio_input",
  "audio": "base64-encoded-pcm-data...",
  "format": "pcm",
  "sample_rate": 16000,
  "channels": 1
}
```

- `audio`: Base64 エンコードされた PCM 音声データ
- `format`: 常に "pcm"（16 ビット符号付き整数）
- `sample_rate`: Nova Sonic は 16000、Gemini/OpenAI は 24000
- `channels`: 常に 1（モノラル）

### サーバー → クライアントイベント

**bidi_audio_stream**

エージェントからの音声レスポンスをクライアントにストリーミングします。

```
{
  "type": "bidi_audio_stream",
  "audio": "base64-encoded-pcm-data...",
  "format": "pcm",
  "sample_rate": 16000,
  "channels": 1
}
```

- `audio`: スピーカーで再生する base64 エンコードされた PCM 音声データ
- `format`: 常に "pcm"（16 ビット符号付き整数）
- `sample_rate`: Nova Sonic は 16000、Gemini/OpenAI は 24000
- `channels`: 常に 1（モノラル）

**bidi_transcript_stream**

ユーザーの発話とアシスタントのレスポンスの両方のリアルタイム文字起こしを提供します。

```
{
  "type": "bidi_transcript_stream",
  "delta": {
    "text": "partial text..."
  },
  "text": "complete text so far",
  "role": "user",
  "is_final": false
}
```

- `delta`: 増分テキスト更新（追加された新しい単語）
- `text`: これまでに蓄積された完全な文字起こしテキスト
- `role`: "user" または "assistant"
- `is_final`: これが最終的な文字起こしかどうかを示すブール値

**bidi_interruption**

ユーザーがエージェントの発話を割り込んだことを通知します。

```
{
  "type": "bidi_interruption"
}
```

受信時、クライアントは以下を行う必要があります：
- 現在の音声を即座に停止
- 音声再生バッファをクリア
- オーディオコンテキストをリセット

**bidi_usage**

会話のトークン使用統計を報告します。

```
{
  "type": "bidi_usage",
  "inputTokens": 22,
  "outputTokens": 15,
  "totalTokens": 37
}
```

- `inputTokens`: ユーザー入力のトークン数
- `outputTokens`: エージェントレスポンスのトークン数
- `totalTokens`: 入力と出力のトークンの合計

**tool_use_stream**

エージェントがツールを実行していることを通知します。

```
{
  "type": "tool_use_stream",
  "current_tool_use": {
    "name": "calculator",
    "input": {
      "operation": "multiply",
      "a": 25,
      "b": 8
    }
  }
}
```

- `current_tool_use.name`: 実行されているツールの名前
- `current_tool_use.input`: ツールに渡されるパラメータ

**tool_result**

ツール実行からの結果を返します。

```
{
  "type": "tool_result",
  "tool_result": {
    "content": [
      {
        "text": "200"
      }
    ]
  }
}
```

- `tool_result.content`: 結果オブジェクトの配列
- `tool_result.content[].text`: 結果の文字列表現

### 音声フォーマットの詳細

すべての音声は base64 エンコードされた PCM（Pulse Code Modulation）として送信されます：

- **エンコーディング**: 16 ビット符号付き整数、リトルエンディアン
- **チャンネル**: 1（モノラル）
- **サンプルレート**: モデル依存
  - Nova Sonic: 16 kHz
  - Gemini Live: 24 kHz
  - OpenAI Realtime: 24 kHz

JavaScript で base64 音声をデコードするには：

```
// base64 をバイトにデコード
const binaryString = atob(base64Audio);
const bytes = new Uint8Array(binaryString.length);
for (let i = 0; i < binaryString.length; i++) {
    bytes[i] = binaryString.charCodeAt(i);
}

// Web Audio API 用に Int16 から Float32 に変換
const int16Data = new Int16Array(bytes.buffer);
const float32Data = new Float32Array(int16Data.length);
for (let i = 0; i < int16Data.length; i++) {
    float32Data[i] = int16Data[i] / 32768.0;
}
```
```
