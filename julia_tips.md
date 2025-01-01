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








