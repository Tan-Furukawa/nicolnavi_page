+++
date = '2025-10-15T13:10:14+09:00'
draft = false
title = 'Tutorial2 - 鉱物の抽出と可視化'
+++

## 1. 概要
セクターゾーニングしたざくろ石（garnet）の顕微鏡動画を題材に、鉱物ごとの粒界抽出と光学的異方性の可視化手順を解説します。Tutorial 1 で学んだ基本操作を土台に、単結晶内部の消光挙動を強調するための設定変更や、鉱物ごとのラベル付与を実践します。

## 2. 前提条件
- NicolNavi アプリケーションが起動済みで、対象プロジェクトが開かれていること
- サンプル動画 `nicolnavi_app/Examples/sector_zoning.avi` を参照できること
- Tutorial 1 の操作（前処理・粒界抽出・ラベル付与）を一通り試していること

## 3. 操作フロー

### 3.1 動画ファイルの読み込み
1. 右画面の `Select` を押します。
2. ファイル選択ダイアログで `nicolnavi_app/Examples/sector_zoning.avi` を選択し、開くを押します。
3. 左画面に動画の先頭フレームが幅 1000 px の正方形画像として表示されることを確認します。
4. `Start` を押し、動画の離散化と回転中心推定を開始します。

### 3.2 回転中心の調整
1. 処理完了後、自動的に回転中心調整画面へ遷移します。

<img src="/images/page/tutorial/tutorial2/pic1.png" style="width: 100%;">

2. このデータでは自動検出された中心が実際の同心円パターンから外れています。`cx` と `cy` をクリックし、赤い十字を実際の中心に合わせてください。

<img src="/images/page/tutorial/tutorial2/pic2.png" style="width: 100%;">
図：中心を手動調整した状態

3. 調整後、`Continue` を押して回転中心を確定します。

### 3.3 解析マップと明度補正の確認
1. 画面遷移後に `Image list` を開き、代表的な解析マップを確認します。

| マップ | 説明 |
| --- | --- |
| `color` | ステージ 1 回転中で最も明るい瞬間の色情報です。 |
| `extinction color` | 最も暗くなる瞬間の色情報です。完全な消光が得られない場合は前工程を見直してください。 |
| `R` | 各ピクセルのレタデーション値です。`Video` タブで指定したレタデーションを探索します。 |
| `extinction angle` | 各ピクセルの光消角マップです。 |
| `raw image` | 未加工の画像です。例えば `phi = 22.5°` を選ぶと、その回転角でのフレームを表示します。 |

2. `color` と `extinction color` を切り替えると、`color` の方が暗く表示される場合があります。

<img src="/images/page/tutorial/tutorial2/pic3.png" style="width: 80%;">

図：`Image list` が `color` のとき

<img src="/images/page/tutorial/tutorial2/pic4.png" style="width: 80%;">

図：`Image list` が `extinction color` のとき

3. `color` では既定で **brightness correction** が有効になっており、各座標 `(x, y)` について一回転中の最大明度を \(B_{\max}(x, y)\)、最小明度を \(B_{\min}(x, y)\) とすると、表示値は

   \[
   I_{\text{corr}}(x, y) = B_{\max}(x, y) - B_{\min}(x, y)
   \]

   として計算されます。画像の干渉色をより正しい色調で表現するために行われる処理です。しかし、撮影時に画素が飽和すると \(B_{\max}(x, y) \approx B_{\min}(x, y)\) となり、`color` の明度が過小評価されて暗く見えます。

4. 飽和が疑われる場合は右画面の **brightness correction** チェックボックスをオフにし、

   \[
   I_{\text{corr}}(x, y) = B_{\max}(x, y)
   \]

   をそのまま表示して明度を確認します。

<img src="/images/page/tutorial/tutorial2/pic5.png" style="width: 80%;">

図：`Image list` が `color` で、`brightness correction`のチェックを外したとき

### 3.4 粒界抽出の実行
1. **brightness correction** の設定を確認したら、`Detect Grain Boundary` を押して粒界の初期推定結果を表示します。
2. `median filter` や `parameters` を必要に応じて調整します。今回は `boundary connectivity` を 5 に変更しました。
3. `Calculate grain boundaries` を再度押し、調整結果を反映します。
4. 粒界が意図どおりになったら、`Continue` を押してラベル付与画面へ進みます。

<img src="/images/page/tutorial/tutorial2/pic6.png" style="width: 30%;">

### 3.5 鉱物ラベルの付与
1. パラメータ欄の `Label Name` に `garnet` を入力し、`+` を押します。同様に `inclusion`、`matrix` も追加します。
2. ラベル一覧から `garnet` を選択し、ざくろ石に該当する粒子を左画面でクリックして教師データを追加します。
3. `inclusion` と `matrix` も同様に、該当する領域を 5～10 個ずつ目安にラベル付けします。
4. 誤判定が多い場合はラベルの追加を繰り返し、結果が安定したら `Done` を押してラベル設定を確定します。今回の例では合計 48 粒子を教師データとして指定しました。

<img src="/images/page/tutorial/tutorial2/pic7.png" style="width: 100%;">

### 3.6 光学的異方性の可視化
1. `Analysis` タブに切り替え、`inclusion` と `matrix` のチェックを外してざくろ石のみを表示します。

<img src="/images/page/tutorial/tutorial2/pic8.png" style="width: 30%;">

2. `map` タブに戻り、`image list` から `extinction angle` を選択します。選択した鉱物以外は黒く表示され、ざくろ石内部のセクターゾーニングが強調されます。

<img src="/images/page/tutorial/tutorial2/pic9.png" style="width: 100%;">

3. 必要に応じて画面上部のメニューから `Save as PDF` を選択し、可視化結果を PDF として保存します。

{{< notice info >}}
光学異方性の可視化は、波状消光を示す単結晶にも有効です。
{{< /notice >}}

## 4. まとめ
- 画素飽和が疑われる場合は **brightness correction** を無効化し、`color` の明度を補正してください。
- 鉱物ラベルを正確に付与すると、`extinction angle` などのマップを鉱物別に抽出でき、鉱物を適切な形状で切り抜いて可視化することが可能になります。


