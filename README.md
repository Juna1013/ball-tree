# Ball Tree 🌳

Ball Tree は、近似なしの厳密な k 近傍探索 (k-NN) を高速化するデータ構造 **Ball Tree** を、NumPy だけでゼロから実装して動きを可視化する Jupyter Notebook です。

点群を「球」で再帰的に分割し、クエリから見て k 番目に近い点より確実に遠い球を丸ごとスキップ (枝刈り) することで、総当たりより少ない距離計算で正確な近傍を求めます。

## What's inside 📓

[main.ipynb](./main.ipynb) は上から順に実行すると以下の流れで進みます。

1. **データと正解の用意** - 3 つの塊を持つ 2 次元データ (210 点) と、総当たりによる k-NN (正解)
2. **ノードの定義** - 点の集合、球の中心・半径、深さ、子ノードを持つ `Node`
3. **構築** - 最も広がりの大きい軸で中央分割し、各ノードを覆う球を作る `build`
4. **構築結果の描画** - 深さごとの球の入れ子構造を可視化
5. **k 近傍探索** - 球までの距離の下限による枝刈りと、近い子から先に探索する `knn`
6. **探索過程の描画** - 調べた葉 (赤) と枝刈りされた球 (青の破線) を可視化
7. **検証** - 総当たり・scikit-learn の `BallTree` と結果を比較し、ランダムなクエリ 200 個でも一致を確認
8. **条件を変える実験** - 次元数を 2 → 50 と増やしたときの距離計算回数の変化 (次元の呪いの確認)

## Setup locally 🖥️

ローカルで Notebook を動かす場合は以下の手順に従ってください。

### Attention

- 事前に [Python](https://www.python.org) 3.9 以上をインストールしておく必要があります。
- 依存ライブラリは [NumPy](https://numpy.org)、[Matplotlib](https://matplotlib.org)、[scikit-learn](https://scikit-learn.org) (検証用)、[Jupyter](https://jupyter.org) です。

### 1. clone git repository

```bash
git clone "https://github.com/Juna1013/ball-tree" && cd "./ball-tree"
```

### 2. create virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. install dependencies

```bash
pip install numpy matplotlib scikit-learn jupyterlab
```

### 4. open the notebook

```bash
jupyter lab main.ipynb
```

VS Code を使う場合は `main.ipynb` を開き、カーネルに `.venv` の Python を選択して `Run All` を実行してください。

## Result 📊

デフォルトの設定 (210 点・k = 5) では、総当たりと同じ結果を、より少ない距離計算で得られます。実際の距離計算回数と枝刈りされたノード数は、探索過程の描画セルの出力で確認できます。

一方で、次元数が大きくなると球が重なり合って枝刈りが効かなくなり、総当たりに近づきます。最後の実験セルでその様子を確認できます。
