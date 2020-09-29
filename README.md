# experimental-mathematics2020
2020年度実験数学C(第3ターム)及びD(第4ターム)のサンプルプログラム

**皆本晃弥『C言語による数値計算入門 解法・アルゴリズム・プログラム (UNIX & Information Science)』サイエンス社, 2005**
のサンプルプログラムをC言語およびPythonで実装しています。

## C言語のコンパイル方法及び実行方法について
program.cファイルのコンパイルはgccコマンドを使って以下のように行います。
```bash
$ gcc -o program.exe program.c
```
すると`program.exe`という名前の実行ファイルができます(名前は任意に指定できます。拡張子も必ずしもつける必要はありません)。<br>
ただし、mathライブラリなどを利用している場合(ファイルの最初に`#include<math.h>`とある場合)は`-lm`オプションが必要となるので注意です。<br>
すなわち、以下のようにしてコンパイルします。
```bash
$ gcc -o program.exe program.c -lm
```
このようにしてできた実行ファイルは以下のようにして実行することができます:
```bash
$ ./program.exe
```

## Pythonプログラムのバグについて

Pythonプログラムは未完成です。主にバグを含むプログラムを以下に列挙します。

|  プログラム名  |                バグ概要               |
| :-----------: | :----------------------------------: |
| program3_3.py |        テキストと出力結果が異なる       |
| program3_4.py |        テキストと出力結果が異なる       |
| program4_5.py | program3_2.pyが上手くインポートできない |

### テキストの誤植(？)

#### プログラム4.3

プログラム4.3の実行結果が、テキストでは
```
初期値x0を入力してください
1.1
答えは x=1.000000 で, 重複次数は 2.000085 です
```
となっていますが、C言語、Python共に実行結果は
```
初期値 x0 を入力してください
1.1
答えは x=1.000000 で, 重複次数は3.841109です
```
となります。

#### プログラム9.3

プログラム9.3の実行結果が、テキストでは
```
QR法の結果は
-24.1485206      0.8743997       1.1467835       1.9115289
-0.0000000      21.5793450       0.0034205      -2.6903577
 0.0000000      -0.0000000      14.3382192      -2.4552665
 0.0000000       0.0000000       0.0000000      12.2309564
固有値は
-24.1485206     21.5793450      14.3382192      12.2309564
```
となっていますが、C言語、Python共に実行結果は
```
QR法の結果は
-24.1485206     -0.8743997      -1.1467835       1.8344712
-0.0000000      21.5793450       0.0034205       2.7408570
 0.0000000      -0.0000000      14.3382192       2.4581943
 0.0000000       0.0000000      -0.0000000      12.2309564
固有値は
-24.1485206     21.5793450      14.3382192      12.2309564
```
となります(正負符号に加え4列目の値も少しだけ異なっています)。


C言語のプログラムは一応見返しましたが誤りは見つかっていないので、誤植の可能性も大いにあり得ます。
念のため確認してください。

## 未実装のPythonプログラムについて

また、未実装のプログラムもあり、理由は上のプログラムに含まれる関数を使うため、上記のプログラムが完成しない限り、
上手く動作する見込みがないためです。未実装のプログラムと、問題となっている依存関係を記述しておきます。

● program8_6.py
- [インポート問題] program4_5.py 同様, program3_4.pyが上手くインポートできない。
- [依存するプログラム] program3_4.py (`cholesky_decomp`, `cholesky_solve` 関数)

● program9_4.py
- [インポート問題] program4_5.py 同様, program3_3.pyが上手くインポートできない。
- [依存するプログラム] program3_3.py (`lu_decomp`, `lu_solve` 関数)

上記のプログラム以外はPythonプログラムの方は動作確認済みです。~~もし時間があればC言語プログラムの動作確認もしていただけると有難いです。~~
**C言語の動作確認も一通り終わりました**(2020/09/28)。
また、把握していないバグ等発見しましたら報告、またはPull requestsをお願いします。

## 実装の仕様について

最後にバグを修正するために役に立つかもしれない点や、実装の意図について書き留めておきます。

## inputファイルの違い(※重要)

Pythonによる実装に際して、Pythonの`input`関数(や、`file.readline`メソッド)とC言語の`scanf`関数の違いにより、一部`input.dat`ファイルの様式を変更させていただいています。
> これによるC言語プログラムへの影響はありませんが、元の様式でPythonのプログラムを実行すると、エラーが生じます。これはC言語が数値1つ1つに対する入力を持つのに対し、Pythonが
> 行ごとに入力を受け取るためです。

- `ch02/input.dat` -> 変更なし
- `ch03/program3_1/input.dat` -> 変更なし
- `ch03/program3_2/input.dat` -> **変更あり**

【テキスト】
```
2.00 4.00 1.00 -3.00
-1.00 -2.00 2.00 4.00
4.00 2.00 -3.00 5.00
5.00 -4.00 -3.00 1.00

 0.00
10.00
 2.00
 6.00
```
【本ファイル】
```
2.00 4.00 1.00 -3.00
-1.00 -2.00 2.00 4.00
4.00 2.00 -3.00 5.00
5.00 -4.00 -3.00 1.00

0.00 10.00 2.00 6.00
```
- `ch03/program3_3/input_lu.dat` -> 変更なし
- `ch03/program3_4/input_cho.dat` -> 変更なし
- `ch05/output_sp.dat` -> 変更なし
- `ch06/input_func.dat` -> 変更なし
- `ch06/input_lag.dat` -> 変更なし
- `ch09/input_eigen.dat` -> **変更あり**

【テキスト】
```
16.00	-1.00	 1.00	 2.00
 2.00	12.00	 1.00	-1.00
 1.00	 3.00	-24.00 2.00
 4.00	-2.00	 1.00	20.00

0.50
0.50
0.50
0.50
```
【本ファイル】
```
16.00	-1.00	 1.00	 2.00
 2.00	12.00	 1.00	-1.00
 1.00	 3.00	-24.00 2.00
 4.00	-2.00	 1.00	20.00

0.50 0.50 0.50 0.50
```

### Dvector, Dmatrixクラス(program2_1.py, program2_2.pyで定義)を用いて実装した理由
  テキストではベクトルや行列の添字を自由に操作できるようにと、dvector, dmatrixが実装されています。簡単に説明すると、Pythonのリストのインデックスは<br>
 インデックス:  0 1 2 3 4 5 6 <br>
    リスト  : [3,1,4,1,5,9,2] <br>
つまり、a = [3,1,4,1,5,9,2] とすると、a[0]で3が得られます。テキストのやりたいことは、インデックスの最初と最後の番号を指定して、 <br>
 インデックス:  3 4 5 6 7 8 9 <br>
    リスト  : [3,1,4,1,5,9,2] <br>
のように設定し、a[3]で3が得られるような仕様にしたいとの話です。行列も同様にインデックス番号を自由につけたいみたいです(最初のインデックスを1にしたいっていうのが
本質的な意図だとは思います)。
関数で管理する方法も考えましたが、a(3)のような呼び出しになるため、直感的にリストのような使い方ができないため、以降のプログラムで繰り返し使うDvectorやDmatrixは
クラスとして定義し、(スライスとかはできませんが)ほとんどリストと同じ感覚で書けるようにしました。もっと良い実装方法があれば提案してください。

### インポート問題の解決案
ディレクトリの構造が現状の最も大きなネックになっているので、最悪今のディレクトリ構造をなくしてすべてのプログラムを同じディレクトリに置き、入力・出力ファイルを置くディレクトリ
を作ることで一応は解決できると思います。ただ、できるだけディレクトリ構造はこのままにしておきたい(プログラムの所在が分かりやすい)ので、このままのディレクトリ構造で解決できれば
お願いします。

### グローバル変数問題
もしかしたらバグがグローバル変数から生じている可能性があります。たとえば
```python
# test1.py
N = 5
def addN(a):
    return a + N
```
というプログラムの`addN`関数をtest2.pyで利用するとき
```python
# test2.py
from test1 import addN
N = 6
def main():
    print(addN(1))
```
と書いた場合、`1`に足されるのはtest2.pyの`N`(=`6`)ではなく、test1.pyの`N`(=`5`)です。各プログラムでグローバル変数を用いているため、意図しない値を用いた計算が
行われいている可能性もあります。この問題を解消する一つの方法は、test1.pyの`addN`関数を
```python
def addN(a, N=N):
    return a + N
```
として、第二引数を取り、デフォルト値としてグローバル変数のNを取る関数に書き換え、test2.pyで`print(addN(1,N))`のように利用することで、`N`をtest2.pyでの`N`とすることができます。

### C言語とPythonの浮動小数点数の型の違い

これが原因でバグが生じるとはあまり思っていませんが、C言語での`float`型はPythonだとNumPyで提供されている`float32`型に対応し、C言語の`double`型はPythonでの`float`型ないし
NumPyの`float64`型に対応します。Pythonの`float`はC言語の`double`なので、テキストより精度が高い計算をしている可能性はなくもないです。しかし、計算に大きな影響を与える問題では
ないと思うので、これで問題が解消される見込みはあまりないと思います。

### 副作用を含む関数

テキストのプログラムでは多くの副作用を含む関数が定義されています。**副作用**(side effect)について簡単に説明すると、たとえば、ある変数`n`に`1`を加える関数を作るとき、
```python
def add1(n):
    return n + 1
```
のように定義すれば、
```python
n = 2
print(add1(n))
print(n)
```
としたとき、`print(n)`での出力結果は元の`n`の値`2`が出力されます。`n`自体の値は変更**しません**。しかし、テキストのプログラムでは
```python
def add1(n):
    n = n + 1
```
のように、`n`を上書きする形で所望の値を得る関数が度々登場します。この場合、
```python
n = 2
add1(n)
print(n)
```
とすると、`print(n)`での出力結果は`n`に`1`が加えられた`3`となります。このように、関数を呼び出す度に外部に何らかの(例えばinput関数での入力やprint関数での出力なども含める)
影響を与える関数は**副作用を持つ**と言います(不正確ではありますが、基本的に数学としての関数として書けないものは副作用を持つと考えてもらってよいです)。
Pythonプログラムの方では、簡単に副作用を解消できる関数に関しては副作用を含まない形に書き換えているものもあり、この実装の仕方が
不適切でバグを生んでいる可能性もあります。もしテキスト通りの実装で上手く行けば、最悪そちらでも良いので、報告お願いします。
