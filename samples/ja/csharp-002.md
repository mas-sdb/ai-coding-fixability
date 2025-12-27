# C# 診断結果サンプル
- モデル：GPT-5.2
- 入力データ

```
## 入力として与える情報
- 言語名 C#
- 言語バージョン 7.3
- 使用する主要ライブラリ Entity Framework 6
- 実行環境 Windows (.NET Framework 4.8.1)
```

# 診断結果
Community Consistency

AI の誤推論ポイント：
- 「.NET は現在 .NET (旧 .NET Core) が主流」という一般知識に引っ張られ、.NET Framework 4.8.1 前提（App.config/Web.config、IIS、GAC、AppDomain、Binding Redirect 等）を無視した提案になりやすい  
- Entity Framework について、EF6 と EF Core の知識が混在しやすく、EF Core 流の設計・API・移行手順を前提にしてしまう  
- C# 言語機能について、C# 8+（nullable reference types、async streams、switch 式、records 等）を当然のように使う提案になりやすい

AI に与えるべき対策：
- 「.NET Framework 4.8.1 / C# 7.3 / EF6 を厳守（.NET / EF Core の回答は禁止）」を冒頭で明示する  
- アプリ種別（WinForms/WPF/ASP.NET MVC/WebForms/Windows Service など）とデータベース種別（SQL Server 等）を固定情報として渡す  
- プロジェクトが採用している典型（DI あり/なし、Repository パターンの有無、DbContext のライフサイクル方針）を明文化しておく  


Documentation Consistency

AI の誤推論ポイント：
- EF6 の公式資料と、EF Core の公式資料（docs.microsoft.com 系）が検索・想起で混線しやすい  
- .NET Framework の設定（接続文字列、プロバイダ、Binding Redirect、machine.config 影響）に関するドキュメントが古い版・断片情報になりやすい  
- C# 7.3 の仕様・機能境界が、C# 最新版の説明に上書きされやすい

AI に与えるべき対策：
- 参照すべき一次情報を固定（「EF6 のドキュメント」「.NET Framework 4.8 系の構成」「C# 7.3 制約」）し、EF Core 記事は除外するルールを書く  
- 「設定は App.config/Web.config を前提」「SDK-style csproj 前提の説明は禁止」など、ドキュメント参照の前提条件を指示する  
- “使える言語機能一覧（禁止/許可）” を AGENT.md 等に列挙する  


Practice Consistency

AI の誤推論ポイント：
- EF6 での実務流儀がプロジェクトごとに分かれやすい（EDMX/Database First、Code First、手動 SQL、Repository/UoW の有無、Lazy Loading の扱い等）  
- DbContext のスコープ（画面単位/リクエスト単位/メソッド単位）やトラッキング方針（AsNoTracking 運用など）を、プロジェクト事情を無視して一般論で決め打ちしやすい  
- 旧来の .NET Framework アプリでは同期 API が多い一方、AI が “常に async 推奨” で統一してしまい、UI スレッドや既存設計と衝突しやすい

AI に与えるべき対策：
- EF の利用スタイルを明示（EDMX/Code First、Migration 運用有無、LazyLoading/ProxyCreation の方針、トラッキング方針）  
- DbContext の生成・破棄責務、トランザクション境界、例外処理（再試行やロギング）をプロジェクト標準として文章化する  
- 「既存の呼び出し規約（同期/非同期）を壊さない」「UI アプリなら ConfigureAwait 方針」などの実装ガイドを固定する  


Dependency Stability

AI の誤推論ポイント：
- EF6 は安定している一方、周辺依存（SQL クライアント、DI コンテナ、JSON、Logging 等）は “最新推奨” が混入しやすく、.NET Framework 互換性やバージョン整合を崩しやすい  
- .NET Framework は Binding Redirect や依存 DLL の競合が起きやすいが、AI がその運用（config 更新やパッケージ固定）を軽視しやすい  
- NuGet 運用が packages.config/PackageReference のどちらかで大きく変わるのに、前提を誤りやすい

AI に与えるべき対策：
- 「主要依存の固定バージョン」「NuGet 方式（packages.config か PackageReference）」「Binding Redirect 運用ルール」を明文化する  
- 新規導入ライブラリは “.NET Framework 4.8.1 対応が明確なもののみ” と制約する（提案時に互換性確認を必須化）  
- EF6 周辺（プロバイダ、SQL クライアント）を勝手に EF Core 系に差し替える提案を禁止する  


API Consistency

AI の誤推論ポイント：
- EF6 と EF Core の API 名が似ているため、誤った拡張メソッドや設定 API（モデルビルド、Include の挙動、Migrations/Scaffold の違い等）を混ぜやすい  
- .NET Framework の BCL API と .NET (旧 Core) の API 差分（存在しない型/メソッド、挙動差）を無視して呼び出しを提案しやすい  
- SQL クライアント（System.Data.SqlClient と Microsoft.Data.SqlClient）混同で、接続・例外・暗号化設定の前提がズレやすい

AI に与えるべき対策：
- 「EF6 の API セットのみ使用」「EF Core 専用 API・拡張は不可」というチェック項目を置く  
- ターゲットが .NET Framework 4.8.1 であることを常にプロンプトに含め、提案 API は “当該フレームワークで存在確認が必要” とルール化する  
- DB 接続ライブラリの採用を明確化（どちらを使うか）し、混在提案を禁止する  


Ecosystem Consistency

AI の誤推論ポイント：
- ビルド/プロジェクト形式（旧 csproj、MSBuild、AssemblyInfo、app.config、Web.config）を SDK-style 前提で語ってしまいがち  
- 実行ホスト（IIS/Windows サービス/デスクトップ）により設定・デプロイ・権限が大きく異なるのに、単一の “現代的” 手順（コンテナ、Kestrel 等）に寄せてしまう  
- Windows 固有（証明書ストア、イベントログ、パフォーマンスカウンタ、COM 等）を使っている場合に、クロスプラットフォーム前提の回答になることがある

AI に与えるべき対策：
- アプリ種別とホスト環境（IIS か、サービスか、デスクトップか）を固定情報として与える  
- プロジェクト形式（旧 csproj/SDK-style の別、.config の有無、CI の MSBuild バージョン）を明記する  
- Windows 依存機能を使う/使わない（イベントログ利用有無など）を宣言して推論範囲を狭める  


Static Semantic Service

AI の誤推論ポイント：
- C# 7.3 には nullable reference types が無く、AI が “NRT 前提の安全策” を提案してしまう（設計・注釈・警告運用が成立しない）  
- Roslyn/Analyzer 前提の高度な静的解析フローを、現行のビルドや IDE 設定を確認せずに前提化しやすい  
- EF6 のクエリ（LINQ to Entities）は “C# の LINQ” と同一視されがちで、翻訳可能性（サーバー側に落ちるか）の制約を静的に見誤りやすい

AI に与えるべき対策：
- 「C# 7.3：NRT なし、最新構文禁止」の静的制約を明記する  
- 採用している Analyzer/Style ルール（.editorconfig、FxCop/StyleCop、Roslynator 等）を列挙し、無いものは前提にしない  
- EF6 の LINQ は “翻訳制約がある” ことをプロジェクト規約にし、クエリ変更時は翻訳可否・SQL 生成確認を必須にする  


Runtime Semantic Service

AI の誤推論ポイント：
- EF6 の実行時挙動（Lazy Loading、Proxy、トラッキング、接続/トランザクション、例外の種類）が設定次第で大きく変わるのに、デフォルト決め打ちで説明しやすい  
- .NET Framework はアセンブリ解決が Binding Redirect に依存しやすく、実行時にのみ出るロード例外を軽視しがち  
- 同期/非同期の混在、UI スレッド、ASP.NET の同期コンテキストなど、実行時デッドロック/ハングの地雷を一般論で見落としやすい

AI に与えるべき対策：
- EF6 のランタイム設定（LazyLoadingEnabled/ProxyCreationEnabled/AutoDetectChangesEnabled 等の方針）を固定して明示する  
- 実行時例外の観測手段（ログ基盤、EF ログ/SQL ログ、イベントログ、ヘルスチェック）を“必ず確認する”手順として書く  
- 非同期方針（UI/ASP.NET での await 運用、同期ブロック禁止ルールなど）をプロジェクト標準として定義する  


Core Semantic Consistency

AI の誤推論ポイント：
- C# 言語仕様は比較的一貫しているが、「C# の新機能」と「ターゲット BCL/.NET Framework の機能」は別物で、AI が “言語が書ける＝実行環境も対応” と誤推論しやすい  
- .NET Framework の後方互換・レガシー機構（AppDomain、CAS の残骸、構成ファイル、古い暗号/通信設定等）により、現代 .NET の常識がそのまま当てはまらないことがある  
- EF6 自体が長期互換を重視しているため、最新の ORM トレンド（EF Core の設計思想）をそのまま適用する提案になりやすい

AI に与えるべき対策：
- 「言語仕様（C# 7.3）」「実行基盤（.NET Framework 4.8.1）」「ORM（EF6）」の3点を毎回セットで固定し、どれかを最新に読み替えないルールを置く  
- “やってはいけない置換” を明文化（例：EF6→EF Core、.NET Framework→.NET への前提すり替え、SDK-style 化前提の回答）  
- 互換性・制約を優先する設計方針（変更を最小化、既存設定尊重、デプロイ形態尊重）を AI への基本指針として与える

