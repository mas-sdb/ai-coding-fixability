# python 診断結果サンプル
- モデル：GPT-5.2
- 入力データ

```
## 入力として与える情報
- 言語名  
  Python

- 言語バージョン  
  Python 3.12

- 使用する主要ライブラリ  
  LangChain  
  OpenAI API クライアント  
  FastAPI（API サーバー）  
  Uvicorn（開発用サーバー）  
  python-dotenv（環境変数管理）

- 実行環境  
  Linux/Azure App Service
  FastAPI + Uvicorn による Web API 実行  
  OpenAI API への外部接続

- プロジェクトのアーキテクチャ概要  
  API 層（FastAPI でエンドポイント提供）  
  FAQ データ読み込み層（JSON / YAML / CSV など）  
  LangChain によるプロンプトテンプレートとチェーン構築  
  OpenAI モデルを使った回答生成  
  シンプルな三層構造（API → チェーン → モデル）
```

# 診断結果
Community Consistency

AI の誤推論ポイント：
- LangChain 周辺は「推奨される書き方」が短期間で変わり、古い流儀（`LLMChain` 前提、`run()` 前提、Memory/Agent の旧APIなど）を混ぜた提案になりやすい
- OpenAI クライアントも世代差があり、コミュニティ記事が `openai.ChatCompletion.create` 系（旧）と `OpenAI().responses.create`/`client.chat.completions.create` 系（新）が混在しやすい
- FastAPI は Pydantic v1/v2 の流儀差が大きく、回答がどちら前提かブレやすい

AI に与えるべき対策：
- 「採用している LangChain の書き方（LCEL を使う/使わない、Runnable 系を使う等）」を明記する
- 「OpenAI Python SDK のメジャーバージョン（旧openaiか新SDKか）」と、使用するAPI種別（Chat Completions / Responses）を明記する
- FastAPI が依存する Pydantic の世代（v2前提など）と、モデル定義・バリデーション方針を明記する


Documentation Consistency

AI の誤推論ポイント：
- LangChain の公式ドキュメントでも移行期のページが多く、同じ概念が別名・別構造で説明されていて、参照ページにより生成コードの前提がズレやすい
- OpenAI の「APIドキュメント」と「SDK README」「サンプル」が更新タイミングずれを起こしやすい（引数名やレスポンス形状の誤推論）
- Azure App Service の起動方法（Startup Command、ASGIサーバ構成、プロキシ配下のパス等）が公式/ブログで差分があり、運用前提を誤りやすい

AI に与えるべき対策：
- プロジェクト内に「参照すべき一次情報リンク」と「採用する記法」を固定した短いガイド（AGENT.md）を置く
- OpenAI レスポンスの「取り出し方（どのフィールドを見るか）」をプロジェクト規約として固定する
- Azure の起動コマンド、ポート、ヘルスチェック、プロキシヘッダ等をREADMEに明記する


Practice Consistency

AI の誤推論ポイント：
- FastAPI で「asyncに統一すべきか」「同期で良いか」が曖昧だと、非同期I/O・HTTP呼び出し・ファイル読み込みの混在提案になりやすい
- FAQデータ（JSON/YAML/CSV）の読み込み場所・更新方法が曖昧だと、実行環境（App Service の読み取り専用領域やデプロイ構造）に合わないパス/配置を提案しやすい
- dotenv はローカル用途が多いのに、本番でも `.env` 前提の実装を提案しやすい

AI に与えるべき対策：
- 「API層は async 統一」「外部API呼び出しはタイムアウト/リトライ必須」など実装スタイルを箇条書きで固定する
- FAQデータのソース（リポジトリ同梱/外部ストレージ/環境変数経由など）と、実行時パスの基準（`Path(__file__)` 起点等）を明示する
- dotenv は「ローカルのみ。本番は App Service の環境変数を使用」と明記する


Dependency Stability

AI の誤推論ポイント：
- LangChain は破壊的変更・パッケージ分割（`langchain`, `langchain-community`, `langchain-openai` 等）・非推奨化が多く、AIが“存在しないimport”や“旧引数”を提案しやすい
- OpenAI SDK もメジャー更新でAPI呼び出し形式が変わり、エラー処理やレスポンス型の誤推論が起きやすい
- FastAPI/Pydantic/Starlette の組み合わせで互換問題が出やすく、AIが想定するバージョンとズレるとそのまま動かない

AI に与えるべき対策：
- 主要依存はすべてバージョン固定（少なくともメジャー固定）し、AIに「このバージョン前提」と伝える
- LangChain 関連は使用パッケージ一覧（どれを入れているか）を明示する
- 依存更新ポリシー（いつ更新するか、互換確認の観点）を短く定義する


API Consistency

AI の誤推論ポイント：
- OpenAI API はモデル種別・エンドポイント種別で入力/出力形状が変わり、メッセージ構造やツール呼び出し周りを混同しやすい
- LangChain の抽象化を挟むと、どこで「システム/ユーザメッセージ」や「構造化出力」を担保するかが曖昧になりやすい
- FastAPI のリクエスト/レスポンスモデルが不明確だと、入力スキーマと実際の処理（チェーン入力）がズレやすい

AI に与えるべき対策：
- 「使用するOpenAI API種別」「モデル名の受け渡し方法」「ツール/構造化出力を使うか」を固定する
- チェーンへの入力スキーマ（例：question, locale, faq_source など）と、返却スキーマ（回答本文、根拠、エラー情報）をプロジェクト規約として明文化する
- エラー時のHTTPステータスとエラーボディ形式を統一する


Ecosystem Consistency

AI の誤推論ポイント：
- ASGIサーバ運用は「uvicorn単体」「gunicorn+uvicorn workers」など複数流派があり、Azure App Service の推奨とズレた提案になりやすい
- ロギング/メトリクス/トレーシングはAzure側の統合（App Service Logs, Application Insights 等）との整合が必要だが、一般的なローカル前提の提案になりやすい
- 設定管理が dotenv / 環境変数 / Azure Key Vault で揺れやすい

AI に与えるべき対策：
- 本番の起動方式（プロセス数、ワーカ、タイムアウト、ポート、プロキシ考慮）を固定し、AIにそれ以外を提案させない
- 観測性の方針（標準loggingに寄せる、JSONログ形式、相関IDの扱い等）を決めて明記する
- シークレット管理の正式ルート（App Service 設定 or Key Vault）を明記する


Static Semantic Service

AI の誤推論ポイント：
- Python は静的に保証される情報が薄いと、LangChain の入出力型・FastAPI の依存注入・OpenAIレスポンス形状を誤って扱いやすい
- Pydantic モデルの使い方（v2の `model_validate` 等）を曖昧にすると、古い静的前提で提案されやすい
- チェーンの「入力キー/出力キー」が暗黙だと、AIが存在しないキー名で配線しがち

AI に与えるべき対策：
- FastAPI の request/response は必ず Pydantic モデルで固定し、フィールド名を仕様として明記する
- チェーンI/Oの契約（必須キー、型、欠損時挙動）をドキュメント化する
- 型チェック（mypy/pyright）やlint設定を導入し、AIには「型を崩す提案は不可」と伝える


Runtime Semantic Service

AI の誤推論ポイント：
- 外部HTTP（OpenAI）呼び出しはタイムアウト、レート制限、ネットワーク断、リトライ、キャンセル（クライアント切断）などが支配的だが、AIが“成功前提”の処理を書きやすい
- async環境でブロッキングI/O（ファイル読み込みや同期HTTP）を混ぜる提案になりやすい
- Azure 環境特有の制約（スケールアウト時の状態共有不可、ローカルディスクの揮発性、起動/停止）を見落としやすい

AI に与えるべき対策：
- OpenAI呼び出しの標準方針（タイムアウト値、リトライ条件、バックオフ、最大待ち）を決めて明記する
- 「ブロッキングI/O禁止」「キャンセル時は処理中断」など、ランタイム規約を明文化する
- 状態管理方針（ステートレス、キャッシュの置き場所、FAQロードのライフサイクル）を決めて記載する


Core Semantic Consistency

AI の誤推論ポイント：
- Python 3.12 は周辺エコシステムの追随差が残ることがあり、AIが「3.10/3.11前提の挙動」や古い互換情報を混ぜやすい
- 例外処理（例外連鎖、グループ例外等）や asyncio の実行モデルは、説明が一般論に寄りやすく、実装での例外境界（API層/チェーン層）を誤りやすい
- 文字列/エンコーディング、パス解決、環境変数の扱いなど“地味な仕様”で環境差（Linux/Azure）を見落としやすい

AI に与えるべき対策：
- 「Python 3.12 固定」を前提に、利用する非同期モデル（asyncio）と例外の扱い方針（どこで握りつぶさないか、ログに残す粒度）を決めて明記する
- ファイルパス解決、文字コード、環境変数名（必須/任意）を仕様として列挙する
- “環境差が出る領域”はテスト観点として明文化し、AIに「推測で実装しない」ルールを与える