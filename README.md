# EMAP API README
## Plan API（電気）
- Plan APIでは、4つのエンドポイントを提供します。
  - /elec/areas : 電力のエリアを取得できます。
  - /elec/providers : 電気事業者一覧を取得できます。
  - /elec/plans : 低圧の電気料金プランを提供・公開している事業者およびプランを取得できます。
  - /elec/fuel_cost_adjustments : 燃料費調整額を取得できます。
  - /elec/renewable_energy_surcharges : 再生可能エネルギー発電促進賦課金を取得できます。
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

## Simulation API（電気）
- APIの仕様の詳細については[こちら](https://emap-sim-base.enechange.jp/apidocs/simulation)をご覧ください。
- POSTするJSON例
  - fee_plan_codeは電気料金プランを一意に識別するコードになります。Simulation APIのみをご契約の場合は、診断対象としたいfee_plan_codeをお伝えいたします。Plan APIから取得することもできます。
  - リクエストのJSONの例をsample_requests/req-sample-gas-tepco.jsonに示します。この例ではfee_plan_codeに東京電力エナジーパートナー従量電灯B（ozkxj|hzozmtmvoztgdbcodibtw-ozkxj）を比較元プランとし、比較対象プランとして東京電力エナジーパートナー従量電灯B、サンプルガス従量電灯B（nvhkgzvbvn|hzozmtmvoztgdbcodibtw-ozkxj）、サンプルガス従量電灯B（ガス割）（nvhkgzvbvn|hzozmtmvoztgdbcodibtwtb-ozkxj）としています。

```sh
# curlの場合
curl -X POST -H "Content-Type: application/json" -H "X-EMAP-USER-KEY:emapapidemouser1234" -d @req-sample-gas-tepco.json https://emap-sim-base.enechange.jp/v001/elec/simulate > emap_simulation.json
curl -X POST -H "Content-Type: application/json" -H "X-EMAP-USER-KEY:emapapidemouser1234" -d '{"request_id":"E00001","postcode":"1000004","fee_plan_code":"ozkxj|hzozmtmvoztgdbcodibtw-ozkxj","contract_ampere":40,"unit_type":1,"base_YYYYmm":"202010","fee_history":["8000","","","","","","","","","","",""],"usage_history":["","","","","","","","","","","",""],"weekday_night_usage_percentage":25,"all_electric":false,"fee_plan_codes":["ozkxj|hzozmtmvoztgdbcodibtw-ozkxj","nvhkgzvbvn|hzozmtmvoztgdbcodibtw-ozkxj","nvhkgzvbvn|hzozmtmvoztgdbcodibtwtb-ozkxj"]}' https://emap-sim-base.enechange.jp/v001/elec/simulate > emap_simulation.json

# wgetの場合
wget -q -O emap_simulation.json https://emap-sim-base.enechange.jp/v001/elec/simulate --post-file req-sample-gas-tepco.json --header='X-EMAP-USER-KEY:emapapidemouser1234' --header 'Content-Type: application/json' --header 'Accept: application/json'

cat emap_simulation.json | jq
```

## [WIP] Simulation API（ガス）
- APIの仕様の詳細については[こちら](https://emap-sim-base.enechange.jp/apidocs/gas-simulation)をご覧ください。
- [WIP] POSTするJSON例
  - request_idはリクエストを一意に特定するためのIDです。リクエストとレスポンスを紐付ける場合などに用います。
  - current_plan_codeおよびtarget_plan_codesはガス料金プランを一意に識別するコードになります。ガスのSimulation APIのみをご契約の場合は、診断対象としたいコードをお伝えいたします。Plan APIから取得することもできる予定です。
  - リクエストのJSONの例をsample_requests/req-gas-simulation-api-sample.jsonに示します。この例ではcurrent_plan_codeが"czkxj|hzozmtmvoztgdbcodibtw-czkxj"であるプランが比較元プランとなります。

```sh
# [WIP] curlの場合
curl -X POST -H "Content-Type: application/json" -H "X-EMAP-USER-KEY:emapapidemouser1234" -d @req-gas-simulation-api-sample.json https://emap-sim-base.enechange.jp/v001/gas/simulate > emap_gas_simulation.json
curl -X POST -H "Content-Type: application/json" -H "X-EMAP-USER-KEY:emapapidemouser1234" -d '{"request_id": "H14640da","postcode": "1000001","current_plan_code": "czkxj|hzozmtmvoztgdbcodibtw-czkxj","target_plan_codes": ["czkxj|hzozmtmvoztgdbcodibtw-czkxj","nvhkgzvbvn|hzozmtmvoztgdbcodibtw-czkxj","nvhkgzvbvn|hzozmtmvoztgdbcodibtwtb-czkxj"],"heater_type": 1,"water_heater_type": 1,"number_of_family": 3,"monthly_type": 1,"monthly_values": ["100","200","300","400","500","600","700","800","900","1000","1100","1200"],"latest_year_month": "202010","load_curve": [1.6,1.4,1.2,1,0.8,0.6,0.4,0.6,0.8,1,1.2,1.4]}' https://emap-sim-base.enechange.jp/v001/gas/simulate > emap_gas_simulation.json

# [WIP] wgetの場合
wget -q -O emap_gas_simulation.json https://emap-sim-base.enechange.jp/v001/gas/simulate --post-file req-gas-simulation-api-sample.json --header='X-EMAP-USER-KEY:emapapidemouser1234' --header 'Content-Type: application/json' --header 'Accept: application/json'

cat emap_gas_simulation.json | jq
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

| エリア名 | 事業者名 | プラン名 | fee_plan_code(※) |
|:-------- |:-------- |:-------- |:-------- |
| 北海道電力エリア | 北海道電力 | 従量電灯B | czkxj&#x7C;hzozmtmvoztgdbcodibtw-czkxj |
| | | 従量電灯C | czkxj&#x7C;hzozmtmvoztgdbcodibtx-czkxj |
| | | eタイム3プラス | czkxj&#x7C;zodhztbtkgpn-czkxj |
| | | 低圧電力 | czkxj&#x7C;gjrtqjgovbz-czkxj |
| | サンプルガス | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtw-czkxj |
| | | 従量電灯B（ガス割）| nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtwtb-czkxj |
| | | 従量電灯C | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtx-czkxj |
| | | 従量電灯C（ガス割）| nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtxtb-czkxj |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-czkxj |
| 東北電力エリア | 東北電力 | 従量電灯B | ojcjfpvzkxj&#x7C;hzozmtmvoztgdbcodibtw-ojcjfpvzkxj |
| | | 従量電灯C | ojcjfpvzkxj&#x7C;hzozmtmvoztgdbcodibtx-ojcjfpvzkxj |
| | | よりそう＋ナイト8（kVA契約） | ojcjfpvzkxj&#x7C;tjmdnjptidbcotg-ojcjfpvzkxj |
| | | よりそう＋ナイト8（実量契約）| ojcjfpvzkxj&#x7C;tjmdnjptidbcotgtedonpmtjp-ojcjfpvzkxj |
| | | 低圧電力 | ojcjfpvzkxj&#x7C;gjrtqjgovbz-ojcjfpvzkxj |
| | サンプルガス | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtw-ojcjfpvzkxj |
| | | 従量電灯C | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtx-ojcjfpvzkxj |
| | | お家ぷらん | nvhkgzvbvn&#x7C;cjhztkgvi-ojcjfpvzkxj |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-ojcjfpvzkxj |
| 東京電力エリア | 東京電力エナジーパートナー | 従量電灯B | ozkxj&#x7C;hzozmtmvoztgdbcodibtw-ozkxj |
| | | 従量電灯C | ozkxj&#x7C;hzozmtmvoztgdbcodibtx-ozkxj |
| | | スマートライフS | ozkxj&#x7C;nhvmotgdaztn-ozkxj |
| | | 低圧電力 | ozkxj&#x7C;gjrtqjgovbz-ozkxj |
| | サンプルガス | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtw-ozkxj |
| | | 従量電灯B（ガス割） | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtwtb-ozkxj |
| | | 従量電灯C | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtx-ozkxj |
| | | 従量電灯C（ガス割） | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtxtb-ozkxj |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-ozkxj |
| 中部電力エリア | 中部電力ミライズ | 従量電灯B | xcpyzi&#x7C;hzozmtmvoztgdbcodibtw-xcpyzi |
| | | 従量電灯C | xcpyzi&#x7C;hzozmtmvoztgdbcodibtx-xcpyzi |
| | | スマートライフプラン | xcpyzi&#x7C;nhvmotgdaz-xcpyzi |
| | | 低圧電力 | xcpyzi&#x7C;gjrtqjgovbz-xcpyzi |
| | サンプルガス | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtw-xcpyzi |
| | | 従量電灯C | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtx-xcpyzi |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-xcpyzi |
| 北陸電力エリア | 北陸電力 | 従量電灯B | mdfpyzi&#x7C;hzozmtmvoztgdbcodibtw-mdfpyzi |
| | | 従量電灯C | mdfpyzi&#x7C;hzozmtmvoztgdbcodibtx-mdfpyzi |
| | | くつろぎナイト12 | mdfpyzi&#x7C;fponpmjbdtza-mdfpyzi |
| | | 低圧電力 | mdfpyzi&#x7C;gjrtqjgovbz-mdfpyzi |
| 関西電力エリア | 関西電力 | 従量電灯A | fzkxj&#x7C;hzozmtmvoztgdbcodibtv-fzkxj |
| | | 従量電灯B | fzkxj&#x7C;hzozmtmvoztgdbcodibtw-fzkxj |
| | | はぴeタイムR | fzkxj&#x7C;cvkdtztodhztmtedonpmtjp-fzkxj |
| | | 低圧電力 | fzkxj&#x7C;gjrtqjgovbz-fzkxj |
| | サンプルガス | 従量電灯A | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtv-fzkxj |
| | | 従量電灯A（ガス割） | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtvtb-fzkxj |
| | | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtwtf-fzkxj |
| | | 従量電灯B（ガス割） | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtwtftb-fzkxj |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-fzkxj |
| 中国電力エリア | 中国電力 | 従量電灯A | zizmbdv&#x7C;hzozmtmvoztgdbcodibtv-zizmbdv |
| | | 従量電灯B | zizmbdv&#x7C;hzozmtmvoztgdbcodibtw-zizmbdv |
| | | ぐっとずっと。プラン 電化Styleコース | zizmbdv&#x7C;yzifv-zizmbdv |
| | | 低圧電力 | zizmbdv&#x7C;gjrtqjgovbz-zizmbdv |
| | サンプルガス | 従量電灯A | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtv-zizmbdv |
| | | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtwtf-zizmbdv |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-zizmbdv |
| 四国電力エリア | 四国電力 | 従量電灯A | tjiyzi&#x7C;hzozmtmvoztgdbcodibtv-tjiyzi |
| | | 従量電灯B | tjiyzi&#x7C;hzozmtmvoztgdbcodibtw-tjiyzi |
| | | でんかeプラン | tjiyzi&#x7C;yzifvtz-tjiyzi |
| | | 低圧電力 | tjiyzi&#x7C;gjrtqjgovbz-tjiyzi |
| | サンプルガス | 従量電灯A | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtv-tjiyzi |
| | | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtwtf-tjiyzi |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-tjiyzi |
| 九州電力エリア | 九州電力 | 従量電灯B | ftpyzi&#x7C;hzozmtmvoztgdbcodibtw-ftpyzi |
| | | 従量電灯C | ftpyzi&#x7C;hzozmtmvoztgdbcodibtx-ftpyzi |
| | | 電化でナイト・セレクト22 | ftpyzi&#x7C;yzifvtyztidbcotnzgzxo-ftpyzi |
| | | 低圧電力 | ftpyzi&#x7C;gjrtqjgovbz-ftpyzi |
| | サンプルガス | 従量電灯B | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtw-ftpyzi |
| | | 従量電灯C | nvhkgzvbvn&#x7C;hzozmtmvoztgdbcodibtx-ftpyzi |
| | | 低圧電力 | nvhkgzvbvn&#x7C;gjrtqjgovbz-ftpyzi |
| 沖縄電力エリア | 沖縄電力 | 従量電灯 | jfdyzi&#x7C;hzozmtmvoztgdbcodib-jfdyzi |
| | | Ee ホームホリデー | jfdyzi&#x7C;zzcjhztcjgdyvt-jfdyzi |
| | | 低圧電力 | jfdyzi&#x7C;gjrtqjgovbz-jfdyzi |

※Simulation API 利用時に必要な値となります。Plan APIでは必要ありません。

#:## 取得期間（燃料費調整額、再生可能エネルギー賦課金）
 - デモ環境では、燃料費調整額と再生可能エネルギー発電促進賦課金に関しては、現在の年を含んで過去３年分のみレスポンスを返します。
