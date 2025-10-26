+++
date = '2025-10-15T13:10:14+09:00'
draft = false
title = 'Tutorial3 - 結晶方位解析'
+++

## 1. 概要
多結晶石英試料を題材に、NicolNavi の結晶方位解析（CPO: Crystallographic Preferred Orientation）ワークフローを習得します。Tutorial 1・2 で学んだ前処理と粒界抽出を前提に、複数条件で撮影したステージ回転動画を組み合わせて c 軸方位を推定し、極図や COI マップで可視化します。

## 2. 前提条件
- NicolNavi アプリケーションが起動済みで、対象プロジェクトが開かれている
- `nicolnavi_app/Examples/cpo/` に配置されたサンプル動画を参照できる
- Tutorial 1 および Tutorial 2 の操作（前処理、粒界抽出、ラベル付与）を一通り試している
- **結晶方位解析は、動画解析に大きなメモリ領域を使用するため、メモリ16GB以上のPCを強く推奨します。それ以下のPCでは、処理の途中で停止するなどの正常に機能しない可能性があります。**

## 3. 使用する動画セット
NicolNavi の CPO 解析は 5 本の動画を入力して処理します。「Data Acquisition」を参照してください。

| 撮影条件 | ファイル名 | 目的 |
| --- | --- | --- |
| XPL Rotated | `XPL.avi` | クロスニコル下で 360° 回転動画 |
| XPL + λ-Plate Rotated | `XPL-lambda.avi` | クロスニコル＋鋭敏色検板を挿入した回転動画|
| XPL + λ-Plate 0° tilt | `0deg-tilt.avi` | ステージ 0° で薄片を傾斜させ、焦点位置を変えて撮影した動画 |
| XPL + λ-Plate 45° tilt | `45deg-tilt.avi` | ステージ 45° で傾斜させ、焦点位置を変えて撮影した動画 |
| XPL + λ-Plate 45° (no tilt) | `45deg-not-tilt.avi` | ステージ 45° のまま水平で撮影した短い動画 |

## 4. 操作フロー

### 4.1 動画ファイルの読み込みとレタデーション設定
1. `map` タブ右上の `Select`（XPL Rotated）を押し、`Examples/cpo/XPL.avi` を選択します。
2. 同じ手順で残り 4 本の動画 (`XPL-lambda.avi`, `0deg-tilt.avi`, `45deg-tilt.avi`, `45deg-not-tilt.avi`) をそれぞれの入力枠に読み込みます。
3. すべての動画が選択されたら、`XPL Rotated` セクションの `max retardation` を設定します。薄片上で最も高いレタデーションを示す石英のレタデーションとして十分な、既定値の300 nm とします。この数値はおおまかな数値で問題ありません。後に詳細に設定する箇所があります。

{{< notice info >}}
ここで入力されるmax retardationの数値は、鉱物の偏角の判定と鉱物のレタデーションの判定に使用されます。
{{< /notice >}}


{{< notice warning >}}

ここで入力されるmax retardationの数値が300 nm以上であると、鉱物のc軸の偏角の判定が失敗する可能性が高まります。結晶方位解析のように、c軸の偏角を求める必要がある場合は、300以下の数字を記入しましょう。また、石英の最大レタデーションが、300 nm以下となるように薄片を調整しましょう。

{{< /notice >}}


<img src="/images/page/tutorial/tutorial3/pic1.png" style="width: 30%;">

### 4.2 前処理と回転中心の確認
1. `Start` を押して動画の離散化と基準画像生成を始めます。Tutorial 1・2 と比べて処理時間が長い点に注意してください。
2. 回転中心調整画面に遷移したら、赤い十字と同心円パターンが概ね一致しているか確認します。今回のサンプルでは自動検出で十分なため、調整不要なら `Continue` を押します。


### 4.3 粒界抽出パラメータの調整
1. `map` タブに戻り、`Detect Grain Boundary` を実行して初期結果を表示します。
2. `median filter` のスライダーを 1 段階右に動かし、その他の感度パラメータは下図を参照します。
3. `Calculate grain boundaries` を再度押して結果を更新します。粒界が適切になったら `Continue` を押し、ラベル付け画面へ進みます。

<img src="/images/page/tutorial/tutorial3/pic2.png" style="width: 30%;">

### 4.4 鉱物ラベルの付与
1. 今回は上下の単結晶石英に挟まれた石英脈の方位を調べる設定です。`Label name` に `single crystal quartz` と `vein quartz` を追加します。
2. それぞれのラベルを選択し、左画面で該当する粒子をクリックして教師データを追加します。今回は17 粒子のラベルを与えたました。(環境により指定しなければならないラベルの個数は異なります。)
3. 誤ラベルが残る場合は、追加で教師データを増やしてください。分類が安定したら `Done` を押してラベルを確定します。

<img src="/images/page/tutorial/tutorial3/pic3.png" style="width: 100%;">

### 4.5 CPO 解析の実行
1. `analysis` タブに切り替え、ビューを `rose diagram` から `CPO` に変更します。
2. `max R` に前工程で設定した最大レタデーション（ここでは 260）を入力し、`Start CPO computation` を押します。

{{< notice info >}}
- 薄片の厚さが判明しているときは、 `max R` `thickness`のラジオボタンから`thickness`を選択しましょう。

- もし薄片の厚さがわからないときは、鉱物の最大レタデーションを使用します。鉱物の最大レタデーションは、`histogram` `retardation`を選択して、`information`からレタデーションの最大値を読み取ることができます。今回は`260`を読み取りました。
{{< /notice >}}

{{< notice warning >}}
`histogram` の最大レタデーションは外れ値の影響を受けやすく、単一粒子の過大評価だけでも実際より高い値が算出される場合があります。代表的な複数粒子の値を確認し、95 パーセンタイルなどを参考に `max R` を再調整してください。
{{< /notice >}}

3. 計算が完了すると、`map` タブの `image list` に COI（crystal orientation index）マップが追加されます。


{{< notice info >}}
ここで入力されるmax Rの数値をもとに、鉱物のレタデーションの再計算・結晶軸の傾きの計算が行われます。
{{< /notice >}}

<img src="/images/page/tutorial/tutorial3/pic4.png" style="width: 30%;">

{{< notice warning >}}
計算中に別タブへ切り替えたり、アプリを最小化すると処理が中断される場合があります。プログレスバーが消えるまで待ってから次の操作に移ってください。
{{< /notice >}}

### 4.6 COI マップの確認
1. `image list` から `map coi` の `φ = 0–360°` を選択します。薄片上で各石英単結晶の c 軸方位を疑似カラーで可視化できます。

{{< notice info >}}
COIマップは基本的に一部分が欠損します。この理由は、薄片を傾けたときに顕微鏡の視野がずれますが、傾ける前後の画像の比較において、このズレに起因する参照できないピクセルが発生するためです。
{{< /notice >}}

<img src="/images/page/tutorial/tutorial3/pic5.png" style="width: 100%;">

### 4.7 極図での可視化
1. `analysis` タブに戻り、`single crystal quartz` のチェックを外して石英脈にフォーカスします。
2. ビューを `polar plot` に切り替え、`φ = 0–360°` を選択すると、各粒子を 1 点としてプロットした極図が表示されます。

<img src="/images/page/tutorial/tutorial3/pic6.png" style="width: 100%;">

3. `Plot for each pixel` をオンにすると、粒子ではなくピクセル単位で c 軸方位を描画できます。局所的な方位ばらつきを確認したいときに有効です。

<img src="/images/page/tutorial/tutorial3/pic7.png" style="width: 100%;">

4. `display points` にチェックを入れると、極図の推定に使われた方位データ点が表示されます。離散化時に点が重なるのを避けるため、既定で 0.5% のノイズが加えられており、必要に応じて `Point noise` で調整できます。

<img src="/images/page/tutorial/tutorial3/pic8.png" style="width: 100%;">

## 5. まとめ
- CPO 解析では、複数条件で撮影した動画を組み合わせて位相差の変化を再構成し、石英の c 軸方位を推定します。
- ラベルを正しく付与することで、石英脈や単結晶領域など関心領域ごとに極図や COI マップを分けて評価できます。
