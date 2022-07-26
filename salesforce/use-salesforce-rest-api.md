# SalesforceのREST API を使用する

[REST API 開発者ガイド](https://developer.salesforce.com/docs/atlas.ja-jp.api_rest.meta/api_rest/intro_rest.htm)  
[クイックスタート](https://developer.salesforce.com/docs/atlas.ja-jp.api_rest.meta/api_rest/quickstart.htm)  
[Qiita:外部システムからSalesforceのREST API を使用する](https://qiita.com/TaaaZyyy/items/7feb7d03cb1f5248d2e9)

Classicの場合

## 「API の有効化」権限があることを確認する

1. [設定] | [ユーザの管理] | [プロファイル] へアクセスします。
2. 対象のプロファイルを開き、[システム権限] をクリックします。
3. [編集]をクリックし、[API の有効化]チェックボックスにチェックを入れます。

## 接続アプリケーションを作成する

1. [設定] | [作成] | [アプリケーション] へアクセスします。
2. ページ下部の接続アプリケーションで[新規]をクリックします。
3. 基本情報に必要事項を入力します。
4. [OAuth設定の有効化]にチェックを入れ、コールバックURL仮アドレスとして「https://localhost」と入力
5. 利用可能なOAuthの範囲を選択。
   * API を使用してユーザデータを管理 (api)
   * いつでも要求を実行 (refresh_token, offline_access)
6. 以降デフォルトで[保存]

## IP制限の編集

1. [設定] | [アプリケーションを管理する] | [接続アプリケーション] へアクセスします。
2. 作成した接続アプリケーションを開き、[ポリシーを編集]をクリック
3. IP制限の緩和を適宜編集して[保存]

## コンシューマ鍵とコンシューマの秘密の取得

1. [設定] | [作成] | [アプリケーション] へアクセスします。
2. 作成した接続アプリケーションを開きます。
3. [コンシューマの詳細を管理] をクリックします。
4. [コンシューマ鍵] と [コンシューマの秘密] の値をコピーして、後から使用するために保存します。

## セキュリティートークンの取得

省略

## ユーザ名パスワード OAuth 認証（非推奨）

1. Talend API Tester を実行します。
2. リクエストを新規で作成し、下記のように設定を行います。
   * METHOD: POST
   * パス: 適切なSalesforce トークン要求エンドポイント  
    (例: https://login.salesforce.com/services/oauth2/token)  
    (例: https://test.salesforce.com/services/oauth2/token)  
   * HEADERS  
    Content-Type: application/x-www-form-urlencoded
3. BODYにFormを選択し、次のパラメータを設定します。
   * grant_type=password
   * client_id=[接続アプリケーションのコンシューマ鍵]
   * client_secret=[接続アプリケーションのコンシューマの秘密]
   * username=[インテグレーション用ユーザのSalesforceユーザ名]
   * password=[インテグレーション用ユーザのSalesforceログインパスワード+セキュリティトークン]
4. Sendボタンを押すと、認証結果が返されます。
5. 成功(200 OK)したらBODYのaccess_tokenを保存

## Queryの実行

[Query](https://developer.salesforce.com/docs/atlas.ja-jp.api_rest.meta/api_rest/resources_query.htm)

1. リクエストを新規で作成し、下記のように設定を行います。
   * METHOD: GET
   * パス: https://_MyDomain_.my.salesforce.com/services/data/v99.9/query/?q= _SOQLクエリ_  
    ※公式ヘルプでは`スペース`を`+`に置き換えるような指示になっているのだが、unexpected_tokenエラーになるので、
    スペースのままで行くと成功した(Talendがエスケープしてくれるようになった？)
   * HEADERS  
    Authorization: Bearer _認証時に保存したaccess_token_ (**Bearerの後ろにスペースが入るので注意**)
2. Bodyは空でOK。Sendボタンを押すと、認証結果が返されます。

## レコードを更新する

[レコードを更新する](https://developer.salesforce.com/docs/atlas.ja-jp.api_rest.meta/api_rest/dome_update_fields.htm)

1. リクエストを新規で作成し、下記のように設定を行います。
   * METHOD: PATCH
   * パス: https://_MyDomain_.my.salesforce.com/services/data/v99.9/sobjects/_オブジェクト名_/_オブジェクトID_  
    ※公式ヘルプでは`スペース`を`+`に置き換えるような指示になっているのだが、unexpected_tokenエラーになるので、
    スペースのままで行くと成功した(Talendがエスケープしてくれるようになった？)
   * HEADERS  
    Authorization: Bearer _認証時に保存したaccess_token_ (**Bearerの後ろにスペースが入るので注意**)
    Content-Type: application/json
2. Bodyに更新したいカラム名:値形式でtext指定

    ```json
    {
        "BillingCity" : "San Francisco"
    }
    ```

3. Sendボタンを押して実行
4. 成功(204 OK)時、レスポンスボディはありません。

## 補足

* 使用したリクエストは Talend API Tester に保存
* Trail Head にはPOSTMANを使ったチュートリアルがある  
  [Postman API Client](https://trailhead.salesforce.com/ja/content/learn/modules/postman-api-client)

---

[リファレンス](https://developer.salesforce.com/docs/atlas.ja-jp.api_rest.meta/api_rest/resources_list.htm)

## リソースのURI

リソースの URI は、ベース URI (https://_MyDomainName_.my.salesforce.com) に続きます。
