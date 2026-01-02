
# Spring Boot Docker 開発用 vs 本番用 比較

| 項目 | 開発用 | 本番用 |
|------|--------|--------|
| **手順** | 1. ソースをそのまま Docker にコピー or マウント<br>2. Maven コンテナ内で `./mvnw spring-boot:run` 実行<br>3. コンテナ上でアプリがライブ起動 | 1. ローカルまたは Docker ビルドステージで `mvn package` 実行し jar を生成<br>2. jar を軽量 JDK イメージにコピー<br>3. コンテナ内で `java -jar app.jar` 実行 |
| **Dockerfile の内容** | Maven + JDK が必要、ソースをそのままコピー or マウント、CMD は `./mvnw spring-boot:run` | ビルドステージで Maven + JDK、最終ステージは軽量 JDK + jar のみ、CMD は `java -jar app.jar` |
| **docker-compose.yml** | volumes でソースをマウントし、command で `./mvnw spring-boot:run` | ビルド済み jar をコンテナにコピー、command で `java -jar app.jar` |
| **メリット** | - ソース変更を即反映可能<br>- 開発中に便利<br>- デバッグやテストがしやすい | - コンテナ軽量・高速<br>- 本番向けで安定<br>- Maven やソース不要で配布容易 |
| **デメリット** | - Maven が毎回依存解決するため起動遅い<br>- 本番環境向きではない<br>- ソースがそのままなのでセキュリティ上少し注意 | - ソース変更したら再ビルド必須<br>- 開発中のライブ修正不可<br>- デバッグには少し工夫が必要 |
| **利用シーン** | 開発・テスト・デバッグ | 本番・ステージング環境・軽量運用 |


# Spring Boot Docker 開発用 vs 本番用：ビルド・実行フロー比較

項目 | 開発用 | 本番用
---|---|---
1. ソース準備 | ソースコードをそのままローカルに置く | ソースコードをローカルに置く
2. Dockerfile ビルド | Dockerfile で Maven + JDK イメージを使い、ソースをコンテナにコピー（またはマウント） | Dockerfile のビルドステージで Maven + JDK で `mvn package` 実行、jar を生成
3. コンテナイメージ作成 | Maven + ソース付きの開発用イメージを作る | 最終ステージで軽量 JDK + jar だけの本番用イメージを作る
4. コンテナ実行 | コンテナ内で `./mvnw spring-boot:run` を実行 → ソース変更は即反映 | コンテナ内で `java -jar app.jar` を実行 → jar の内容で固定
5. 動作確認 | 開発中の修正をすぐ反映して確認可能 | 本番稼働用として安定・高速に起動
6. 更新時の手順 | コード変更後はコンテナを再起動すれば反映 | コード変更後は再度 `mvn package` → Dockerfile ビルド → コンテナ再作成

