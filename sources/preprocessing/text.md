## text_to_word_sequence

```python
keras.preprocessing.text.text_to_word_sequence(text,
                                               filters='!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n',
                                               lower=True,
                                               split=" ")
```

文章を単語のリストに分割します．

- __戻り値__: 単語（文字列）のリスト．

- __引数__:
    - __text__: 文字列．
    - __filters__: 句読点などフィルタする文字を含むリスト（あるいはコレクション）．デフォルトは基本的な句読点，タブ，改行を含む'!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n'です．
    - __lower__: 真理値．テキストを小文字にするかどうか．
    - __split__: 文字列．単語を分割するセパレータ．

---

## one_hot

```python
keras.preprocessing.text.one_hot(text,
                                 n,
                                 filters='!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n',
                                 lower=True,
                                 split=" ")
```

文章を単語インデックス（語彙数n）のリストにone-hotエンコードします．

ハッシュ関数として`hash`を使用する`hashing_trick`のラッパーです．

- __戻り値__: [1, n]の整数から構成されるリスト．各整数は単語をエンコードします（単一性は保証されません）．

- __引数__:
    - __text__: 文字列．
    - __n__: 整数．語彙数．
    - __filters__: 句読点などフィルタする文字を含むリスト（あるいはコレクション）．デフォルトは基本的な句読点，タブ，改行を含む'!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n'です．
    - __lower__: 真理値．テキストを小文字にするかどうか．
    - __split__: 文字列．単語を分割するセパレータ．

---

## hashing_trick

```python
keras.preprocessing.text.hashing_trick(text,
                                       n,
                                       hash_function=None,
                                       filters='!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n',
                                       lower=True,
                                       split=' ')
```

テキストを固定長のハッシュ空間におけるインデックスの系列に変換します．

- __戻り値__:
        単語のインデックスを表す整数のリスト（単一性を保証しない）
- __引数__:
    - __text__: 文字列
    - __n__: ハッシュ空間の次元数
    - __hash_function__: デフォルトはpythonの`hash`関数で、'md5'か文字列を整数に変換する任意の関数．
            'hash'は安定したハッシュ関数ではありません，なので
            実行ごとに一貫しませんが，'md5'は安定なハッシュ関数です．
    - __filters__: 句読点などフィルタする文字を含むリスト（あるいはコレクション）．デフォルトは基本的な句読点，タブ，改行を含む'!"#$%&()*+,-./:;<=>?@[\]^_`{|}~\t\n'です．
    - __lower__: 真理値．テキストを小文字にするかどうか．
    - __split__: 文字列．単語を分割するセパレータ．

---

## Tokenizer

```python
keras.preprocessing.text.Tokenizer(num_words=None,
                                   filters='!"#$%&()*+,-./:;<=>?@[\\]^_`{|}~\t\n',
                                   lower=True,
                                   split=" ",
                                   char_level=False)
```

テキストをベクトル化する，または/かつ，テキストをシーケンス（= データセット中でランクi（1から始まる）の単語がインデックスiを持つ単語インデックスのリスト）に変換するクラス．

- __引数__: `text_to_word_sequence` と同じです．
    - __num_words__: Noneまたは整数．利用する単語の最大数（もしこの引数が与えられた場合，データセット中の頻度上位num_wordsの単語に制限されます）．
    - __char_level__: Trueなら，全文字はトークンとして扱われる

- __メソッド__:

    - __fit_on_texts(texts)__:
        - __引数__:
            - __texts__: 学習に使う文章のリスト．

    - __texts_to_sequences(texts)__
        - __引数__:
            - __texts__: シーケンスに変換する文章のリスト．
        - __戻り値__: シーケンスのリスト（入力文章ごとに1つ）．

    - __texts_to_sequences_generator(texts)__: 上記のジェネレータ版．
        - __戻り値__: 入力文章ごとに1つのシーケンス．

    - __texts_to_matrix(texts)__:
        - __戻り値__: `(len(texts), num_words)` のshapeをもつNumpy 配列．
        - __引数__:
            - __texts__: ベクトル化する文章のリスト．
            - __mode__: "binary", "count", "tfidf", "freq" のいずれか（デフォルト: "binary"）．

    - __fit_on_sequences(sequences)__:
        - __引数__:
            - __sequences__: 学習に使うシーケンスのリスト．

    - __sequences_to_matrix(sequences)__:
        - __戻り値__: `(len(sequences), num_words)` のshapeをもつNumpy 配列．
        - __引数__:
            - __sequences__: ベクトル化するシーケンスのリスト．
            - __mode__: "binary", "count", "tfidf", "freq" のいずれか（デフォルト: "binary"）．

- __属性__:
    - __word_counts__: 単語（文字列）とそれがfit中に表れた回数をマッピングする辞書．`fit_on_texts`が呼ばれた後にのみセットされます．
    - __word_docs__: 単語（文字列）とfit中に表れた文書/文章の数をマッピングする辞書．`fit_on_texts`が呼ばれた後にのみセットされます．
    - __word_index__: 単語（文字列）とそのランク/インデックス（整数）をマッピングする辞書．`fit_on_texts`が呼ばれた後にのみセットされます．
    - __document_count__: 整数．訓練に利用された文書（文章／シーケンス）数．`fit_on_texts`，または`fit_on_sequences`が呼ばれた後にのみセットされます．
