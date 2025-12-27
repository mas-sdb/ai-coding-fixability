# C# 診断結果サンプル
- モデル：GPT-5.2
- 入力データ

```
## 入力として与える情報
- 言語名  C#
```

# 診断結果
---

Community Consistency

AI の誤推論ポイント：
- 「.NET Framework（レガシー）」と「.NET（現行：.NET 6+）」の文脈が混線しやすい（ASP.NET、構成、依存関係解決、ビルド/実行形態が別物）。
- ASP.NET Core でも「MVC / Razor Pages / Minimal APIs / gRPC / Worker」など流派が多く、AI が別流派の定石で回答しがち。
- DI、ログ、設定、HTTP、ORM（EF Core/Dapper等）の“標準寄りの定番”はあるが、現場選好が分かれやすい。

AI に与えるべき対策：
- 使っているターゲット（例：.NET 8、ASP.NET Core Minimal APIs、EF Core、xUnit 等）と「採用しない流派」を AGENT.md に明示。
- 依存注入/設定/ログ/HTTP/ORM の「採用ライブラリ・採用パターン（層構造、責務分割）」を文章で固定する。
- レガシー資産がある場合は「.NET Framework の知識を参照してよい範囲」を明記する。

---

Documentation Consistency

AI の誤推論ポイント：
- Microsoft Learn は最新寄りだが、検索でヒットする旧Docs/ブログ/StackOverflowが混ざると古い推奨（例：古い構成方法、古いAPI）に引っ張られる。
- 同じ機能でも「言語（C#）の説明」と「ランタイム/ライブラリ（.NET）」の説明が別ページに分散し、バージョン前提がずれやすい。
- `System.Text.Json` と Newtonsoft.Json など、事実上の二大系統があり、ドキュメント参照がぶれやすい。

AI に与えるべき対策：
- 「参照してよい一次情報」を指定（Microsoft Learn の該当バージョン、採用ライブラリ公式Docs、社内ADR等）。
- 主要トピック（HTTP、JSON、DI、Logging、Configuration、Testing、ORM）ごとに「このプロジェクトの正」を短い箇条書きで用意。
- 採用していない旧技術（例：Web.config、WCF、古いASP.NET）を明示的に禁止/例外条件を記載。

---

Practice Consistency

AI の誤推論ポイント：
- コーディング規約（命名、nullable、例外方針、async方針、LINQ多用など）がチームごとに差が出やすい。
- “Clean Architecture / Onion / Vertical Slice / 伝統的3層”など構成の流派差が大きく、AI が別構成を混ぜる。
- テストも xUnit/NUnit/MSTest、モック手法、Integration Test の組み方が揺れやすい。

AI に与えるべき対策：
- `.editorconfig` と静的解析（Roslyn analyzers）前提を明記し、nullable/警告扱い/書式を固定する方針を記述。
- ディレクトリ構造・レイヤ境界・命名規則・例外/ログ方針・async利用ルールを AGENT.md に固定。
- テスト方針（ユニット/統合の境界、利用フレームワーク、モック方針）を明文化。

---

Dependency Stability

AI の誤推論ポイント：
- NuGet は成熟している一方、周辺パッケージ（OpenAPI、Auth、Cloud SDK、Observability、フロント連携等）は更新が速く、破壊的変更に当たりやすい。
- 推移的依存関係の差で「ローカルでは動くがCIで壊れる」などが起きると、AI が原因箇所を誤認しやすい。
- `.NET` のターゲットフレームワーク更新に伴い、互換性やAPI可用性が変わる。

AI に与えるべき対策：
- 依存管理方針を固定（中央管理 `Directory.Packages.props` の採否、更新頻度、許容範囲、互換性チェック）。
- lock運用（`packages.lock.json` 等）や Renovate/Dependabot ルールを用意し、AIに「勝手にメジャー更新しない」指示を入れる。
- ターゲットフレームワーク（例：net8.0）とサポート範囲を明示し、複数TFMならその理由と制約を書く。

---

API Consistency

AI の誤推論ポイント：
- BCL（標準ライブラリ）は過負荷（オーバーロード）が多く、似た名前のAPIが大量にあるため、微妙に違う呼び出し（同期/非同期、カルチャ、エンコーディング等）を選びがち。
- HTTP/JSON などは「推奨の使い方」が変遷しており、AIが旧推奨（例：不適切な `HttpClient` 使い回し方針など）を混ぜやすい。
- `DateTime`/`DateTimeOffset`、`Guid`、`CultureInfo`、`CancellationToken` など境界条件で誤用しやすいAPIが多い。

AI に与えるべき対策：
- プロジェクト標準の採用APIを宣言（HTTPは何を使う、JSONはどれ、日時はどれ、キャンセルは必須か等）。
- ラッパー/共通ユーティリティの有無を明記し、「直接BCLを叩かずに共通層を使う」等のルールを文章で固定。
- 重要な設計判断（例：日時はUTC統一、例外をドメインに漏らさない等）をADRとして短く残す。

---

Ecosystem Consistency

AI の誤推論ポイント：
- ツールチェーン（dotnet CLI / MSBuild / Visual Studio / Rider / CI）による挙動差（生成物、アナライザ、SDK解決）が出やすい。
- Windows前提の知識（パス、証明書、IIS等）とクロスプラットフォーム前提が混ざると、AIが環境依存の解を出しやすい。
- Source Generators、AOT、トリミング等の採否でビルド/実行制約が大きく変わる。

AI に与えるべき対策：
- `global.json` でSDKを固定する運用の有無、CIのOS/SDK、ビルドコマンドを明記。
- 実行環境（Linuxコンテナ、Windowsサービス、Azure等）と制約（AOT/トリミング有無、証明書運用）を明文化。
- IDE依存手順ではなく、CLI/CIで再現できる手順を“正”として指示する。

---

Static Semantic Service

AI の誤推論ポイント：
- C# は型情報が強い一方、`dynamic`、Reflection、DIコンテナ、Source Generators などで「見えない結線」が増えると、AIが静的に追えず誤推論する。
- Nullable reference types の有効/無効で意味論が変わり、AI が null 安全性を誤解しやすい。
- Analyzer/StyleCop/カスタムルールがあると、AIが“コンパイルは通るが規約違反”の提案をしやすい。

AI に与えるべき対策：
- Nullable の設定（有効か、どのプロジェクトで有効か）と警告扱い（WarningsAsErrors など）を明記。
- 使用しているアナライザと重要ルール（例外：許容するケース）を AGENT.md に列挙。
- Reflection/DI/Generator を多用する箇所は「エントリポイント（登録箇所）と規約」を文章で説明し、探索の手がかりを与える。

---

Runtime Semantic Service

AI の誤推論ポイント：
- 例外の種類や発生タイミング（特に async/await、I/O、LINQ遅延評価）で挙動が読み違えられやすい。
- 同期/非同期のコンテキスト、スレッドプール、キャンセル、タイムアウト、再試行など運用面の意味論が絡むと誤推論しやすい。
- カルチャ/タイムゾーン/エンコーディング/ファイルシステム差など、環境依存の実行時要因を見落としやすい。

AI に与えるべき対策：
- 例外/リトライ/タイムアウト/キャンセルの“標準方針”を定義（どこで捕捉、どうログ、どう返す）。
- 実行環境の前提（カルチャ固定、UTC、コンテナ、Kestrel設定等）を明文化。
- 重要機能は統合テスト/実行時検証が正とし、AIに「静的に断定しない」指示を入れる（再現手順・ログ観測箇所の提示を優先）。

---

Core Semantic Consistency

AI の誤推論ポイント：
- C# は継続進化しており（records、pattern matching、required、primary constructors等）、言語バージョン差で「書ける/書けない」「推奨される書き方」が変わる。
- .NET もバージョンでBCL/APIや既定挙動が変わることがあり、AIが別バージョン前提で提案しやすい。
- 後方互換性は強いが、“古い書き方が今も動く”ために、AIが最新方針に揃えず混在させることがある。

AI に与えるべき対策：
- `TargetFramework` と `LangVersion`（固定/最新追従どちらか）を明示し、採用する言語機能の範囲を宣言。
- “このプロジェクトでは使わない機能/記法”（例：古いAPI、特定の構文、特定の互換モード）を禁止リスト化。
- 互換性に関わる判断（例：ライブラリは広いTFMを維持、アプリは最新固定など）を短い方針として残す。