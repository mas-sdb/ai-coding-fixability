# Java 診断結果サンプル
- モデル：GPT-5.2
- 入力データ

```
## 入力として与える情報
- 言語名  
  Java

- 言語バージョン  
  Java 8

- 使用する主要ライブラリ  
  Spring Boot  
  Spring MVC  
  Hibernate（JPA）  
  Lombok
  Maven

- 実行環境  
  組み込み Tomcat（Spring Boot）  
  Linux
  MySQL

- プロジェクトのアーキテクチャ概要  
  コントローラ層（REST API）  
  サービス層（ビジネスロジック）  
  リポジトリ層（JPA/Hibernate による DB アクセス）  
  エンティティ層（DB マッピング）  
  三層アーキテクチャをベースにした構成
```

# 診断結果
Community Consistency

AI の誤推論ポイント：
- Spring Boot / Spring MVC の「推奨レイヤリング」や例外処理方式（ControllerAdvice 等）がチーム・年代で揺れやすく、AI が別流派（例：Controller にビジネスロジックを寄せる、Entity をそのまま返す）を選びがち。
- JPA/Hibernate の運用流儀（Fetch 戦略、DTO 投影、トランザクション境界、N+1 対策）が現場差が大きく、AI が一般的だがプロジェクトに合わないやり方を混ぜがち。
- Lombok の許容度（利用範囲、@Data の是非、Builder の使い方）がチームごとに差があり、AI が強めに Lombok を推して設計を崩しがち。

AI に与えるべき対策：
- 「このプロジェクトの流儀」を明文化（例：Entity を API レスポンスに使う/使わない、DTO 変換層の有無、サービス層のみ @Transactional 等）。
- レイヤ責務（Controller/Service/Repository/Entity）と例外・バリデーションの置き場所を AGENT.md に固定ルールとして記載。
- Lombok の使用許可アノテーションと禁止事項（例：@Data を避ける等）を列挙。


Documentation Consistency

AI の誤推論ポイント：
- Spring はバージョン差分が大きく、AI が Java 8 非対応の Spring Boot 3 系（jakarta.* 前提）ドキュメント知識を混入しやすい。
- Hibernate/JPA は「Hibernate 依存の機能」と「JPA 標準」が混ざりやすく、AI が標準外 API を標準のように扱いがち。
- MySQL の方言・タイムゾーン・文字コードなどは記事ごとに推奨が割れ、AI が環境前提と違う設定を提案しがち。

AI に与えるべき対策：
- pom.xml の Spring Boot / Spring / Hibernate の実バージョンを AI に必ず参照させ、「そのバージョンの公式リファレンスのみ」を優先するルールを付与。
- 「javax.* を使う（jakarta.* は使わない）」のような名前空間方針を明記（Java 8 + 旧世代前提の誤参照を防ぐ）。
- DB まわりは方針を固定（タイムゾーン、照合順序、Dialect、DDL 方針、命名規則）し、AI に推測させない。


Practice Consistency

AI の誤推論ポイント：
- REST 設計（ステータスコード、エラーレスポンス形式、ページング、PATCH/PUT の使い分け）がブレやすく、AI がエンドポイントごとに流儀を変えがち。
- 例外処理・ログ（例：例外を握りつぶす、スタックトレースの扱い、ログ粒度）がばらつくと、AI が場当たり的に try-catch を増やしがち。
- Repository/Service の責務分割（Repository に複雑ロジックを入れる／Service に寄せる）が揺れると、AI が層をまたぐ実装を生成しがち。

AI に与えるべき対策：
- API 仕様（エラー JSON 形、共通レスポンス有無、ステータスコード表）を固定し、全エンドポイント共通のテンプレを文章で定義。
- 例外は「捕捉する層」「変換する層（ControllerAdvice 等）」「ロギング方針」を明文化。
- 命名規則・パッケージ構成・DTO/Entity/VO の役割を規約として提示。


Dependency Stability

AI の誤推論ポイント：
- Spring Boot のメジャー差分（2.x と 3.x）で要求 Java が違うため、AI が依存更新案を出すと即ビルド不能（Java 8 前提崩壊）になりやすい。
- Hibernate はバージョンで挙動（生成 SQL、方言、遅延ロード、ID 生成、NamingStrategy 等）が変わり、AI が一般論の設定を提案して事故りやすい。
- Lombok は IDE/コンパイラ/annotation processing の相性があり、AI が依存追加だけで「動く前提」の話をしがち。

AI に与えるべき対策：
- 「Java 8 固定」「Spring Boot は 2.x 系固定（実バージョン明記）」のように互換制約を最上位に書く。
- 依存更新は「まず dependency:tree と互換表を確認」「Boot の BOM に従う」など、更新手順をルール化。
- Lombok は「annotation processing 必須」「CI で javac ビルドを通す」等の前提を明記。


API Consistency

AI の誤推論ポイント：
- Spring MVC のアノテーションや引数解決（@RequestBody/@RequestParam/@PathVariable、BindingResult 等）は似た書き方が多く、AI が微妙に不整合な組合せを出しやすい。
- JPA は EntityManager/Repository/QueryDSL 等の選択肢が多く、AI がプロジェクトで未採用の API を混ぜやすい。
- トランザクション（@Transactional）の伝播・readOnly・例外ロールバック条件など、微妙な API 契約を AI が誤解しやすい。

AI に与えるべき対策：
- 採用 API を明示（例：Spring Data JPA を使う/使わない、Criteria は使わない等）。未採用の選択肢は禁止として書く。
- @Transactional の適用方針（付与する層、readOnly の基準、checked 例外時の扱い）を文章で固定。
- Controller の入力/出力の型方針（DTO のみ等）と Validation の適用方法を統一ルールとして提示。


Ecosystem Consistency

AI の誤推論ポイント：
- Maven（プラグイン、プロファイル、resource filtering）× Spring Boot（repackage）× Lombok（annotation processing）の組み合わせで、AI が「Gradle 前提」や「IDE 任せ」な解決策を混ぜがち。
- 組み込み Tomcat と Linux 運用の前提（ログ出力、ポート、環境変数、JVM オプション）を AI がコンテナ前提/クラウド前提で誤置換しやすい。
- MySQL 接続（ドライバ、SSL、文字コード、serverTimezone）設定が環境依存で、AI が一般的な JDBC URL を提案してズレやすい。

AI に与えるべき対策：
- ビルド/起動の正（mvn コマンド、Jar 起動方法、必要な環境変数、application.properties の優先順位）をドキュメント化。
- 利用する Maven plugin とバージョン、Java 8 の source/target 設定、Lombok の扱いを固定。
- DB 接続文字列と必須パラメータ（タイムゾーン等）をテンプレとして明示。


Static Semantic Service

AI の誤推論ポイント：
- Java は型情報が強い一方、Lombok が生成するメソッド/コンストラクタがソース上に見えず、AI が「存在しないメソッド」前提で話を進めがち。
- Hibernate のエンティティはプロキシ化され得るため、静的には単純な POJO に見えても、equals/hashCode/toString の設計が重要なのに AI が見落としがち。
- Java 8 制約下で、AI が後年の言語機能（var、records、sealed、switch 式等）を使う前提の設計を提案しやすい。

AI に与えるべき対策：
- Java 8 で使える文法・標準 API の範囲を明記（使用禁止機能リストを置く）。
- Lombok の採用アノテーション一覧と「生成されるもの」を前提知識として与える（AI に推測させない）。
- Entity の equals/hashCode/toString の方針（ID ベース等）と、Lazy 関連を触らないルールを文章で指定。


Runtime Semantic Service

AI の誤推論ポイント：
- Spring の AOP/プロキシにより、@Transactional が「同一クラス内呼び出しでは効かない」など実行時の罠を AI が無視しがち。
- Hibernate の Lazy Loading は実行時にのみ破綻（LazyInitializationException）するため、AI が「Controller で Entity を返す」など危険な経路を作りがち。
- MySQL のタイムゾーン/照合順序差で実行時にだけ不具合が出るのに、AI が環境差を軽視しがち。

AI に与えるべき対策：
- トランザクション境界とセッション寿命（Open Session in View を有効/無効どちらか）を明記し、その前提でのみ提案させる。
- Lazy/Fetch の運用ルール（API レスポンスに Entity を直返ししない、必要データはサービス層で確定させる等）を固定。
- 実行環境の前提（Linux、MySQL 設定、JDBC URL 必須項目）を「変更しない前提」として提示。


Core Semantic Consistency

AI の誤推論ポイント：
- Java 8 は古いが安定している一方、AI が新しめの Java の常識（モジュール、最新 GC 前提の最適化、jakarta 化された周辺知識）を混入しやすい。
- null/Optional、Stream の副作用、日時 API（java.time）はプロジェクト流儀が出やすく、AI が一貫しない設計（Optional をフィールドに使う等）を生成しがち。
- 例外設計（checked/unchecked）やシリアライズ（Jackson と Java 型の相互作用）で、AI が一般論のまま不整合を作りがち。

AI に与えるべき対策：
- 「Java 8 とその標準ライブラリ範囲で完結」「javax 名前空間維持」のように時代境界を明文化。
- Optional/Stream/日時型の使用規約（どこで使うか、DTO に持たせる型、null の扱い）を決めて提示。
- JSON 変換方針（日時フォーマット、null 出力、命名戦略）と例外→エラー応答のマッピング規約を固定して与える。