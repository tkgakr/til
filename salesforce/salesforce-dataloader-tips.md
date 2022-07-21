# Salesforce Detaloader Tips

## CSV ファイルの 「日付」 および 「日付/時間」形式

[公式ヘルプ](https://help.salesforce.com/s/articleView?id=000325035&type=1)  
データローダーで「日付」、「日付/時間」、および「時間」データ型項目をアップロードする場合は、組織のタイムゾーンの影響を受けるため注意が必要。  
そのため元データのエクスポートの際にGMTとなるようにフォーマットしておく。

### 「日付」項目で使用可能な形式

* YYYY-MM-DD
* YYYY-MM-DD hh:mm:ss
* YYYY-MM-DD hh:mm:ss
* YYYY-MM-DDThh:mm:ssZ **←**
* YYYY-MM-DDThh:mm:ss.sssZ

### 「日付/時間」項目で使用可能な形式

* YYYY-MM-DD hh:mm:ss
* YYYY-MM-DDThh:mm:ssZ **←**
* YYYY-MM-DDThh:mm:ss.sssZ

### 「時間」項目で使用可能な形式

* hh:mm:ss.sssZ (例: 07:00:00.000Z)