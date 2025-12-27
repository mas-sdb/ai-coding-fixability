# Java 診断結果サンプル
- モデル：GPT-5.2
- 入力データ

```
## 入力として与える情報
- 言語名 java
```

# 診断結果
Community Consistency

AI の誤推論ポイント：
- 「Java＝Spring」という前提で話を進め、Jakarta EE / Micronaut / Quarkus / Play など別系統の流儀を無視する
- ビルド文化が Maven / Gradle（Groovy DSL / Kotlin DSL）で割れており、設定・ディレクトリ慣習・依存管理の前提を取り違える
- チームや年代で「例外設計（checked を避ける/活用する）」「null の扱い（Optional の使い方）」「DI のやり方（Spring/JSR-330）」が分かれ、標準解を誤選択しやすい

AI に与えるべき対策：
- 使っているフレームワーク（例：Spring Boot 3 / Jakarta EE 10 等）とビルドツール（Maven/Gradle、DSL種別）を明記する
- コーディング規約（例外方針、Optional/null 方針、DI/アノテーション方針、パッケージ構成）を AGENT.md に短く固定する
- 既存コードの「代表例（典型パターンが載っているファイル群）」への参照パスを指示して、そこに寄せるよう誘導する

Documentation Consistency

AI の誤推論ポイント：
- Java SE（Oracle/OpenJDK）公式ドキュメント、JEP、各フレームワークのドキュメント、古いブログ情報が混在し、API/推奨が世代で食い違う
- Java EE → Jakarta EE のパッケージ移行（javax.* → jakarta.*）を取り違え、古い記述に引っ張られる
- Spring/Security/Hibernate などはメジャーバージョン差で設定・推奨が変わり、文書の対象バージョンを誤認しやすい

AI に与えるべき対策：
- ターゲット Java バージョン（例：17/21）と主要依存のメジャーバージョンを「固定値」として冒頭に宣言する
- 参照優先順位（例：リポジトリ内ドキュメント > 公式リファレンス > Javadoc > ブログ）を明記する
- Jakarta 移行有無、使用してよい名前空間（javax/jakarta）を明示する

Practice Consistency

AI の誤推論ポイント：
- パッケージ構成が「レイヤード（controller/service/repo）」「ドメイン駆動」「フィーチャー単位」などで大きく異なり、既存構造を壊す生成をしやすい
- Lombok/MapStruct/AutoValue/Immutables など生成系ライブラリの有無で “書くべきコード量” が変わるのに、前提を外す
- テスト慣行（JUnit 4/5、Mockito、Spring Test、Testcontainers）や命名規約の差で、混在や非推奨を持ち込む

AI に与えるべき対策：
- 採用アーキテクチャ（パッケージ方針、層、依存方向）を一文で固定し、逸脱禁止を明示する
- 使っている補助ツール（Lombok 等）と「使ってよい/禁止」を明確化する
- テスト基盤（JUnit 世代、モック方針、統合テスト方針）を固定し、既存テストの置き場所・命名に合わせるよう指示する

Dependency Stability

AI の誤推論ポイント：
- Spring Boot / Hibernate / Jackson / Netty などはバージョン整合が重要で、AI が単体で最新版の書き方を混ぜると依存衝突を起こしやすい
- Java バージョン制約（例：ライブラリが 17+ 必須）や、Jakarta 移行による破壊的変更を見落としやすい
- “便利そうな新規依存追加” を提案しがちで、既存の BOM/親 POM/Version Catalog との整合を壊す

AI に与えるべき対策：
- 依存管理方式（Maven BOM/親POM、Gradle version catalog 等）と「追加は要相談」を明記する
- 依存更新のルール（Renovate/Dependabot、許容範囲、互換性確認の手順）を指示する
- 「まず既存依存で実現できるかを確認する」ことを行動規範として書く

API Consistency

AI の誤推論ポイント：
- 同名・類似 API が多い（例：日時、コレクション、HTTP、JSON、ロギング）ため、プロジェクト標準を外して別流派を混ぜやすい
- 例外モデル（checked/unchecked、独自例外階層、HTTPエラー変換）や戻り値（Optional/nullable）ポリシーを誤ると、呼び出し側まで崩れる
- フレームワークの抽象（Repository/Template/Client）を飛び越え、直接低レベル API を叩く提案をしがち

AI に与えるべき対策：
- 「標準API一覧」（JSON は Jackson、HTTP は WebClient/RestTemplate どちら、日時は java.time、ロギングは SLF4J 等）を固定する
- エラー処理規約（例外の投げ方/握り方、エラーコード、バリデーション）を AGENT.md に明記する
- 既存の抽象レイヤーを優先して使う（新規に別クライアントを導入しない）ルールを設定する

Ecosystem Consistency

AI の誤推論ポイント：
- 実行基盤が多様（サーバ、コンテナ、Lambda、Android、OSGi、モジュール有無）で、前提を外すと設定やI/Oが不適合になる
- Java のモジュール（JPMS）や、ネイティブイメージ（GraalVM）など制約がある環境では reflection/動的生成が制限され、AI が一般的解を出すと失敗する
- ロギング（SLF4J + Logback/Log4j2）、設定（Spring Config/MicroProfile Config）、DI など “組み合わせ” で正解が変わる

AI に与えるべき対策：
- デプロイ形態（JVMのみ/ネイティブ/コンテナ）、実行環境制約（K8s、FIPS、プロキシ、メモリ制限等）を明記する
- 使っている基盤セット（DI、設定、HTTP、ORM、ログ）を固定し、代替案を勝手に混ぜないよう指示する
- reflection/動的プロキシ可否（GraalVM 等）を宣言し、禁止/制限がある場合は明文化する

Static Semantic Service

AI の誤推論ポイント：
- Java は静的型付けだが、ジェネリクスの型消去・生配列/生ジェネリクス混在・ワイルドカードで推論が崩れやすい
- アノテーション駆動（DI/ORM/シリアライズ）やコード生成（Lombok等）は「見えない意味論」が多く、AI が存在しないメソッド/フィールドを仮定しやすい
- null 安全は型で表現されにくく、Optional の運用次第で誤った取り扱いを生成しやすい

AI に与えるべき対策：
- 「静的解析で守る前提」を明記（Error Prone / SpotBugs / Checkstyle / Nullness annotations 等の採用有無）
- Lombok/生成ツール使用時は、生成されるもの（getter/setter/constructor 等）の前提をリポジトリ規約として明示する
- null/Optional 方針（フィールド/引数/戻り値のどこで使うか）を固定する

Runtime Semantic Service

AI の誤推論ポイント：
- 反射・クラスローダ・プロキシ（Spring/Hibernate）により、実行時にのみ現れる挙動（遅延ロード、トランザクション境界、循環参照）が多い
- 例外が checked/unchecked で混在し、ラップ/再スロー/ログ出力の規約を外すと運用障害につながりやすい
- 並行性（スレッド、プール、CompletableFuture、仮想スレッド）で実行順序やコンテキスト伝播が絡むと、AI が単純化して誤る

AI に与えるべき対策：
- トランザクション境界、遅延ロード方針、セッション管理（Open Session in View 等）の採否を明文化する
- 例外とログの運用規約（握りつぶし禁止、ラップ階層、監視連携）を固定する
- 並行処理の標準（Executor の管理、仮想スレッド採用有無、タイムアウト方針）を明記する

Core Semantic Consistency

AI の誤推論ポイント：
- Java の言語機能は世代差が大きく（8/11/17/21…）、AI が「records/sealed/pattern matching/var/virtual threads」前提で提案し、ターゲットに合わないことがある
- 互換性維持の歴史により、古いAPI・古い慣習が残り続け、どれを使うべきかの選択で誤りやすい
- JVM 上の “同じJava” でも、ベンダー差・フラグ・GC・モジュール境界で挙動/制約が変わりうる

AI に与えるべき対策：
- ターゲット Java バージョンと使用可能言語機能（採用/禁止）を明文化する
- 「標準として使う API 世代」（例：java.time を使う、古い Date/Calendar は触らない等）を固定する
- 実行JDK（Temurin/Oracle 等）と主要ランタイム設定（GC、--add-opens 必要性、モジュール利用有無）を宣言する