# Data_structures_and_algorithms

データ構造とアルゴリズムについて講義で学んだことなどをメモとして残したいと思う。

## 参考資料

本稿の作成に当たっては柴田望洋氏の"新・明解 Javaで学ぶアルゴリズムとデータ構造"を参照した。ソフトウェアに携わるすべてのエンジニアに必要なアルゴリズムの知識について体系的に学べる書籍である.是非参考にしていただきたい。

## Call by Refernce と Call by value

Call by Refernce（参照渡し） と Call by value（値渡し）は引数の渡し方を表す。以下にその例を示す。
 - 基本データ型：char,int,float,doubleなど
 - 参照型：配列、インスタンスなど

 - Call by value：基本データ型
 - Call by Refernce：参照型

Call by Refernceの例を以下に示す。

    int a[]={1};
    int b[]=a;
    b[0]=2;
    system.out.println(a[0]);

この例の場合は２が出力される。
このように、Call by valueは変数の値を直接渡すことに対して、Call by Refernceはアドレスを渡すという違いがある。
上記の例では'b[0]=2;'によって配列aのアドレスそのものが書き換えられている。

## 探索

探索とはある条件を満たすデータを探し出すことである。
探索は何らかの項目に着目する。この着目する項目のことをキー(key)と呼ぶ。
探索方法には以下のような物がある。
 - 線形探索
 - 二分探索
 - ハッシュ法
   - チェイン法
   - オープンアドレス法

## 線形探索(逐次探索)

線形探索は要素が直線上に並んだ配列から、目的とするキー値をもつ要素に一致するまで先頭から順番に要素を走査するこで実現する。
以下にその例を示す

    while(true){
      if(i==n) return -1;
      if(a[i]==key) return i;
      i++
    }

## 番兵法

線形探索は繰り返しのたびに２つの終了条件をチェックしているため、ムダが多い。そこでアルゴリズムの改良を行い、コストを半分に抑えるのが次に紹介する番兵法である。
番兵法は探索の前に、探索するキー値と同じ値を末尾の要素に格納する。このとき、格納するデータを番兵とする。このようにすると、目的の値が存在しなくても、終了条件が成立するため、繰り返しの終了判定を減らすことが出来る。以下に例を示す。

    a[n]=key;
    while(true){
      if(a[i]==key)
        break;
    i++;
    }
    return i==n ? -1 : i;

## 二分探索

二分探索は要素がキーの昇順もしくは降順にソートされている配列から探索を行うアルゴリズムである。以下にその手順を示す。

 1. 昇順または降順にソート
 1. 配列の中央に着目
 1. 探索対象の絞り込み
    1.  a[pc]<keyのとき、plをpc+1
    2.  a[pc]>keyのとき、prをpc-1
 1. 終了条件
    1. a[pc]とkeyが一致
    2. 探索範囲がなくなる。

### a[pc]<keyのとき

a[pl]~a[pc]はkeyよりも小さいことが明らかである。よって、探索対象外の探索範囲は中央a[pc]より後方のa[pc+1]~a[pr]になる。そこで、plの値をpc+1に更新する。

### a[pc]>keyのとき

a[pl]~a[pc]はkeyよりも大きいことが明らかである。よって、探索対象外の探索範囲は中央a[pc]より前方のa[pl]~a[pc-1]になる。そこで、prの値をpc-1に更新する。

### 終了条件

1. a[pc]とkeyが一致
2. 探索範囲がなくなった。

よって、__pl<=pr__ plがprよりも大きくなって探索失敗
以下に二分探索を行うプログラムを示す。

```Java
for (int i=i;i<num;i++){
  do{
    System.out.print("x["+i+"]:");
    x[i]=stdIn.nextInt();
  } while (x[i]<x[i-1]); //１つ前の要素より小さければ再入力
}
```

## 計算量(オーダ)

プログラムの実行速度や実行時間は動作するハードやソフトによって変わるため、アルゴリズムの性能を客観的に評価するための尺度として、用いられるのが計算量である。
計算量は以下の２つに分かれる。

1. 時間計算量  
   実行に要する時間を評価したもの
2. 領域計算量  
   どのくらいの記憶域やファイル域が必要であるかを評価したもの

この他に各ステップの実行回数のオーダを算出する __実行回数__ などがある。

以下に線形探索における各ステップの実行回数とそのオーダを示す。

||線形探索|実行回数|オーダ|
|:--:|:--:|:--:|:--:|
|1|int i=0;|1|O(1)|
|2|while(i < n){|$\frac{n}{2}$|O(n)|
|3| if(a[i]==key)|$\frac{n}{2}$|O(n)|
|4|   return i;|1|O(1)|
|5|   i++;|$\frac{n}{2}$|O(n)|
|6|return -1;|1|O(1)|

線形探索のアルゴリズムのオーダを求めると、以下の様になる。

O(1)+O(n)+O(n)+O(1)+O(1)=O(n)

__このように、２つの計算からなるアルゴリズムは、より大きい方の計算量に支配される。３つ以上も同様に最も大きい計算量に支配される。__

以下に計算量と増加率の図を示す。

![計算量と増加率](/img/geogebra-export.png) 

## ハッシュ法

### ソート済み配列への追加

ソート済みの配列にデータを追加する手順を以下に示す。
  - 挿入したいデータの位置を二分探索する
  - その位置以降の全要素を1つずつずらす。
  - 挿入する。

削除も同様に行うことができる。要素の移動に要する計算量は$O(n)$と大きくなっている。

## ハッシュ法

> ハッシュ法は探索だけでなく、データの追加や削除を効率良く行うための手法である。データを格納する位置を単純な演算で求めることにより実装する。

以下に示すのは配列のキー値を配列の要素数13で割った余りのキー値とハッシュ値の対応である。

|キー値|ハッシュ値（13で割った余り）|
|:--:|:--:|
|2|2|
|8|8|
|11|11|
|20|7|
|23|10|
|29|3|
|45|6|

- ハッシュ値  
データをアクセスする目印
- ハッシュ関数  
例)キーを素数で割った余り
- ハッシュ値を参照  
データ挿入
- バケット  
ハッシュ法の各要素
ハッシュ値がインデックスとなるように、キー値を格納した配列を __ハッシュ表__ という。ハッシュ表を以下に示す。

![ハッシュ表]](/img/hassyu_add.png) 

図のように、データの追加に伴って要素をずらす必要がないため、高速にデータ挿入を行う事ができる。データ追加のオーダは$O(1)$

### 衝突(collision)

データを挿入する際に、既にバッケトが埋まっている場合、格納する場所が重複する。これを __衝突__ という。  
このような衝突が起こらないように、ハッシュ関数はできるだけ、ハッシュ値が偏らないように決めなければいけない。

解決方法として、以下の2つが挙げられる。
- チェイン法
- オープンアクセス法

### チェイン法

>チェイン法は同一のハッシュ値を持つデータの線形リストである。チェイン法の配列は線形リストの先頭アドレスを格納している。

以下にチェイン法によるハッシュ法の図を示す。

![チェイン法](/img/tyeinn.png)

#### チェイン法（要素の挿入）

>42を挿入すると仮定する。42のハッシュ値は3であり、ハッシュ表のtable[3]には29と16を連結したリストへのアドレスが格納されている。このリストには42が存在しないため、リストの先頭に42を挿入する。これは42を格納したノードを新たに生成し、ノードの参照をtable[3]に入れる。さらに、挿入したノードの後続ノードへのアドレスが29を格納したノードになるように変更する。  

要素の挿入の手順は以下のようになる。
- ハッシュ関数によってキー値をハッシュ値に変換する。
- ハッシュ値をインデックスとするバケットに着目する。
- バケットが参照する線形リストを先頭から順に線形探索し、キー値と同じ値が見つかればキー値は登録済みで挿入失敗。探索して見つからなけれはリストの先頭位置にノードを挿入する。

## オープンアドレス法

>__オープンアドレス法__ はハッシュ法で衝突が発生した際に、__再ハッシュ__ を行うことによって、空いているバケットを探し出す手法である。__クローズドハッシュ法__ とも呼ばれる。  

### 要素の挿入
衝突が発生すると再ハッシュを行うが、再ハッシュを行うためのハッシュ関数は自由に決めることが出来る。主にキー値に1を加えて再ハッシュを行う事がある。  
ただし、この方法の場合、一度衝突が発生すると連鎖的に衝突が起こり、塊ができてしまう。  
オープンアドレス法は空きバケットに出会うまで再ハッシュを繰り返すため __線形探索法__ と呼ばれる。

## ジェネリクス

>型に依存しないクラスやメソッドを型安全な手法で実現する。ジェネリクスなクラスとインターフェースはクラス名やインターフェースの直後に<Type>といった形式のパラメータをつけて宣言する。


```Java
  class クラス名 <型変数> {}
  interface インターフェース名 <型変数>{}
```

これらは引数として __型__ を受け取るので処理の対象となるオブジェクトの型に依存しない。

# データ構造

## スタック

>スタックはデータを一時的に蓄えるためのデータ構造である。
データの出し入れは __後入れ先出し__ (LIFO/Last In First Out)で行われる。つまり、最後に入れたデータが最初に取り出される。  
スタックにデータを入れる操作を __プッシュ__ (push)  
スタックからデータを取り出す操作を __ポップ__ (pop)と呼ぶ。  
プッシュとポップが行われる側を __頂上__ (top)とし、その反対側を __底__ (bottom)とする。

### スタックの実現
スタックを実現するプログラムを以下に示す。

```Java
//--- コンストラクタ ---//
	public IntStack(int capacity) {
		ptr = 0;
		max = capacity;
		try {
			stk = new int[max];	// スタック本体用の配列を生成
		} catch (OutOfMemoryError e) {	// 生成できなかった
			max = 0;
		}
	}

```

```Java
//--- スタックにxをプッシュ ---//
	public int push(int x) throws OverflowIntStackException {
		if (ptr >= max)	// スタックは満杯
			throw new OverflowIntStackException();
		return stk[ptr++] = x;
	}
```
受け取ったデータxを配列の要素stk[ptr]に格納し、インクリメントする。返り値はプッシュした値である。

※等価演算子(==)ではなく関係演算子(>=)を利用する意味

プログラムのミスにより、ptrが書き換えられた場合、ptrがmaxより大きくなる可能性がある。配列の上限を超えたアクセスを防ぐ役割がある。

```Java
//--- スタックからデータをポップ（頂上のデータを取り出す） ---//
	public int pop() throws EmptyIntStackException {
		if (ptr <= 0)	// スタックは空
			throw new EmptyIntStackException();
		return stk[--ptr];
	}
```
スタックの頂上からデータをポップし、その値を返す。スタックポインタptrの値をデクリメントしてからstk[ptr]に値を返す。


