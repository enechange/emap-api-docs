# EMAP API README
## Plan API
- Plan APIでは、4つのエンドポイントを提供します。
  - /elec/areas : 電力のエリアを取得できます。
  - /elec/providers : 電気事業者一覧を取得できます。
  - /elec/plans : 低圧の電気料金プランを提供・公開している事業者およびプランを取得できます。
  - /elec/fuel_cost_adjustments : 燃料費調整額を取得できます。
- APIの仕様の詳細については[こちら](https://emap-sim-base.enechange.jp/apidocs/plan)をご覧ください。

### 実行例
```sh
# curlの場合
curl -H 'X-EMAP-USER-KEY:emapapidemouser1234' 'https://emap-sim-base.enechange.jp/v001/elec/plans' > emap_plans.json

# wgetの場合
wget --header='X-EMAP-USER-KEY:emapapidemouser1234' 'https://emap-sim-base.enechange.jp/v001/elec/plans' -O emap_plans.json

cat emap_plans.json | jq
```
 - X-EMAP-USER-KEYはデモ用の値となります。ご契約後に正式な値をお伝えいたします。
 - EMAP APIについては以下をご覧ください。
   - [https://enechange.co.jp/emap-plan-lp/](https://enechange.co.jp/emap-plan-lp/)

## Simulation API
- APIの仕様の詳細については[こちら](https://emap-sim-base.enechange.jp/apidocs/simulation)をご覧ください。
- POSTするJSON例
  - fee_plan_codeは電気料金プランを一意に識別するコードになります。Simulation APIのみをご契約の場合は、診断対象としたいfee_plan_codeをお伝えいたします。Plan APIから取得することもできます。
  - リクエストのJSONの例をsample_requests/req-sample-gas-tepco.jsonに示します。この例ではfee_plan_codeに東京電力エナジーパートナー従量電灯B（ozkxj|hzozmtmvoztgdbcodibtw-ozkxj）を比較元プランとし、比較対象プランとして東京電力エナジーパートナー従量電灯B、サンプルガス従量電灯B（nvhkgzvbvn|hzozmtmvoztgdbcodibtw-ozkxj）、サンプルガス従量電灯B（ガス割）（nvhkgzvbvn|hzozmtmvoztgdbcodibtwtb-ozkxj）としています。

```sh
# curlの場合
curl -X POST -H "Content-Type: application/json" -H "X-EMAP-USER-KEY:emapapidemouser1234" -d @req-sample-gas-tepco.json https://emap-sim-base.enechange.jp/v001/elec/simulate > emap_simulation.json
curl -X POST -H "Content-Type: application/json" -H "X-EMAP-USER-KEY:emapapidemouser1234" -d '{"request_id":"E00001","postcode":"1000004","fee_plan_code":"ozkxj|hzozmtmvoztgdbcodibtw-ozkxj","contract_ampere":40,"base_YYYYmm":"202010","fee_history":["8000","","","","","","","","","","",""],"usage_history":["","","","","","","","","","","",""],"weekday_night_usage_percentage":25,"holiday_night_usage_percentage":null,"floor_space":45,"number_of_rooms":2,"number_of_family":3,"all_electric":false,"fee_plan_codes":["ozkxj|hzozmtmvoztgdbcodibtw-ozkxj","nvhkgzvbvn|hzozmtmvoztgdbcodibtw-ozkxj","nvhkgzvbvn|hzozmtmvoztgdbcodibtwtb-ozkxj"]}' https://emap-sim-base.enechange.jp/v001/elec/simulate > emap_simulation.json

# wgetの場合
wget -q -O emap_simulation.json https://emap-sim-base.enechange.jp/v001/elec/simulate --post-file req-sample-gas-tepco.json --header='X-EMAP-USER-KEY:emapapidemouser1234' --header 'Content-Type: application/json' --header 'Accept: application/json'

cat emap_simulation.json | jq
```

## デモ環境について
- EMAP APIではデモ環境を用意しております。デモ環境はEMAP APIのご利用を検討されている方はどなたでもご利用できます。

### ご利用時間
  9:00〜21:00
 - 予告なくメンテナンス等でご利用できない場合がございます

### 利用できる郵便番号
 - APIには郵便番号で絞り込みできるものがありますが、デモ環境では利用できる郵便番号に制限があります。
 - 利用できる郵便番号は以下になります。

| エリア名 | 郵便番号 | 住所 |
|:-------- |:-------- |:-------- |
| 北海道電力エリア | 0010013 | 北海道札幌市北区北十三条西 |
| 東北電力エリア | 9800004 | 宮城県仙台市青葉区宮町 |
| 東京電力エリア | 1000004 | 東京都千代田区大手町 |
| 中部電力エリア | 4530838 | 愛知県名古屋市中村区向島町 |
| 北陸電力エリア | 9300014 | 富山県富山市館出町 |
| 関西電力エリア| 5300001 | 大阪府大阪市北区梅田 |
| 中国電力エリア | 7300001 | 広島県広島市中区白島北町 |
| 四国電力エリア | 7600001 | 香川県高松市新北町 |
| 九州電力エリア | 8120006 | 福岡県福岡市博多区上牟田 |
| 沖縄電力エリア | 9000004 | 沖縄県那覇市 |

### 取得可能プラン
 - 以下にデモ環境で取得できるプランを列挙します。
 - デモ環境で取得できる単価は最新の値に追従していない場合があります。
 - サンプルガスはデモ用に用意した事業者になります。取得できる単価は仮想の値となります。

| エリア名 | 事業者名 | プラン名 |
|:-------- |:-------- |:-------- |
| 北海道電力エリア | 北海道電力 | 従量電灯B |
| | | 従量電灯C |
| | | eタイム3プラス |
| | | 低圧電力 |
| | サンプルガス | 従量電灯B |
| | | 従量電灯B（ガス割）|
| | | 従量電灯C |
| | | 従量電灯C（ガス割）|
| | | 低圧電力 |
| 東北電力エリア | 東北電力 | 従量電灯B |
| | | 従量電灯C |
| | | よりそう＋ナイト8 |
| | | よりそう＋ナイト8(実量契約) |
| | | 低圧電力 |
| | サンプルガス | 従量電灯B |
| | | 従量電灯C |
| | | お家ぷらん |
| | | 低圧電力 |
| 東京電力エリア | 東京電力エナジーパートナー | 従量電灯B |
| | | 従量電灯C |
| | | スマートライフS |
| | | 低圧電力 |
| | サンプルガス | 従量電灯B |
| | | 従量電灯B（ガス割） |
| | | 従量電灯C |
| | | 従量電灯C（ガス割） |
| | | 低圧電力 |
| 中部電力エリア | 中部電力ミライズ | 従量電灯B |
| | | 従量電灯C |
| | | スマートライフプラン |
| | | 低圧電力 |
| | サンプルガス | 従量電灯B |
| | | 従量電灯C |
| | | 低圧電力 |
| 北陸電力エリア | 北陸電力 | 従量電灯B |
| | | 従量電灯C |
| | | くつろぎナイト12 |
| | | 低圧電力 |
| 関西電力エリア | 関西電力 | 従量電灯A |
| | | 従量電灯B |
| | | はぴeタイムR |
| | | 低圧電力 |
| | サンプルガス | 従量電灯A |
| | | 従量電灯A（ガス割） |
| | | 従量電灯B |
| | | 従量電灯B（ガス割） |
| | | 低圧電力 |
| 中国電力エリア | 中国電力 | 従量電灯A |
| | | 従量電灯B |
| | | ぐっとずっと。プラン 電化Styleコース |
| | | 低圧電力 |
| | サンプルガス | 従量電灯A |
| | | 従量電灯B |
| | | 低圧電力 |
| 四国電力エリア | 四国電力 | 従量電灯A |
| | | 従量電灯B |
| | | でんかeプラン |
| | | 低圧電力 |
| | サンプルガス | 従量電灯A |
| | | 従量電灯B |
| | | 低圧電力 |
| 九州電力エリア | 九州電力 | 従量電灯B |
| | | 従量電灯C |
| | | 電化でナイト・セレクト22 |
| | | 低圧電力 |
| | サンプルガス | 従量電灯B |
| | | 従量電灯C |
| | | 低圧電力 |
| 沖縄電力エリア | 沖縄電力 | 従量電灯 |
| | | Ee ホームホリデー |
| | | 低圧電力
