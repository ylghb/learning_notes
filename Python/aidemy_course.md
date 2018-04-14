# Aidemy.net

## Numpy
「y = x[:]」または「y = list(x)」

Pythonのリストとndarrayの相違点としては、ndarrayのスライスは配列のコピーではなく view であることです。 viewとは、もとの配列と同じデータを指している ことを指します。つまりndarrayのスライスの変更は、オリジナルのndarrayを変更するということになります。前節で確認した通り、スライスを コピーとして扱いたい場合にはarr[:].copy() とします。

個々の要素つまりスカラー値にたどり着くには、インデックスを2つ指定する必要があります。つまり、arr[1][2]またはarr[1, 2]のようにアクセスする必要があります。

あるndarray配列から ある特定の順序で行を抽出 するには、 その順番を示す配列 をインデックス参照として渡せばいいです。

スライスとは異なり、ファンシーインデックス参照は常に元データのコピーを返し、新しい要素を作成します。

np.sort()関数はソートした配列のコピーを返す関数 である点です。
b = np.sort(a, axis=1) # 对a按每行中元素从小到大排序
argsort()メソッドは ソート後の配列のインデックス を返します。


## Pandas

Seriesに要素を追加する場合、追加する要素もまたSeries型である必要があります。
```
series = series.append(pd.Series([3], index=["grape"]))
```


### bool 型のシーケンス フィルタリング
series[series >= 5]とすると値が5以上の要素のみを含むSeriesを得ることができます。また、series[ ][ ]のように[ ]を複数個後ろに付け加えることで複数の条件を付け加えることができます。

```
series = series[series >= 5][series < 10]
```

### DataFrameの生成
```
pd.DataFrame([Series, Series, ...])
```

### 行を追加する
df.append("Series型のデータ", ignore_index=True)

ただし、dfのカラムとdfに追加するSeries型のデータのインデックスが一致しない場合は、dfにて新しいカラムが追加され値が存在しない要素はNaNで埋められます。

```
data = {"fruits": ["apple", "orange", "banana", "strawberry", "kiwifruit"],
        "year": [2001, 2002, 2001, 2008, 2006],
        "time": [1, 4, 5, 6, 3]}
df = pd.DataFrame(data)
series = pd.Series(["mango", 2008, 7], index=["fruits", "year", "time"])

df = df.append(series, ignore_index=True)
```

### 列を追加する
df["新しいカラム"]にSeriesもしくはリストを代入
```
data = {"fruits": ["apple", "orange", "banana", "strawberry", "kiwifruit"],
        "year": [2001, 2002, 2001, 2008, 2006],
        "time": [1, 4, 5, 6, 3]}
df = pd.DataFrame(data)

df["price"] = [150, 120, 100, 300, 150]
print(df)
```

### データの参照
loc, ilocを扱います。locは 名前による参照 を行い、ilocは 番号による参照 を行います。 


### 番号による参照

```
# iloc[]を使ってdfの2行目から5行目までの4行と、"banana", "kiwifruit"の2列を含むDataFrameをdfに代入してください
df0 = df.iloc[range(1,5), [2, 4]]
df1 = df.iloc[1:5,[2,4]]
```

### 行または列の削除
```
df_1 = df.drop(range(0, 2))

# drop()を用いてdfの列"year"を削除
df_2 = df.drop("year", axis=1)
```

### フィルタリング
```
df[df.index % 2 == 0]
# df.loc[df["カラム"]を含む条件式]

# フィルタリングを用いて、dfの"apple"列が5以上
# かつ"kiwifruit"列が5以上の値をもつ行を含むDataFrameをdfに代入してください
df = df.loc[df["apple"] >= 5]
df = df.loc[df["kiwifruit"] >= 5]
#df = df.loc[df["apple"] >= 5][df["kiwifruit"] >= 5]でもOK
```


### 連結
DataFrame同士を一定の方向についてそのままつなげる操作を 連結
pandas.concat("DataFrameのリスト", axis=0)

インデックスやカラムが一致していないDataFrame同士を連結する場合、 共通のインデックスやカラムでない行や列に NaN を持つセルが作成 されます。


新たに"X"と"Y"のカラムが既存のカラムの上位に追加されていることが確認できます。
df["X"]で"X"のラベルがついているカラムを参照でき、
df["X", "apple"]とすることで"X"カラムの中の"apple"カラムを参照できます。



### 結合
特定の Key を参照してつなげる操作を 結合


### 内部結合
Key列に共通の値がない行は破棄されます。
andas.merge(df1, df2, on=Keyとなるカラム, how="inner")
左側のDataFrameに属していたカラムには_x、右側に属していたカラムには_yが接尾詞としてつけられます


### 外部結合

Key列に共通の値がない行も残ります。
pandas.merge(df1, df2, on=Keyとなるカラム, how="outer")

### 同名でない列をKeyにして結合する
pandas.merge(左側DF, 右側DF, left_on="左側DFのカラム", right_on="右側DFのカラム", how="結合方法")

### インデックスをKeyにして結合する
left_index=True, right_index=Trueで指定

### 要約統計量を得る
df.describe()はdfの列ごとの 個数 、 平均値 、 標準偏差 、 最小値 、四分位数 、 最大値 


### DataFrameの行間または列間の差を求める

```
# dfの各行について、2行後の行との差を計算したDataFrameをdf_diffに代入してください
df_diff = df.diff(-2, axis=0)

    apple  orange  banana  strawberry  kiwifruit
1       6       8       6           3         10
2       1       7      10           4         10
3       4       9       9           9          1
4       4       9      10           2          5
5       8       2       5           4          8
6      10       7       4           4          4
7       4       8       1           4          3
8       6       8       4           8          8
9       3       9       6           1          3
10      5       2       1           2          1
    apple  orange  banana  strawberry  kiwifruit
1     2.0    -1.0    -3.0        -6.0        9.0
2    -3.0    -2.0     0.0         2.0        5.0
3    -4.0     7.0     4.0         5.0       -7.0
4    -6.0     2.0     6.0        -2.0        1.0
5     4.0    -6.0     4.0         0.0        5.0
6     4.0    -1.0     0.0        -4.0       -4.0
7     1.0    -1.0    -5.0         3.0        0.0
8     1.0     6.0     3.0         6.0        7.0
9     NaN     NaN     NaN         NaN        NaN
10    NaN     NaN     NaN         NaN        NaN
```

## Matplotlibによるデータの可視化