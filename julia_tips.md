## 1 . julia からpythonをimportするpyCallのパス変更

  using PyCall を宣言すると、julia でpythonライブラリを使える。この際、デフォルトではjuliaインストール時に自動的にインストールされるminiconda（juliaのパス内に入る）にパスがはられていて、これを呼び出す。バージョンも最新ではないため、例えば、juliaで書き出したpickleファイルを、最新のanaconda環境のpythonプログラムから読み込もうとすると、バージョンの違いでエラーになったりする。

この問題に対処するため、PyCallで読み込むpythonへのパスを内蔵のminicondaから、anacondaに変更する方法がある。以下その手順  

(1) vs conde のターミナル julia プロンプトで以下のように入力

julia> using PyCall

julia> ENV["PYTHON"]="/anaconda/bin/python" 

　　　" "内は、anaconda内のpython.exeへのパス。

 julia> Pkg.build("PyCall")　PyCallのデフォルトpythonパスをリコンパイル

参考リンク　[conda パッケージを Juliaから追加しよう #Anaconda - Qiita](https://qiita.com/tenfu2tea/items/d2ac1427eaed7a548287)

1. 呼び出す Python パスの修正のコード（仮想環境を呼び出すのではないので、このコードのrm(Pkg.dirは不要）

(2) juliaを再起動

ターミナルでjuliaを再起動する。

Restarting the REPL in VSCode is easy. **Press Alt + J and then Alt + R**.



## 2. juliaからpythonの自作関数を呼び出す方法

同じディレクトリにtest_impourt.pyを作り、その関数名def test(a,b)を呼び出す構文は以下の通り



```julia
pushfirst!(PyVector(pyimport("sys")["path"]), @__DIR__)
a=2
b=3
result = pyimport("test_import")[:test](a,b)
println(result)
```

```python
def test(a,b):
    return a + b
```

参考リンク

[Problem importing python file from current directory · Issue #502 · JuliaPy/PyCall.jl · GitHub](https://github.com/JuliaPy/PyCall.jl/issues/502#issuecomment-433285447)

[Juliaで自作Pythonパッケージを呼び出す手法 #Python - Qiita](https://qiita.com/takahashi-ry/items/3ff9af58fc3c9175574a)



## 3. Juliaでpickleを読み書きする

```julia
using Pandas

kern_params = read_pickle("./dataset/kernel_params.pickle");
```

のようにJuliaのPandasライブラリでコーディングするのが、最もシンプル。ただしこれを使うと、DataFrameを使うときに、using DataFramesとusing Pandasとでバッティングするので、以下のように、DataFrames.DataFrameのように明示する必要がある。

``` julia
latents_bar = DataFrames.DataFrame(load(File(format"CSV", "./dataset/latents_gplvm.csv"); delim=',', header_exists=true))
```

参考リンク  

[How to load python pickle from Julia? - Stack Overflow](https://stackoverflow.com/questions/65720584/how-to-load-python-pickle-from-julia)



一方、pickleの書き出しは、以下のようにpython pickleを使った方がよさそう

```julia
using PyCall
@pyimport pickle

out = open("dataset/kernel_params.pickle","w")
pickle.dump(kern_params, out)
close(out)
```




