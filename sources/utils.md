<span style="float:right;">[[source]](https://github.com/fchollet/keras/blob/master/keras/utils/generic_utils.py#L16)</span>
### CustomObjectScope

```python
keras.utils.generic_utils.CustomObjectScope()
```

`_GLOBAL_CUSTOM_OBJECTS`をエスケープできないスコープを提供します．

`with`では，名前によってcustomオブジェクトにアクセス可能です．
グローバルなcustomオブジェクトへの変更は`with`で囲まれた中でのみ持続し，
`wuth`から抜けると，グローバルなcustomオブジェクトは`with`の最初の状態に戻ります．

__例__

`MyObject`というcustomオブジェクトの例です．

```python
with CustomObjectScope({'MyObject':MyObject}):
    layer = Dense(..., kernel_regularizer='MyObject')
    # save, load, etc. will recognize custom object by name
```

---

<span style="float:right;">[[source]](https://github.com/fchollet/keras/blob/master/keras/utils/generic_utils.py#L15)</span>
### HDF5Matrix

```python
keras.utils.io_utils.HDF5Matrix(datapath, dataset, start=0, end=None, normalizer=None)
```

Numpy 配列の代わりに使えるHDF5 datasetの表現です．

### 例

```python
x_data = HDF5Matrix('input/file.hdf5', 'data')
model.predict(x_data)
```

`start`と`end`を指定することでdatasetをスライスできます．

normalizer関数（やラムダ式）を渡せます． normalizer関数は取得されたすべてのスライスに適用されます．

__引数__

- __datapath__: 文字列，HDF5ファイルへのパス
- __dataset__: 文字列，datapathで指定されたファイル中におけるHDF5 datasetの名前
- __start__: 整数，指定されたdatasetのスライスの開始インデックス
- __end__: 整数，指定されたdatasetのスライスの終了インデックス
- __normalizer__: 読み込まれた時にデータに対して適用する関数

__戻り値__

array-likeなHDF5 dataset．

---

<span style="float:right;">[[source]](https://github.com/fchollet/keras/blob/master/keras/utils/generic_utils.py#L300)</span>
### Sequence

```python
keras.utils.data_utils.Sequence()
```


datasetのようなデータの系列にfittingのためのベースオブジェクト．

Sequenceは`__getitem__`と`__len__`メソッドを実装します．


### 例

``` python
from skimage.io import imread
from skimage.transform import resize
import numpy as np

__Here, `x_set` is list of path to the images__

# and `y_set` are the associated classes.

class CIFAR10Sequence(Sequence):
    def __init__(self, x_set, y_set, batch_size):
        self.X,self.y = x_set,y_set
        self.batch_size = batch_size

    def __len__(self):
        return len(self.X) // self.batch_size

    def __getitem__(self,idx):
        batch_x = self.X[idx*self.batch_size:(idx+1)*self.batch_size]
        batch_y = self.y[idx*self.batch_size:(idx+1)*self.batch_size]

        return np.array([
            resize(imread(file_name), (200,200))
               for file_name in batch_x]), np.array(batch_y)
```

---


### to_categorical

```python
to_categorical(y, num_classes=None)
```
整数のクラスベクトルから2値クラスの行列への変換します．

例えば，`categorical_crossentropy`のために使います．

__引数__

- __y__: 行列に変換されるクラスベクトル（0から`num_classes`までの整数）
- __num_classes__: 総クラス数

__戻り値__

入力のバイナリ行列表現

---

### normalize

```python
normalize(x, axis=-1, order=2)
```

Numpy配列の正規化

__引数__

- __x__: 正規化するNumpy 配列．
- __axis__: 正規化する軸．
- __order__: 正規化するorder（例: L2ノルムでは2）．

__戻り値__

Numpy 配列の正規化されたコピー．

---

### convert_all_kernels_in_model

```python
convert_all_kernels_in_model(model)
```

モデル内の全convolution kernelsをTheanoからTensorFlowへ変換．

TensorFlowからTheanoへの変換も可能です．

__引数__

- __model__: 変換対象となるモデル．

---

### plot_model

```python
plot_model(model, to_file='model.png', show_shapes=False, show_layer_names=True, rankdir='TB')
```

Kerasモデルをdotフォーマットに変換しファイルに保存します．

__引数__

- __model__: Kerasのモデルインスタンス
- __to_file__: 保存するファイル名
- __show_shapes__: shapeの情報を表示するかどうか
- __show_layer_names__: レイヤー名を表示するかどうか
- __rankdir__: PyDotに渡す`rankdir`引数，プロットのフォーマットを指定する文字列：'TB' はvertical plot，'LR'はhorizontal plot．

---

### custom_object_scope

```python
custom_object_scope()
```

`_GLOBAL_CUSTOM_OBJECTS`をエスケープできないスコープを提供します．

`with`では，名前によってcustomオブジェクトにアクセス可能です．
グローバルなcustomオブジェクトへの変更は`with`で囲まれた中でのみ持続し，
`wuth`から抜けると，グローバルなcustomオブジェクトは`with`の最初の状態に戻ります．

__例__

MyObjectというcustomオブジェクトの例です．

```python
with custom_object_scope({'MyObject':MyObject}):
    layer = Dense(..., kernel_regularizer='MyObject')
    # save, load, etc. will recognize custom object by name
```

__引数__

- __*args__: customオブジェクトに追加する名前とクラスのペアを格納した辞書の可変長リスト．

__戻り値__

`CustomObjectScope`型のオブジェクト．

---

### get_custom_objects

```python
get_custom_objects()
```

customオブジェクトのグローバル辞書への参照を返します．

custumオブジェクトの更新や削除は`custom_object_scope`の使用が好まれますが，
`_GLOBAL_CUSTOM_OBJECTS`への直接的なアクセスに`get_custom_objects`も利用可能です．

__例__

```python
get_custom_objects().clear()
get_custom_objects()['MyObject'] = MyObject
```

__戻り値__

クラス名へのグローバルな辞書 (`_GLOBAL_CUSTOM_OBJECTS`) ．

---

### serialize_keras_object

```python
serialize_keras_object(instance)
```

---

### deserialize_keras_object

```python
deserialize_keras_object(identifier, module_objects=None, custom_objects=None, printable_module_name='object')
```

---

### get_file

```python
get_file(fname, origin, untar=False, md5_hash=None, cache_subdir='datasets', file_hash=None, hash_algorithm='auto', extract=False, archive_format='auto', cache_dir=None)
```

既にキャッシュされていないならURLからファイルをダウンロードします．

デフォルトでは，`original`のURLからダウンロードされたファイルは，
キャシュディレクトリ`~/.keras`下の，そのサブディレクトリ`datasets`下の，`fname`で保存します．
したがって最終的な`example.txt`の保存位置は，`~/.keras/datasets/example.txt`です．

tar，tar.gz，tar.bz，zipファイルは展開可能です．ダウンロード後にファイルのハッシュ値を渡すことでハッシュ値を検証します．
ハッシュ計算はshasumとsha256sumのコマンドラインプログラムです．

__引数__

- __fname__: ファイル名．もし絶対パス`/path/to/file.txt`が指定されたファイルは，その位置に保存します．
- __origin__: ファイルの置かれているURL．
- __untar__: 'extract'により非推奨．真理値，ファイルを展開するかどうか．
- __md5_hash__: 'file_hash'により非推奨．ファイル検証に使うmd5ハッシュ．
- __file_hash__: ダウンロード後のハッシュ文字列．sha256とmd5をサポート．
- __cache_subdir__: ファイルを保存するKerasのキャシュディレクトリ下のサブディレクトリ．絶対パス`/path/to/folder`が指定された場合は，そこに保存します．
- __hash_algorithm__: ファイル検証のためのハッシュアルゴリズムの選択．'md5'か'sha256'か'auto'．デフォルトの'auto'は使用されているアルゴリズムを検出します．
- __extract__: Trueならtarやzipのようなアーカイブファイルを展開します．
- __archive_format__: 展開するアーカイブフォーマット．オプションは，'auto'，'tar'，'zip'，None．'tar'はtar，tar.gz，tar.bzファイルを含みます．デフォルト値の'auto'は['tar', 'zip']です．Noneか空リストでは何も該当しません．
- __cache_dir__: キャシュファイルの保存位置，Noneでは[Keras Directory](https://keras.io/ja/getting-started/faq/#where-is-the-keras-configuration-filed-stored)のデフォルト.

__戻り値__

ダウンロードされたファイルへのパス．
