# rwseについて

製作者: Kaoru Matsui

**rwse**(R wavelet shrinkage estimation)は，ソフトウェアフォールト発見数データから，離散型非同次ポアソン過程における強度関数を推定するプログラムです．

# rwseの使い方
このリポジトリをcloneした後, 必要なパッケージをインストールしてrwse直下で`/src/WaveletTransform.R`をインポートして使用してください.

詳しい使い方はrwse/examples/ExampleUsage.Rをご覧ください．

# rwseで使える各関数について
## loadData()
データセットを読み込むための関数です. txt形式で, 1行目にテスト時刻, 2行目にフォール発見数が記載されているものに限ります.
```
loadData(
    dataPath = "データセットのパス"
)
```
## wse()
wseを行う関数です. 引数は以下をとることができます. データ変換, 閾値決定アルゴリズム, 閾値法の詳しい内容は後述します.
```
wse(
   data = データセット
   dt =  ("none", "A1", "A2", "A3", "B1", "B2", "Fi", "Fr"), #データ変換の指定
   thresholdName = ("ldt", "ut", "lut", "lht"), #閾値決定アルゴリズムの指定
   thresholdMode = ("h", "s"), #閾値法の指定
   var = データ変換の際の分散を指定(デフォルトは1),
   index = 分割データのデータ長を指定,
   initThresholdvalue = 閾値の初期値(適当で良い),
)
```
### データ変換
データ変換は`dt`によって指定することができます. rwseでは次の表のデータ変換が実装されています.
| 変数名 | 変換名 |
| ------------- | ------------- |
| none  | データ変換を行わない  |
| A1  | Anscombe transformation 1  |
| A2  | Anscombe transformation 2  |
| A3  | Anscombe transformation 3  |
| B1  | Bartlet transformation 1  |
| B2  | Bartlet transformation 2  |
| Fi  | Fisz transformation  |
| Fr  | Freeman transformation |

### 閾値決定アルゴリズム
閾値決定アルゴリズムは`thresholdName`によって指定することができます. rwseでは次の表の閾値決定アルゴリズムが実装されています.
| 変数名 | 閾値決定アルゴリズム名 | 備考 |
| ------------- | ------------- | ------------- |
| ldt | Level-dependent-Threshold | dt="none"を指定した場合のみ適用化 |
| ut | Universal-Threshold | dt="none"以外を指定した場合のみ適用化 |
| lut | Level-dependent Universal Threshold | dt="none"以外を指定した場合のみ適用化 |
| lht | Level-out-half Cross-validation Threshold | dt="none"以外を指定した場合のみ適用化 |

### 閾値法
閾値法は`thresholdMode`によって指定することができます. rwseでは次の表の閾値法が実装されています.
| 変数名 | 閾値決定アルゴリズム名 |
| ------------- | ------------- |
| s | Soft thresholding method |
| h | Hard thresholding method |

### 実行結果
wse()は推定値, スケーリング係数, ウェーブレット係数, 閾値処理後のウェーブレット係数を返します. データ構造は以下となります.
```
result = wse()

result
$estimationData #推定値
$cs #スケーリング係数
$ds #ウェーブレット係数
$denoiseDs #閾値処理後のウェーブレット係数
```

## tipsh()
tipshアルゴリズムを用いてwseを行う関数です. 各引数以下をとることができます. データ変換, 閾値法は`wse()`と同様です.
```
tipsh(
   data = データセット
   thresholdMode = ("h", "s"), #閾値法の指定
   var = データ変換の際の分散を指定(デフォルトは1),
   index = 分割データのデータ長を指定,
)
```
### 実行結果
tipsh()は推定値, スケーリング係数, ウェーブレット係数, 閾値処理後のウェーブレット係数を返します. データ構造は以下となります.
```
result = tipsh()

result
$estimationData #推定値
$cs #スケーリング係数
$ds #ウェーブレット係数
$denoiseDs #閾値処理後のウェーブレット係数
```

## createResult()
hard閾値法とsoft閾値法の実行結果を同時に.csvと.Rdata形式で保存する関数です.
```
createResult(
    hard = hard閾値法の実行結果,
    soft = soft閾値法の実行結果,
    index = 分割データのデータ長を指定(ファイル名に影響),
    resultPath = 実行結果を格納するパス
)
```
