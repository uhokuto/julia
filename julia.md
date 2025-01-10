# juliaプログラミング

### 1. [配列の作成と操作](https://qiita.com/A03ki/items/007be353411d19952ef7)  
- 配列のindexは、0でなくて1から始まることに注意
- 配列の各要素にブロードキャストする処理はかならず . が入る。
- juliaでは、vectorは縦ベクトル（数式と同じ）。[1,2,3]のようにカンマ区切りにするとベクトルになる
- [1 2 3]だとarray, もしくは1行のmatrix となる
- 多次元配列は、行が次元方向、列がデータ方向がjuliaのデフォルト [こちら](https://zenn.dev/hyrodium/articles/3fa3882e4bca04)が参考になる
- スライシングは、A[:,10:100]のように、numpyと同様な構文だが、A[:,10:]はエラーになる。A[:,10:end] と書く
- 多次元配列を初期化するには、Array{Float64}(undef, 2, 3)　2行3列 [こちら](http://www.nct9.ne.jp/m_hiroi/light/juliaa03.html)も参考
- numpyとjuliaの配列を対照したものは[こちら](https://qiita.com/take_me/items/6933e61714c5359338e0)
- 配列のデータ型変換 labels = convert.(Int,Matrix(labels_df)) . に注意！　配列の各要素にブロードキャストする処理はかならず . が入る。
- 同様に、配列の標準化はこんな感じ　Y = (data .- mean(data, dims=2)) ./  std(data, dims=2)  
- index抽出(np.where) : index = findall(x -> x  == l, labels)
- numpyのreshapeでは、x.reshape(-1,1) のように-1を使うが、juliaでは reshape(x,(:,1))のとおり、：を使う  


### 2. [ベクトル⇔行列変換](https://qiita.com/mametank/items/e7f687ad7a2e468c7403)
- 行列dataをベクトルvに変換するには、v=vec(data), v=vcat(data...)でOK。行方向が次元方向のため、dataが2x10 ならば、1行目のデータのおしりに2行目が連結するのでなくて、1列目のデータ2つの次に2列名のデータ。。。というように10列目まで繰り返し連結されてベクトル化される
- これをもとの行列に復元するにはreshape(v, 2,5)でOK



### 3. [トレース,転地,逆行列など](https://qiita.com/ttabata/items/5abe14d5c510e8ca0f54)
- [行列式,対角行列など](https://qiita.com/WolfMoon/items/49478603ee0c3498743e)

### 4. [ライブラリインストール](https://programing.style/archives/524)
[こちら](https://qiita.com/skiing_LAL10/items/0c0132a34629fbc8a91f)も参考。

### 5. [DataFrame](https://qiita.com/ttabata/items/51446731a703b5aca524)
- [こちら](https://r-de-r.github.io/stats/Julia-stats3.html)は非常に詳しい
- スライシングは、df[:,20:end]のようにほとんどnumpyと同じ(endが入る。インデックスは1から)
- [実装例](test_autograd.jl):データセットは、次元方向が行方向なので転置している
- ある列でソートするコーディング  
   例：df = sort!(df, [order(:labels)])
  　"labels"という列でソートする。なぜか:labelsとしないとエラーになる 
- dataframeを2次元配列から作成
```julia
df = DataFrame(data, [:X1, :X2]) # dataは2次元配列　2列とする。X1, X2がcolumnsに相当
```
- dataframeの連結
```julia
df2 = hcat(df, df1) # 行方向に連結ならvcat
```
- dataframeの書き出し
```julia
CSV.write("./dataset/df.csv", df)
```

### 6. [行列の積とブロードキャスト](https://genkaiphd.hatenablog.com/entry/2020/12/06/150628)
- Aを行列とする。exp(A)なら多変数指数関数、exp.(A)ならAの要素に対するべき乗
- ベクトルの内積は[こちら](https://evrythingonmac.blogspot.com/2018/09/julia-numpy.html)
- [こちら](https://qiita.com/haru1843/items/3956dab2fd0d448cd02b)も参考になる

### 7. [三角行列](https://qiita.com/haru1843/items/3956dab2fd0d448cd02b)

### 8. [関数function](https://qiita.com/phigasui/items/0f1becf4555d7f1fe728)
- 戻り値をreturnのように明治しなくてよい。関数の最後の式の評価値が自動的に戻り値になる

### 9. [配列の繰り返し](https://stackoverflow.com/questions/24846899/tiling-or-repeating-n-dimensional-arrays-in-julia)
- numpyではtileに相当
- [無名関数](https://qiita.com/hidemotoNakada/items/d3ef24236d5ef6ea5bc5)

### 10. [行列の行・列方向の合計・平均など](https://qiita.com/take_me/items/6933e61714c5359338e0)
- dataが10x50の行列とすると、 mean(data,dims=2)は、列方向の平均(10次元)　 mean(data,dims=1)は行方向の平均（50次元）
- mean!() という[関数](https://docs.julialang.org/en/v1/stdlib/Statistics/)もあるが、複雑な割に上記と同じなので不要かも

### 11. [forループ](https://qiita.com/ttabata/items/2a53825101b0b75fb589)
- elif -> elseif 、再度はend で閉じる、for (i,n) in enumerete(x) のように()でくくる　あたりが異なる

### 12. [matplotlib](https://www.eureka-moments-blog.com/entry/2020/09/06/102455#2-Julia%E3%81%ABPyCall%E3%81%A8Matplotlib%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
- python matplotlibを呼び出して使うのが簡単。julia独自な[グラフライブラリ](https://zenn.dev/ohno/articles/8d1de713faf2dc)もあるが、グラフの重ね合わせなどで不明なエラーがでるので使いにくい。

### 13. SVD
U,S,V = svd(transpose(Y))  juliaとしては例外的に、行方向がデータ、列方向が次元であるようなデータを引数にとる。 U はデータ数×次元数　(numpy svdと値は一致する)

### 14. using （重要）
異なるライブラリの同名のmethodを使うときに vs codwを再起動しないと、ライブラリを開放しないことがある
 
### 15. [辞書型](https://qiita.com/bonten999/items/e5d0edf9bc4563584e3c)

### 16. [内包表記](https://qiita.com/keta-sts136/items/c079aab0b4edf29c0472)
なかなか網羅的な記事がないので以下の例は重要
1. labelと一致するindexをlabelsから取り出す。キー=label : value=indexの配列　pythonでは : をjuliaでは => とする
```julia
unique_l = unique(labels)
c_idx = Dict(l => findall(x -> x  == l, labels)  for l in unique_l)
```
2. クラスcに対応するindexでデータxから取り出し、クラス内平均-mean_i[c]を引いて　key=c : value=x[:,idx] .-mean_i[c]とする

```julia
xi_diff = Dict(c => x[:,idx] .-mean_i[c] for (c, idx) in c_idx)
``` 

### 17. 辞書型のvalueが配列であるような場合のvalueの処理
1. 辞書型から配列形式のvalueを取り出す構文は　values(xi_diff)
2. このvalueは辞書型のため、ベクトルに型変換するには　xi_diff_array = collect(values(xi_diff)) 以下の例は、valueに2次元配列が入っているケース  
```
xi_diff_array=collect(values(xi_diff))
4-element Vector{Matrix{Float64}}:
 [983.6206376816492 1054.9065783543588 … 1196.8800924883187 887.3513493620744; -656.0456674349306 -59.804967454423185 … -622.9205235263146 -606.061044368608]
 [208.2489536253116 726.8079465389807 … 118.02321334075634 493.1044935278291; 973.6915689886978 709.9452343281241 … 902.7602372510216 869.7194556048279]
 [21.220253069526795 873.2788773798554 … 552.667766789454 362.9354370794696; -504.8348346172914 -609.4468939929411 … -576.3044088195217 -885.3073713077102]
 [-1015.1643932900632 -1041.390527674118 … -945.3945106922858 -1088.638945760477; -299.4559609742 -233.3694368905333 … -118.04418886605212 -167.2466713132841]
 ```

 3. キーが4種類なので、2次元配列のvalueを4つもつようなvectorになる。これらの2次元配列をconcatして１つの2次元配列にするには、 xi_diff_val = hcat(xi_diff_array...)　　hcatは（）に連結したい配列をカンマ区切りで並べるが、このケースでは、連結したい配列がxi_diff_arrayのベクトル要素に入っているので、xi_diff_arrayが複数の配列のように見なすために ... をつける。上の例だと以下のようになる。

 ```
  hcat(xi_diff_array...)
2×500 Matrix{Float64}:
  983.621  1054.91   1187.84    699.161   882.344   831.768  …  -869.383  -960.826  -987.048  -945.395  -1088.64
 -656.046   -59.805  -696.326  -614.382  -480.307  -563.091     -123.052  -239.534  -149.523  -118.044   -167.247
```

### 18. 配列index
np.whereに相当するindexには、CartesianIndex型と普通のvector型の2通りがあり、どちらも、配列に対してこのindexに対応する要素を取り出せる。（次元方向が行、データ方向が列のようなデータに対しても、ちゃんと取り出せる）　以下、理由はわからないが、この2つのindex型に関する重要な注意点。  

labels = df[:,1:1]として、findall(np.whereに相当）するとindexは、CartesianIndex型になる。
CartesianIndex型の配列をint のindex配列に変換するには以下。
```
indices = hcat(findall(x -> x  == label, labels))
[ convert.(Int,i[1]) for i in indices]
```

df[:,1:1]の代わりにdf[:,1]とやると、dataframeでなくて、値の部分がvectorで取り出される。
これでfindall(np.whereに相当）するとなぜか、CartesianIndex型のindexでなくて、整数型のindexになる！　これを次元方向が行方向、データ方向が列方向であるような行列に適用するとうまくindexでスライシングできる



