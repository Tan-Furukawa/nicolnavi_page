+++
date = '2025-10-15T13:10:14+09:00'
draft = false
title = 'Tutorial1 - NicolNaviの基本操作'
+++

## 1. 概要
泥質片岩の顕微鏡動画を用いて NicolNavi の基本操作を習得するためのチュートリアルです。記載された手順に従うと、ステージ回転動画の前処理から粒界抽出、鉱物ラベルの設定、モード組成および形状定向配列の確認までを一通り実施できます。

## 2. 前提条件
- NicolNavi アプリケーションが起動済みで、対象プロジェクトが開かれていること
- サンプル動画 `nicolnavi_app/Examples/pelitic-schist-xpl.avi` を参照できること

## 3. 画面構成
- 左画面：解析対象画像のビューア。粒界やラベルの表示を確認できます。
- 右画面：番号付きボタン、パラメータ入力欄、チェックボックスが並ぶ操作パネルです。
- 画面中央部：`Image list` で左画面に表示する画像を選択します。

## 4. 操作フロー

### 4.1 動画ファイルの読み込み
1. 右画面の `1 Select` を押します。
2. ファイル選択ダイアログで `nicolnavi_app/Examples/pelitic-schist-xpl.avi` を選択し、開くを押します。
3. 左画面に動画の先頭フレームが幅 1000 px の画像として表示されることを確認します。

<img src="/images/page/tutorial/tutorial1/pic1.png" style="width: 30%;">

### 4.2 前処理の実行
1. `2 Start` を押し、動画の離散化と回転中心推定を開始します。
2. 処理が完了すると、自動的に回転中心調整画面へ遷移します。

<img src="/images/page/tutorial/tutorial1/pic2.png" style="width: 100%;">

### 4.3 回転中心の調整
1. 左画面の同心円状パターンの中心に赤い十字が一致しているか確認します。
2. 中心がずれている場合は `＋` `−` ボタンで座標を微調整します（今回の例ではほとんどずれていないためスキップ可能です）。
3. 調整後、`3 Continue` を押して回転中心を確定します。

### 4.4 解析結果の確認

1. 自動遷移後の画面で `Image list` を開き、以下の代表的な解析マップを確認します。

| マップ | 説明 |
| --- | --- |
| `color` | ステージ 1 回転中で最も明るい瞬間の色情報です。 |
| `extinction color` | 最も暗くなる瞬間の色情報です。完全な消光が得られない場合は前工程を見直してください。 |
| `R` | 各ピクセルのレタデーション値です。`Video` タブで指定したレタデーションを探索します。今回は最大レタデーションを `max retardation = 300` のまま使用しているため、石英以外の鉱物では値の信頼性が低い点に注意してください。 |
| `extinction angle` | 各ピクセルの光消角マップです。 |
| `raw image` | 加工していない画像です。たとえば、$\phi = 22.5\degree$を選択すると、顕微鏡ステージが、22.5$\degree$回転したときの動画のフレームを示します。|

<img src="/images/page/tutorial/tutorial1/pic3.png" style="width: 100%;">

{{< notice warning >}}
`extinction color` 全体が暗くならない場合は、カメラ画素の飽和や不適切な前処理が疑われます。このような例に対する処理は、Tutorial2で行います。
{{< /notice >}}

### 4.5 粒界抽出パラメータの調整
1. 右画面のパラメータ欄で目的に合わせて初期値を設定します。
2. `6 Detect Grain Boundary` を押し、左画面に粒界線が重ね表示されることを確認します。
3. `4 median filter` と `5 parameters` を下図のように調整します。
4. `6 Calculate grain boundaries` を再度押して結果を更新します。

<img src="/images/page/tutorial/tutorial1/pic4.png" style="width: 30%;">

<img src="/images/page/tutorial/tutorial1/pic5.png" style="width: 100%;">
図：左は初期設定での粒界推定結果、右はパラメータ調整後の結果

| パラメータ | 説明 |
| --- | --- |
| **median filter** | ノイズ低減用の平滑化サイズ。大き過ぎる値は微細構造を消します。 |
| **boundary detection sensitivity** | 粒界抽出の感度。高くすると細部まで抽出しますが誤検出も増えます。 |
| **boundary connectivity** | 途切れた境界を連結とみなす最大距離（px）。大き過ぎる値は不要な連結を生みます。 |
| **smallest grain size** | `px^2` 単位の最小粒径。値未満の領域はノイズとして除去されます。 |

{{< notice info >}}
粒界抽出は試行錯誤が前提です。`Show grain boundary` チェックボックスを活用し、結果を逐次確認してください。
{{< /notice >}}

{{< notice warning >}}
単一の設定で全鉱物ペアを完全に分離できるとは限りません。対象によっては鉱物ごとに条件を変えて処理する必要があります。
{{< /notice >}}

### 4.6 粒界抽出結果の確定
1. 抽出結果が意図どおりになったことを確認します。
2. `7 Continue` を押し、次のラベル付与画面へ進みます。

<img src="/images/page/tutorial/tutorial1/pic6.png" style="width: 100%;">

## 5. 鉱物ラベルの付与と確認

### 5.1 ラベル付与の準備
1. パラメータ欄の `8 Label Name` に `mica` などの鉱物名を入力します。
2. `9 Add Label` を押し、ラベル付与モードへ切り替えます。
3. 同じ手順で `other minerals` のラベルも追加します。

<img src="/images/page/tutorial/tutorial1/pic7.png" style="width: 100%;">

4. ラベル一覧の `10 mica` を選択し、左画面で該当鉱物の粒子をクリックして教師データを 5～10 個程度追加します。

<img src="/images/page/tutorial/tutorial1/pic8.png" style="width: 30%;">
<img src="/images/page/tutorial/tutorial1/pic9.png" style="width: 100%;">

### 5.2 ラベル精度の向上


1. `10 mica` と `11 other minerals` のラジオボタンを切り替え、それぞれの粒子に該当する部分を左画面から選択して教師データを追加します。
2. 初期状態では、誤判定が多数ありますが、ラベルの追加を繰り返すことで誤判定が減少していきます。
3. 画面表示の透過度や粒界表示はチェックボックスで切り替え、ラベルが正確に割り当てられているか確認してください。
4. 必要に応じてラベル名横のカラーボックスをクリックし、表示色を変更します。

<img src="/images/page/tutorial/tutorial1/pic10.png" style="width: 100%;">
図：`other minerals` を選択した後の表示例（個々の環境で色分布は異なります）

{{< notice info >}}
ラベル付与にはラベル伝搬法を使用しています。全粒子数のおよそ 20% 以上を教師データとして指定すると分類結果が得られます。今回の例では 144 個の粒子にラベルを付与しました。
{{< /notice >}}

### 5.3 ラベル設定の確定
1. すべての粒界領域に適切なラベルが付いていることを確認します。
2. `12 Done` を押し、ラベル設定を確定します。

<img src="/images/page/tutorial/tutorial1/pic11.png" style="width: 100%;">

## 6. 岩石組織の定量化（雲母の例）

### 6.1 ローズダイアグラムで定向配列を確認する
1. `13 other minerals` チェックボックスをオフにします。
2. ビュー選択で `14 rose diagram` および `15 shape preferred orientation` を選びます。
3. 右画面の `16 information` に表示される統計値を読み取ります。

<img src="/images/page/tutorial/tutorial1/pic12.png" style="width: 30%;">
<img src="/images/page/tutorial/tutorial1/pic13.png" style="width: 100%;">

表示例：

```text
Mean orientation: bulk: 171.952°, mica: 171.952°
Circular variance: bulk: 0.458, mica: 0.458
```

平均配向角（Mean orientation）は雲母の形状定向配列を示し、円分散（Circular variance）は 0 に近いほど配向が揃い、1 に近いほどランダム性が高いことを表します。

### 6.2 SPO ビューと楕円近似を確認する
1. `14 rose diagram` の代わりに `SPO` を選択します。
2. `Image list` から `ellipse` を選び、粒子を楕円近似した図を表示します。

<img src="/images/page/tutorial/tutorial1/pic14.png" style="width: 100%;">

### 6.3 粒子形状の散布図を確認する
1. ビューを `scatter` に切り替えます。
2. 軸設定を `x: ellipse major axis length`、`y: ellipse minor axis length` に変更します。
3. 散布図と回帰直線から、粒子形状のおおよその軸比を読み取ります（例：回帰直線 `y = 0.224 x` なら軸比約 0.224:1 = 1:3.2）。

<img src="/images/page/tutorial/tutorial1/pic15.png" style="width: 100%;">

### 6.4 モード組成（面積比）の確認
1. ビューを `histogram` に変更します。
2. `16 other minerals` チェックボックスをオンにします。
3. 解析量を `17 retardation` から `size` に変更し、面積に基づくヒストグラムを表示します。

<img src="/images/page/tutorial/tutorial1/pic16.png" style="width: 100%;">
<img src="/images/page/tutorial/tutorial1/pic17.png" style="width: 100%;">

表示例：

```text
Mean: bulk: 394.808, mica: 558.211, other minerals: 383.620
Std Dev: bulk: 1807.601, mica: 493.803, other minerals: 1863.461
Min: bulk: 14.000, mica: 17.000, other minerals: 14.000
Max: bulk: 36946.000, mica: 1814.000, other minerals: 36946.000
Mode: bulk: 24.000, mica: 130.000, other minerals: 24.000
Count: bulk: 593, mica: 38, other minerals: 555
Integral ratio: mica: 9.06%, other minerals: 90.94%
Count ratio: mica: 6.41%, other minerals: 93.59%
```

`Integral ratio` はヒストグラムの積分値を用いた面積比を表します。上記の例では、雲母（mica）のモード組成は約 9.1% と読み取れます。

## 7. まとめ
- 本書の手順で、ステージ回転動画の前処理から粒界抽出、鉱物ラベル付与、組織解析まで一通り実行できます。
- 粒界抽出とラベル伝搬は試行錯誤が必要です。結果が安定しない場合はパラメータと教師データのバランスを再調整してください。
- 解析結果は `information` 欄の数値およびグラフ表示で必ず検証し、撮影条件や試料特性との整合性を確認してください。
